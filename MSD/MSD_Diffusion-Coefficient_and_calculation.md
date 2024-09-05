# 均方位移mean squre displacement (MSD) 和 扩散系数Diffusion Coefficient的理解与计算
## MSD 定义
  粒子位移的平方的平均值称为均方位移（mean squre displacement, MSD）。若以$r_i(t)$表示时间t时刻粒子i的位置，则有：
$$MSD=R(t)=<|r(t)-r(0)|^2>$$
  其中，尖括号表示平均值。
  根据统计原理，当粒子数N足够多，计算时间t足够长（lim N→+∞, lim t→+∞）,系统在任一瞬间均可当作时间零点，所计算的平均值应相同。
  !!我的问题：何为时间零点？为何平均值相同需要N和t的极限？!!


## Reference

动力学性质计算与检验——以计算体系扩散性质为例.ipynb
扩散系数计算时NPT和NVT系综的影响NPT-NVT Simulation Settings on Diffusion Coefficient Calculation.ipynb
拿分子动力学算个扩散系数，怎么这么多讲究.ipynb