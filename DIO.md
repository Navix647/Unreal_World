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



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MDgwMTA0MDQsLTExNzMzNDk3MjYsOD
I5MTA5MTRdfQ==
-->