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
主驱动总成平台、主驱峰值功率、主驱峰值扭矩、主驱速比
辅驱总成平台、辅驱峰值功率、辅驱峰值扭矩、辅驱速比

动力性仿真
百公里加速、80~120kph超越加速、最高车速kph、最大爬坡度%、峰值功率扭矩




基于电驱动总成的关键参数(如电机功率、扭矩及减速器速比)，建立考虑不同总成参数下的损耗机制的实时电机效率Map模型，从而构建更高精度的CLTC工况能耗仿真模型，计算3系56系和789系车型的CLTC能耗，要求仿真结果与公开数据之间的平均误差小于5%。

如何计算平均误差、使用真实工况下的数据比较。刚好程博那边有数据。













175,117像素开始
80,670


让我把需求再明确一下：
分别写三个函数：
1.裁剪图片函数crop_images(img_file,outdir)
裁剪完后将图片保存到outdir上。按照OCR_%number顺序从01开始即OCR_01.jpg,OCR_02.jpg等等
2.读取图片文件，并调用GOT2模型识别 GOT_OCR(img_file)
调用GOT2模型识别到的内容,返回String
3.将2函数返回的String内容写到excel的一列上。write_to_excel(string)


写一个main函数先调用1函数得到一个目录里的裁剪图片，按OCR_%number顺序调用2函数得到String并调用3函数把内容弄到excel的一列上，行的话根据2函数返回的换行进行换行。
请以以上需求为准完成代码编写




2026年3月15日前，基于电机驱动总成的关键参数（如电机功率、扭矩及减速器速比），建立考虑不同总成参数下的损耗机制的实时电机效率Map模型，从而构建更高精度的CLTC工况能耗仿真模型，计算3系56系和789系车型的CLTC能耗，要求仿真结果与公开数据之间的平均误差小于5%。交付《CLTC能耗计算模型》及配套MATLAB实现代码。




给出这个代码，CLTC工况假定已存入到CLTC_Data.mat里了，他是一个名为cyc_data的结构体包含time_s 1801x1 double, speed_kmh 1801x1 double,speed_ms 1801x1 double, acc_ms2 1801x1 double, slope 1801x1 double的数据

3系 海鸥Plus
5系海豚
6系 秦Max
7系 海狮07 EV
8系 钛7
9系腾势N8


海鸥
海豚
海狮06 2026 606
海狮07 EV
钛7
腾势N9
整车参数包括：风阻系数、轮胎滚动半径、滚动阻力系数
CLTC百公里耗电量


海鸥 405km续航版
峰值功率55
峰值扭矩135
整备质量1240
CLTC续航405
电池电量：38.88
百公里耗电量：10.1




海狮07 EV  2025智航版
CLTC续航:610
峰值功率:230
峰值扭矩:380
整备质量:2210


海豚 2025 420
峰值功率70
峰值扭矩180
整备质量：1435kg
CLTC续航420
电池电量44.928
百公里耗电量：10.5

海狮06 2026 605旗舰版
峰值功率240
峰值扭矩305
整备质量2040
CLTC续航605
电量69.07
百公里耗电量12.7



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgwNjQzNzI2OSwyODQyMzU3NzIsLTEwOT
E3NTEzMDQsLTEwNDk2MDU1NTgsLTE5MzMxNTc2NzgsLTE4NzI3
MjY1ODAsMTcyOTAyNjQ0MiwtMTgyMzk3MTYyMCwtOTE1NzM3OT
YzLC0xMzM5NzUxMDYwLC0yMTk1MTA3MjhdfQ==
-->