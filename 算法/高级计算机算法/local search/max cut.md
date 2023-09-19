通过解的领域中找一些点放入解

让解的质量变好

想要通过局部搜索得到局部最优

**难点：怎样从一个局部最优跳到另一个局部最优**

随机梯度下降，整体的解的质量在变好，但是变好的过程中可能出现解的质量时好时坏



```c
local search:
1. find a "good" initial sollution S0,
2. S <- S0;
3. repeat 
	 if(存在S`属于N(S) val(S`) better than val(S) N(S)表示S的领域)	then S<-S`
	 else s is local optimal return S;
```

换入换出的过程需要枚举，时间取决于换的多少，换的越多越有可能跳出局部最优

多换多可以将枚举的数据集缩小，节省时间

# max-cut

given G = (v,e)

partition V into (S, V - S) to maximize the **number of edges**

crossing S. 点集分成两部分，使得交叉边最多

NP 难问题

加强版：如果每条边都有一个权值的话，使得横跨两边的权值之和最大

可能存在两种变好的方法（变好：权值之和变大）

1.  将S中的点移到另一个集合中
2.  将另一个集合中的点移到S中

这两种都可能存在原来是交叉的边不交叉，原来不交叉的边交叉，比较权值的改变

S中的点越来越多，内部的边也越来越多，导致可能将点从S中拿出去会变好

如果这两种情况都不会增加，则停止。此时为局部最优解。

OPT / OBJ ≤ 2 连接一个点的所有边，至少得有一半是横跨两个集合的

## lsmc 算法
![[Pasted image 20221205100845.png]]

```c++
1 . S<-空集;
2 . repeat 
if 存在 v 属于 V \ S 如果放入S的权值和比不放入S的权值和大
则将v放入S S <- S + v
else if 存在 v 属于 S 如果从S中拿出后的权值大于不拿出
则将v拿出S S <- S - v
else return S;
```

### 算法的时间复杂度
#### 不带权值
每一个点在一次执行过程中都要尝试一遍
不在S中的点尝试放入S
在S中的点尝试拿出S
所有点考虑完了，算法就执行完了 
所以算法的时间复杂度是O(E)
#### 带权值
带权值的话算法的时间复杂度可能很难控制到多项式内
S的权值和可能很大，算法的时间复杂度和权值相关

### lemma
1. for each vertex v, $w\left(\sigma\left(S\right)\cap\sigma\left(v\right)\right) > w(\sigma(v)) \div 2$
交叉边的权值和大于V中的权值和的一半
> 如果小于一半的话就会换了（将S换成V/S）

2. 如果S是局部最优的 则$w\left(\sigma\left(S\right)\right)$ 大于等于所有边权值的和的一半大于等于OPT/2
	因为w(E)是所有边的权值之和，OPT不可能大于w(E)
	因为一条边和两个点相邻
	$w\left(\sigma\left(S\right)\right)$ = 1/2 $*\ \  \sum_{v \in V} w\left(\sigma\left(S\right)\cap\sigma\left(v\right)\right) \geq 1/2*w(E) \geq OPT \cdot 1/2$
	

### 改进的LSMC

```ad-hint
1. S = {$v^*$s $v^*$ = argmax(w($\sigma(v)$))}
2. repaet 
if 存在 v 属于 V \ S w($\sigma(S+v)$) > (1 + $\frac{\epsilon}{n}$) $\cdot$ w($\sigma(S)$)
else if 存在 v 属于 S w($\sigma(S-v)$) > (1 + $\frac{\epsilon}{n}$) $\cdot$ w($\sigma(S)$)
3. return S
```

### lemma
```ad-hint
for each vertex v 
$w(\sigma(s)) \geq \frac{1}{2\cdot(1 + \frac{\epsilon}{4})} \cdot w(E)$
$\alpha_v = w(\sigma(s))\cap\sigma(v))$
$\beta_v = w(\sigma(v)) - \alpha_v$
```
解的近似率变差了，epxl是大于0小于1的数
最大都是1/2.5 * w(E)
epxl 越放松（小） 近似率越好
epxl 越严格（大） 近似率越差 
通过控制放进去和拿出来的点，来控制迭代次数
如果epxl 越大，则迭代的次数越少

外面的边的权值不能比里面的边的权值大太多
$\sigma$(S)是S中的点的交叉边的权值之和

算法的时间复杂度
epxl = n => O(log n)
v* 表示点的连接的边的权值之和最大的那个点，也就是大于平均值
$w(so) = w(\sigma(v*)) \geq 2*(w(E)) \div n$ 
每次权值增加(1 + epxl/n)倍，最后增加到w($\sigma(S)$)局部最优解后无法增加

也就是说，不要一有改进就把点放进来，而是要改进到一定的程度


