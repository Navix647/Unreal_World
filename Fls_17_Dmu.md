MCAL部分的内容，是AUTOSAR直接绕过服务层等上层接口直接调用硬件设备进行操作的API。这个就像JAVA里面的虚拟机，不同平台设备有不同的虚拟机。但是这个一般不需要开发人员操心，一般都由硬件厂商提供好。AUTOSAR或者Bsw开发人员只需要了解接口的使用即可。


Fls_17_Dmu这部分的内容是操作Flash驱动直接对Flash进行写入或者读取或者擦除等动作。擦除即让比特位归0的操作，而编程Programing或者说写入Write则是让比特位为1的操作

Fls_17_dmu的API调用分为：同步Synchronous和异步Asynchronous的方式。某些接口本身就是同步，会等到返回结果，某些是异步，只是提交任务，具体执行则是由调度函数完成。
这点和OS的代码类似。

```c
#define Fls_17_dmu_write Fls_Write
#define  







> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzI0NzQ3MzI0XX0=
-->