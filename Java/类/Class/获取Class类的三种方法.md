Class.forName()

类名.class()

对象.getClass()


Java运行时系统始终为所有的对象维护一个被称为运行时的类型标识

这个信息跟踪着每个对象所属的类。虚拟机利用运行时类型信息选择相应的方法执行。

Object类中的getClass()方法将会返回一个Class类型的实例。

还可以调用静态方法forName获得类名对应的Class对象。

