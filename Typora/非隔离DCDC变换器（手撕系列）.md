## 一、能量存储与传输器件

DC-DC功率变换电路利用能量存储器（<font color = red>电感器</font>）件进行滤波，利用能量传输器（<font color = red>电容器</font>）件来传输电能，以改变电压和电流的幅度。

### 1.1 电感器

​	参考创客皮特的电感基础系列文章：

- [电感基础1——为什么把‘线’绕成‘圈’就是电感？什么是电感？](https://zhuanlan.zhihu.com/p/149748238)
- [电感基础2——电感的单位、电压电流关系、时间常数和阻抗 ](https://zhuanlan.zhihu.com/p/149830125)
- [电感基础2——电感的单位、电压电流关系、时间常数和阻抗](https://zhuanlan.zhihu.com/p/149830125)
- [电感基础4——什么是LC电路的“谐振频率”？](https://zhuanlan.zhihu.com/p/156456031)
- [电感基础5——电感选型时需要考虑什么？额定电流、饱和电流、自谐振频率……](https://zhuanlan.zhihu.com/p/157376214)

### 1.2 电容器

​	参考创客皮特的电感基础系列文章：

- [电容基础1——储能和滤波](https://zhuanlan.zhihu.com/p/145810291)
- [电容基础2——充放电时间常数](https://zhuanlan.zhihu.com/p/146297149)
- [电容基础3——阻抗和容抗](https://zhuanlan.zhihu.com/p/146444827)
- [电容基础4——电容种类和ESR（等效串联电阻），超级电容能否代替电池？](https://zhuanlan.zhihu.com/p/148037515)
- [电容基础5——RC低通滤波器和RC高通滤波器 ](https://zhuanlan.zhihu.com/p/148159402)

### 1.3  磁通平衡与伏秒平衡、电荷平衡与安秒平衡！

请参考：[1 磁通平衡与伏秒平衡、电荷平衡与安秒平衡！ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/349733079)

## 二、 单电感统一模型

### 2.1 基本理论

<font color=red>核心思想：有且仅有、降本增效</font>

主要的非隔离型DCDC变换器分为两类，单电感模型、双电感模型

​                                                                                                                            <span id="三个端子">单电感模型-三个端子</span>

![image-20240129104431493](Typora_Png/image-20240129104431493.png)

​                                                                                                                         	<span id="两个端子">*单电感模型-两个端子*</span>

![image-20240130100458637](Typora_Png/image-20240130100458637.png)

电感的公式：<font size=5>$V_L = L\frac{di_L}{dt}$</font>      $\rightarrow$     <font size=5>$\Delta i_L = \frac{1}{L} \int_{0}^{T}V_Ldt$</font>

​	由上式可知：电流为电压的积分，电压可以突变，电流不能突变

​	对于一个稳定的系统，稳定后电感要处于平衡状态，即一个周期T内电感的电流变化量要为0，否则会一直上升或下降，从而脱离平衡状态（详见1.3节）。

![image-20240129170441103](Typora_Png/image-20240129170441103.png)

电感电流、电压在一个周期T内的变化如图所示，也就是电感两端电压的积分为0，意味着稳定后电感电压的平均值为0

​                                                                                                         <font color=red size=5>$V_{avg} = \frac{1}{T} \int_{0}^{T}V_Ldt$</font>

对于单电感模型：$arg(V_L)==0  \rightarrow  arg(V_X-V_Y)==0  \rightarrow  arg(V_X)==arg(V_Y)$

​	[两个端子时](#两个端子)：$arg(V_X)==arg(V_Y)     \rightarrow   $           <font color=red>$V_X == V_Y$</font>

​	[三个端子时](#三个端子)：$arg(V_{X1},V_{X2})==arg(V_Y) == V_Y$

​						$V_{avg} = \frac{1}{T}(\int_{0}^{T_{X1}}V_{X1}dt + \int_{0}^{T_{X2}}V_{X2}dt)  =\frac{1}{T}\int_{0}^{T_Y}dt$

​				$\Rightarrow \int_{0}^{T_{X1}}V_{X1} = \int_{0}^{V_{X2}}V_{X2}dt = \int_{o}^{T}V_Ydt$

​	    		$\Rightarrow T_{X1}V_{X1} + T_{X2}V_{X2} = T_YV_Y$    <font color=purple> ($T = T_Y = T_{X1} + T_{X2}$)</font>

<font color=red>当时间全部作用在$V_{X1}$上时：$T_YV_{X1}=T_YV_Y   \Rightarrow V_Y = V_{X1}$</font>

<font color=red>当时间全部作用在$V_{X2}$上时：$T_YV_{X2}=T_YV_Y  \Rightarrow  V_Y = V_{X2}$</font>

​				<font color=red>所以对于单电感模型来说：$V_Y$必然介于$V_{X1}$和$V_{X2}$之间。</font>

​				对于单电感DCDC变换器来说，有三个电平0、$V_{in}$、$V_{out}$。所以有且仅有三种可能：

|       |        $V_X$        |   $V_Y$   |              备注              |
| :---: | :-----------------: | :-------: | :----------------------------: |
| 端口  |    $X_1$、$X_2$     |    $Y$    |                                |
| 类型1 |     0，$V_{in}$     | $V_{out}$ |    $0<V_{out}<V_{in}$，Buck    |
| 类型2 |    0，$V_{out}$     |  $V_in$   |   $0<V_{in}<V_{out}$，Boost    |
| 类型3 | $V_{in}$，$V_{out}$ |     0     | $V_{out}<0<V_{in}$，Buck-Boost |

### 2.2 三种基本电路

​				单电感模型如下图示：

![image-20240129104431493](Typora_Png/image-20240129104431493.png)

#### 2.2.1 buck电路（0 < $V_{out}$ < $V_{in}$）

​	根据单电感模型设计：<font color=red>$V_Y$必然介于$V_{X1}$和$V_{X2}$之间，当$0 < V_{out} < V_{in}$ 时</font>![](Typora_Png\Snipaste_2024-02-23_13-23-48.png)

开关管有MOSFET、IGBT等，选用开关管进行填充	

![](Typora_Png\Buck_Switch.png)

​	只考虑能量单向流动，可以用续流二极管（便宜）代替开关管，根据能量流动分析，只有如下情况符合设计逻辑：![](Typora_Png\Buck_二极管.png)

由单电感统一模型的结论$\Rightarrow T_{X1}V_{X1} + T_{X2}V_{X2} = T_YV_Y$    <font color=purple> ($T = T_Y = T_{X1} + T_{X2}$)</font>

对于开关管：$T = T_{on} + Y_{off}$

​						$d = \frac{T_{on}}{T}$

在连续状态下，计算输出电压和输入电压的比例关系：$\Rightarrow T_{X1}V_{X1} + T_{X2}V_{X2} = T_YV_Y$ 

​																							$\Rightarrow dT\cdot V_{in} + 0\cdot \frac{(1 -d)}{T} = V_{out}\cdot T$

​																							$\frac{V_{out}}{V_{in}} = d$

电路输出分析：

​	开关管闭合时，

#### 2.2.2 Boost电路（0 < $V_{in}$ < $V_{in}$)

根据单电感模型设计：<font color=red>$V_Y$必然介于$V_{X1}$和$V_{X2}$之间，当$0 < V_{out} < V_{in}$ 时</font>

![](Typora_Png\Boost_One.png)

为适应正常情况下左侧输入，右侧输出的看图习惯，并用开关管进行填充：![](Typora_Png\Boost_switch.png)

只考虑能量单向流动，可以用续流二极管（便宜）代替开关管，根据能量流动分析，只有如下情况符合设计逻辑：![](Typora_Png\Boost-erjiguan.png)

在连续状态下，计算输出电压和输入电压的比例关系：$\Rightarrow T_{X1}V_{X1} + T_{X2}V_{X2} = T_YV_Y$ 

​																							$\Rightarrow V_{in}\cdot T = dT\cdot 0 + (1-d)T\cdot V_{out}$

​																							$\frac{V_{out}}{V_{in}} = \frac{1}{1-d}$

#### 2.2.3 Buck-Boost电路($V_{out} < 0 < V_{in}$)

根据单电感模型设计：<font color=red>$V_Y$必然介于$V_{X1}$和$V_{X2}$之间，当$V_{out} < 0 < V_{in}$时，由此可知$V_{out}$和$V_{in}$是反向的。</font>,

![](Typora_Png\Buck_Boost.png)

根据看图习惯，改为一边输入，一边输出，并用开关管进行填充：![](Typora_Png\bUCK_bOOSTsWITCH.png)

只考虑能量单向流动，可以用续流二极管（便宜）代替开关管，根据能量流动分析，只有如下情况符合设计逻辑：![](Typora_Png\02231700.png)

在连续状态下，计算输出电压和输入电压的比例关系：$\Rightarrow T_{X1}V_{X1} + T_{X2}V_{X2} = T_YV_Y$ 

​																							$\Rightarrow dT\cdot V_{in} - (1 - d)T\cdot V_{out} = 0$

​																							$\frac{V_{out}}{V_{in}} = \frac{d}{1-d}$

当$d < 0.5$时： $\frac{V_{out}}{V_{in}} < 1$     $\rightarrow$  $V_{out} < V_{in}$   $\rightarrow$ Buck电路

当$d > 0.5$时： $\frac{V_{out}}{V_{in}} > 1$     $\rightarrow$  $V_{out} > V_{in}$   $\rightarrow$ Boost电路

## 三、 双电感统一模型

### 3.1 基本理论

​	双电感统一模型实际上就是把两个单电感统一模型相互连接在一起了，怎么将两个电感串联起来还不影响单电感统一模型的结论呢？最终选择用一个电容<font color=red>$C_f$</font>将两个电感隔开。<font color=red>$C_f$</font>上的电流可以突变，但是电压不能突变，正好和电感相反，所以可以通过它调节换态后的电流方向。

​	双电感统一模型的电路图如下：

![](Typora_Png/2024-02-24_15-04-28.png)

​	我们把整体当作两个单电感统一模型来逐一分析：

对于$L_1$：B点相当于[单电感统一模型](#三个端子)的X端，A点相当于Y端，根据单电感统一模型的结论：$T_{x1}V_{x1} + T_{x2}V_{x2} = T_yV_y (T = T_y = Y_{x1} + T_{x2})$

​	对于单电感统一模型，由于电感特性，要保证电感磁通平衡，也就是A端和B端的平均电压要相等。

​				得到$L_1$两端平均电压计算公式：$dT\cdot V_B + (1-d)T\cdot (V_C + V_f) = V_A\cdot T$                                                             

​												       化简得：<span id="1式">$V_A = d\cdot V_B + (1-d)\cdot (V_C + V_f)$</span>                                                       [ <font color=red>1式</font>](#1式)

对于$L_2$：C点相当于[单电感统一模型](#三个端子)的X端，D点相当于Y端，根据单电感统一模型的结论：$T_{x1}V_{x1} + T_{x2}V_{x2} = T_yV_y (T = T_y = Y_{x1} + T_{x2})$

​	对于单电感统一模型，由于电感特性，要保证电感磁通平衡，也就是C端和D端的平均电压要相等。

​				得到$L_2$两端平均电压计算公式：$dT\cdot (V_B - V_f) + (1-d)T\cdot V_C = V_D\cdot T$                                                             

​												       化简得：<span id="2式">$V_D = d\cdot (V_B - V_f) + (1-d)\cdot V_C$</span>                                                       [ <font color=red>2式</font>](#1式)

​	[ <font color=red> 1式</font>](#1式)展开：$V_A = dV_B + V_C + V_f - dV_C - dV_f$

​	[ <font color=red> 2式</font>](#1式)展开：$V_D = dV_B -dV_f + V_C -dV_C$

​	观察后继续化简[<font color=red>1式</font>](#1式)：<span id="3式">$V_A = dV_B - dV_f + V_C - dV_C + V_f$</span>

  [ <font color=red>1式</font>](#1式) - [ <font color=red>2式</font>](#1式)得：<span id="3式">$V_A - V_D = V_f$</span>															                                               							[<font color=red>3式</font>](#3式)

将[<font color=red>3式</font>](#3式)代入[ <font color=red>1式</font>](#1式)或[ <font color=red>2式</font>](#1式)得：$V_A = dV_B + V_C + V_A - V_D -dV_C - dV_A+ dV_D$

​							 			 $dV_A - dV_B = V_C - V_D - dV_C + dV_D$

​					     			   $d(V_A - V_B) = (1-d)V_C - (1 - d)V_D$ 

​				    			得：<span id="4式"> $d(V_A - V_B) = (1 - d)(V_C - V_D)$</span>	                                                                                     [<font color=red>3式</font>](#3式)

接下来就是把$0 、 V_{in} 、 V_{out}$分配到A、B、C、D这四个位置上去，四个位置分配三个电压，意味着肯定有两个点位是相同的，不妨设相同的点位为$V_{x1}$,剩下两个分别是$V_{x2}、V_{x3}$,在分配时要注意以下几点：

1. A点 $\neq$ B点
2. C点 $\neq$ D点
3. A点 $\neq$ D点

> 具体原因如下：
>
> 1. A点 $\neq$ B点
>
>    将$L_1$当作一个单电感统一模型分析，A点相当于Y端，B点相当于X端，根据电感磁通平衡条件可知$V_A = d\cdot V_B + (1-d)\cdot (V_C + V_f)$
>
>    若$V_A = V_B$，意味着$d = 1$，此时明显不符合电感磁通平衡条件，所以说A点 $\neq$ B点。
>
> 2. C点 $\neq$ D点
>
> ​	  将$L_2$ 当作一个单电感统一模型分析，C点相当于X端，D端相当于Y端，根据电感磁通平衡条件可知$V_D = d\cdot (V_B - V_f) + (1-d)\cdot V_C$
>
> ​	  若$V_C = V_D$，意味着d = 0，此时明显不符合电感磁通平衡条件，所以说C点 $\neq$ D点
>
> 3. A点 $\neq$ D点
>
>    由电感磁通平衡条件可知$avg(V_A) = avg(V_B)$、$avg(V_C) = avg(V_D)$，代入[<font color=red>3式</font>](#3式)（$V_A - V_D = V_f$）结论得到$(V_B - V_C = V_f)$,若$V_A = V_D$,则相当于两个电感之间是通过导线连接，不符合双电感统一模型基本理论。

遵循上述分配注意点，分配情况有如下六种：

|    A     |    B     |    C     |    D     |
| :------: | :------: | :------: | :------: |
| $V_{x1}$ | $V_{x2}$ | $V_{x1}$ | $V_{x3}$ |
| $V_{x1}$ | $V_{x3}$ | $V_{x1}$ | $V_{x2}$ |
| $V_{x3}$ | $V_{x1}$ | $V_{x2}$ | $V_{x1}$ |
| $V_{x2}$ | $V_{x1}$ | $V_{x3}$ | $V_{x1}$ |
| $V_{x2}$ | $V_{x1}$ | $V_{x1}$ | $V_{x3}$ |
| $V_{x3}$ | $V_{x1}$ | $V_{x1}$ | $V_{x2}$ |

有双电感统一模型可知，它是由两个单电感统一模型经过一个电容连接在一起的，所以说双电感统一模型关于电容两边是对称，根据单电感模型有三种电压分配情况，也就是说双电感统一模型每种类型也有三种分配方式，所以上述分配情况根据对称性可简化为如下情况：

|        |    A     |   B(d)   |  C(1-d)  |    D     | 组合数M |
| :----: | :------: | :------: | :------: | :------: | :-----: |
| 类型一 | $V_{x1}$ | $V_{x2}$ | $V_{x1}$ | $V_{x3}$ |    3    |
| 类型一 | $V_{x1}$ | $V_{x3}$ | $V_{x1}$ | $V_{x2}$ |    3    |
| 类型二 | $V_{x2}$ | $V_{x1}$ | $V_{x1}$ | $V_{x3}$ |    3    |

详细的分配方式及电路名称见下表：

|        |     A     |   B(d)    |  C(1-d)   |     D     |    类型    |
| :----: | :-------: | :-------: | :-------: | :-------: | :--------: |
| 类型一 |     0     | $V_{in}$  |     0     | $V_{out}$ |    ZETA    |
| 类型一 |     0     | $V_{out}$ |     0     | $V_{in}$  |   SEPIC    |
| 类型一 | $V_{in}$  |     0     | $V_{in}$  | $V_{out}$ | Semi-Z INV |
| 类型一 | $V_{in}$  | $V_{out}$ | $V_{in}$  |     0     | Semi-Q INV |
| 类型一 | $V_{out}$ | $V_{in}$  | $V_{out}$ |     0     |   New#1    |
| 类型一 | $V_{out}$ |     0     | $V_{out}$ | $V_{in}$  |   New#2    |
| 类型二 | $V_{in}$  |     0     |     0     | $V_{out}$ |    Cuk     |
| 类型二 |     0     | $V_{in}$  | $V_{in}$  | $V_{out}$ | Boost(new) |
| 类型二 |     0     | $V_{out}$ | $V_{out}$ | $V_{in}$  | Buck(new)  |

### 3.2 基本电路

#### 3.2.1 ZETA电路

​	将ZETA类型的电位填补到双电感统一模型中去，电路如下：![](Typora_Png/2024-02-28_18-52-49.png)

根据电源侧能量流动的方向来决定开关方向，用开关管经行填充，将其变左侧输入、右侧输出的形式：

![](Typora_Png/2024-02-28_19-22-28.png)

只考虑能量单向流动，为了降本增效，可以用续流二极管（便宜）代替开关管，根据能量流动分析，只有如下情况符合设计逻辑：

![](Typora_Png/2024-02-28_19-50-28.png)

在连续状态下，计算输出电压和输入电压的比例关系：$\Rightarrow d\cdot (V_A - V_B) = (1-d)(V_C - V_D)$

​													  代入实际的点位关系：$\Rightarrow d\cdot (0 - V_{in}) = (1 -d)(0 - V_{out})$

​																							$\Rightarrow \frac{V_{out}}{V_{in}} = \frac{d}{1-d}$	       <font color=red>Buck-Boost电路</font>

当$d < 0.5$时： $\frac{V_{out}}{V_{in}} < 1$     $\rightarrow$  $V_{out} < V_{in}$   $\rightarrow$ Buck电路

当$d > 0.5$时： $\frac{V_{out}}{V_{in}} > 1$     $\rightarrow$  $V_{out} > V_{in}$   $\rightarrow$ Boost电路

注：ZETA是Buck-Boost电路，且输出电流连续，输入电流不连续，所以输出好滤波，输入不好滤波，且输出电压是正极性。

**输出电流连续性分析：**

> ​	二极管闭合时，$V_{in}$提供电压，$L_1$电流不能突变，开始继续能量。$C_f$开始充电，电容两边电压相等时，给$L_2$提供电压，最终输出侧$V_{out}$有电流流过。电流流动方向如图：![](Typora_Png/2024-02-28_20-33-47.png)
>
> ​	二极管断开时，$L_1$电流不能突变，向二极管提供能量，电容开始放电。$L_2$电流也不能突变，也开始向二极管释放能量，输出侧有电流。电流流动方向如图：
>
> ![](Typora_Png/2024-02-28_20-38-05.png)
>
> ​	根据上述分析可以看出，ZETA电路的输出电流是连续的，相对于单电感统一模型，该电路的输出更稳定，能更好的滤波。

**输出同电压方向及电路类型分析：**

> 方式一、	由输出与输入电压比例关系：<font color=red>$\frac{V_{out}}{V_{in}} = \frac{1}{1-d}$</font>
>
> ​									得到$\Rightarrow V_{out}与V_{in}$同向
>
> 方式二、利用$V_{out}$的输出范围分析
>
> ​	由单电感统一模型可知：<font color=#FF44FF>$V_Y$必然介于$V_{X1}$和$V_{X2}$</font>
>
> ​	对于$L_2$:<font color=#FF44FF>$0 < V_{out} < (V_{in} - V_f)$</font> 
>
> ​			又：<font color=#FF44FF>$v_f = V_{A} - V_D$</font>                                     ZETA电路的$V_A = 0$
>
> ​			则：<font color = #FF44FF>$0 < V_{out} < V_{in} + V_{out}$</font>
>
> ​			一般情况下，<font color=#FF44FF>$V_{in} > 0$</font>,那么必然<font color=#FF44FF>$V_{out} > 0$</font>，$V_{in}$与$V_{out}$同向。而且$V_{out}$既可以大于$V_{in}$，也可以小于$V_{in}$，所以 此电路为<font color=red>Buck-Boost电路</font>.
>
> ​	对于L1：$ V_{in} < 0 < 0 + V_f$
>
> ​			又：<font color=#FF44FF>$v_f = V_{A} - V_D$</font>                                     ZETA电路的$V_A = 0$，$V_D = V_{out}$
>
> ​			则：<font color=#FF44FF>$V_{in} < 0 < - V_{out}$</font>                                两边同乘以-1得$-V_{out} < 0 < V_{in}$
>
> ​			一般情况下，<font color=#FF44FF>$V_{in} > 0$</font>,那么<font color=#FF44FF>$V_{out} > 0$</font>时等式才能成立,故$V_{in}$与$V_{out}$同向。而且$V_{out}$既可以大于$V_{in}$，也可以小于$V_{in}$，所以 此电路为<font color=red>Buck-Boost电路</font>.

**分析一下两个Buck-Boost电路开关管和二极管得耐压关系：**

单电感模型Buck-Boost电路：

![](Typora_Png\02231700.png)

开关导通：

​				二极管承受的电压			$\rightarrow V_{in} + V_{out}$

<img src="Typora_Png/2024-03-01_19-52-16.png" style="zoom:50%;" />

开关关断：

​				开关管承受得电压				$\rightarrow V_{in} + V_{out}$

<img src="Typora_Png/2024-03-01_19-58-12.png" style="zoom:50%;" />

单电感模型Buck-Boost电路：![](Typora_Png/2024-02-28_19-50-28.png)

开关导通：

​				二极管承受的电压				$\rightarrow V_{in} - V_f = V_{in} - (0 - V_{out}) = V_{in} + V_{out}$

<img src="Typora_Png/2024-03-01_20-05-58.png" style="zoom:50%;" />

开关断开：

​				开关管承受的电压				$\rightarrow V_{in} - V_f = V_{in} - (0 - V_{out}) = V_{in} + V_{out}$

<img src="Typora_Png/2024-03-01_20-11-48.png" style="zoom:50%;" />

电感和二极管选取时，电压和电流都要预留1.5-2倍得余量，以防止过流。电流的选择是电路峰值电流得1.5-2倍。

#### 3.2.2 SEPIC电路

![](Typora_Png/2024-03-11_17-13-49.png)

将SEPIC电路的电位填补到双电感统一模型中去，电路模型如下：

![](Typora_Png/2024-03-11_17-21-16.png)

为更直观的分析电路，将输出变为负载，再根据输入侧在左、输出侧在右的形式变换电路图：

因一般把可控开关作用的时间和周期的比值取占空比，此时根据电流流向分析，$V_c$点的开关是可控的d，所以$V_b$就是$1-d$，由于双电感统一模型是关于电容$C_f$对称的，所以说反一下也没有问题。![](Typora_Png/2024-03-11_17-43-06.png)

根据电源侧能量流动情况来分析开关的方向，利用续流二极管代替其中一个开关管(降本增效)，用开关管和续流二极管填充：

![image-20240311175328838](Typora_Png/image-20240311175328838.png)

**电流流动情况分析：**

​	开关管闭合时：由电流的方向可知，此时输出侧是没有电流流过的。

​	开关管承受的电压：$V_{in} + V_{out}$



 >电容的特性是隔直通交，此时电容上没有电流流过，电容呈现左正右负的电压特性，通过开关管和电感$L_1$形成闭合的回路。从这里也可看出输出端无电流。

![image-20240311175821869](Typora_Png/image-20240311175821869.png)

​	开关管断开时：由电流方向可知，此时输出$V_out$是有电流产生的。

> 之前开关管闭合时，给电容充电，当开关管断开时，输入$Y_{in}$和电感$L_2$给$V_f$充电，且当电源向输出释放能量，$L_1$也向负载输出能量。

![image-20240311211403603](Typora_Png/image-20240311211403603.png)

由以上分析可知，SEPIC电路的输出也是不连续的，但是输入的电流是经过电感的脉动比较小。

**输出比例关系推导**

在连续状态下，计算输出电压和输入电压的比例关系：$\Rightarrow d\cdot (V_C - V_D) = (1-d)(V_A - V_B)$

​													  代入实际的点位关系：$\Rightarrow d\cdot (0 - V_{in}) = (1 -d)(0 - V_{out})$

​																							$\Rightarrow \frac{V_{out}}{V_{in}} = \frac{d}{1-d}$	       <font color=red>Buck-Boost电路</font>

当$d < 0.5$时： $\frac{V_{out}}{V_{in}} < 1$     $\rightarrow$  $V_{out} < V_{in}$   $\rightarrow$ Buck电路

当$d > 0.5$时： $\frac{V_{out}}{V_{in}} > 1$     $\rightarrow$  $V_{out} > V_{in}$   $\rightarrow$ Boost电路

注：SEPIC是Buck-Boost电路，且输入电流连续，输出电流不连续，所以输入好滤波，输出不好滤波，且输出电压是正极性。

**输出极性分析**

> 方式一、	由输出与输入电压比例关系：<font color=red>$\frac{V_{out}}{V_{in}} = \frac{1}{1-d}$</font>
>
> ​									得到$\Rightarrow V_{out}与V_{in}$同向
>
> 方式二、利用$V_{out}$的输出范围分析
>
> ​	由单电感统一模型可知：<font color=#FF44FF>$V_Y$必然介于$V_{X1}$和$V_{X2}$</font>
>
> ​	对于$L_2$:<font color=#FF44FF>$0 < V_{in} < (V_f - V_{out})$</font> 
>
> ​			又：<font color=#FF44FF>$v_f = V_{A} - V_D$</font>                                     ZETA电路的$V_A = 0$
>
> ​			则：<font color = #FF44FF>$0 < V_{in} < V_{out} - V_{in}$</font>
>
> ​			一般情况下，<font color=#FF44FF>$V_{in} > 0$</font>,那么必然<font color=#FF44FF>$V_{out} > 0$</font>，$V_{in}$与$V_{out}$同向。而且$V_{out}$既可以大于$V_{in}$，也可以小于$V_{in}$，所以 此电路为<font color=red>Buck-Boost电路</font>.
>
> ​	对于L1：$ V_{out} < 0 <  V_f$
>
> ​			又：<font color=#FF44FF>$v_f = V_{A} - V_D$</font>                                     ZETA电路的$V_A = 0$，$V_D = V_{in}$
>
> ​			则：<font color=#FF44FF>$V_{out} < 0 < - V_{in}$</font>                                两边同乘以-1得$-V_{out} < 0 < V_{in}$
>
> ​			一般情况下，<font color=#FF44FF>$V_{in} > 0$</font>,那么<font color=#FF44FF>$V_{out} > 0$</font>时等式才能成立,故$V_{in}$与$V_{out}$同向。而且$V_{out}$既可以大于$V_{in}$，也可以小于$V_{in}$，所以 此电路为<font color=red>Buck-Boost电路</font>.

#### 3.2.3 Semi-Z INV电路

![image-20240314163935359](Typora_Png/image-20240314163935359.png)

将Semi-Z INV电路的电位填补到双电感统一模型中去，电路模型如下：![image-20240317194927822](Typora_Png/image-20240317194927822.png)

为了直观的分析电路，将输出用负载代替，再根据输入侧在左、输出侧在右的形式变换电路图：

![image-20240317195626132](Typora_Png/image-20240317195626132.png)

根据电源侧能量流动情况来分析开关的方向，发现最后没有必要用二极管的形式去填充，下面来看一下两个开关通断情况下的电路图。

​	B点开关管闭合，C点开关管断开时：此时不能用续流二极管代替

![image-20240319184431739](Typora_Png/image-20240319184431739.png)

​	C点开关管闭合，B点开关管断开时：此时电流也会从开关管流过，所以也不能用二极管代替开关管

![image-20240319184948341](Typora_Png/image-20240319184948341.png)

下面用通式的角度分析一下电路：$d(V_A - V_B) = (1 -d)(V_C - V_D)$

​           代入Semi-Z INV电路电位:   $d(V_{in} - 0) = (1 - d)(V_{in} - V_{out})$

​                                          化简得：$\frac{V_{out}}{V_{in}} = 1 + \frac{d}{d - 1}$

> 若d = 0.5   则$\frac{V_{out}}{V_{in}} = 0$
>
> 若d < 0.5    则$\frac{V_{out}}{V_{in}} > 0$
>
> ​	设d = 0.1   得$\frac{V_{out}}{V_{in}} = 1 + \frac{0.1}{-0.9} = \frac{8}{9}$
>
> 若d > 0.5    则$\frac{V_{out}}{V_{in}} < 0$
>
> ​	设的= 0.9	得$\frac{V_{out}}{V_{in}} = 1 + \frac{0.9}{-0.1} = -8$ 

从另一个角度分析电路：

<img src="Typora_Png/image-20240319192548721.png" alt="image-20240319192548721" style="zoom:75%;" />

由以上分析情况可知，该电路的输出是可正、可负，所以他应该属于逆变电路的一种。由前面的电路分析可知，双电感统一模型是关于电容对称的，可以肯定Semi-Q电路也是逆变类电路，所以这两种电路该处不做深究。

#### 3.2.4 New#1电路

![image-20240319193344820](Typora_Png/image-20240319193344820.png)

将New#1电路的电位补充到双电感统一模型中去，电路模型如下：

![image-20240319200656805](Typora_Png/image-20240319200656805.png)

由于电路的特殊性，我们直接用通式角度分析电路：$d(V_A - V_B) = (1 - d)(V_C - V_D)$

代入New#1电路的电位：$d(V_{out} - V_{in}) = (1 - d)(V_{out} - 0)$

化简得：$\frac{V_{out}}{V_{in}} = \frac{d}{2d - 1}$

下面是该函数得函数图形（用https://www.desmos.com/calculator?lang=zh-CN绘制）：

分析函数图形可知：

​	0.5 < d < 1时，$2d - 1 > 0$，且$d > 2d - 1$,所以此时电路为Boost电路，且输入输出同向；

​	$0 < d < 0.5$时，$2d - 1 < 0$,证明$V_{out < 0}$，且$\frac{V_{out}}{V_{in}}$可能大于-1，也可能小于-1，所以此时为Buck-Boost电路($0$到$\frac{1}{3}$为Buck，$\frac{1}{3}到0.5$为Boost)。	

![image-20240319202825943](Typora_Png/image-20240319202825943.png)

​	比值有正有正、有负，所以该电路类型也是类似于逆变类电路，根据双电感统一模型对称性可知，另一种电路也是类似的逆变类电路，这里不做进一步的深入分析。

![image-20240319195951894](Typora_Png/image-20240319195951894.png)

#### 3.2.5 Cuk电路

![image-20240321212549055](Typora_Png/image-20240321212549055.png)

将Cuk电路的电位填充到双电感统一模型中去，为了直观的分析电路，用负载代替输出，电路模型如下：

![image-20240321212504690](Typora_Png/image-20240321212504690.png)

**首先用通式的方式去分析电路：**$d(V_A - V_B) = (1 - d)(V_C - V_D)$

​                代入Cuk电路的电位：$d(V_{in} - 0) = (1 - d)(0 - V_{out})$

​                                          展开：$d \cdot V_{in} = -V_{out} \cdot (1 - d)$

​				  	                化简得：$\frac{-V_{out}}{V_in}= \frac{d}{1 - d}$

> 因为d的取值范围为0~1，所以说$\frac{-V_{out}}{V_{in}}$一定是大于0的，故Cuk电路的输出与输入是反向的。且该电路为Buck-Boost电路。当$d > 0.5$时，$\frac{V_{out}}{V_{in}} > 1$,此时为Boost电路。
>
> 当$d = 0.5$时，$\frac{V_{out}}{V_{in}} = 1$,此时输入等于输出。
>
> 当$d > 0.5$时，$\frac{V_{out}}{V_{in}} > 1$,此时为Boost电路。
>
> 当$d < 0.5$时，$\frac{V_{out}}{V_{in}} < 1$,此时为Buck电路。

**其次可以用另一方面看$V_{out}$的输出范围：**

1、从$L_2$电感来看

​	$V_{out}$介于0和$-V_f = -(V_{in} - V_{out})$之间，即:$V_{out}$介于0和$V_{out} - V_{in}$之间。

2、从$L_1$电感来看

​	$V_{in}$介于0和$V_f = V_{in} - V_{out}$之间，即$V_{out}$介于0和$V_{in} - V_{out}$之间。

> 设输入方向为正，则$V_{in} > 0$
>
> ​	于是对于$L_2$，$0 < V_{in} < V_{in} - V_{out}$
>
> ​    为了满足上式：$V_{out} < 0$
>
> ​	代入$L_2$电感：$V_{out} -V_{in} < V_{out} < 0$
>
> ​	只考虑大小，不考虑方向的情况下，$V_{out}$可能大于$V_{in}$，也可能小于$V_{in}$，由此也可知，Cuk电路为Buck-Boost电路。

根据电源侧能量流动情况来分析开关的方向，利用续流二极管代替其中一个开关管(降本增效)，用开关管和续流二极管填充：

![image-20240322100011617](Typora_Png/image-20240322100011617.png)

> *优点：输入测和输出侧的电流都是连续的，比较好滤波*
>
> *缺点：输出电压为负，使用时不方便；*
>
> ​			*开关器件和二极管的耐压为电容上的电压，即$V_{in} + |V_{out}|$，耐压等级比普通的Buck电路*和Boost电路高。

电流工作状态详细分析：假设工作在Buck电路，则$d < 0.5$，(Boost模式 也能得到同样的结论)：

​	开关闭合：$L_1$储能，$C_f$放电给$V_{out}$，同时一部分能量存储到$L_2$中。

![image-20240322101156238](Typora_Png/image-20240322101156238.png)

​	开关闭合：$V_{in}$和$L_1$储存的能量一起给$C_f$充电，同时$L_2$将上阶段存储的能量释放给$V_{out}$。

#### 3.2.6 Boost(new)&Buck(new)电路

![image-20240322105655895](Typora_Png/image-20240322105655895.png)

首先我们来看Boost电路，将Boost(new)电路的电位填充到双电感统一模型中去，电路模型如下：

![image-20240322110720633](Typora_Png/image-20240322110720633.png)

**首先用通式的方式去分析电路：**$d(V_A - V_B) = (1 - d)(V_C - V_D)$

   代入Boost(new)电路的电位：$d(0 - V_{in}) = (1 - d)(V_{in} - V_{out})$

​                                          展开：$-dV_{in} = (1 - d)V_{in} - (1 - d)V_{out}$

​									  化简得：$\frac{V_{out}}{V_{in}} = \frac{1}{1 - d}$

​	由上式可知，该电路为Boost电路，且输入与输出同向。

**其次可以用另一方面看$V_{out}$的输出范围：**

1、从$L_2$电感来看

​	$V_{out}$介于$V_{in}$和$V_{in} - V_f = V_{in} - (0 - V_{out}) = V_{in} + V_{out}$之间，即$V_{out}$介于$V_{in}$和$V_{in} + V_{out}$之间。

2、从$L_1$电感来看

​	0介于$V_{in}$和$V_{in} + V_f = V_{in} + (0 -V_{out}) = V_{in} - V_{out}$之间，即0介于$V_{in}$和$V_{in} - V_{out}$之间。

> 设输入方向为正，则$V_{in} > 0$
>
> 对于$L_1$：$V_{in} - V_{out} < 0 < V_{in}$
>
> 为满足上式，$V_{out} > V_{in}$
>
> 对于$L_2$：$V_{in} < V_{out} < V_{in} + V_{out}$也是成立得，可以推出该电路为Boost电路，且输入与输出同向。

为更直观的分析电路，将输出变为负载，再根据输入侧在左、输出侧在右的形式变换电路图：

根据电源侧能量流动情况来分析开关的方向，利用续流二极管代替其中一个开关管(降本增效)，用开关管和续流二极管填充：

![image-20240322132652057](Typora_Png/image-20240322132652057.png)

初步认为稳定后的工作状态如下(有待仿真验证)：

​	开关开通时： $L_1$充电，$C_f$和$V_{in}$一块给$L_2$充电，同时给$V_{out}$放电。

![image-20240322133422533](Typora_Png/image-20240322133422533.png)

​	开关断开时：$V_{in}$和$L_2$通过续流二极管一块给$V_{out}$放电，同时$L_1$放电给$C_f$。

![image-20240322133547610](Typora_Png/image-20240322133547610.png)

和原来的Boost电路相比
	优点：输出侧电流连续，易滤波

​	缺点：使用器件多，成本高，控制复杂

#### 3.2.7 Buck-Boost电路总结对比

## 四、主电路参数计算 

### 4.1 电感参数计算

​	此处选用单电感模型Buck电路为例，主要学习参数计算的方法，不同的电路都可以用这种方式来分析电路。

![image-20240325182714640](Typora_Png/image-20240325182714640.png)

​	做一个设计，首先要知道设计要求：

|       参数        |  值  |      |     参数     |        值        |
| :---------------: | :--: | ---- | :----------: | :--------------: |
| 输入电压$V_{in}$  | 400V |      | 电感电流纹波 |  $\leq 20$%(4A)  |
| 输出电压$V_{out}$ | 100V |      | 电容电压纹波 | $\leq$1%(4V、1V) |
|   额定功率$P_o$   | 2KW  |      |  效率$\eta$  |       95%        |
|   额定电流$I_o$   | 20A  |      |  开关频率f   |       16K        |

下面来看主电路电感的电压和电流变化情况：![image-20240325184401735](Typora_Png/image-20240325184401735.png)

此处电流纹波指的是稳定后电感电流偏离稳定值得大小(<font color = purple>也可以说是平均值到最大、最小值的差值大小。</font>)，电压纹波也是这样的。

设计电路就是要对其中的电感、开关管、二极管、输入电容、输出电容等进行选型，并作进一步控制：

​	1、开关管和二极管的选型

​		依靠耐压值和耐流值做选型，一般留1.5~2倍余量。

​	2、电感的选型

​		由单电感统一模型-Buck电路分析结果可知：$\frac{V_{out}}{V_{in}} = d$

​		代入设计要求，占空比$d = \frac{V_{out}}{V_{in}} = \frac{100V}{400V} = 0.25$时满足设计要求。

​		由电感的公式：<font size=5>$V_L = L\frac{di_L}{dt}$</font>      $\rightarrow$     <span id="电感电流公式1"><font size=5>$\Delta i_L = \frac{1}{L} \int_{0}^{T}V_Ldt$</font></span>                                                  [ <font color=red>电感电流公式1</font>](#电感电流公式1)

​		根据电感在一个周期内储能过程中电流的纹波情况，可以求出电感的参数为：<font size=6>$L = \frac{V_L \cdot \Delta t}{\Delta i_l}$</font>



​		其中$V_L$为电感两端的电压：**$V_{in} - V_{out}$**

​	    $\Delta t$为电感储能过程中的时间：$d\cdot T$或$(1 - d)\cdot T$

​		$\Delta i_L$为电感电流纹波大小：$20\% \cdot I_o\cdot 2$

​		代入数据：$L = \frac{(400V - 100V)(\frac{100V}{400V}\cdot \frac{1}{16000})}{20\%\cdot 20 \cdot 2}  \approx 0.0005859375H$

取电感L大小为0.6mH，计算该参数下临界连续时电流的纹波：$\Delta i_L = \frac{1}{L}\cdot V_L\cdot \Delta t = \frac{V_{in} - V_{out}}{L}\cdot d\cdot T = \frac{(400V-300V)\times 0.25\times 0.0000625}{0.0006} = 7.8152A$

所以电感电流纹波为$\frac{7.8}{2} = 3.9$时为临界连续状态(也意味着电路的最小电流为3.9A)。

### 4.2 输出电容参数计算---理想电容器估算

根据电感特性知，电感电流是由直流和交流分量构成的三角波。假设电感器电流的三角部分能够完全流过电容器，流过电容器的三角波电流产生交流电压，从而在直流输出之上产生纹波分量。电容器两端的交流电压由下式给出：

​                                                     										<font color=red size = 5>$V_{ac} = \frac{1}{C} \cdot \int_{0}^{t}i_C(t) \cdot dt$</font>

式中，$i_c$为电容器电流。电感电流是三角波，利用傅里叶思想将其看为直流分量和交流分量组成，此时只考虑交流分量，在交流电流$i_c$为正期间，交流电压增加，当$i_c$为负时衰减。如下图所示。对$i_c$保持为正期间的电流积分得到$u_ac$的峰-峰值电压：

<img src="Typora_Png/image-20240407194409893.png" alt="image-20240407194409893" style="zoom:30%;" />

​																							<font color=red size=5>$\Delta V_{ac} = \frac{1}{C} \int_{t_1}^{t_2}i_c(\tau)d\tau$</font>

上式右侧的积分对应于电感器电流为正部分和时间轴包围而成的三角形的面漆，利用$|t_2 - t_1| = \frac{T_s}{2}$计算阴影三角形的面积，于是：

​                                                                                           <font color=red size=5>$\Delta V_{ac} = \frac{1}{C}\cdot (\frac{1}{2})(\frac{1}{2}\Delta i_L)(\frac{1}{2}T_s)$</font>

代入 [ <font color=red>电感电流公式1</font>](#电感电流公式1)得：                      <font color=red>$\Delta V_{ac} = \frac{1}{8C}\cdot \Delta i_L\cdot T = \frac{1}{8C}\cdot \frac{V_{in}-V_{out}}{L} dT\cdot T = \frac{V_{in}-V_{out}}{8CL}dT^{2} = \frac{V_{out}}{8CL}(1 - d)T^{2}$</font>

变形得：$C = \frac{V_{out}}{8\Delta V_{ac}L}(1-d)T^2$       因为$\Delta V_{ac}$要小于1%

代入数据得：$C > \frac{100V}{8\times 2\times 100V\times 0.01\times 0.0006}\times 0.75\times 0.0000625^2 \approx 0.000030418F \approx 30uF$

## 五、平均等效模型

### 5.1 电容和电感的平均模型

**基本思想：**忽略一些次要的因素，保留系统的主要行为，以简化模型。这里忽略的是开关频率分量，主要保留原始信号的低频部分，取一个平均的概念。

#### 5.1.1 模型角度分析

电力电子变换器工作在开关状态，电路结构也随着开关状态的切换而变化，因此建模较为困难。为了简化分析，忽略开关过程，使用电气量在一个开关周期内的平均替代其实际值进行建模，即对各电气量均取开关周期平均算子：

​                                                                                     <font color=red>$<x(t)>_{T_s} = \frac{1}{T_s}\int_{t - T_s}^{t}x(\tau)d\tau$</font>

以Buck变换器电感电流为例，在启动后至进入稳态工作前，滤波电感电流从零开始增大，输出电容从电压为零开始充电，直至达到稳态输出：

> PS：稳态时电感电压满足伏秒平衡

![img](https://pic1.zhimg.com/v2-58a778bda9b4a4778189b79a1f416604_r.jpg)

对电感电流取开关周期平均：![img](https://pic2.zhimg.com/v2-322330781d99edbb5afb7743dda9b8ad_r.jpg)

可以看出，引入开关周期平均算子后电感电流的高频纹波被忽略，只保留了低频分量，而这正是我们所期望的，因为相比于开关频率，控制器输入的是一个低频调制信号 $V_c(t)$ ，输出$V(t)$也随之低频变化，其中的高频纹波是我们所不关心的。

![img](https://pic1.zhimg.com/80/v2-a8167a443954f5588390413e46edb90c_720w.webp)

总结得到电感和电容的平均模型为：

​	电感：$<V_L(t)>_{T_s} = L\frac{d<i(t)>_{T_S}}{dt}$

​	电容：$<i_C(t)>_{T_s} = C\frac{d<V_C(t)>_{T_s}}{dt}$

#### 5.1.2 数学角度分析

![image-20240415140636861](Typora_Png/image-20240415140636861.png)

### 5.2 采用平均模型分析单电感DCDC变换器等效框图

​	**基本套路：**得到电感和负载侧滤波电容的平均时域数学模型，从而经过拉氏变换得到输出电压和输入电压的关系，即传递函数，然后画出电路的等效框图，惊醒控制器的设计。（这里没有考虑扰动的影响，后面再做分析。）

#### 5.2.1 Buck电路等效框图

![image-20240415151202473](Typora_Png/image-20240415151202473.png)

要建立数学模型，肯定要基于电路的基本定理去列计算方程式，首先分析开关切换时的状态，在开关切换时可以等效城下面这个电流图：![image-20240415152350999](Typora_Png/image-20240415152350999.png)

图中r表示电感的等效电阻，$V_g$的波形为如下方波：

![image-20240415153303756](Typora_Png/image-20240415153303756.png)

由电路的平均模型可得到下式，也就是$V_g$中的直流分量为$dV_{in}$:

​																	$<V_g(t)>_T = \frac{1}{T}\int_{t}^{t+T}V_g(\tau)d\tau = \frac{1}{T}dT<V_{in}(t)>_T = d<V_{in}(t)>_T$

看Buck电路等效模型，根据KVL定理得到电感和电容平均数学模型：

​																	$<V_g(t)> - <V_o(t)> = d<V_{in}(t)> - <V_o(t)>$

​																					    						$=r\cdot <i_L(t)> + L\frac{d<i_L(t)>}{dt}$

看Buck电路等效模型，根据KCL定理得到电感和电容平均数学模型：

​																	$<i_L(t)> = C_o\frac{d<V_o(t)>}{dt} + \frac{<V_o(t)>}{R}$

对KCL、KVL方程进行拉氏变换，则可得到他们的频域模型为：

​																	$DV_{in}(s) - V_o(s) = rI_L(s) + LsI_L(s)$

​																	$I_L(s) = CsV_o(s) + \frac{V_o(s)}{R}$

则可得平均模型下的等效控制框图如下：

![image-20240416182746692](Typora_Png/image-20240416182746692.png)

#### 5.2.2 Boost电路等效框图

 ![image-20240416191830411](Typora_Png/image-20240416191830411.png)

首先分析开关切换时的状态，因为开关将电容和电感分在两侧了，所以不能直观的分析出等效控制框图。首先来推导一下时域下的平均模型，根据开关的切换状态分析一下，开关开通和关断时有：![image-20240416195652889](Typora_Png/image-20240416195652889.png)

看Boost电路，电感在开关闭合和开通时的平均电压时相等的，且由KCL定理知：
$$
\begin{align}
<V_{in}(t)> - <V_{ab}(t)>  &= <V_{in}(t)> - (1-d)<V_{out}(t)>  \notag  \\
	\label{eq公式5.2.2-1}
							&= r<i_L(t)> + L\frac{d<i_L(t)>}{dt}  \tag{公式1}
\end{align}
$$

$$
\begin{align}
\label{eq公式5.2.2-2}
<i_2(t)> = (1 - d)<i_l(t)> = C\frac{d<V_o(t)>}{dt} + \frac{<V_o(t)>}{R}   \tag{公式2}
\end{align}
$$

将$$\ref{eq公式5.2.2-1}$$和$$\ref{eq公式5.2.2-2}$$经过拉氏变换可得：
$$
\begin{align*}
&V_{in}(s) - (1 - D)V_o(s) = (r + L)i_L(s) \\
&(1 - D)i_L(s) = (Cs + \frac{1}{R})V_o(s)
\end{align*}
$$
则可得平均模型下的等效控制框图如下：

![image-20240417200146212](Typora_Png/image-20240417200146212.png)

Buck和Boost电路等效框图做对比：![image-20240417200408093](Typora_Png/image-20240417200408093.png)

#### 5.2.3 Buck-Boost电路等效框图

![image-20240418195621395](Typora_Png/image-20240418195621395.png)

由2.2.3节知，Buck-Boost电路输出电压和输入电压比例关系式为：
$$
\frac{V_{out}}{V_{in}} = -\frac{d}{1-d}  \notag
$$
由此可以看出Buck-Boost电路的输入和输出电压是反向的，所以我们在这里去反方向为参考电压。分析开关开通和关断情况下电路情况：

![image-20240418202356719](Typora_Png/image-20240418202356719.png)

下图时电感电流的调节过程：

![image-20240418202828445](Typora_Png/image-20240418202828445.png)

开始时会有个输出电压逐渐增加，从而电感电流逐步增加的过程，所以初始过程电感两端电压平均值不为零。所以不能直接把$<V_{ab}>$看成0，它是有个变化过程。

根据KVL定理：开关闭合时ab两端的电压+开关断开时ab的电压=整个过程电感的电压
$$
\begin{align*}
d<V_{in}> + (1-d)<-V_{out}> &= <V_L(t)> \\
&=r<i_L(t)> + L\frac{d<i_L(t)>}{dt}
\end{align*}
$$
根据KCL定理：开关闭合时$i_2$没有电流+开关断开时$i_2$的电流=整个过程电感的电流
$$
\begin{align*}
<i_2(t)> &= (1-d)<i_L(t)>  \\
		&=C\frac{d<V_{out}(t)>}{dt} + \frac{V_{out}}{R}
\end{align*}
$$
将上面两个公式进行拉氏变换：
$$
\begin{align*}
{}d<V_{in}> + (1-d)<-V_{out}> = r<i_L(t)> + L\frac{d<i_L(t)>}{dt}  \rightarrow &{\color{red}DV_{in}(s) - (1 - D)V_o(s) = RI_l(s) + LsI_l(s)}  \\
(1-d)<i_L(t)> =C\frac{d<V_{out}(t)>}{dt} + \frac{V_{out}}{R}  \rightarrow  &{\color{red}(1-D)I_l(s) = CsV_o(s) + \frac{1}{R}V_o(s)}
\end{align*}
$$

则可得平均模型下的等效控制框图如下：

![image-20240612184629277](Typora_Png/image-20240612184629277.png)

Buck、Boost、Buck-Boost电路等效框图对比：![image-20240612185854625](Typora_Png/image-20240612185854625.png)

## 六、Buck变换器

### 6.1 Buck电路的开环模型分析+仿真验证

**单电感DCDC变换器控制框图设计**

​	基本套路：根据前面所得电路的等效框图，观察电路本身传递函数对应的伯德图，从而设计电压环控制、电流环控制或者电压电流双闭环控制，当采用基本的PI控制稳定性不好时，要加入补偿控制器等等。

​	目前分析过程中，直接把d看成无扰动的常量了，就是把它看成稳定值，其余的看成变量，这一点可能和大家通常看到的那种使用小信号模型的不一样，这个后面会详细介绍为什么不用那个。

#### 6.1.1 Buck-控制框图设计开环控制

​	首先来看Buck电路平均模型的等效关系式：
$$
\begin{align*}
&DV_{in}(s) - V_o(s) = r\cdot I_L(s) + LsI_L(s) \\
   &I_L(s) = CsV_o(s)+ \frac{V_o(s)}{R}
\end{align*}
$$
它的等效框图如下：

![image-20240710173121115](Typora_Png/image-20240710173121115.png)

由等效框图可以得到主电路对应的传递函数：
$$
\begin{align*}
反馈环节传递函数 &= \frac{前向通道传递函数}{1 + 前向通道传递函数\cdot 反馈通道传递函数}  \\
G(s) &= \frac{}{1 + \frac{1}{Ls+r}\cdot \frac{\frac{1}{Cs}}{1 + \frac{}{\frac{1}{Cs}\cdot \frac{1}{R}}}}
\end{align*}
$$


### 6.2Buck变换器单电压环比例积分控制详解



如果涉及闭环控制，原来电路的等效传递函数就变为开环传递函数的一部分，那如果想实现对给定直流信号的无静差控制，则需要引入积分控制。

> <font color=blue>什么是无静差控制？</font>
>
> ​	<font color=#8600FF>静差</font>（或称余差、稳态误差），指的是反馈控制系统的被调量（输出量）再系统稳定时与系统的给定量之间的偏差。
>
> ​	<font color=#8600FF>无静差</font>：指反馈系统的被调量（输出量）在系统稳态时等于系统的给定量，误差为0 。



后面做恒海工会大会的













## 结束



