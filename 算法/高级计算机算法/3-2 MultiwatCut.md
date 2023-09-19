# 问题
![[截屏2022-11-29 20.56.44.png]]
找到边集E'使得原先的连通图去掉E'之后，任意两个terminal不连通。

# Isolating Cuts
![[截屏2022-11-30 14.47.42.png]]
独立割将一个terminal和其他的terminal割开
可以通过最小割的方式计算minimum cost s-ti-cut
# Algorithm MultiwayCut
![[截屏2022-11-30 14.52.12.png]]
计算k个点的独立割，然后去掉最贵的割Ck
保证总消费<= (1-1/k) * （c(Ci) i = 1~k求和）
因为最贵的割Ck肯定大于平均值
# Approximation Factor
![[截屏2022-11-30 14.56.01.png]]
Ai表示对于一个点而言最优独立割
（Ai i=1~k求和） <= 2 * c(A) = 2 * OPT
**因为一个割边最多在两个Ai中被重复计算**
# Analysis Sharp?
![[截屏2022-11-30 15.01.14.png]]









