定义业务逻辑相等性

使用Objects::equals(a,b)须知:
1. 若ab均空,返回true
2. 若a或b一者为空,返回false
3. 若ab均不为空,返回a.equals(b)[^1]

使用Arrays.equals()须知:
1. 严格按照数组元素的顺序和值来判断两个数组是否相等

重写equals()须知:
1. 必须同步覆盖hashCode()
2. x.equals(y) == true ⇒ x.hashCode() == y.hashCode()[^2]

[^1]: equals在设计上就要求要做到x.equals(y) = y.equals(x)


[^2]: 这是为什么要做第一条的原因,也是重写equals()时的要求
