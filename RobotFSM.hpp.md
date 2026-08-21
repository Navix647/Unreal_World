```cpp
#include "robot_states_defs.hpp"
#include "robot_events_defs.hpp"

namespace crobot{

namespace fsm{
class RobotFSM{
public:
	RobotFSM():mCurrentState(Unready{}){
	}
	// 在 RobotFSM 类中实现
	template<typename Event>
	void dispatch(Event&& event) {
	 // 使用 std::visit 访问当前状态
	 current_state_ = std::visit(
	 [&](auto&& current_state) -> State {
	 using Current = std::decay_t<decltype(current_state)>;
	 using EventType = std::decay_t<Event>;
	 // =========================================================
	 // 1. 【Unready（未就绪）】—— 只处理初始化事件
	 // =========================================================
	 if constexpr (std::is_same_v<Current, Unready>) {
	 // 场景：伺服使能 + 回零成功 -> 进入 Idle
	 if constexpr (std::is_same_v<EventType, ServoReady>) {
	 // 动作：记录日志，通知上位机“伺服已使能”
	 spdlog::info("Servo enabled, waiting for homing...");
	 // 注意：这里先不跳转，等待 HomingSuccess
	 return current_state; 
	 }
	 else if constexpr (std::is_same_v<EventType, HomingSuccess>) {
	 spdlog::info("Homing success, system ready.");
	 return Idle{std::chrono::system_clock::now()};
	 }
	 else if constexpr (std::is_same_v<EventType, ServoFaultTriggered>) {
	 spdlog::error("Fault during init: {}", event.error_msg);
	 return Fault{event.error_code, false};
	 }
	 }
	 // =========================================================
	 // 2. 【Idle（空闲）】—— 等待下发任务
	 // =========================================================
	 else if constexpr (std::is_same_v<Current, Idle>) {
	 if constexpr (std::is_same_v<EventType, StartTask>) {
	 spdlog::info("Task {} started.", event.task_id);
	 // 初始进度为0，速度也为0
	 return Running{0.0, 0, false}; 
	 }
	 // 如果 Idle 状态下突然报故障（极少见，但防御）
	 else if constexpr (std::is_same_v<EventType, ServoFaultTriggered>) {
	 return Fault{event.error_code, false};
	 }
	 }
	 // =========================================================
	 // 3. 【Running（运行中）】—— 核心处理区（包含减速避障）
	 // =========================================================
	 else if constexpr (std::is_same_v<Current, Running>) {
	 // ★ 场景 A：检测到障碍物（触发减速，但不切状态！）
	 if constexpr (std::is_same_v<EventType, ObstacleDetected>) {
	 spdlog::warn("Obstacle at {}mm! Start decelerating.", event.distance_mm);
	 // 动作：修改状态内的数据，标记为“正在减速”
	 // 注意：复制当前状态，修改标志位，继续留在 Running
	 auto new_state = current_state;
	 new_state.is_decelerating = true;
	 // 此处可以根据距离计算目标速度，赋值给 new_state.current_speed_rpm
	 return new_state;
	 }
	 // ★ 场景 B：减速完成，速度归零 -> 切入 Holding（保持）
	 else if constexpr (std::is_same_v<EventType, SpeedReachedZero>) {
	 spdlog::info("Speed zero, holding position.");
	 return Holding{current_state.progress, 0};
	 }
	 // 场景 C：障碍物清除，且当前处于减速状态 -> 恢复加速
	 else if constexpr (std::is_same_v<EventType, ObstacleCleared>) {
	 if (current_state.is_decelerating) {
	 spdlog::info("Obstacle cleared, resuming.");
	 auto new_state = current_state;
	 new_state.is_decelerating = false;
	 // 恢复目标速度
	 return new_state;
	 }
	 return current_state; // 没在减速就忽略
	 }
	 // 场景 D：正常任务结束
	 else if constexpr (std::is_same_v<EventType, StopCmd>) {
	 if (event.is_graceful) {
	 spdlog::info("Task gracefully completed.");
	 return Completed{0.0}; // 假设终点位置为0
	 }
	 }
	 // 场景 E：运行中突发硬件故障
	 else if constexpr (std::is_same_v<EventType, ServoFaultTriggered>) {
	 spdlog::critical("Servo fault during motion!");
	 return Fault{event.error_code, false};
	 }
	 }
	 // =========================================================
	 // 4. 【Holding（暂停保持）】—— 避障停稳后的等待区
	 // =========================================================
	 else if constexpr (std::is_same_v<Current, Holding>) {
	 // 场景 A：障碍物移开，恢复运行
	 if constexpr (std::is_same_v<EventType, ResumeCmd>) {
	 spdlog::info("Resuming from hold.");
	 return Running{current_state.progress, 0, false}; // 从断点继续
	 }
	 // 场景 B：等待超时（障碍物一直没走）-> 自动取消任务
	 else if constexpr (std::is_same_v<EventType, HoldTimeout>) {
	 spdlog::warn("Hold timeout! Aborting task.");
	 // 在工业场景，超时通常进入 Idle 并清空任务，或者进入 Fault 防止碰撞
	 // 这里选择回到 Idle，要求上位机重新下发任务
	 return Idle{std::chrono::system_clock::now()};
	 }
	 // 场景 C：用户强制取消
	 else if constexpr (std::is_same_v<EventType, AbortCmd>) {
	 spdlog::warn("User aborted during hold.");
	 return Idle{std::chrono::system_clock::now()};
	 }
	 }
	 // =========================================================
	 // 5. 【Completed（正常完成）】—— 任务收官
	 // =========================================================
	 else if constexpr (std::is_same_v<Current, Completed>) {
	 // 任务完成后，如果收到新任务，重新进入 Running
	 if constexpr (std::is_same_v<EventType, StartTask>) {
	 spdlog::info("Starting next task.");
	 return Running{0.0, 0, false};
	 }
	 // 或者收到复位/空闲指令，回到 Idle
	 else if constexpr (std::is_same_v<EventType, StopCmd>) {
	 return Idle{std::chrono::system_clock::now()};
	 }
	 }
	 // =========================================================
	 // 6. 【Fault（硬件故障）】—— 死锁态，只认复位
	 // =========================================================
	 else if constexpr (std::is_same_v<Current, Fault>) {
	 // 只有复位故障事件能解锁
	 if constexpr (std::is_same_v<EventType, ResetFault>) {
	 if (event.force_reset || current_state.is_recoverable) {
	 spdlog::info("Fault reset, going to Unready for re-homing.");
	 // 注意：故障清除后必须重新回零，所以回 Unready
	 return Unready{false, false};
	 }
	 }
	 // 其他任何事件在 Fault 状态下统统忽略（安全原则）
	 }
	 // =========================================================
	 // 【默认兜底】：如果上面所有的匹配都没命中，原地不动！
	 // =========================================================
	 spdlog::warn("Unhandled event {} in current state, ignored.", typeid(EventType).name());
	 return current_state; 
	 },
	 mCurrentState// 结束 visit
	 );
	}

private:
	RobotState mCurrentState;
};


}


}


```


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQyNjk0NzUzOF19
-->