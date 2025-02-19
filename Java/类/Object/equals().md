> Objects.equlas(a,b)

目的是防备a或b可能为空
如果两个参数都为null,Objects.equals(a, b)调用将返回true；
如果其中一个参数为null，则返回false；
否则，如果两个参数都不为null，则调用a.equals(b)[^1]。

> 对于数组类型的域，可以使用静态的Arrays.equals方法检测相应的数组元素是否相等。




[^1]: equals在设计上就要求要做到x.equals(y) = y.equals(x)


