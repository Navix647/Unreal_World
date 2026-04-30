
前期准备：

带Skeleton Joint的骨骼几何体：Bone Geometry（Polygon Mesh） with Joint Skeleton
没有穿模交叉的肌肉几何体：Muscle Geometry（Polygon Mesh）
带Skeleton Joint的皮肤几何体：Skin Geometry （Polygon Mesh）with Joint Skeleton
 也就是表现层模型，可以理解为就是你要做模拟的模型。
Otis Solver通过模拟附着在骨骼上的肌肉以及组织运动来模拟真实的体积和表现的，因此骨骼几何体必须带动画，实践中一般是通过带动画的Skin Geometry把它的动画重定向到骨骼几何体上。


需要注意：
1.几何体都是封闭的多边形Mesh
2.几何体都必须是封闭的流形
3.没有缝隙和孔洞

每一块肌肉和骨骼都必须要有**muscle_id**属性











> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkyMzAwNzgyMl19
-->