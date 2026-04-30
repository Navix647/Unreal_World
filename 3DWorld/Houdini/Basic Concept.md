

# Houdini Basic
## Houdini Scene
视图窗口里的一切就是一个Scene


## Houdini Basic Element --- Object
模型在场景层级（Scene level）由**几何体对象（Geometry objects）**来表示。在几何体对象容器内部，是定义形状的各个几何体节点Surface Nodes

## Transforming objects vs. surfaces

Keep in mind that the normal unit of transformation (moving, rotating, and scaling) is the  _object_. Transforming surfaces at the geometry level (for example, using the  [Transform surface node](https://www.sidefx.com/docs/houdini/nodes/sop/xform.html "The Transform operation transforms the source geometry in object space using a transformation matrix.")) is more accurately  _deformation_, and can potentially take much longer for Houdini to calculate than just transforming static object geometry at the scene level.

This is not to say that transforming at the geometry level is bad - often you  _need_  an object’s shape to deform over time. However, look for opportunities to accomplish your goals by transforming  _objects_  before you consider transforming  _surfaces_, because the former is more efficient.


## Primitives
Primitive是Houdini里特有的概念，其他DCC都没有出现过这个概念。
在Houdini里指的是一种几何体单元，层级比Object低，但是比Point高

注意！！！：
在传统 3D 软件中，“面”通常只意味着多边形（Polygon）。但在 Houdini 中，`Primitive` 就像是一个**抽象基类**，在这个抽象类之下，派生出了各种完全不同的数据结构，但它们都可以被同一种逻辑（SOP 节点）去遍历和处理。

Houdini supports several different types of primitives:
以下这几个都是Primitive！！！
-   [Polygon face](https://www.sidefx.com/docs/houdini/model/primitives.html#polygons)
    
-   [Packed primitives](https://www.sidefx.com/docs/houdini/model/packed.html),  [Packed Disk primitives](https://www.sidefx.com/docs/houdini/model/packed.html), and  [Packed Disk Sequence primitives](https://www.sidefx.com/docs/houdini/model/packed.html)
    
-   [Packed USD primitives](https://www.sidefx.com/docs/houdini/model/primitives.html#usd)
    
-   [Polygon soup](https://www.sidefx.com/docs/houdini/model/primitives.html#polysoup)
    
-   [Volume](https://www.sidefx.com/docs/houdini/model/volumes.html)
    
-   [Tetrahedron](https://www.sidefx.com/docs/houdini/model/primitives.html#solid_matter)
    
-   [Bezier/NURBS surfaces](https://www.sidefx.com/docs/houdini/model/primitives.html#nurbs)
    
-   Polygon/Bezier/NURBS curves
    
-   [Metaballs](https://www.sidefx.com/docs/houdini/model/primitives.html#metaballs)





> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUxNDA5NDYwM119
-->