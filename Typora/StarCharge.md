[TOC]



## 碎碎念：

在逆变器项目中已经很久了，基于大学本科时的才疏学浅，工作中遇到了很多问题，也学会了许多，思来想去想把工作中遇到的问题整理出来，供自己查阅及复习，如果后面有时间的情况下，能够将这些资料整理整理分享至知乎或CSDN。

​	所有的文档都会整理在这一个文档中，防止自己后面忘记找不到文档。

# 一、一阶低通滤波器原理及离散序列设计

![img](Typora_Png/v2-2e214f126842f6976b0c63c118df58f4_720w.webp)

由KVL方程知：
$$
V_i(t) = V_o(t) + R\cdot C\frac{dV_0(t)}{dt}   \tag{KVL公式}
$$
两端进行拉氏变换：
$$
\begin{align*}
&V_i(s) = V_o(s) + RC\cdot s\cdot V_o(s)    \\
&V_i(s) = (1 + RCs)\cdot V_o(s)				\\
由传递函数概念推出：&G(s) = \frac{V_o(s)}{V_i(s)} = \frac{1}{RCs + 1}
\end{align*}
$$
由惯性环节形式1：$\displaystyle G(s) = \frac{Y(s)}{X(s)} = \frac{1} {\tau s + 1} $                      可以看出时间常数$\tau = RC$

由惯性环节形式2：$\displaystyle G(s) = \frac{Y(s)}{X(s)} = \frac{\omega _c}{s + {\omega}_c}$					可以看出其截止角频率$\displaystyle {\omega}_c = \frac{1}{RC}$  

>注意：
>$$
>\begin{align*}
>截止频率：&{\color{red}{f_c =\frac{1}{2\pi RC} = \frac{{\omega}_c}{2 \pi} = \frac{1}{2{\pi}{\tau}}}} \tag{截止频率转换}\\
>&{\color{red}{\tau = RC}}
>\end{align*}
>$$
>

$$
离散化一般采用=\left\{
\begin{align*}
1、 & 后向差分法 \rightarrow \quad S = \frac{1 - Z^{-1}}{T_s} \\
2、 & 前向差分法 \rightarrow \quad S = \frac{Z - 1}{T_s}  \\
3、 & 双线性差分法\rightarrow S = \frac{2}{T_s}\frac{1 - Z^{-1}}{1 +Z^{-1}}
\end{align*}
\right.
$$

## 1.1、后向差分法

令$\displaystyle S = \frac{1 - Z^{-1}}{T_s}$，代入$\displaystyle G(s) = \frac{Y(s)}{X(s)} = \frac{{\omega}_c}{s + {\omega}_c}$
$$
\begin{align*}
得到:&\frac{Y(z)}{X(z)} = \frac{{\omega}_c}{\frac{1 - Z^{-1}}{T_s} +{\omega}_c} \quad  \quad \quad\quad\quad\quad\quad\quad\quad\quad\quad\quad  \\
通分:&\frac{Y(z)}{X(z)} = \frac{{\omega}_cT_s}{1 - Z^{-1} + {\omega}_cT_s}  \\
展开:&T_s{\omega}_cX(z) = (1 + {\omega}_cT_s)Y(z) - Y(z)Z^{-1}   \\
移项:&(1 + {\omega}_cT_s)Y(z) = T_s{\omega}_cX(z) + Y(z)Z^{-1}   \\
为了消&除(1 + {\omega}_C)并将其化简的更简洁,等式右边引入：  \\
&{\color{blue}{\omega}_c{T_s}Y(z)Z^{-1}-{\omega}_c{T_s}Y(z)Z^{-1}}   \\
\rightarrow &(1 + {\omega}_c{T_s})Y(z) = T{\omega}_cX(z) + (1 + {\omega}_c{T_s})Y(z)Z^{-1} -T_s{\omega}_c Y(z)Z^{-1}  \\
\rightarrow&(1 + {\omega}_c{T_s})Y(z) = T_s{\omega}_c(X(z) - Y(z)Z^{-1}) + (1 + {\omega}_cT_s)Y(z)Z^{-1}   \\
两边同&时除以：1 + T_s{\omega}_c     \\
&Y(z) = \frac{T_s{\omega}_c}{1 + T_s{\omega}_c}\cdot (X(z) - Y(z)Z^{-1}) + Y(z)Z^{-1}   \\
\end{align*}
$$

$$
引入离散采样数据:\left\{
\begin{align*}
1、 & 上一时刻 \rightarrow \quad Y(z)Z^{-1}  = y(n - 1) \\
2、 & 当前时刻 \rightarrow \quad Y(z)X^0 = y(n)  \\
3、 & 下一时刻 \rightarrow \quad Y(z)Z^1 = y(n + 1)    \rightarrow未使用
 \end{align*}
\right.
$$

得到：
$$
\begin{align*}
y(n) = y(n - 1) + \frac{T_s{\omega}_c}{(1 + T_s{\omega}_c)}[x(n) - y(n -1)]
\end{align*}
$$
程序中通常使用：
$$
\begin{align*}
y(n) = \frac{T_s{\omega}_c}{(1 + T_s{\omega}_c)}x(n) + (1 - \frac{T_s{\omega}_c}{(1 + T_s{\omega}_c)})y(n - 1)
\end{align*}
$$
程序中通常会算出两个变量：
$$
\begin{align}
&{\color{red}{A = \frac{T_s{\omega}_c}{(1 + T_s{\omega}_c)}}} \tag{}  \\
\label{eq公式1.1.1-1}&{\color{red}{B  =  1 -A}}   \tag{后向差分公式}   \\
&{\color{blue}{y(n) = A\cdot x(n) + By(n - 1)}}  \tag{}
\end{align}
$$

## 1.2、双线性差分法

令$\displaystyle S = \frac{2}{T}{\cdot}\frac{1 -Z^{-1}}{1 + Z^{-1}}$,代入$\displaystyle G(s) = \frac{Y(s)}{X(s)} = \frac{{\omega}_c}{s + {\omega}_c}$

得：
$$
\begin{align*}
\frac{Y(z)}{X(z)} = \frac{{\omega}_c}{\frac{2}{T_s}\cdot \frac{1 - Z^{-1}}{1 +Z^{-1}} + {\omega}_c}
\end{align*}
$$
通分：
$$
\begin{align*}
\frac{Y(z)}{X(z)} = \frac{{\omega}_c{T_s(1 + Z^{-1})}}{2(1 - Z^{-1}) + {\omega}_c{T_s{(1 +z^{-1})}}}
\end{align*}
$$
展开：
$$
\begin{align*}
{\omega}_cT_sX(z) + {\omega}_cT_sX(z)Z^{-1} = 2Y(z) - 2Y(z)Z^{-1} + {\omega}_cT_sY(z) + {\omega}_cT_sY(z)Z^{-1}
\end{align*}
$$
移相分配：
$$
\begin{align*}
(2 + {\omega}_cT_s)Y(z) = 2Y(z)Z^{-1} + {\omega}_cT_s(X(z) - Y(z)Z^{-1}) + {\omega}_cT_sX(z)Z^{-1}  \\
\end{align*}
$$
右边引入：$\displaystyle + {\omega}_cT_sY(z)Z^{-1} - {\omega}_cT_sY(z)Z^{-1} $
$$
\begin{align*}
(2 + {\omega}_cT_s)Y(z) = (2 + {\omega}_cT_s)Y(z)Z^{-1} + {\omega}_cT_s[(X(z) - Y(z)Z^{-1}) + (X(z)Z^{-1} - Y(z)Z^{-1})]
\end{align*}
$$
两边同除以$2 + {\omega}_cT_s$:
$$
\begin{align*}
Y(z) = Y(z)Z^{-1} + \frac{{\omega}_cT_s}{2 + {\omega}_cT_s}{\cdot}[(X(z) - Y(z)Z^{-1}) + (X(z)Z^{-1} - Y(z)Z^{-1})]
\end{align*}
$$

$$
引入离散采样数据:\left\{
\begin{align*}
1、 & 上一时刻 \rightarrow \quad Y(z)Z^{-1}  = y(n - 1) \\
2、 & 当前时刻 \rightarrow \quad Y(z)X^0 = y(n)  \\
3、 & 下一时刻 \rightarrow \quad Y(z)Z^1 = y(n + 1)    \rightarrow未使用
 \end{align*}
\right.
$$

得到：
$$
\begin{align*}
y(n) = y(n - 1) + \frac{{\omega}_cT_s}{2 + {\omega}_cT_s}{\cdot}[(x(n) - y(n - 1)) + (x(n-1) - y(n -1))]
\end{align*}
$$
化简系数:
$$
\begin{align*}
y(n) = (1 - \frac{2{\omega}_cT_s}{2 + {\omega}_cT_s})y(n - 1) + \frac{{\omega}_cT_s}{2 + {\omega}_cT_s}x(n) + \frac{{\omega}_cT_s}{2 + {\omega}_cT_s}x(n - 1)
\end{align*}
$$
程序中通常会算出两个变量：
$$
\begin{align}
&{\color{red}{A = (1 - \frac{2{\omega}_cT_s}{2 + {\omega}_cT_s})}}  \tag{}  \\  
\label{eq公式1.2.1-1}&{\color{red}{B = \frac{1 - A}{2}}}      \tag{双线性变化差分公式}   \\
&{\color{blue}{y(n) = Ay(n - 1) + Bx(n - 1) + Bx(n)}}      \tag{}
\end{align}
$$


我们的代码如下，算出了两个变量：

```C
#pragma CODE_SECTION(Alg_f32LPFCalc,"ramfuncs")
float32_t Alg_f32LPFCalc(FILTER_1ST_FRAME_T *RegAddr, float32_t f32DataIn)
{
    float32_t f32Out;
    f32Out = RegAddr->f32PreOut * RegAddr->f32A + RegAddr->f32B * f32DataIn + RegAddr->f32B * RegAddr->f32PreIn;
    RegAddr->f32PreOut = f32Out;			//这里用的是双线性差分变换的方式做的离散化
    RegAddr->f32PreIn = f32DataIn;       

    return f32Out;
}

void Alg_vFilterCoeffUpdate1st(FILTER_1ST_FRAME_T *RegAddrs)
{
    float32_t f32Omega;
    f32Omega = RegAddrs->f32Freq * 2.0f * PAI;
    RegAddrs->f32A = 1 / (f32Omega * RegAddrs->f32Period + 1);		//这里的系数代入的是后向差分法的系数
    RegAddrs->f32B = (1 - RegAddrs->f32A) * 0.5;
}
```

> 见上述代码所示$y(n) = A * y(n - 1) + B * x(n) + B * x(n - 1)$
>
> 理所当然得出：$A = (1 - \frac{2{\omega}_cT_s}{2 + {\omega}_cT_s})$
>
> ​				  		$ B = \frac{{\omega}_cT_s}{2 + {\omega}_cT_s}$
>
> 于是得到：$A = 1 - 2B$
>
> 神奇的是这里他没有采用$$\ref{eq公式1.2.1-1}$$ ，而是选择用$$\ref{eq公式1.1.1-1}$$代入运算，这里表示不理解，但又无可奈何。所以：
> $$
> \begin{align*}
> &{\color{blue}{A = \frac{1}{{\omega}_cT_s + 1}}}  \\
> &{\color{blue}{B = \frac{1 - A}{2}}}
> \end{align*}
> $$

下面给出了$$\ref{eq公式1.2.1-1}$$的代码：

```C 
void Alg_vBilinearFilterCoeffUpdate1st(FILTER_1ST_FRAME_T *RegAddrs)
{
    float32_t f32Omega;
    float32_t f32Tmp;

    f32Omega = RegAddrs->f32Freq * 2.0f * PAI;
    f32Tmp   = 2 - f32Omega * RegAddrs->f32Period;
    RegAddrs->f32A = f32Tmp / (f32Omega * RegAddrs->f32Period + 2);
    RegAddrs->f32B = (1 - RegAddrs->f32A) * 0.5;
}
```

# 二、 PID控制器

[PID算法(1) PID算法的原理推导1_pid控制推导-CSDN博客](https://blog.csdn.net/mayuxin1314/article/details/135217697?spm=1001.2014.3001.5502)

[经典位置式与增量式PID原理 (qq.com)](https://mp.weixin.qq.com/s/gtlW5khINFABqSxHTCMzkg)

[PID算法原理推导及应用 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/704188968)

## 2.1 PID解释

### 1、PID含义解释

​		P是Proportion，比例的意思，I是Integral，积分意思，D是Differential，微分的意思。

### 2、PID通俗解释

​		以有一个水缸有点漏水(而且漏水的速度还不一定固定不变)，通过加水让水维持在要求水面高度的某个位置，一旦发现水面高度低于要求水面高度的某个位置，就要往水缸里加水的例子来说明PID含义。

​		如：小家伙接到任务后就一直守在水缸旁边，时间长就觉得无聊，就跑到房里看小说了，每20分钟来检查一次水面高度。因水漏得太快，每次小家伙来检查时，水都快漏完了，离要求的高度相差很远。于是小家伙改为每5分钟来检查一次，结果每次来水都没怎么漏，不需要加水，来得太频繁做的是无用功。几次试验后，确定每10分钟来检查一次。这个检查时间就称为采样周期，即T。为了让水面高度维持在某个位置，开始小家伙用瓢加水，水龙头离水缸有十几米的距离，经常要跑好几趟才加够水，于是小家伙又改为用桶加，一加就是一桶，跑的次数少了，加水的速度也快了，但好几次将缸给加溢出了，不小心弄湿了衣服几次，小家伙又动脑筋，我不用瓢也不用桶，就用盆，几次下来，发现刚刚好，不用跑太多次，也不会让水溢出。这个加水工具的大小就称为比例系数，即P。在加水过程中，小家伙又发现水虽然不会加过量溢出了，但是有时会高过要求位置比较多，还打湿了衣服。于是小家伙又想了个办法，在水缸上装一个漏斗，每次加水不直接倒进水缸，而是倒进漏斗让它慢慢加。这样溢出的问题解决了，但加水的速度又慢了，有时还赶不上漏水的速度。从而他试着变换不同大小口径的漏斗来控制加水的速度，最终找到了满意的漏斗。这个漏斗控制加水时间就称为积分时间，即I。经过几番折磨，小家伙终于喘了一口，但任务要求突然严了，水位控制的及时性要求大大提高，一旦水位过低，必须立即将水加到要求位置，而且不能高出太多，否则不给工钱。小家伙又为难了！于是小家伙又开努脑筋，终于让小家伙想到一个办法，放一盆备用水在旁边，一发现水位低了，不经过漏斗就是一盆水下去，这样及时性是保证了，但水位有时会高多了，小家伙可就恼啦。但方法总比困难多，小家伙在要求水位的水平面水缸处凿一孔，再接一根管子到下面的备用桶里，这样多出的水会从上面的孔里漏出来。这个水漏出的快慢就称为微分时间，即D。 虽然微分的比喻有点牵强，但能帮助理解就行了。举例中小家伙的试验是一步步独立做，但实际加水工具、漏斗口径、溢水孔的大小同时都会影响加水的速度。

## 2.2 PID分类

### 1、并联型PID(Parallel PID)

并联型PID的三个环节由比例，积分和微分项并联而成，其结构简图如下：

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/bbd7f4e08f34c21b52419101414a8e47.png)

​	其传递函数为：
$$
\begin{align*}
G(s) = K_p + \frac{K_i}{s}+K_d*s
\end{align*}
$$

### 2、串联型PID(Cascade PID)

串联型PID的三个环节由比例，积分和微分项串级而成，结构简图如下：

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/29dba4b7112c55b7a9e58320ff10a79f.png)

其传递函数为：
$$
\begin{align*}
G ( s ) = k ( 1 + \frac{1 }{s τ_i} ) ( 1 + s τ_d )
\end{align*}
$$

### 3、标准PID(Standard or mixed or Ideal PID)

标准型PID与上述二者都不同，其结构简图如下：

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f40c2fdab7e24e5b464cad75b4802213.png)

其传递函数为：
$$
\begin{align*}
G ( s ) = k ( 1 + \frac{1 }{s τ_i}  + s τ_d )
\end{align*}
$$

三者最重要的区别在于不同结构的参数对于控制器行为影响的不同。并联型PID实现了比例项，积分项和微分项的完全解耦，调节其中的$K_p、K_i$与$K_d$ 即可独立的作用在比例，积分和微分项上；而标准形式的$K_p$ 将同时影响比例，积分和微分三项行为。串联型类似。

## 2.3 并联型PID传递函数推导

​		并联型的模拟 PID 控制系统原理框图如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/ju1DzqX8iaOmiaCIV9yjqhQSJqBMSU7eWePFKe1ibicXQAOeuA4VhGNcpFIV5bJyQiarxMJiaun8k3v9VZNib4nx6m5icQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

​		框图中$r(t)$是给定值，$y(t)$是系统的实际输出值，给定值与实际输出值构成控制偏差$e(t)=r(t)-y(t)$。

​		$e(t)$作为PID控制的输入，$u(t)$作为PID控制器的输出和被控对象的输入。所以魔力PID控制器的关系为：
$$
\begin{align*}
u(t)=K_p[e(t)+\frac{1}{T_i}\int_{0}^{t}e(tdt)+T_d\frac{de(t)}{dt}]
\end{align*}
$$
​		其中，$K_p$为比例系数，$T_i$为积分时间常数，$T_d$为微分时间常数，$\int_{0}^{t}$为误差累积，$dt$为采样周期，$de(t)$为误差的微分。

​		$K_p、T_i、T_d$为可调节参数，为方便进行参数调节，上式可以简化为：
$$
\begin{align*}
u(t)=K_pe(t)+K_i\int_{0}^{t}e(t)dt+K_d\frac{de(t)}{dt}
\end{align*}
$$
​		对上式进行拉氏变换，上式变为：
$$
\begin{align*}
U(s)=K_pE(s)+K_i\frac{1}{s}E(s)+K_dsE(s)
\end{align*}
$$
则对于PID控制器，其输出关于误差信号的传递函数为：
$$
\begin{align*}
\frac{U(s)}{E(s)}=K_p+K_i\frac{1}{s}+sK_d
\end{align*}
$$

## 2.4 并联型PID离散化算法公式

### 1、位置式PID算法(Position PID calculation)

$$
\begin{align*}
u(t)=K_pe(t)+K_i\int_{0}^{t}e(t)dt+K_d\frac{de(t)}{dt}
\end{align*}
$$

拉氏域下：
$$
\begin{align*}
U(s)=K_pE(s)+K_i\frac{1}{s}E(s)+K_dsE(s)
\end{align*}
$$
但这是连续的表达方式，无法通过程序代码实现，因此需要对其进行离散化。

工程中常用的离散化方法有后向差分法和双线性变换法，分别为：
$$
离散化一般采用=\left\{
\begin{align*}
1、 & 后向差分法 \rightarrow \quad S = \frac{1 - Z^{-1}}{T_s} \\
2、 & 双线性差分法\rightarrow S = \frac{2}{T_s}\frac{1 - Z^{-1}}{1 +Z^{-1}}
\end{align*}
\right.
$$
其中 Ts 为采样周期，其他还有如前向差分法、一节保持器法、零阶保持器法、脉冲响应不变法等，不做介绍。

这里使用最简单的后向差分法，直接利用 s与 z 的关系进行替换即可：
$$
\begin{align*}
U(z)=K_pE(z)+K_i\frac{T_s}{1-z^{-1}}E(z)+K_d\frac{1-z^{-1}}{T_s}E(z)
\end{align*}
$$
其中，令$Y(z)=K_i\frac{T_s}{1-z^{-1}}E(z)$：
$$
\begin{align*}
&Y(z)=K_i\frac{T_s}{1-z^{-1}}E(z)    \\
展开:&Y(z)(1-z^{-1}) = K_iT_sE(z)    \\
&Y(z)-Y(z)z^{-1}=  K_iT_sE(z)        \\
&Y(z)=  K_iT_sE(z) +Y(z)z^{-1}
\end{align*}
$$

$$
引入离散采样数据:\left\{
\begin{align*}
1、 & 上一时刻 \rightarrow \quad Y(z)Z^{-1}  = y(n - 1) \\
2、 & 当前时刻 \rightarrow \quad Y(z)X^0 = y(n)  \\
3、 & 下一时刻 \rightarrow \quad Y(z)Z^1 = y(n + 1)    \rightarrow未使用
 \end{align*}
\right.
$$

$$
\begin{align*}
得到:&Y(z)=K_iT_sE(z) + Y(z-1)   \\
根据滞后定理:&Y(z-1) = K_iT_sE(z-1) + Y(z-2)    \\
将积分项整理得:&Y(z)=K_iT_s[E(z)+E(z-1)+E(z-2)+\cdot\cdot\cdot+E(z-k+1)+\cdot\cdot\cdot+E(2)+E(1)]    \\
&Y(z)=K_iT_s\sum_{k=1}^{k=z}{E(k)}
\end{align*}
$$

最终形式为：
$$
\begin{align*}
U(z)=K_pE(z)+K_iT_s\sum_{k=1}^{k=z}{E(k)}+\frac{K_d}{T_s}[E(z)-E(z-1)]
\end{align*}
$$
下面给出了位置式PID算法得代码：

```c
void Alg_vPosPICalc(FILTER_PI_FRAME_T *RegAddr,float32_t f32FeedBack)
{
    RegAddr->Vars.f32Err = RegAddr->Vars.f32Ref - f32FeedBack;

    RegAddr->Vars.f32Integral += RegAddr->Vars.f32Err * RegAddr->Coeff.f32ki;

    RegAddr->Vars.f32Integral = MIN(RegAddr->Vars.f32Integral, RegAddr->Coeff.f32UpLimit);
    RegAddr->Vars.f32Integral = MAX(RegAddr->Vars.f32Integral, RegAddr->Coeff.f32DnLimit);

    RegAddr->Vars.f32Out = RegAddr->Vars.f32Integral + RegAddr->Vars.f32Err * RegAddr->Coeff.f32kp;

    RegAddr->Vars.f32Out = MIN(RegAddr->Vars.f32Out, RegAddr->Coeff.f32UpLimit);
    RegAddr->Vars.f32Out = MAX(RegAddr->Vars.f32Out, RegAddr->Coeff.f32DnLimit);

}
```

### 2、增量式PID算法(Incremental PID calculation)

拉氏域下：
$$
\begin{align*}
U(s)=K_pE(s)+K_i\frac{1}{s}E(s)+K_dsE(s)
\end{align*}
$$
这里使用最简单的后向差分法，直接利用 s与 z 的关系进行替换即可：
$$
\begin{align*}
U(z)=K_pE(z)+K_i\frac{T_s}{1-z^{-1}}E(z)+\frac{K_d}{T_s}[E(z)-E(z-1)]
\end{align*}
$$
其中，令$Y(z)=K_i\frac{T_s}{1-z^{-1}}E(z)$：
$$
\begin{align*}
&U(z)=K_pE(z)+Y(z)+\frac{K_d}{T_s}[E(z)-E(z-1)]   \\
&U(z-1)=K_pE(z-1)+Y(z-1)+\frac{K_d}{T_s}[E(z-1)-E(z-2)]    \\
\Delta U=U(z)-U(z-1)：&\Delta U=K_p[E(z)-E(z-1)]+K_iT_sE(z)+\frac{K_d}{T_s}[E(z)-2E(z-1)+E(z-2)]
\end{align*}
$$
$\Delta U$为增量，则PID得输出就为：
$$
\begin{align*}
&U(z)=U(z-1)+\Delta U   \\
&U(z)=U(z-1)+K_p[E(z)-E(z-1)]+K_iT_sE(z)+\frac{K_d}{T_s}[E(z)-2E(z-1)+E(z-2)]
\end{align*}
$$
下面给出了增量式PID算法得代码：

```c
void Alg_vIncPICalc(FILTER_PI_FRAME_T *RegAddr,float32_t f32FeedBack)
{
    float32_t f32Out;

    RegAddr->Vars.f32Err = RegAddr->Vars.f32Ref - f32FeedBack;

    f32Out = RegAddr->Coeff.f32ki * RegAddr->Vars.f32Err + \
        RegAddr->Coeff.f32kp * (RegAddr->Vars.f32Err - RegAddr->Vars.f32PreErr);
    f32Out += RegAddr->Vars.f32PreOut;

    f32Out = MAX(f32Out, RegAddr->Coeff.f32DnLimit);
    f32Out = MIN(f32Out, RegAddr->Coeff.f32UpLimit);

    RegAddr->Vars.f32Out = f32Out;

    RegAddr->Vars.f32PreOut = RegAddr->Vars.f32Out;
    RegAddr->Vars.f32PreErr = RegAddr->Vars.f32Err;
}
```



# 三、交流电压采样值RMS

[什么是RMS电压？RMS电压计算方法介绍-IC先生 (mrchip.cn)](https://www.mrchip.cn/newsDetail/1168)

https://www.cnblogs.com/simpleGao/p/16708523.html



# 四、 电弧检测程序策略

## 4.1 数据采集

​	数据采集在CLA模块中实现，将ADC采样配置为对ADCINC3过采样4次，他们应用于SOC0、SOC3、SOC6、SOC9，每次采样使用20个SYSCLK周期的采样持续时间，，配置方式见：

```c
const ADCSOC_CFG_TBL_T DcpCfgTbl_atAdcSoc[] =
{
    {ADCC, ADC_SOC0 , ADCIN_C3 , ADC_TRIG_EPWM11_SOCA}, /** Sample ADCINC3 , AFCI      , Triggered by EPWM11_SOCA*/
    {ADCC, ADC_SOC1 , ADCIN_C2 , ADC_TRIG_EPWM11_SOCA}, /** Sample ADCINC2 , MUX NTC   , Triggered by EPWM11_SOCA*/
    {ADCC, ADC_SOC2 , ADCIN_C4 , ADC_TRIG_EPWM11_SOCA}, /** Sample ADCINC4 , MUX SAFETY, Triggered by EPWM11_SOCA*/

    {ADCC, ADC_SOC3 , ADCIN_C3 , ADC_TRIG_EPWM11_SOCA}, /** Sample ADCINC3 , AFCI      , Triggered by EPWM11_SOCA*/
    {ADCC, ADC_SOC4 , ADCIN_C12, ADC_TRIG_EPWM11_SOCA}, /** Sample ADCINC12, Ug_L1N    , Triggered by EPWM11_SOCA*/
    {ADCC, ADC_SOC5 , ADCIN_C14, ADC_TRIG_EPWM11_SOCA}, /** Sample ADCINA14, Ug_L12    , Triggered by EPWM11_SOCA*/

    {ADCC, ADC_SOC6 , ADCIN_C3 , ADC_TRIG_EPWM11_SOCA}, /** Sample ADCINC3 , AFCI      , Triggered by EPWM11_SOCA*/
    {ADCC, ADC_SOC7 , ADCIN_C15, ADC_TRIG_EPWM11_SOCA}, /** Sample ADCINC15, Ug_L2N    , Triggered by EPWM11_SOCA*/
    {ADCC, ADC_SOC8 , ADCIN_C14, ADC_TRIG_EPWM11_SOCA}, /** Sample ADCINA14, Ug_L12    , Triggered by EPWM11_SOCA*/

    {ADCC, ADC_SOC9 , ADCIN_C3 , ADC_TRIG_EPWM11_SOCA}, /** Sample ADCINC3 , AFCI      , Triggered by EPWM11_SOCA*/
    {ADCC, ADC_SOC10, ADCIN_C12, ADC_TRIG_EPWM11_SOCA}, /** Sample ADCINC12, Ug_L1N    , Triggered by EPWM11_SOCA*/
    {ADCC, ADC_SOC11, ADCIN_C15, ADC_TRIG_EPWM11_SOCA}, /** Sample ADCINC15, Ug_L2N    , Triggered by EPWM11_SOCA*/
};
```

```c
void HalDrv_vAdcSOCSetup(uint16_t u16AdcNo, uint16_t u16SocNo, uint16_t u16AdcIn, uint16_t u16TrigSrc)
{

    uint32_t* pBaseAddr = (uint32_t *)(&((*ADC[u16AdcNo]).ADCSOC0CTL.all));

    EALLOW;
    *(pBaseAddr + u16SocNo) = ((uint32_t)ADC_ACQPS) + (((uint32_t)u16AdcIn) << 15) + (((uint32_t)u16TrigSrc) << 20);
//#define ADC_ACQPS       (0x14)
    EDIS;
}
```

​	按照配置，当 ePWM11 匹配其周期并生成 SOCA 信号时，如果 ADC 处于空闲状态，则 ADC 将立即开始采样通道 ADCINC3 (SOC0)。如果 ADC 繁忙，则 SOC0 获得优先级时，ADCINC3 将开始采样（参见 ADC 转换优先级）。SOC0 转换完成后，SOC3 将开始转换 ADCINC3，SOC3 的结果将放置在 ADCRESULT3 寄存器中。所有四个转换最终将按顺序完成，SOC0、SOC3、SOC6 和 SOC9 的结果分别位于 ADCRESULT0、ADCRESULT3、ADCRESULT6 和 ADCRESULT9 中。

​	

# 五、 底层配置

下面是V12代码中的配置方式，根据代码进一步学习配置方式：

```c
void bswcfg_vMcuSetup(void)
{

    HalDrv_vSysInit();

    bswcfg_vGpio();
    bswcfg_vXBarInput();

    bswcfg_vInterrupt();

    /** 初始化外围设备*/
    bswcfg_vTimers();

    bswcfg_vCla();

    bswcfg_vAdc();

    bswcfg_vEPwm();

    bswcfg_vEcap();

    bswcfg_vSci();

    bswcfg_vCmpss();
    bswcfg_vXBarEPwm();
    bswcfg_vXint();
}
```

## 4.1 配置顺序

