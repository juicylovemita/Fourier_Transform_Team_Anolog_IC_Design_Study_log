# 文件信息

	此文件是“傅里叶变换队”学习模拟IC设计所写的学习日志(Study_log)，主要学习资料来自农姗姗老师提供的网盘文件。
	此日志初次创建的格式为txt，后期将转移为md格式。
	团队成员：黄宇杰、林正淇、朱思成
	创建时间：2025年3月31日16点29分
	创建者：朱思成
	起始记录时间：2025年3月27日
	截止记录时间：/

[网盘文件链接](https://pan.baidu.com/s/1VITbTJj5VlxN4cbrWcuzOA?pwd=s621)

# EDITORS_ATTENTION!
	日志格式：
	（二级标题）
		## 
		时间(编辑者:材料文件1路径;材料文件2路径;其他资源链接...)
	(三级标题)

	### 时间点1+简报(精确到小时):
		日志正文1;
	### 时间点2+简报(精确到小时):
		日志正文2;
	### 时间点3+简报(精确到小时):
		日志正文3;

	...

# Logs

## 2025年3月27日(朱思成:zyhc\虚拟机\安装流程及案例）
### 5P.M.开始与创建原理图:
~~发现曾益慧创隐藏了往期模拟IC设计培训视频~~；在虚拟机配置好的前提下，依照word文件的教程，绘制了一个OPAM的原理图，这次checkandsave之后发现以前遇到的报错不见了，原理图通过。详细过程如下；

进入虚拟机，进入home，进入eda'文件夹，进入project文件夹；
在该路径下右键，打开终端；
输入csh并回车，输入virtuoso&并回车，此时Cadence Virtuoso软件已打开；

随后建立测试库，点击Tools,点击Library Manager,点击File，New，Library，Directory栏填/home/eda/project,name栏填lab_practice.点击OK。工艺库选择见面选择“依附于一个已存在的工艺库”（Attach...)，TechnologyLibrary栏选择UMC_18_CMOS;
 至此，完成新库的创建。
 回到LibraryManager，选中刚刚新建的库，点击New来新建设计单元（Cell）和视图（View），层级关系为：库>设计单元>视图。设计单元命名为OPAM，即运算放大器，视图选择原理图schematic，完成电路原理图的创建，，自动打开原理图界面。
 
 原理图绘制主要是两个工具:添加器件、添加导线。快捷键i添加器件。library点browse，在新界面勾选Show Categories，随后选择想要的器件。这里选择UMC_18_CMOS/MOS/的N33MM和P33MM，即3.3V的NMOS和3.3V的PMOS，***注意View一栏中选symbol***，回到Add Instance，在Name填入器件名称，随后即可放置器件。鼠标选中器件后按Q~~释放剑气~~可以进去器件的属性界面，这里可以修改器件名称，宽、长、栅指数量、个数等属性；

快捷键W为连线快捷键

快捷键P为加PIN快捷键，依次加上VDD，GND，IIN，VINP，VINN五个输入PIN和OUT一个输出PIN。***此外这里还需要选择PIN类型，教程中没有写到***。我是VDD是power，GND是gnd，其他都是signal，也只能先这样，在网上查了一下都没什么说法。

然后器件属性：
|VDD=3.3V|  |
|--|--|
| C0=1.011831p F | Ibias=54uA |
|W1=18.5um  |L1=0.35um  |
|W2=1.1um|L2=0.6um|
| W3=5.0um | L3=0.7um |
|W4=20.8um| L4=3.0um |
|W10a=1160um|L10a=20um|
| W10b=1160um | L10b=20um |

这个器件属性的地方~~那个教程文档太坑爹了~~要注意和原理图结合起来看，W1a的意思就是M1a器件的Width，以此类推。

另外M10要注意，他宽度太宽，要用上栅指结构或者multipliers，两个方法做出来的东西仿真没有太大区别，但是结构会不一样。

然后电路图：
![OPANM电路原理图](/imgs/2025-04-01/Fa02K5kmJfGd1GAf.png)

做完之后Checkandsave.

### 9P.M~~被教程困惑~~:

word文件中所给的电路参数表过于难以理解，合作分析之后成功理解：要联合示例原理图中的符号理解，并且部分参数在当前阶段没用；

### 10P.M.创建自定义Symbol:
将OPAM创建了一个自建Symbol，详细过程如下：
在原理图编辑窗口，点击Create，Cell view，From Cell view，在打开的Cell view From Cell view窗口中，编辑symbol名，Tool/Datatype选择Symbol，点击OK，下一个窗口中安排PIN的位置，IIN VINP VINN放在left，OUT放在right，VDD放在top，GND放在bottom，安排好后点击OK。此时返回library manager可以看见自定义的symbol已经生成到了lab_practice的opam单元中了。

随后搭建仿真电路图。在该路径下新建opam_test单元，搭建如下电路![仿真电路](/imgs/2025-04-01/tRqRUPJennsxe1nL.png)

电路参数如下图
![仿真电路参数](/imgs/2025-04-01/Aqhs7VCgqku3Br9X.png)

在该原理图内，选中symbol后按shift+e可以查看symbol内部电路，按ctrl+e可退出查看内部电路。

## 2025年3月28日(朱思成:zyhc\虚拟机\安装流程及案例）

### 4P.M.Symbol的SpectreDC仿真:
 搭建用于仿真OPAM的Symbol的电路。进行DC仿真，成功得到正确的输入输出波形，详细过程如下：
 进入原理图界面，点lanuch，点ADEL，调出AnalogEnvironmentSpectre，点choose，选dc分析，sweepvariable选componentparameter，点selectcomponent，这时自动返回原理图界面，单击VCM源，跳出componentparameter，双击选dc，扫描范围0-3.3V，点击OK；


 回到ADE界面，OUTPUTS，tobeploted，selectondesign，这时会返回原理图，鼠标单击output致其变为紫色，如果想看电流大小，就点击原理图中的端口，出现圆圈。ADE窗口的analyses下面多了一行dc，这时就可以开始仿真了；

点击绿色icon（Netlist&Run）

仿真结束之后就会自动出现总电流（直流工作点）和输出电压与共模输入电平的关系图。


### 5P.M.Symbol的AC仿真:
首先是AC仿真的目的。就是在一个静态工作点上施加一个小信号，然后根据输出的信号来分析放大系数频率响应等等特性。
两个输入端幅值设为500mV，相位设置为0。ACmagnitude两个都设为0.5V，Amplitude两个都设0.5mV；

设完后调出分析设置，选ac分析，频率变化选1-1G，然后点NetlistandRun；

results窗口选Direct plot，选ACmagnitude&phase，在原理图窗口点输出线致其变色，然后按esc；

 进行AC仿真，成功得到正确的幅频特性曲线和相频特性曲线；

按v唤出光标，拖动光标可以读数。
 
### 6P.M.AC仿真结果的处理:
tools-calculator调出calculator；
选vf，回原理图，点输出out，右下角白色窗口选择测量bandwidth，再点击计算按钮。按照教程的说法，可以直接得到带宽等数据；
但是发现找不到计算结果；

## 2025年4月1日(朱思成:）
### 8P.M.迭代为MarkDown和GitHub:
 将此日志文件换为MarkDown格式，并上传至GitHub。
    [GitHub链接](https://github.com/juicylovemita/Fourier_Transform_Team_Anolog_IC_Design-Study_log)
    并将二位队友拉进来了，他们应该会在下面的日志EDITOR中出现。
### 9P.M.更新日志:
根据农老师的要求改进了上面的日志：更加详细。
~~写到力竭了~~

## 2025年4月2日(朱思成:）
### 4P.M.更新日志:
根据农老师的要求改进了上面的日志：更加详细。

## 2025年4月3日(朱思成:）
### 2P.M.看拉扎维教材
感觉学了这么长时间，有的时候只知道操作，但是不知道这样操作的意义，所以决定先去看课本，快速地浏览一下。
从第一章看起，主要看了一下MOS管的原理以及特性曲线，还有一些重要的参数等等。
### 8P.M.zyhc明天有IC设计培训直播
不过有假期行程，只能等回来看回放了。
