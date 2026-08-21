```cpp
#pragma once
#include <variant>
#include <string>
namespace robot { namespace fsm {
// ---------- 1. 控制指令事件（来自上位机/示教器） ----------
struct StartTask { 
 int task_id;               // 任务编号
 std::string trajectory_file; // 轨迹文件路径
};
struct PauseCmd {               // 用户主动暂停
 bool is_emergency = false;  // 是否紧急暂停（区别对待）
};
struct ResumeCmd {              // 避障解除/恢复运行
 bool resume_from_hold = true;
};
struct StopCmd {                // 正常停止（清空任务，回Idle）
 bool is_graceful = true;    // 是否走减速停止曲线
};
struct AbortCmd {               // 用户主动取消当前任务
 std::string reason;         // 例如："User cancelled at HMI"
};
struct ResetFault {             // 故障复位（只在Fault状态有效）
 bool force_reset = false;   // 是否强制复位（需密码权限）
};
// ---------- 2. 物理/传感器事件（来自底层驱动） ----------
struct ObstacleDetected {       // ★★★ 避障最关键的事件
 float distance_mm;          // 障碍物距离
 int urgency_level;          // 0~3 紧急等级
};
struct ObstacleCleared {        // 障碍物已移开
 float safe_distance_mm;     // 当前安全距离
};
struct ServoReady {             // 驱动器已使能且无报警
 int servo_id = 0;
};
struct HomingSuccess {          // 回零完成
 double home_offset_mm;      // 原点偏移
};
struct HomingFailed {           // 回零失败（撞限位或编码器异常）
 int error_subcode;
};
// ---------- 3. 定时器/超时事件（内部触发） ----------
struct HoldTimeout {            // 在Holding状态下等待超时
 int elapsed_ms;
};
// ---------- 4. 驱动器故障事件 ----------
struct ServoFaultTriggered {    // 替代原来模糊的"Abnormal"
 int error_code;             // 如 0xE100 (过流)
 std::string error_msg;
};
// ---------- 事件变体（最重要的聚合） ----------
using RobotEvent = std::variant<
 StartTask,
 PauseCmd,
 ResumeCmd,
 StopCmd,
 AbortCmd,
 ResetFault,
 ObstacleDetected,
 ObstacleCleared,
 ServoReady,
 HomingSuccess,
 HomingFailed,
 HoldTimeout,
 ServoFaultTriggered
>;
} } // namespace



```


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MDkzNDQ5Ml19
-->