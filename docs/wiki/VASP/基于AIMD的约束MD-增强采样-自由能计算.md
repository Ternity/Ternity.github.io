# 基于AIMD的约束MD/增强采样/自由能计算


-----
## 约束MD(Constrained molecular dynamic)

约束MD本质上是约束条件下的最优化问题，因此自然想到了利用拉格朗日乘子法。具体而言，使用[shake](http://dx.doi.org/10.1016/0021-9991(77)90098-5)算法实现约束的分子动力学，该算法本质使用了拉格朗日乘子法来寻找有约束条件的最小化问题。系统的拉格朗日量扩展可以写为：

$$ \mathcal{L}^*(q,p)=\mathcal{L}(q,p)+\sum_{i=1}^{r} \lambda_{i}\sigma_{i}(q) \qquad  $$

其中, $\lambda_i$是与几何约束$\sigma_i(q)$相关的拉格朗日乘子,而几何约束定义为：

$$\sigma_{i}(q)=\xi_{i}(q)-\xi_{i}$$

其中，$\xi_i(q)$是几何参数，$\xi_i$是模拟期间固定的$\xi_i(q)$的值。

shake算法中，拉格朗日乘子$\lambda_i$采用迭代方法确定：

1. 标准MD，蛙跳算法计算新位置​$q_{i}^{t+\Delta_{t}}$​
2. 利用新位置​$q_{i}^{t+\Delta_{t}}$计算所有拉格朗日乘子$\lambda_i$
3. 通过添加由于恢复力（与$\lambda_k$成正比）的贡献来更新速度和位置
4. 迭代2-4步骤直至$|\sigma_{i}(q)|$小于预定义的容差(SHAKETOL参数)，或迭代次数超过SHAKEMAXITER参数

## 缓慢生长方法(slow-growth approach)

### 理论基础

VASP当中实现了[近似的缓慢生长方法](https://www.vasp.at/wiki/index.php/Slow-growth_approach), 该方法同样定义了一个几何参数​$\xi$​, 其特征值以速度$\dot{\xi}$从初态1线性变化到末态2。执行1→2变换的功为：

$$ \omega_{1\rightarrow2}^{irrev}=\int_{\xi(1)}^{\xi(2)}(\frac{\partial V(q)}{\partial \xi}) \dot{\xi}dt \qquad  $$

当​​$\lim_{\dot{\xi} \to 0}$​​, 功​$\omega_{1\rightarrow2}^{irrev}$​ 对应于末态2和初态1之间的自由能差。这个功是基于[Jarzynski恒等式](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.78.2690)的、与自由能相关的不可逆功(本来，​​$\lim_{\dot{\xi}\to 0}$，功是可逆功，但是受限于有限的MD轨迹长度，$\dot{\xi}$不可能无限小，因此功是不可逆功):

$$ exp^{{-{\frac  {\Delta A_{{1\rightarrow 2}}}{k_{B}\,T}}}}={\bigg \langle }exp^{{-{\frac  {w_{{\rightarrow 2}}^{{irrev}}}{k_{B}\,T}}}}{\bigg \rangle}  $$

通过上面公式计算的自由能（将PMF近似视为自由能），需要对​$exp^{\{-\frac{w_{{\rightarrow 2}}^{{irrev}}}{k_{B}\,T}\}}$​在1→2变换的许多实现中取平均值。

基于缓慢增长方法计算的自由能是近似自由能，精度取决于$\dot{\xi}$。而，精度的判断方法可以通过正向（反应物→产物）和逆向(产物→反应物)转变计算得到的自由能分布的滞后性判断；当然，也可以使用Jarzynski恒等式从一系列缓慢增长模拟中计算。Jarzynski恒等式提供了缓慢增长这种非平衡态和从关于状态分布的配分函数得到的自由能这种平衡态之间的桥梁。

关于Jarzynski恒等式的模拟方法的详细说明，请参阅[参考文献](https://pubs.acs.org/doi/abs/10.1021/jp044556a)J.P.C.B.和[参考文献](https://doi.org/10.1103/PhysRevLett.78.2690)P.R.L.。（著名的热力学第二定律唯一的等式表述形式）

> ### Note: 该方法最有用的是，在启动任何更准确和耗时的模拟之前，快速测试集体变量的质量。

### VASP参数

缓慢生长方法在VASP中仅支持NVT和NVE系综,不允许体积变化,同时仅支持anderson和Nose-Hoover两种控温器.

在<a href="onenote:#基于AIMD的约束MD\增强采样\自由能计算&section-id={8365C904-43EF-4FFB-9900-552AFAB494A1}&page-id={A6EFC5DD-E66F-48DC-A7FD-DF96EA8B8395}&object-id={CF448C7A-7973-4317-93FD-5ECB3FD31E99}&D9&base-path=https://njfueducn-my.sharepoint.com/personal/180401224_njfu_edu_cn/Documents/笔记本/各种参考资料/使用方法/VASP.one">ICONST file</a>中定义几何约束,且几何约束的STATUS参数设置为0.在INCAR中,需要使用参数<a href="onenote:#基于AIMD的约束MD\增强采样\自由能计算&section-id={8365C904-43EF-4FFB-9900-552AFAB494A1}&page-id={A6EFC5DD-E66F-48DC-A7FD-DF96EA8B8395}&object-id={8695ED68-ACC7-4594-81B1-0BCD865C4A1C}&8C&base-path=https://njfueducn-my.sharepoint.com/personal/180401224_njfu_edu_cn/Documents/笔记本/各种参考资料/使用方法/VASP.one">INCREM</a>为STATUS=0的集合约束设置<a href="onenote:#基于AIMD的约束MD\增强采样\自由能计算&section-id={8365C904-43EF-4FFB-9900-552AFAB494A1}&page-id={A6EFC5DD-E66F-48DC-A7FD-DF96EA8B8395}&object-id={1CB40155-BE60-483F-A2E3-4B1D84E24AFE}&F3&base-path=https://njfueducn-my.sharepoint.com/personal/180401224_njfu_edu_cn/Documents/笔记本/各种参考资料/使用方法/VASP.one">变化速度</a>$\dot{\xi}$.当需要计算自由能梯度时候,使用LBLUEOUT=.TRUE.

## [Blue-moon ensemble](https://www.vasp.at/wiki/index.php/Blue-moon_ensemble)

简单来说,约束MD产生有偏差的统计平均值,而其正确的平均值可以通过蓝月亮系综平均值(Blue Moon Ensemble Average)获得。进而可以得到自由能梯度和积分得到自由能。且,通过蓝月亮系综平均值,可以从约束MD得到无偏MD的势能平均值(PMF),官方提供了脚本[how to](https://www.vasp.at/wiki/index.php/Blue-moon_ensemble) and [Free energy profile](https://www.vasp.at/wiki/index.php/Nuclephile_Substitution_CH3Cl_-_SG).

具体来说，blue moon ensemble average公式：

$$ a(\xi)=\frac{\left<|\mathbf{Z}|^{-1/2}a(\xi^*)\right>_{\xi^*}}{\left<|\mathbf{Z}|^{-1/2}\right>_{\xi^*}}  $$

其中​$\left<\dots\right>_{\xi^∗}$​表示约束MD得到的括号内量的统计平均值。​$\mathbf{Z}$​是一个质量度量张量，定义:

$$ \mathbf{Z}_{\alpha , \beta} = \sum_{i=1}^{3N} m_i^{-1} \nabla_i \xi_\alpha \cdot \nabla_i \xi_\beta , 其中\alpha=1,\dots,r,\beta =1, \dots,r  $$

然后可以拿到自由能梯度（基于Blue Moon Ensemble）：

$$ \left(\frac{\vartheta A}{\vartheta \xi_k}\right)_{\xi^*} = \frac{1}{\left\langle |\mathbf{Z}|^{-1/2} \right\rangle_{\xi^*}} \left\langle |\mathbf{Z}|^{-1/2} \left[ \lambda_k + \frac{k_B T}{2|\mathbf{Z}|} \sum_{j=1}^{r}\left( \mathbf{Z^{-1}} \right)_{kj} \sum_{i=1}^{3N} m_i^{-1} \nabla_i \xi_j \cdot \nabla_i |\mathbf{Z}| \right] \right\rangle_{\xi^*}  $$

其中$\lambda_{\xi_k}$是拉格朗日乘子方法中约束坐标$\xi_k$的对应拉格朗日乘子。

然后自由能就可以通过自由能梯度的积分得到，正如[缓慢生长方法中](onenote:https://njfueducn-my.sharepoint.com/personal/180401224_njfu_edu_cn/Documents/笔记本/各种参考资料/使用方法/VASP.one#基于AIMD的约束MD/增强采样/自由能计算&section-id={8365C904-43EF-4FFB-9900-552AFAB494A1}&page-id={A6EFC5DD-E66F-48DC-A7FD-DF96EA8B8395}&object-id={23C3A93F-9438-4EA4-B9DA-894BDA63D4D8}&E8)提到的那样：

$$\Delta A_{1 \to 2}=\int_{\xi(1)}^{\xi(2)}\left( \frac{\vartheta A}{\vartheta \xi}\right)_{\xi^*} \cdot d\xi  $$

当设置`LBLUEOUT=True`时候，REPORT文件将会有<a href="onenote:#基于AIMD的约束MD\增强采样\自由能计算&section-id={8365C904-43EF-4FFB-9900-552AFAB494A1}&page-id={A6EFC5DD-E66F-48DC-A7FD-DF96EA8B8395}&object-id={1F96F061-4EE6-41F7-8274-9086418EF1B4}&A7&base-path=https://njfueducn-my.sharepoint.com/personal/180401224_njfu_edu_cn/Documents/笔记本/各种参考资料/使用方法/VASP.one">一系列输出</a>,其中第二项为$|\mathbf{Z}|^{-1/2}$，第四项为$|\mathbf{Z}|^{-1/2}*(\lambda+GkT)$。然后自由能梯度即为二者之间的商。

GkT是热力学积分项，通常情况下，它的值很小（10<sup>-13</sup>），可以忽略。因此自由能的梯度基本等于λ，而拉格朗日乘数法中，λ是施加在约束坐标上的**力**。因此可以直接λ对约束坐标的微分进行积分，得到自由能。

## MetaDynamics方法

under construction

## ICONST file

[description](https://www.vasp.at/wiki/index.php/ICONST)：定义和控制几何参数/坐标的`文件`，例如原子间距、键角、晶格矢量等。在分子动力学模拟中，这些参数/坐标可以被约束或受到偏置势的影响(监视、控制)。输出将会写入REPORT。

两种类型，基本坐标(键长、键角等)和复杂坐标（基本坐标线性组合）。复杂坐标必须在基本坐标之后定义。即：

```shell
flag item(1) ... item(N) status
...
...
flag c_1 ... c_M status
```

`flag`指定了基本坐标的类型(键、角等)。`item`以空格分隔，按在POSCAR中出现顺序（从1开始）指定离子。`status`指定了status。假设文件前M行指定了M个基本坐标，最后一行指定了一个复杂坐标，其中`c_1`……`c_M`是以空格分隔的复杂坐标`c_i`列表，用于第i行定义的基本坐标。比如：

```
R 1 6 0 # 定义原子 1 和 6之间距离
R 1 5 0 # 定义原子 1 和 5之间距离
S 1 -1 0 # 定义基本坐标1和2之间的线性组合
```

#### flag类型：

R：两原子距离

A：三原子构成的角，第二个原子为顶点

T：四个原子构成的二面角(torsional)

M: 原子1到原子2和3构成键的中心的距离（垂直平分线的长度）

B：原子1和2构成键的中间到原子3和4构成键的中心的距

W: 函数 ​​$\frac{1-(R/c)^M}{1-(R/c)^N}$​​，其中 R 是原子1和2之间的键长（Å单位），c 是由item(3)指定的参考键长，指数M和N分别由 item(4) 和 item(5) 定义。其实就是配位数的定义

X、Y 和 Z：与晶格矢量 a、b 和 c 相关联的分数坐标。

cX、cY 和 cZ：笛卡尔坐标 x、y 和 z。

LR：某个晶格矢量 item(1) 的长度。

LA：两个晶格矢量 item(1) 和 item(2) 之间的夹角。

LV：晶胞体积（在这种情况下没有定义 item(i)）。

还有S基本坐标线性组合、C向量模、D坐标数、IS path方法、IZ path方法等

#### STATUS类型：

`0`：坐标被约束；`4`：坐标受到费米型阶跃函数影响 `5`：MetaD中集体变量；`7`：坐标被监视；`8`：坐标受谐振势影响

更多使用案例请见[官方文档](https://www.vasp.at/wiki/index.php/ICONST)

> ### Tips: 可以使用该文件结合INCAR参数INCREM实现盒子指定角度(αβγ)的控制/不变

```
ICONST file:
LA 1 2 0
LA 1 3 0
LA 2 3 0
INCAR file:
INCREM = 0.000000001 0.000000001 0.000000001 #file ICONST to contral lattic and atoms
```



> ### Tips: 可以使用该文件结合INCAR参数VALUE\_MAX VALUE\_MIN实现指定坐标的上下限

```
ICONST file:
cZ 1 7 # 1号原子的z坐标受监控
INCAR file:
VALUE_MAX = 10 # z-axis of atoms 1 must < 10
```

## INCREM

default = 0

[description](https://www.vasp.at/wiki/index.php/INCREM): 在 slow-growth approach中控制转换速度的参数。

该参数定义了，在[slow-growth 模拟](https://www.vasp.at/wiki/index.php/MDALGO#Slowgro)(MDALGO=1|2), [ICONST](onenote:#AIMD&section-id={8365C904-43EF-4FFB-9900-552AFAB494A1}&page-id={FC2F6A49-6022-4C33-BF5D-992AC8305FAC}&object-id={9296AB25-575C-4C52-9100-401EC18735C7}&14&base-path=https://njfueducn-my.sharepoint.com/personal/180401224_njfu_edu_cn/Documents/笔记本/各种参考资料/使用方法/VASP.one)文件中`STATUS=0`的受控的结构参数(controlled geometric parameter), 每一时间步长增长的大小。

[ICONST](onenote:#AIMD&section-id={8365C904-43EF-4FFB-9900-552AFAB494A1}&page-id={FC2F6A49-6022-4C33-BF5D-992AC8305FAC}&object-id={9296AB25-575C-4C52-9100-401EC18735C7}&14&base-path=https://njfueducn-my.sharepoint.com/personal/180401224_njfu_edu_cn/Documents/笔记本/各种参考资料/使用方法/VASP.one)文件中`STATUS=0`的受控的结构参数都必须提供该参数。

## LBLUEOUT

default = .FALSE.

[description](https://www.vasp.at/wiki/index.php/LBLUEOUT): 设置为.True.，将自由能梯度输出到REPORT文件,位置位于每个MD步骤之后.

输出如下:

```
>Const_coord
   cc> R    2.16325   2.16325  0.485234E-05

>Blue_moon
       lambda         |z|^(-1/2)      GkT           |z|^(-1/2)*(lambda+GkT)
b_m> 0.585916E+01 0.215200E+02 -0.117679E+00 0.123556E+03
```

如果有多个约束坐标，那么将按顺序打印各个约束坐标的b\_m>。

lambda对Const\_coord积分即为自由能。

## Reference:

[Constrained molecular dynamics - VASP Wiki](https://www.vasp.at/wiki/index.php/Constrained_molecular_dynamics)

[Slow-growth approach - VASP Wiki](https://www.vasp.at/wiki/index.php/Slow-growth_approach)

[Blue-moon ensemble - VASP Wiki](https://www.vasp.at/wiki/index.php/Blue-moon_ensemble)

[Nuclephile Substitution CH3Cl - SG - VASP Wiki](https://www.vasp.at/wiki/index.php/Nuclephile_Substitution_CH3Cl_-_SG)

[Free Energy Calculation Using Blue-moon Method - My Community (vasp.at)](https://wiki.vasp.at/forum/viewtopic.php?t=17582)


-----


