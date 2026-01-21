
Caution！：

1.原来开启Root Motion不管你是Montage也好还是Everything，都会导致Multi Thread失效
所以一定要关闭使用Root Motion。 所以为啥不用Root Motion？ 就是因为价值不高，性能损失严重，优化不好，网络性能差。


2.动画蓝图中尽量使用Fast Path，那么就不能有蓝图逻辑，比如And逻辑和Or逻辑，一些对变量的运算都不能有，最推荐的办法就是直接Property Access，或者使用Node Function，这些都可以保持Fast Path，这里同样包含Transition Rule的计算。想要快就必须得想办法保证Fast Path。



3.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2MDgwNzM0NDcsNzMwOTk4MTE2XX0=
-->