
Distance Match实现“动画推进”核心是不再使用时间这一概念来推进,而是使用距离来推进动画进程。让动画和距离进行匹配。



# 核心距离匹配节点 (Distance Matching)

这些函数的核心逻辑是：**不再根据秒数（Time）更新动画，而是根据角色移动的物理距离（Distance）来决定动画该播到哪一帧。**
Distance Match通常涉及 **Animation Locomotion Library** 插件中的函数。以下是所有用于推进 `Sequence Evaluator` 时间的核心蓝图节点及其详细分类：


## 1. 核心距离匹配节点 (Distance Matching)
### Advanced Time by Distance Match
这是最常用的节点，主要用于**循环动作**（如走路、跑步）。这个就完全相当于是根据移动距离这个物理量来作为动画播放进度条，而不是传统的时间。
#### 节点逻辑

传统的动画播放是 $CurrentTime = PreviousTime + DeltaTime$。而此节点采用的逻辑是：

$$CurrentTime = CurveLookup( CurrentDistance + DistanceTraveled )$$

系统不再问“过了 0.03 秒，动画该播到哪？”，而是问“角色在这一帧移动了 5 厘米，动画曲线上累积移动 5 厘米对应的位置是哪里？”。
-   **功能**：根据本帧角色移动的距离，计算出动画应该前进多少时间。
    
-   **核心输入**：
    |参数名称|数据类型|描述|
    |-|-|-|
    |**Update Context**|Struct|动画更新上下文（AnimUpdateContext），通常由 Update 节点的引脚提供。|
    |**Sequence Evaluator**|Struct|需要操作的序列评估器节点引用。注意：必须是 Sequence Evaluator，不能是 Sequence Player。|
    |**Distance Traveled**|||
    |**Distance Curve Name**|||
    
    -   **Update Context**: 动画更新上下文。
        
    -   **Distance **: 本帧移动的实际物理距离（通常由 `Character Movement` 组件获取）。
        
    -   **Distance Curve Name**: 动画序列中记录位移信息的曲线名称（默认为 `Distance`）。
        
-   **原理**：它会查找动画曲线，找到当前距离对应的曲线值，然后将动画指针“推进”到对应位置。这样可以彻底解决“滑步”问题。



### Distance Match to Target

主要用于**非循环动作**（如下落着地、停止跑动、跳跃起跳）。

-   **功能**：计算当前位置到“目标点”的剩余距离，并将动画匹配到该距离对应的帧。
    
-   **应用场景**：比如角色离停止点还有 50 厘米，该节点会让动画立即跳转到“停止动画”中离结束还有 50 厘米姿态的那一帧。
    
-   **核心输入**：
    
    -   **Distance to Target**: 到目标的物理距离。


### **Set Explicit Time by Percentage**

-   **功能**：虽然不是直接基于距离，但常用于将 0-1 的比例映射到动画。
    
-   **场景**：比如根据拉弓的蓄力程度（0% 到 100%）来强制设定动画位置。



## 2.状态改变时的特殊推进节点

### **D. Sequence Evaluator: Advance Time**

这是用时间推进动画进度的节点，如果用这个的话那和Sequence Player区别不大，主要是自己维护D额理他Time

-   **功能**：在 Evaluator 内部累加 `Delta Time`。
    
-   **区别**：它和普通的 Sequence Player 类似，但它允许你在特定的逻辑流中手动触发时间累加，而不是每帧自动播放。
    

### **E. Set Explicit Time (直接设置显式时间)**

-   **功能**：最暴力的推进方式，直接给定一个 `float` 值。
    
-   **场景**：当你自己写了一套复杂的数学公式计算时间（比如基于物理模拟的反馈）时使用。

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA0MDkyNzYyNywxNzYyMjk4MzcyLC04NT
A2NjI1OTgsMTkzNzU4NzYyNiwxNjQwNzM3MzhdfQ==
-->