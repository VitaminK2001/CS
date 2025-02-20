默认情况下,`Object` 类的 `equals` 方法实现和 `==` 是一样的，
即比较两个对象的引用是否相等(是否指向内存中的同一个地址)。
```java
public boolean equals(Object obj) {
    return (this == obj);  // 直接比较内存地址
}
```