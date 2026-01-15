
## 时序公式（考虑组件空间变化）

### 定义坐标系

-   **W**: 世界坐标系（固定）
    
-   **C(t)**: 时刻t的组件坐标系（随角色移动而变换）
    
-   **T_C→W(t)**: 时刻t从组件空间到世界空间的变换矩阵
    
-   **T_W→C(t)**: 时刻t从世界空间到组件空间的变换矩阵
    

### 核心时序公式

#### 1. 锁定时刻 (t₀)

text

复制

下载

α(t₀) = 1.0
// 记录锁定时的世界位置（固定参考系）
P_lock_world = T_C→W(t₀) × P_component(t₀)

// 记录锁定时的组件位置（相对参考系）
P_lock_component = P_component(t₀)

#### 2. 后续帧 (tᵢ, i > 0) - 关键修正

**问题**：在 tᵢ 时刻，组件坐标系 C(tᵢ) 已经不同于锁定时刻的 C(t₀)

**正确计算流程**：

text

复制

下载

// 当前帧动画驱动的组件空间位置
P_anim_component(tᵢ) = 动画系统输出的骨骼位置

// 如果没有Foot Lock，脚部应有的世界位置
P_expected_world(tᵢ) = T_C→W(tᵢ) × P_anim_component(tᵢ)

// 期望的世界位置（锁定位置）
P_desired_world = P_lock_world  // 这是固定的世界坐标

// 需要将期望世界位置转换到当前组件空间
P_desired_component(tᵢ) = T_W→C(tᵢ) × P_desired_world

// 计算需要应用的偏移量（在当前组件空间中）
Offset_component(tᵢ) = P_desired_component(tᵢ) - P_anim_component(tᵢ)

// 应用Alpha混合
Offset_applied(tᵢ) = Offset_component(tᵢ) × α(tᵢ)

// 最终组件空间位置
P_final_component(tᵢ) = P_anim_component(tᵢ) + Offset_applied(tᵢ)

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjI1NTU3NjUyXX0=
-->