
## Sequence Player vs Sequence Evaluator





https://www.gfxcamp.com/numerion-carbon-v3/
|特性 | Sequence Player (序列播放器) | Sequence Evaluator (序列评估器) |
|特性   | Sequence Player (序列播放器)  | Sequence Evaluator (序列评估器) |
|--|--|--|
| 核心逻辑  |**自动更新**。根据每一帧的 DeltaTime 自动累加播放时间。 |**静态采样**。本身不随时间移动，除非你通过输入显式改变 `Explicit Time`。|
|控制方式|基础循环动画|Locomotion 2.0 (Distance Matching)、姿态混合、需要精确对齐的动作。|
|性能|略高| 极高|

### 核心概念：Explicit Time（显式时间）

在 Sequence Evaluator 中，最重要的参数是 **Explicit Time**。

-   **Sequence Player**：内部有一个自动增加的计时器，每帧累加 `DeltaTime`。
    
-   **Sequence Evaluator**：它像是一个“指针”，你输入一个具体的时间值（例如 1.5秒），它就直接采样并输出该时间点的姿态。




Locomotion System比想象中费时间
 


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzQ0OTUzMDM5LDE4Njc5MjAxMiwtOTkzOT
I1OTk2LDQ0NTAxOTExMl19
-->