# Skinning
> 这里负责记录Skinning研究所留下来的资料整理，包括论文的解读，Skinning的方法。大体上掌握Dual Quaternion Skinning、Linear Blend Skinning、还有最新的Direct Delta Mush。 有余力之余看看CoR、Elastic Implicit Skinning（HRBF）都是些关于Real-time的方法。

## Linear Blend Skinning - LBS
这个方法已经是目前工业界的标准，通过骨骼绑定来实现刚体运动



## Dual Quaternion Skinning - DQS
通过Dual Quaternion来实现顶点跟随Joint的平移和旋转即所谓的Rigid Motion（刚体运动）



## Optimized Centers of Rotation Skinning - CoR



## Elastic Implicit Skinning

> Written with [StackEdit](https://stackedit.io/).



## Direct Delta Mush Skinning - DDM


### Improved Variant Compression
原始的DDM算法有以下问题：
1. 存储空间问题
每个顶点预先计算了一个4x4的多维权重矩阵，这意味着，相比于DQS、LBS等算法，它需要16倍之于这些算法的存储空间

2. 算法效率问题
原始算法需要在实时运行的时候对每个顶点都做一次3x3的奇异值分解SVD。这个计算比较影响效率，对于实时应用而言有很大的瓶颈问题

因此DDM原作者团队提出了一种精炼方法来压缩存储空间，同时提高计算效率。
它们提出了两层模型（two-layer Model）
First Layer：是一个比原DDM方法更小的模型，输入骨骼的变换（和DQS、LBS一样的骨骼）计算出一系列**虚拟骨骼（Virtual Bone）的变换**。
输入：骨骼变换
输出：一系列虚拟骨骼变换
Second Layer：是一个更大的但代价更小的LBS模型
输入：一系列虚拟骨骼的变换，即First Layer的输出
输出：每个顶点的skinning，即蒙皮变换


















## Terminology
### Candy Wrapper Effect
指的是joint旋转导致网格体在关节处变得像糖果包装一样体积完全消失的效果
![](https://pic4.zhimg.com/v2-7f7194e1b27eccf03346c0d92daa7421_1440w.jpg)
### Bulge
凸起;鼓起, 凸起；鼓起, 暂时的激增；突然上涨

### Artifact
Artifact这个单词表达的是一种非自然的东西。例如人造的东西，是一种含义很模糊、宽泛的单词。中文世界里没有与之对应的词。在计算机图形学里，像锯齿这种显得不够自然的东西或者说产物就可以叫artifact反正就是feel it。不需要解构它。


### Homogeneous Coordinate
齐次坐标


### polygon budgets
一个计算机图形学的术语，指的是多边形数量上限
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NDg0NDgzODQsLTEyNDAyOTI1MzVdfQ
==
-->