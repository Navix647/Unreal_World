
Distance Match实现“动画推进”核心是不再使用时间这一概念来推进,而是使用距离来推进动画进程。让动画和距离进行匹配。



# 核心距离匹配节点 (Distance Matching)

这些函数的核心逻辑是：**不再根据秒数（Time）更新动画，而是根据角色移动的物理距离（Distance）来决定动画该播到哪一帧。**
Distance Match通常涉及 **Animation Locomotion Library** 插件中的函数。以下是所有用于推进 `Sequence Evaluator` 时间的核心蓝图节点及其详细分类：

## Advanced Time by Distance Match
这是最常用的节点，主要用于**循环动作**（如走路、跑步）。

-   **功能**：根据本帧角色移动的距离，计算出动画应该前进多少时间。
    
-   **核心输入**：
    
    -   **Update Context**: 动画更新上下文。
        
    -   **Distance**: 本帧移动的实际物理距离（通常由 `Character Movement` 组件获取）。
        
    -   **Distance Curve Name**: 动画序列中记录位移信息的曲线名称（默认为 `Distance`）。
        
-   **原理**：它会查找动画曲线，找到当前距离对应的曲线值，然后将动画指针“推进”到对应位置。这样可以彻底解决“滑步”问题。



## Distance Match to Target

主要用于**非循环动作**（如下落着地、停止跑动、跳跃起跳）。

-   **功能**：计算当前位置到“目标点”的剩余距离，并将动画匹配到该距离对应的帧。
    
-   **应用场景**：比如角色离停止点还有 50 厘米，该节点会让动画立即跳转到“停止动画”中离结束还有 50 厘米姿态的那一帧。
    
-   **核心输入**：
    
    -   **Distance to Target**: 到目标的物理距离。






> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk1Nzc1MzIwMSwxNjQwNzM3MzhdfQ==
-->