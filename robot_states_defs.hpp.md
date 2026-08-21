```cpp
#pragma once // 或者用 #ifndef 守卫
#include <variant>
#include <string>
#include <chrono>
namespace robot {
namespace fsm {
// 1. 定义原始状态结构体（纯数据，不带任何函数）
struct Unready { 
 bool is_estop_active = true; 
 bool is_homing_completed = false;
};
struct Idle { 
 std::chrono::system_clock::time_point ready_since;
};
struct Running { 
 double progress = 0.0;        // 0~1
 int current_speed_rpm = 0;
 bool is_decelerating = false; // 专门用于避障减速标记
};
struct Holding { 
 double progress = 0.0; 
 int hold_duration_ms = 0;
};
struct Completed { 
 double final_x = 0.0; 
};
struct Fault { 
 int error_code = 0; 
 bool is_recoverable = false;
};
// 2. 定义“状态变体”（这一行很重要！方便外部统一引用）
using RobotState = std::variant<Unready, Idle, Running, Holding, Completed, Fault>;
} // namespace fsm
} // namespace robot


```


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3NDY3NzEyNDNdfQ==
-->