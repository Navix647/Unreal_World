# DIO
DIO驱动模块
AUTOSAR将Port口的使用分成了两个模块，其中一个是Port驱动模块，这个模块主要专注于配置和设置Port中Pin的属性，相对是一个静态的东西。而Dio驱动则用来读取或写入数字IO Pin。
这里有三个重要概念：
- DIO Channel
DIO Channel在这里其实指的是Port上的一个Pin
- Port
Port在这里指的是由硬件分成的多个DIO Channel，由硬件决定。
- DIO Channel Group
DIO Channel Group可以看成是逻辑Port，由几个相邻的DIO Channel构成的Channel组，只不过Dio Channel Group必须是Port的子集。

在AUTOSAR MCAL标准中
Port驱动实现Port Pin的配置，DIO驱动实现在已配置的Port上进行输入输出的操作，因此它与Port驱动不冲突。用户必须保证其他MCAL驱动所使用的Port Pin不会与DIO驱动的冲突。

相对比较简单的一个MCAL驱动模块。





> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzczNzM3MzMyLDExODcyODExODcsLTE0MD
gwMTA0MDQsLTExNzMzNDk3MjYsODI5MTA5MTRdfQ==
-->