我希望通过实际例子说明遗传算法的工作原理
假设使用遗传算法初始生成了以下五个方案
方案一：功率100扭矩120速比6   
方案二：功率120扭矩110速比7
方案三：功率140扭矩150速比8
方案四：功率160扭矩140速比9
方案五：功率110扭矩130速比10

分别通过目标模型计算出两个目标
方案一：零百加速时间8.5秒 能耗14.2kwh
方案二：零百加速时间8秒 能耗14kwh
方案三：零百加速时间7秒 能耗13.5kwh
方案四：零百加速时间7.5秒 能耗14.5kwh
方案五：零百加速时间7.8秒 能耗15.5kwh

现在我已经通过非支配排序算法
计算出五个方案的被支配方案数量分别如下

| 方案 |被方案支配  |被支配方案数量|Rank|
|--|--|--|--|
| 三 | 无 |0|0|
| 二 | 三 |1|1|
| 四 | 三 |1|1|
| 一 | 二、三 |2|2|
| 五 | 三、四 |2|2|

因此排序为Rank0：方案三，Rank1：方案二、方案四，Rank2：方案一、方案五

接下来需要说明需要保留哪些方案做交叉变异即繁衍后代，然后通过这个实际例子说明如何迭代即如通过交叉和变异迭代得到新的方案并循环直到收敛到帕累托前沿。需要附上一些公式来说明为什么通过交叉变异可以往帕累托前沿收敛，请你通过一页ppt形式说明

至于如果有人问到如果所有方案都在Rank0的话，怎么筛选的时候需要保留一页附录说明另外一个指标即分散程度





```
|libigl
|-cmake
|-docs
|-build
|-external
|-include
|-tests
|-tutorial
|-CMakeList.txt
|-LibiglOptions.cmake.sample
|-README.md
```
CMake Error at CMakeLists.txt:46 (add_executable):
Target  "MyLibiglProject" links to target "igl::opengl_glfw" but the target was not found. Perhaps a find_package() call is missing for an IMPORTED target, or an ALIAS target is missing?





完成电驱动总成参数匹配算法的可视化系统开发，覆盖初始化、迭代搜索、收敛分析与结果输出全流程，支持交互式回放优化过程。构建包含多款车型数据的数据支撑分析，基于前后端分离架构实现web端动态展示。系统支持帕累托前沿演化动画、参数敏感性分析图表，数据库包含10款以上车型数据。交付完整Python代码




电池方案
电池节数、电池容量、标称电压、标称电量
整车参数
整备质量、半载质量、满载质量、前轮滚动半径/m、后轮滚动半径/m、轴距/mm、滚阻系数、迎风面积/m2、风阻系数、驱动形式、卡钳拖滞阻力、轮毂拖滞力矩/Nm

动力性目标
百公里加速s、最高车速kph、最大爬坡度、30min最高车速、4%爬坡车速(kph)、12%爬坡车速(kph)

经济性目标
里程km
行车能耗kWh/100km

总成









> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxOTUxMDcyOF19
-->