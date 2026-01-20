# 目前尚存问题
- [x] 停止状态有点问题
- [x] 如何解决溜冰感
- [ ] 


1. 停止状态有点问题
因为停滞状态是HoldRotation模式，可以参考一下GameSampleAnimationProject
2. 如何解决溜冰感？ 
是否需要调整CMC参数？如何调整？如Friction、Factor等  
**"Detect Surface Types in Unreal Engine 5 - Slippery ice, Snow, Grass & More!(Movement System)"**视频



下设置的参数，可以作为参考，为何出现溜冰感。



#### **❄️ 冰面 (Ice) - 极度滑行**

这是视频中调整最极端的设置，目的是让玩家感到“失控”和“惯性”。

-   **Ground Friction:** `0.0` (或极低值如 `0.05`)
    
    -   _解释:_ 几乎没有摩擦力，角色一旦移动就会一直滑行，很难改变方向。
        
-   **Braking Deceleration Walking:** `20.0` (默认通常是 2048.0)
    
    -   _解释:_ 极低的数值意味着当你松开键盘/手柄时，角色不会立即停下，而是会向前滑行很长一段距离。
        
-   **Max Acceleration:** `1000.0` (降低)
    
    -   _解释:_ 在冰面上起步较慢，模拟打滑的感觉。
        
-   **Max Walk Speed:** `800.0` (增加)
    
    -   _解释:_ 作者稍微提高了冰面的最大速度，配合低摩擦力，模拟出“由于惯性越滑越快”的失控感。
        

#### **🌨️ 雪地 (Snow) - 高阻力**

雪地的重点是“滞重感”，模拟脚陷在雪里的感觉。

-   **Max Walk Speed:** `250.0` (默认通常是 600.0)
    
    -   _解释:_ 大幅降低移动速度，让玩家感觉到在深雪中行走的吃力。
        
-   **Ground Friction:** `8.0` (默认是 8.0，保持不变或略增)
    
    -   _解释:_ 雪地摩擦力大，不会滑行。
        
-   **Max Acceleration:** `1500.0` (降低)
    
    -   _解释:_ 在雪地里起步比较迟缓。
        

#### **🌿 草地 / 默认地面 (Grass / Default) - 标准控制**

这通常对应 Unreal Engine 角色移动组件的默认参数，或者是你游戏中设定的标准手感。

-   **Ground Friction:** `8.0`
    
-   **Braking Deceleration Walking:** `2048.0`
    
    -   _解释:_ 松开按键后能迅速停下，手感精准。
        
-   **Max Walk Speed:** `600.0`
    
-   **Max Acceleration:** `2048.0`

3. Arch动画能否实装？
4. 开始动画是否应该学习Pivot？第一步重心前移之后再执行位移，因为目前这套动画确实不是立刻移动。
5. 如何减少目前的Rotation Matching的耦合？
6. Physic Rotation原理？ 居然和Rotation Rate有关
目前的方案里面，用到的CustomRotationYaw其实是输入到PhysicRotation里面的，但是这个东西也是插值的，所以最终就是和Rotation Rate有关






Predict where the character will change direction during a pivot based on its current movement properties and parameters from the movement component. This uses prediction logic that is heavily tied to the UCharacterMovementComponent. Each parameter corresponds to a value from the UCharacterMovementComponent with the same name. Because this is a thread safe function, it's recommended to populate these fields via the Property Access system.

Target is Anim Character Movement Library

## Inputs

Type

Name

Description

vector

Acceleration

vector

Velocity

real

Ground Friction

## Outputs

Type

Name

Description

vector

Return Value

The predicted pivot position in local space to the character. The size of this vector will be the distance to the pivot.






vector

Velocity

boolean

Use Separate Braking Friction

real

Braking Friction

real

Ground Friction

real

Braking Friction Factor

real

Braking Deceleration Walking

## Outputs

Type

Name

Description

vector

Return Value

The predicted stop position in local space to the character. The size of this vector will be the distance to the stop location.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjI1NjM0OTAyLC0xNzA5OTI4NDQ2LDE2Mj
A5NDEzNDAsODA1MjgxNjEwLC0xMjMyMzUwMTI0LC0xMjM1NjEx
MDUxXX0=
-->