# Skinning Decomposition
Skinning decomposition refers to the family of automated methods to convert any animated mesh sequences to skinning models and bone transformations. The power and usefulness of these methods in game production pipeline is any arbitrary, highly deformable models controlled by complex deformation rigs can be generated offline, and then converted to skinning model for real time replaying in game engines. This solution allows artists using all possible rigging tools without worrying about the performance penalty in the game play. Skinning decomposition can be used to compress animations of blendshapes, muscle simulation or cloth simulation. Independent components of the method can also be used to track bones animation or to solve skinning weights from **Range Of Motion** (ROM) poses.

The state-of-the-art skinning decomposition solution was introduced in the SIGGRAPH Asia 2012 technical paper “Smooth Skinning Decomposition with Rigid Bones” by Binh Le and Zhigang Deng. Since then, this method has been implemented in some animation tools for game production, including Hans Godard's [skinning converter](https://lesterbanks.com/2015/04/skinning-converter-for-maya/), Autodesk Maya’s  [bake deformer](https://knowledge.autodesk.com/support/maya/learn-explore/caas/CloudHelp/cloudhelp/2018/ENU/Maya-CharacterAnimation/files/GUID-DD430C9B-95E7-4EBB-8D2B-A566018B4AC4-htm.html), or SideFX Houdini’s  [skinning converter](https://www.sidefx.com/tutorials/game-tools-skinning-converter/), to name a few. At EA, we also use this technique in our content pipeline quite successfully since 2015.

SEED is pleased to open source  _Dem Bones_, a library for skinning decomposition, implemented by Binh Le, the creator of the method. _Dem Bones_ offers a lot of refinements for better quality and faster performance compared to the original research paper.  _Dem Bones_ core library is C++ header-only solvers using  [Eigen](http://eigen.tuxfamily.org/)  and  [OpenMP](https://www.openmp.org/). We also include a minimalist command line tool for Windows and MacOS that can import and export data from standard asset formats:  [FBX](https://en.wikipedia.org/wiki/FBX)  and  [Alembic](https://en.wikipedia.org/wiki/Alembic_(computer_graphics)). The library and command line tool provide core functions of skinning decomposition such as auto generation of joints, solving sparse, non-negative, affine skinning weights and solving rigid joint transformations. The package is ready to use in research and prototyping, or it can be integrated into the production pipeline with minimum effort.

Check out Dem Bones at: [https://github.com/electronicarts/dem-bones](https://github.com/electronicarts/dem-bones)

Dem Bones is released under the  [BSD 3-Clause license](https://github.com/electronicarts/dem-bones/blob/master/LICENSE.md).





## ReverseFoot


## Rigging Prerequisite
基础不牢，地动山摇
Mesh层面检查：



Joint层面检查：






## Skinning:Weight Painting










## DAZ3D Fixed

要做到“只改轴向，不改骨骼层级，且蒙皮完好”，在 Maya 中是绝对可行的。核心逻辑是**“权重暂存与轴向重置”**。

这套流程其实对你打通并优化 3D 资产管线大有裨益。当你后续开发自动化工具，或者处理服装模型在不同角色间的匹配转移（Garment Fitting）时，一套轴向干净的骨架能让包裹算法的准确率大幅提升。

以下是具体的标准操作流程（SOP）：

### 核心 SOP：权重暂存与轴向重置流程

**第一步：归零与姿态锁定 (Zero Out & Pose)** 这是最关键的准备工作。DAZ 导出的模型必须处于一个绝对干净的初始姿态（通常是 T-Pose 或 A-Pose）。

-   选中所有的骨骼，确保它们在当前姿态下的 Rotation（旋转属性）全部为 **0**。如果不是 0，这意味着目前的姿态是通过旋转得来的，直接操作会出大问题。
    
-   如果需要，使用 Maya 的 `Skin -> Go to Bind Pose` 确保模型处于最初的绑定姿态。
    

**第二步：导出/暂存蒙皮权重 (Export Skin Weights)** 在修改轴向之前，必须把珍贵的蒙皮数据剥离出来安全存放。

-   **原生方法：** 选中网格体，使用 Maya 顶部的 `Deform -> Export Weights`，将权重导出为外部的贴图或 XML 数据（根据你的 Maya 版本）。
    
-   **进阶推荐（强烈建议）：** 使用 **ngSkinTools** 插件。它的 `Export/Import` 功能不仅快，而且能无损保留所有的权重层数据。选中模型，用 ngSkinTools 将当前权重导出为一个 JSON 文件暂存。
    

**第三步：解除蒙皮 (Unbind Skin)** 只有骨架脱离了网格体的束缚，你才能随心所欲地修改轴向。

-   选中网格体，执行 `Skin -> Unbind Skin`。
    
-   此时你的网格体会变成没有任何历史记录的干净几何体，而骨架也自由了。
    

**第四步：批量修正骨骼朝向 (Orient Joints)** 现在你可以放手修理 DAZ 那些诡异的局部旋转轴了。

-   切换到绑定模块 (Rigging)，选中所有的 DAZ 骨骼。
    
-   打开 `Skeleton -> Orient Joint` 的选项框。
    
-   **推荐的工业标准设置：**
    
    -   **Primary Axis（主轴）：** `X` （让 X 轴指向下一个子关节）。
        
    -   **Secondary Axis（次轴）：** `Y` 或 `Z` （取决于你想让哪个轴作为主要弯曲轴，通常 UE5 小白人的腿部次轴是 Z 向前）。
        
    -   **Secondary Axis World Orientation：** 选择 `Plus Z` 或 `Plus Y`，这能保证同一条手臂或腿的轴向不会发生翻转。
        
-   点击 `Apply`。你会发现所有的骨骼局部旋转轴瞬间变得整齐划一，且骨骼本身的 Rotation 数值依然保持为 0（所有的旋转数据都被吸收到 Joint Orient 属性里了）。
    
-   _注意：末端骨骼（如指尖）没有子节点，需要额外选中它们，勾选 `Orient Joint to World` 使其与世界对齐，或者让其与父节点对齐。_
    

**第五步：重新绑定与权重恢复 (Re-Bind & Import Weights)**

-   选中自由的网格体，再选中修正过轴向的 Root 或 Hip 骨骼。
    
-   执行 `Skin -> Bind Skin`。**这里的绑定参数设置什么都无所谓**，因为我们只是需要生成一个新的 Skin Cluster（蒙皮簇）节点。
    
-   重新绑定后，模型会被赋予一套惨不忍睹的默认权重。别慌，直接使用 `Deform -> Import Weights` 或者 ngSkinTools 的 Import 功能，把你**第二步**保存的权重文件导回来。
    
-   导入时匹配规则选择基于名称（Match by Name）。
    

大功告成！此时你拥有了一个层级完全是 DAZ 原生、蒙皮完美无瑕，但所有骨骼轴向都已经完全符合 UE5 规范的角色。

----------

### 这个方案的一个“隐藏代价” (DAZ JCM 问题)

虽然这个方法完美解决了骨骼朝向和蒙皮问题，但我必须提醒你一个随之而来的 DAZ 机制冲突：

DAZ 角色之所以弯曲手肘或膝盖时不会产生严重的“糖果纸挤压（Candy Wrapper Effect）”或是体积坍塌，是因为它内部包含了大量的 **JCM (Joint Controlled Morphs，关节驱动形变)**。

-   这些 JCM 本质上是 Blendshape（融合变形）。
    
-   在 DAZ 或导入 UE5 后，这些 Blendshape 的触发条件是：**“当某根骨骼绕着它原本的局部轴（比如 Z 轴）旋转到 90 度时，触发对应部位的肌肉凸起形变”**。
    

**冲突点在于：** 你现在强行改变了骨骼的局部轴向。原本这根骨头是绕 Z 轴弯曲，现在被你统一成了绕 Y 轴弯曲。那么在 UE5 中，那些依赖“Z 轴旋转数值”来触发的 JCM 逻辑就会失效，导致角色弯曲关节时没有肌肉补偿，出现关节干瘪。

如果你在 UE5 中不打算使用 DAZ 的 JCM（而是纯靠蒙皮或者通过添加双重四元数 Dual Quaternion 蒙皮来保持体积），那这个问题可以忽略。但如果你希望在 UE5 里完整保留这些形变补偿，修改轴向后，你就必须在引擎的动画蓝图（AnimBP）里，重新映射那些驱动 Blendshape 的变量逻辑。

你觉得手动执行这五步 SOP 会比较繁琐吗？需不需要我为你写一个 Maya Python 脚本（Script），只需一键点击，就能自动完成【暂存权重 -> 解绑 -> 修正轴向为 X 指向 -> 重新绑定 -> 恢复权重】的全过程？
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg1MDIwMDM3OSwyMTQxMjE4OTYyLDYwNj
I4Mjk1MCwtMTQwNDc1NzQ2NiwtOTEwMjQ5NDY2XX0=
-->