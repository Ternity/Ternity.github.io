# 偶极、极化、电场、力场、恒电荷、氧化还原能力

## Fe-N<sub>4</sub>/C 恒电荷但是考虑电极电势的CMD方法
已有工作[ACS Catal. 2023, 13, 11080−11090](https://pubs.acs.org/doi/full/10.1021/acscatal.3c02169)实现了Classical MD 研究Fe-N4-C体系，非常值得参考，其使用恒定电荷方法，电荷由AIMD给出，向电极注入或抽取电子以模拟不同电势。

然而，这种方法总电荷是不变的，并且每个原子的电荷是固定的。因此当发生反应时候，催化剂的电子数理应迅速发生变化、氧化还原性能(电负性)巨变，但是固定电荷方案难以实现这一点。实际上，作者调节了力场参数来适应不同吸附物时的能量学正确性(视化学吸附为范德华作用，Fe-O之间不成键)，但是仍然难以观察反应过程。只适合观察界面溶剂结构。

## MLFF 预测电荷及在电催化中应用

- MLFF类型：
  - 电荷的预测，从偶极矩出发预测电荷（TNEP-->charge）；
  - 从Wannier中心出发预测电荷(DeepWannier--> charge)；
  - 直接预测电荷的几种[MLFF力场](onenote:..\基础学习\化学.one#恒电势方法-电荷-极化力场系列&section-id={0E819CD7-F224-46DE-BD42-ECAA072213BE}&page-id={33F8A7B9-69A3-45FF-8F8B-BCD5C8946EAA}&end&base-path=https://njfueducn-my.sharepoint.com/personal/180401224_njfu_edu_cn/Documents/笔记本/理论计算与模拟)(诸如SchNet，PaiNN，HDNNP，SoopketNet，MACE，DPMD等等).


但是这些MLFF方案有个大问题，和电势联系不起来。一方面，就算我在电极上设置初始电荷(电荷\--电荷密度+电容\--电势)，电荷也会被model预测，进而破坏了<font color=Red>电势的恒定</font>；另一方面，对于电催化反应，即使使用了晶胞外推法([cell-extrapolation](https://ternity.github.io/wiki/%E6%81%92%E7%94%B5%E5%8A%BF%E6%96%B9%E6%B3%95-%E7%94%B5%E8%8D%B7-%E6%9E%81%E5%8C%96%E5%8A%9B%E5%9C%BA%E7%B3%BB%E5%88%97/DFT%E9%9D%99%E6%80%81-AIMD-Geo-Opt%E4%B8%8B%E7%9A%84%E6%81%92%E7%94%B5%E5%8A%BF/Norskov%E5%85%B3%E4%BA%8E%E8%83%BD%E5%9E%92%E7%9A%84%E6%81%92%E7%94%B5%E5%8A%BF%E6%A0%A1%E6%AD%A3%E6%80%9D%E8%B7%AF/)),界面电荷的转移情况我们可以predict，但是我们只能在这个材料固定的电势下(即盒子不断扩大，电势不再变时候的值),无法调节电势，无法观察能量学随着电势发生的变化。最后，上面这些电荷预测方案，除了HDNNP，没有总电荷恒定的，也没有全局电荷考虑(因为预测的电荷值是局部环境的量)，这样一是主动学习时候，不方便做带电体系(因为无法保证每一帧预测的总电荷为0)的DFT验证，二是对于全局电荷分布不准（这就是王飞腾说的DFT优化电子结构，溶液里有一个Cl，若整体电中性，则必然溶液中的Cl带负电但是电极带正电。如果没有全局电荷平衡，那么根据局部环境预测的电荷将认为Cl是带负电而电极仍为电中性）。

## 可极化力场电荷预测及在电催化中应用
- 经典力场类型：
  - ReaxFF<br>
  - [经典力场下的恒电势方法(非反应)](onenote:..\基础学习\化学.one#经典力场下的恒电势方法(非反应)&section-id={0E819CD7-F224-46DE-BD42-ECAA072213BE}&page-id={BA2CE166-085B-4994-AE6D-1D8B02F14CA5}&end&base-path=https://njfueducn-my.sharepoint.com/personal/180401224_njfu_edu_cn/Documents/笔记本/理论计算与模拟)

上面提到的MLFF预测电荷，相较于一开始提到的ACS Catal., 反应是可以做了，但是两个缺点：1\. 没做电荷平衡，无法控制恒定电荷；2\. 没做“全局”电荷平衡，电荷的预测是局部而不是考虑全局的结构。最常见的、考虑全局电荷平衡的、可以做反应的就是反应力场Reaxff了。然而，ReaxFF同样是也有两个问题：1\. 不准，拟合难，对于电催化通常没有合适的力场参数，而且电荷平衡时候电负性是定值不变，氧化还原性质不变，不符合电化学过程；2\. 只能ConstQ不能ConstP。


以FCP-LAMMPS、华科毕晟博士的论文为代表，可以恒电势，但是似乎只变化电极电荷，电解质电荷不变；另外为了加速Qeq过程，可以电荷变化的电极，它的原子位置不变，即矩阵M不变，这样对于电极结构重构或者像单原子催化剂这种单原子上下浮动进而影响自旋态的情况难以考虑。另外是不能做反应，因为基于CMD。

> ## under construction


## 其他概念观点

电极（催化剂）中过量的电子将产生极强的还原性（O~2~将在极低的能垒中被快速还原）



[Dipole（偶极子）和Polarizability（极化率）都是描述分子电性质的重要参数，但它们描述的是不同的物理现象1](https://chemistry.stackexchange.com/questions/51292/difference-between-polarizability-and-dipole-moment)[2](https://physics.stackexchange.com/questions/274794/difference-between-polarizability-and-dipole-moment)。

1. [**Dipole（偶极子）**==：偶极子是一个向量量，它有大小和方向。偶极矩是一个数学计算，关于在化合物中电荷的不均匀分布1。换句话说，化合物的偶极矩越高，该化合物就越极性1==](https://chemistry.stackexchange.com/questions/51292/difference-between-polarizability-and-dipole-moment)==。==
2. [**Polarizability（极化率）**==：极化率是一个描述分子在外部电场作用下形成偶极子的倾向的度量1==](https://chemistry.stackexchange.com/questions/51292/difference-between-polarizability-and-dipole-moment)[==2==](https://physics.stackexchange.com/questions/274794/difference-between-polarizability-and-dipole-moment)[==。极化率表示电子（因此电荷）在施加电场时重新排列的程度1。当一个可极化的分子体验到任何类型的静电作用时，都会产生诱导偶极矩1==](https://chemistry.stackexchange.com/questions/51292/difference-between-polarizability-and-dipole-moment)==。==

[==这两个属性可以有关联，但是可以找到具有高极化率但无偶极矩的分子，特别是如果它们是对称的1==](https://chemistry.stackexchange.com/questions/51292/difference-between-polarizability-and-dipole-moment)==。希望这个解答能帮到你！==



来自 <[https://copilot.microsoft.com/?FORM=undexpand&](https://copilot.microsoft.com/?FORM=undexpand&)\>

