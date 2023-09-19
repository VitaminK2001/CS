**Statement存在Sql注入的问题**
因为用户提供的信息参与编译过程。Statement执行sql语句是通过发送sql语句给DBMS，DBMS进行sql编译，而sql语句是拼接的，有用户提供的非法信息[[JDBC六步]]，导致原sql语句的含义扭曲

**解决sql注入**
只要用户提供的信息不参与sql语句的编译过程，就解决了。必须使用java.sql.PreparedStatement接口。
PreapredSatement的原理是，预先对sql语句框架进行编译，然后再给sql语句传值。
stmt执行的sql语句中，在用户传入的信息处填写占位符，放入获取预编译的数据库操作对象方法，ps = conn.prepareStatement(sql)

PreparedStatement是Statement的子接口，Statement是数据库操作对象，PreparedStatement是预编译的数据库操作对象。


PreparedStatement编译一次执行多次，Statement是编译一次执行一次，效率不如PreparedStatement。

PreparedStatement会做类型的安全检查，在setString,setInt方法的时候确定传值的类型，而Statement不会。
