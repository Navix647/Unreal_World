

# Turn In Place
即原地转向。

## 关键变量：

### RootYawOffset

背景：玩家输入控制了玩家控制器旋转，然后该控制器旋转控制了Actor的旋转，而Actor的旋转会导致角色模型旋转。我们的目标是建立一个值来计算偏移从而让角色不旋转，抵消掉Actor旋转的影响，等到角色通过动画来抵消旋转影响。
原本：
Enhanced Input -> Player Controller -> Camera -> Actor Rotation -> Character Mesh Rotation
现在：
Enhanced Input -> Player Controller -> Camera -> Actor Rotation -> Character Mesh Rotation + RootYawOffset = UnChanged Rotation






> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwODExMDk0ODFdfQ==
-->