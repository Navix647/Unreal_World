
一、导入骨骼网格体
骨骼网格体即模型要在模型雕刻阶段就要雕刻好表捕所需的表情体(blendshape)，后面表捕的效果直接由雕刻的表情体决定。

导入fbx时，需要确保勾选了导入变形目标选项，一般UE默认是未勾选的，否则无法导入表情体。

表情体在UE中称作变形目标，直接存储在骨骼网格体中，打开骨骼网格体编辑视图，点击窗口/变形目标预览器可以打开变形目标的预览视图，在视图中可以修改变形目标的权重值来查看变形目标的变形效果和多个变形目标的混合效果。

变形目标在UE中不可再次修改，需要修改变形目标，只能到3D建模阶段中修改后再次导入。

每一个变形目标都是一个动画曲线，导入后UE5会自动生成对应的动画曲线，我们可以在添加动画曲线时看到对应变形目标名称的曲线。对于动画曲线的定义，可以查看我的这篇博文。

二、表情捕捉
1.基于LiveLinkFace的表捕流程
在开始开发表捕之前，我们应该首先要了解表捕的流程，了解全貌才能有条不紊。

我这里使用的是苹果的LiveLinkFace来实现表情捕捉的，所以流程也是基于LiveLinkFace的流程，其他的实现方案可能会有所不同。

首先我们要确保启动了UE5的LiveLink插件，一般UE5会默认启用LiveLink插件，当UE5启用了LiveLink插件后，在编辑器的窗口/虚拟制片/LiveLink可以打开LiveLink源管理视图。对于LiveLink源管理视图的具体参数即使用详情见官方文档。

首先LiveLinkFace会使用ARKit标准识别人脸的52个表情，并计算出变化值存储到对应的52个参数中，然后由LiveLink协议将52个参数的参数名及参数值实时同步到UE编辑器中，然后在动画蓝图中使用LiveLinkPose节点接收指定源传送进来的数据，根据接收到数据修改对应的Pose权重，再通过Pose驱动对应的表情体(动画曲线)，然后通过PoseAssest将各个修改后的Pose融合成一个整体，于是一个完整的表情就被驱动起来了。

苹果ARKit BlendShape ：[ARFaceAnchor.BlendShapeLocation | Apple Developer Documentation](https://developer.apple.com/documentation/arkit/arfaceanchor/blendshapelocation)。

2.LiveLinkFace连接UE
首先我们需要将LiveLinkFace连接到UE以便数据通信。

在苹果应用商店下载LiveLinkFace，进入App后进入设置/LiveLink界面，点击添加，为LiveLinkFace添加数据发送地址，地址就是UE编辑器运行的电脑的IP地址，端口可以使用默认的1111端口。

![img](https://i-blog.csdnimg.cn/blog_migrate/5574bd6e002f9bd9bb16aba571041162.png#pic_center)
然后需要使苹果手机与UE编辑器所在电脑处于同一局域网中，因为目前LiveLink只支持同一局域网内的数据同步。

LiveLinkFace在UE中没有单独的管理源，所以LiveLinkFace的数据源会直接通过UE的LiveLink消息总线进行传输，我们可以在LiveLink管理视图的源/消息总线源下看到接收到数据源，点击数据源后，就可以在主题命名下看到对应的数据源了，如果LiveLinkFace使用的不是默认端口，则我们需要在对应的源类型的Settings/Advanced/Port Number`设置对应的端口。



接收到数据源之后，管理视图看起来就是这样子的：



Subject Name：数据同步的主体；
Role：应用的LiveLink协议规则，LiveLink支持多个协议规则，包括用于动画或骨骼数据的LiveLinkAnimationRole、基础规则LiveLinkBasicRole、用于相机同步的LiveLinkCameraRole和LiveLinkTransformRole、专门用于相机镜头的LiveLinkLensRole、用于光源数据的LiveLinkLightRole等；
带颜色的小圆点：小圆点有黄色和绿色两种状态，黄色状态表示数据主体有连接上，但是在规定时间段内没有接收到数据，绿色表示数据接收正常。
需要注意，根据LiveLink的协议，LiveLink的数据传输只会传输给数据源端搜索到的第一个同一地址同一端口的接收端，及如果在一台主机上启动了两个UE使用同一个端口接收数据，只会有一个UE能接收到数据。

3.编辑动画曲线
由于模型雕刻流程中可能为了更灵活的控制雕刻出来的表情体可能会多于ARKit的52个表情，或者可能表情体命名规范不符合ARKit标准，此时我们就需要UE中再做一层映射，其中编辑动画曲线就是其中一步。

我们可以双击骨骼资产，再骨骼资产编辑视图中点击创建资产/创建动画/当前姿势，使用当前Pose创建一段空的动画序列，创建的空的动画序列只是一个Pose所以只有一帧。



我们需要将序列扩充到52帧，一帧对应一个Pose，一个Pose对应一个ARKit表情，双击打开动画序列，在曲线编辑视图右键，选择在开始处附加，然后输入51，这样我们就得到了一个52帧的动画序列。



然后单击曲线编辑视图的曲线下拉三角，在菜单中选择添加曲线，然后我们就能看到表情体名称对应的曲线了，把我们需要用的曲线全部添加进来。



然后在每一帧使用需要的曲线K出我们需要的表情，以便后一步我们生成Pose资产：



当然这一步其实也是可以直接由动画师一步到位，及动画直接提供一个52帧，每帧对应一个表亲Pose的动画序列，UE导入后可以直接使用这个动画序列生成Pose资产。

4.生成Pose资产
通过上一步，我们就拥有了一个有52个表情帧的动画序列，我们可以右键动画序列，在创建/创建姿势资产中，使用这个动画序列创建相应的姿势资产。

在创建姿势资产视图中，提供一个Pose Name列表给我们指定每一个Pose的名称，一行为一个Pose名称，行序号与帧序号对应。



需要注意，第一行必须是Default名称，或者任何无意义的名称，用于对应动画序列的第0帧。

输入的Pose名称需要符合ARKit的标准命名，否者将无法正确映射。

这里提供一份ARKit的标准表情Pose命名：

Default
EyeBlinkLeft
EyeLookDownLeft
EyeLookInLeft
EyeLookOutLeft
EyeLookUpLeft
EyeSquintLeft
EyeWideLeft
EyeBlinkRight
EyeSquintRight
EyeLookDownRight
EyeLookInRight
EyeLookOutRight
EyeLookUpRight
EyeWideRight
JawForward
JawLeft
JawRight
JawOpen
MouthFunnel
MouthPucker
MouthLeft
MouthRight
MouthSmileLeft
MouthSmileRight
MouthFrownLeft
MouthFrownRight
MouthDimpleLeft
MouthDimpleRight
MouthStretchLeft
MouthStretchRight
MouthRollLower
MouthRollUpper
MouthShrugLower
MouthShrugUpper
MouthPressLeft
MouthPressRight
MouthLowerDownLeft
MouthLowerDownRight
MouthUpperUpLeft
MouthUpperUpRight
BrowDownLeft
BrowDownRight
BrowInnerUp
BrowOuterUpLeft
BrowOuterUpRight
CheekPuff
CheekSquintLeft
CheekSquintRight
NoseSneerLeft
NoseSneerRight
TongueOut



然后我们需要设置Pose资产的Asset Details/Additive/Additive/Base Pose为Default姿势，基础姿势设置的不对会导致表情映射错位，致使表情驱动异常。

Pose资产配置好之后，我们就可以到动画蓝图中应用了。

备注：

其实第三第四步在UE中是可以省略的，只需要动画师提供一个包含了52个表情帧的动画序列，模型师在雕刻表情体时按照ARKit的命名标准来命名表情体，在导入时UE就会自动生成符合ARKit表情命名标准的曲线，这样在UE中也就不需要再做映射了。当然方便的代价就是丧失一定的灵活性，需要根据具体情况取舍。

5.驱动表情体
在动画蓝图中使用LiveLinkPose接收LiveLinkFace的数据源，通过姿势资产映射数据到相应的表情体上，于是表情就被驱动了。



当LiveLinkFace与UE连接上之后，可以直接在LiveLinkPose节点的LiveLinkSubjectName直接指定接收的数据源，也可以通过LiveLinkSubjectName变量来传入。

然后将动画蓝图应用到角色上，就可以测试效果了：

由于CSDN的gif图最大只支持5MB，所以请移步查看

6.表情扩展
ARKit的52个标准表情都是一些人体的基础表情，有的时候因为一些需求，可能需要使用到一些较为夸张的表情，这些表情是真人做不出来的，此时我们就需要对表情进行扩展。

扩展表情的方式有两种，一种是直接在雕刻表情体时就把表情调得夸张一些，然后直接驱动；另一种是将一个夸张的表情单独调一个表情体，然后在动画蓝图中使用程序驱动，通过条件触发。

但是需要注意的是，过于夸张的表情在做融合的时候可能会产生比较诡异的效果，这个就需要具体情况具体调整了。

第一种方式在UE中不需要再做调整，这里就略过了，这里主要记录第二种方式。

首先我们需要导入一个带有夸张表情体的骨骼网格体，但由于ARKit的52个标准表情中没有与我们夸张表情对应的参数，所以我们需要自己涉及一套算法来分割组合52个参数，使用分割组合的参数来驱动夸张表情。如：我们有一个大笑的夸张表情，为了不影响原来的笑的表情，我们可以将MouthSmileLeft、MouthSmileRight两个参数分割成小于0.4的部分和大于0.4的部分，小于0.4的部分我们不做处理，大于0.4的部分用于驱动夸张表情。



首先我们需要了解LiveLink协议传输数据的数据结构，根据官方文档的说明：

主体由以下数据组成：

名称。
不会变更的静态数据（例如动画角色骨架）。
一个或多个"帧"数据（例如动画角色的骨骼变换）。
名称：用于使用LiveLinkSubjectName来获取对应的数据；
静态数据：数据的键值，及参数名，不同的协议规则，数据类型不同；
帧数据：与静态数据数量对应的参数值，不同的协议规则，数据类型不同；
LiveLinkFace使用的是LiveLinkBasicRole协议规则，我们可以使用EvaluateLiveLinkFrame节点来获取指定名称的数据源传输过来的静态数据和帧数据：



在LiveLinkFace传输过来的数据中，StaticData就是52个表情的参数名，FrameData则是参数名对应的参数值和元数据，参数值取值范围为0-1；

如上面的逻辑，我拆分了MouthSmileLeft和MouthSmileRight用于驱动夸张表情，同时组合了JawOpen参数用于控制表情的触发，当三个参数满足触发条件之后，则取MouthSmileLeft和MouthSmileRight中最大值，将其0.6-1的取值映射到范围映射到0-0.5上，然后将值存储到对应的曲线上，但表情退出时使用插值进行平滑退出。

当我们将所有的夸张表情曲线值更新关闭之后，就可以在AnimGraph视图中使用ModifyCurve节点将曲线值更新到实际的曲线上了，更新上去之后，表情就被驱动了。



这种方式有个弊端，就是对平滑处理的要求比较高，否则很容易出现表情的跳跃，导致表情过渡不自然。

还有一种比较好的处理方式，就是将一个表情拆分成标准部分和夸张部分，当要做夸张表情的时候将夸张表情叠加到标准表情之上，这样就算平滑处理的不是很好也不会出现表情的跳跃，但是这种方式又对表情雕刻有很高的要求，建模师要能够有这个能力把一个表情拆分成标准部分和夸张部分。

三、动作捕捉
1.基于Xsens惯性动捕的动作捕捉
和表捕一样我们也需要先了解动捕的整个流程。

Xsens的动捕服在收集完动捕数据之后通过连在主机上的专门的接收天线传输到Xsens的动捕软件里，动捕软件再将数据应用到标准骨架上，然后动捕软件再通过LiveLink协议传输到UE编辑器上，这也要求运行动捕软件的主机和运行UE的主机在同一局域网内或是在同一主机上。XSens传输过来的数据再通过骨骼重定向，就可以直接映射到我们数字人的骨骼上了，然后就可以像动画序列驱动骨骼一样使用动捕数据驱动骨骼了。

2.Xsens动捕软件
Xsens动捕服连接Xsens动捕软件和Xsens动捕软件连接UE，我所用的环境是已经配置好的，也没有自己亲手配置过，就不臆想误导别人了，可以参参考这两篇文章：

Xsens动捕使用说明 - 知乎 (zhihu.com)

Xsens惯性动捕连接虚幻UE5教程 - 知乎 (zhihu.com)

3.安装MVN Live Link插件
Xsens的动捕数据有专门的接收总线，要能在UE中接Xsens动捕数据，需要MVN Live Link插件的支持，目前我发现Epic商城的MVN Live Link插件已经暂停提供了，这里提供了一份UE4.27、5.0和5.1版本的插件百度云地址：https://pan.baidu.com/s/1axao8Uf-0BSyqYzxqmuEug ?password=vrmu。

解压后将插件拷贝到引擎的Engine\Plugins\Marketplace目录下。

弄好之后启动UE到插件管理视图启动LiveLinkMvnPlugin插件。

需要注意的是Xsens动捕软件内置了MVN的支持，也需要设置一下。

启动插件后，我们就可以在LiveLink管理视图的源处看到专门的Xsenx Live Source接收专线。通过设置好端口，就可以接收到XSens传输过来的数据了。



4.骨骼重映射
MVN插件已经为我们设定好了映射资源，在启动了MVN插件之后，我们右键骨骼资产，可以在右键菜单中看到Xsens选项，选择Create Remap Asset可以直接创建对应骨骼的重映射资源。



双击打开创建的资源，在细节面板中：

Bones Names/Naming Convention选项我们可以选择Unreal/Metahuman，这是MVN插件为UE的数字人标准骨骼映射好的映射资源，如果自己的数字人主要骨骼层级符合UE数字人标准，就可以直接使用，如果有差异，则可以在Live Link Mvn Remap处手动设置映射关系；
Reference Pose/TPose Animation 选项设置映射资源需要使用T-Pos资产，这里要注意，MVN插件必须使用T-Pos，如果骨骼资产是A-Pos，我们可以在骨骼资产编辑视图编辑骨骼到T-Pos，然后在创建资产/创建动画/当前姿势来创建一个T-Pos动画序列，当然也可以直接交由动画师提供一个T-Pos动画；
Reference Pose/Is Forward Y：在应用动捕数据后，出现方向不正确是勾选；
Live Link Mvn Remap/Remapping Assets：设置骨骼重映射；
Live Link Mvn Remap/Skeleton：设置映射资源所使用的骨骼资产，创建资产是会默认设置好。
5.驱动骨骼
然后直接使用LiveLinkPose节点，设置LiveLinkSubjectName到对应的数据源，设置LiveLinkPose节点的Details/Retarget/Retarget Asset为上一步创建的映射资产，然后就可以通过Xsens动捕数据驱动数字人骨骼运动了。



四、物理发型
1.启动GRoom插件
UE的物理发型功能由GRoom插件提供，一般UE引擎默认是未启用GRoom插件的，所以我们需要自己去启用。

同时也要启动Alembic GRoom Importer插件来支持GRoom缓存的导入。

为了使UE支持毛发渲染，我们还需要启用Project Settings/Engine/Rendring/Support Compute Skin Cache选项。

2.创建GRoom资产
GRoom资产是在模型建模阶段制作好的头发资源，以abc缓存文件的格式导入到UE，GRoom插件读取到之后，会自动生成GRoom资产。



上面第一个发丝图标的就是GRoom资产。

进入GRoom编辑视图，在LOD选项卡中可以配置GRoom的LOD信息，在Strands选项卡中可以配置发丝的粗细等，在Physics选项卡中可以启用GRoom的物理模拟。

3.创建绑定
GRoomBinding资产可以让GRoom适配到骨骼网格体上，并且使GRoom跟随骨骼网格体运动。

右键GRoom资产，Create Binding可以使用指定的GRoom资产创建GRoomBinding资产，创建时需要指定资产绑定的骨骼网格体。



在第二小节中右侧的资产图标就是GRoomBinding的图标。

4.应用GRoom资产
创建角色蓝图，为角色添加Skeletal Mesh组件，并赋予骨骼网格体，然后再为角色添加GRoom组件，依附于Skeletal Mesh组件之下，在GRoom组件的GRoom/GRoom Asset设置GRoom资产，在GRoom/Binding Asset设置GRoomBinding资产，在Materials设置头发的材质。这样一个物理发型就应用到角色身上了。为了使骨骼网格体能应用绑定，还需要将骨骼网格体的AssetData/LOD0/LOD Info/Skin Cache Usage选项设置为Enabled。

有时我们为发现设置了绑定资产之后，头发会消失，此时我们删掉原来的绑定资产，然后重新创建绑定资产，如果还是没有效果，则需要删掉GRoom资产，重新导入。

当相机离头发很近时头发消失
最近做虚拟人，遇到一个问题，即当相机靠进到虚拟人一定距离时，人物的头发直接不渲染了。

如图：

由于CSDN的gif图最大只支持5MB，所以请移步查看

这可能是UE为了性能需求，为GRoom设置了渲染边界，超出阈值时就不渲染了，我们需要将所有的GRoom和GRoom所依附的骨骼网格体的Readering/Bounds Scale，UE4是Readering/Bounds Scale，UE5则是Readering/Advanced/Bounds Scale，设置到合适的值，设置这个值会影响到物体聚焦的距离，设置得太大，聚焦时相机会跑得非常远。

这里我设置成了5，然后再看一下效果：

由于CSDN的gif图最大只支持5MB，所以请移步查看

GRoom在设置Binding Asset后不显示
有时在使用GRoom时发现配置了绑定资产后GRoom会消失，而去掉绑定资产后GRoom又会显示，导致这个问题的原因可能是骨骼网格体使用GRoom的必要配置项没启用。

首先确保Project Settings/Engine/Tending/Optimizations/Support Compute Skin Cache设置是勾选上的；

然后打开使用的骨骼网格体编辑视图，将Asset Details/LOD 0/LOD Info/Skin Cache Usage设置为Enabled。

-SL3Amq2j-1714308968770)]

GRoom在设置Binding Asset后不显示
有时在使用GRoom时发现配置了绑定资产后GRoom会消失，而去掉绑定资产后GRoom又会显示，导致这个问题的原因可能是骨骼网格体使用GRoom的必要配置项没启用。

首先确保Project Settings/Engine/Tending/Optimizations/Support Compute Skin Cache设置是勾选上的；

然后打开使用的骨骼网格体编辑视图，将Asset Details/LOD 0/LOD Info/Skin Cache Usage设置为Enabled。

然后绑定就生效了。



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2NTIwMTA0ODldfQ==
-->