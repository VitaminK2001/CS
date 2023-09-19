Java DataBase Connectivity
本质：一套接口 在java.sql.* 包下
接口：抽象的协议
> 用来解耦合，提升扩展力

**面向接口编程**，就是面向抽象编程

数据库厂商是接口的实现者，
不同的数据库对接口实现不同
我们调用接口连接不同的数据库

数据库对接口的实现用.class文件，是一个jar包（驱动），需要从网上下载，MySql驱动 Oracle驱动 SqlServer驱动

