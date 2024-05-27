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
>{\color{red}{截止频率：f_c =\frac{1}{2\pi RC} = \frac{{\omega}_c}{2 \pi} = \frac{1}{2{\pi}{\tau}}}} \tag{截止频率转换}
>$$
>

