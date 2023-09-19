注册驱动
> 连接哪个品牌数据库
> Class.forName("com.mysql.jdbc.Driver");

获取连接
>  JVM进程和数据库进程建立通道，进程间通信，重量级，使用后要关闭
>  conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/Database_name")

获取数据库操作对象
> 专门执行sql语句的对象
> stmt = connn.createStatement()
> 
> ps = conn.prepareStatement(sql)

执行sql语句
> DQL, DML
> String sql = "select * from t_user where loginName = '"+loginName+"'and lginPwd = '"+loginPwd+"'";
> rs = stmt.executeQuery(sql);
>
> ps.setString(1, ...)
> ps.setStirng(2, ...)
> rs = ps.execcuteQuery()

处理查询结果集
> 只有执行select语句时，才有这一步

释放资源
> Java和数据库是进程间通信
