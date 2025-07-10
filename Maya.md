# Control Rig
# 旋转平面 IK 解算器

旋转平面解算器非常适用于对要保持在同一平面上的关节链设定姿势（例如手臂和腿）。例如，由旋转平面 IK 控制柄驱动的手臂的肩部、肘部和腕部关节都保持在与肘部旋转相同的平面内。该平面本身可以通过极向量从肩部关节进行旋转。

![](https://help.autodesk.com/cloudhelp/2024/CHS/Maya-CharacterAnimation/images/GUID-4ED0E266-585F-499E-ACCB-DE5793A84A47.png)

旋转平面 IK 控制柄使用旋转平面解算器来计算其 IK 链中所有关节的旋转，但不会计算关节链的整体方向。相反，旋转平面 IK 控制柄可以通过极向量和扭曲圆盘直接控制关节链的方向，而不是通过 IK 解算器计算方向。单链解算器和旋转平面解算器之间的区别就在于此。另请参见主题  [IK 解算器](https://help.autodesk.com/view/MAYAUL/2024/CHS/?guid=GUID-952FC4B3-19A6-4055-B034-3A7D15EC66D6)中的“单链和旋转平面 IK 控制柄之间的差异”。

![](https://help.autodesk.com/cloudhelp/2024/CHS/Maya-CharacterAnimation/images/GUID-E7B94792-9B2A-4C98-9545-A502EF3C7470.png)

另请参见下面的“极向量”(Pole vector)和“扭曲圆盘”(Twist disc)描述。

## 旋转平面 IK 控制柄组件

扭曲圆盘

扭曲圆盘是一种操纵器，可以用来扭曲或旋转关节链。扭曲圆盘位于 IK 链的末关节处。

平移极向量通常会使 IK 链指向错误方向。在移动极向量以防止翻转后，可以使用扭曲盘来重新定向平面。

关节链平面

关节链平面是一个平面，它包含了关节链中所有关节并通过轴来设定姿势。关节链平面围绕控制柄向量旋转。操纵极向量时，关节链平面将围绕控制柄向量旋转。

参照平面

要使关节链平面旋转和扭曲关节链，必须相对于某些其他平面旋转平面以便测量扭曲的角度。关节链平面进行相对旋转的平面是参照平面。

极向量

极向量是一种操纵器，用于更改 IK 链的方向。此外，极向量还用于控制翻转。

由于移动极向量可以更改参照平面的方向，因此移动极向量还可以直接更改关节链的方向，正如操纵扭曲盘可以更改关节链的方向一样。这是因为已将关节链的方向或扭曲角度，定义为参照平面和关节链平面之间方向的差异。

重要：  定位 IK 控制柄时，如果控制柄向量和极向量互相交叉或指向完全相反的方向，则关节链可能突然翻转。可以通过移动极向量避免此翻转，以使控制柄向量不与之相交或指向其相反方向。

#### 旋转圆盘

旋转圆盘是一种指示器，用于显示扭曲盘旋转 IK 链的程度。旋转圆盘位于 IK 链的始关节处。

参照平面指示器

参照平面指示器是旋转盘上的绿色点，可以通过移动来反映极向量的移动。

扭曲指示器

扭曲指示器是参照平面指示器和旋转盘上的关节链平面指示器之间的绿色曲线。扭曲指示器可显示关节链相对于参照平面的方向。

关节链平面指示器

关节链平面指示器可显示关节链平面相对于参照平面的方向。关节链平面指示器出现在旋转圆盘中。





# IK 控制柄概述
在 IK 链中，IK 控制柄开始处的关节称为起始关节，IK 控制柄末端的关节称为末关节。起始关节和末关节之间的所有关节都受 IK 控制柄及其解算器驱动。在场景视图中，会将 IK 控制柄绘制为包含 IK 链的起始关节和末关节的一条线。

IK 控制柄的末端（默认情况下它位于 IK 链的最后一个关节处）称为末端效应器。移动 IK 控制柄时，IK 解算器会在其计算中使用末端效应器的位置和方向，以相应地旋转 IK 链中的关节。末端效应器会始终尝试跟随 IK 控制柄的位置。但是，根据 IK 链的旋转限制和完全延伸长度，末端效应器可能无法接触到 IK 控制柄。

若要将 IK 控制柄移动到末端效应器的位置，请使用“动画”(Animation)菜单集，然后选择“关键帧  >  IK/FK 关键帧  >  将 IK 移动到 FK”(Key > IN/FK Keys > Move IK to FK)。

![](https://help.autodesk.com/cloudhelp/2024/CHS/Maya-CharacterAnimation/images/GUID-EF2CF139-88A3-4E6B-8719-2C06DF1FCAEA.png)

注：  只能从“Hypergraph”查看 IK 控制柄的末端效应器。

若要创建 IK 控制柄，请使用“IK 控制柄工具”(IK Handle Tool)或“IK 样条线控制柄工具”(IK Spline Handle Tool)。请参见  [IK 控制柄工具](https://help.autodesk.com/view/MAYAUL/2024/CHS/?guid=GUID-DF30CFD7-88EB-421F-9701-69318A1F3D16)和  [IK 样条线控制柄工具](https://help.autodesk.com/view/MAYAUL/2024/CHS/?guid=GUID-820CF89C-257C-4EA9-A474-6E0146C9099F)。

## 常规 IK 控制柄组件

若要显示所有当前 IK 控制柄的组件，请参见[查看所有 IK 控制柄组件](https://help.autodesk.com/view/MAYAUL/2024/CHS/?guid=GUID-E7374D31-30DA-4BA7-B651-32D76ECE3143)。

### 控制柄线

控制柄线是一条贯穿关节链中所有关节和骨骼的线，而关节链由 IK 控制柄控制。控制柄线开始于起始关节的局部轴，结束于末关节的局部轴。

若要查看控制柄线的图像，请参见[旋转平面 IK 解算器](https://help.autodesk.com/view/MAYAUL/2024/CHS/?guid=GUID-9942FFB5-65C2-46E2-B5A3-297667A9FB5D)。

### 控制柄向量

控制柄向量是从 IK 链的起始关节绘制到 IK 控制柄的末关节（末端效应器）的线。这是旋转平面所使用的轴。

若要查看控制柄向量的图像，请参见[旋转平面 IK 解算器](https://help.autodesk.com/view/MAYAUL/2024/CHS/?guid=GUID-9942FFB5-65C2-46E2-B5A3-297667A9FB5D)。
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5Njk5ODI0NTEsODkyMTc4OTQzLDQ0OT
MxODQ2Miw3MzA5OTgxMTZdfQ==
-->