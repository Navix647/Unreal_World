Wad
1.Guerrillero Outfit
2.Solid Fit Textures
3.Maho Outfit
4.Hakama and Kimono Outfit
5.Razors Edge Outfit
6.Seven Seas
7.Everyday Cowgirl
8.Roma Dance Outfit
9.Leather and Lace Flirty Outfit
10.Kurohana dForce Outfit
11.CB Delphine Clothing Set
12.dForce Etain Sage
14.Shadow Lurker  Outfit
15.CMKD Fury Outfit

Ha
Sloane H
Faye H


Bund
Ethereal Celestial Forest
KLY Furious Mona

Sh/Bo




Be




Gl


Po
FG Spell Casting 

An




针对包含根骨骼旋转的动画可以从哪些地方入手：
1.调整动画播放速率匹配CMC的速度和旋转速率

2.通过RootMotion方案（我坚持不使用）

3.使用Angle Match方法，就是Distance Match的推广，这个我没想明白
也不是Distance Match推广，也是通过曲线驱动，只不过在动画层修改物理逻辑层上的一个变化
首先获取：
1.进入状态时，角色的World Rotation，然后j

4.通过曲线驱动，只不过不在ANS的Tick上修改而是在角色蓝图上OnUpdateMovement上面采样修改
这个也是ALSv4实现Turn In Place的方案。
有时间再看看ALSv4的Turn In Place实现方案。





> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2ODkwODE5MzQsLTQ0NDc1NzY0MSwxMT
I0OTgxODUyLDQ2OTk4NjQ3MCwxMjY1MTE5MTgxLDkyNjg5MTEz
MywyMDI4MDAxNTMyLDEwNjQ3OTM1MDYsLTEyMTY0NzIwOTksLT
IzNzg0MDk4NiwxNzgwMTA2MTcyLC0xMzc4MDQ0MDg5LDM2MTUz
MDg5LDEzNTYwNjA0MzksLTE0NTI5NTgyNzUsNzMwOTk4MTE2XX
0=
-->