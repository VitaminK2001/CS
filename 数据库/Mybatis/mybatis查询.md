## mybatis作用
mybatis是优秀的持久层框架，避免所有JDBC的代码，包括手动设置参数setInt setString以及获取结果集 
## 持久层
持久层就是将程序在持久状态和瞬时状态转化的过程
## 持久化
为什么需要持久化，因为有些信息不能丢掉

# 第一个mybatis程序
搭建环境 --> 导入mybatis程序 --> 编写代码 --> 测试

# 导入依赖
```xml
<dependencies>  
<!--        mysql驱动-->  
        <dependency>  
            <groupId>mysql</groupId>  
            <artifactId>mysql-connector-java</artifactId>  
            <version>8.0.17</version>  
        </dependency><!--        mybatis-->  
        <dependency>  
            <groupId>org.mybatis</groupId>  
            <artifactId>mybatis</artifactId>  
            <version>3.5.2</version>  
        </dependency><!--        junit-->  
        <dependency>  
            <groupId>junit</groupId>  
            <artifactId>junit</artifactId>  
            <version>4.12</version>  
        </dependency>    </dependencies>
```

# 创建模块
子模块的pom.xml文件中有父项目存在，所以不需要重新导包
```xml
<parent>  
    <artifactId>MybatisStudy2</artifactId>  
    <groupId>org.example</groupId>  
    <version>1.0-SNAPSHOT</version>  
</parent>
```

在父模块中也有子模块的存在
```xml
<modules>  
    <module>mybatis-01</module>  
</modules>
```

# 配置mybatis核心配置文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE configuration  
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
        "https://mybatis.org/dtd/mybatis-3-config.dtd">  
<!--核心配置文件-->  
<configuration>  
<!--    s表示能够配置多套环境-->  
    <environments default="development">  
        <environment id="development">  
<!--            默认用jdbc事务管理-->  
            <transactionManager type="JDBC"/>  
            <dataSource type="POOLED">  
                <property name="driver" value="com.mysql.jdbc.Driver"/>  
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>  
                <property name="username" value="root"/>  
                <property name="password" value="password"/>  
            </dataSource>        </environment>    </environments>    <mappers>        <mapper resource="org/mybatis/example/BlogMapper.xml"/>  
    </mappers></configuration>
```
这是从官网复制下来的，唯一的改动是url username 和 password


# 获取SqlSession对象编写Mybatis工具类
SqlSession对象，可以执行Sql语句
通过SqlSessionFactory获得对象，直接从封装的Utils类中写getSession()方法
```java
public class MybatisUtils {  
    private static SqlSessionFactory sqlSessionFactory;  
    static {  
        try {  
            //使用mybatis的第一步 获取SqlSession对象  
            String resource = "mybatis-config.xml";  
            InputStream inputStream = Resources.getResourceAsStream(resource);  
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);  
        } catch (IOException e) {  
            e.printStackTrace();  
        }  
    }  
    //既然有了SqlSessionFactory顾名思义我们可以在其中获得SqlSession的实例  
    public static SqlSession getSession(){  
        return sqlSessionFactory.openSession();  
    }  
}
```

# 编写代码
## 实体类
```java
public class User {  //和数据库表明对应
    private int id;  
    private String name;  
    private String pwd;
    省略get和set方法
}
```

## 编写DAO接口
```java
public interface UserDao {  
    List<User> getUserList();  
}
```

## 编写接口实现类UserMapper.xml
这是存放sql语句的xml文件
原来JDBC的方法是写一个UserDao的接口，其中有各种方法
然后写UserDaoImpl实现UserDao接口
现在用UserMapper写接口实现类
```java
public interface UserDao {  
    List<User> getUserList();  
}
public class UserDaoImpl implements UserDao{  
//前面还有注册驱动 获取连接 获取数据库操作对象 （处理查询结果集）执行sql 释放连接
    @Override  
    public List<User> getUserList(){  
        //执行sql语句  
        String sql = "select * from mybatis.user";  
    }  
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper  
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">  
<!--namespace绑定一个对应的Dao/Mapper接口-->  
<mapper namespace="com.chico.dao.UserDao">  
<!--    select查询语言的id对应方法名-->  
<!--    返回结果要写全路径名-->  
    <select id = "getUserList" resultType="com.chico.pojos.User">  
            select * from mybatis.user;  
    </select>  
</mapper>
```
![[截屏2022-11-30 21.04.20.png]]
resultType表示类型（返回多个）
resultMap表示集合（返回一个）

## Junit测试
UserMapper.xml是UserDao接口的实现类
通过getMapper方法获得接口的实例
然后执行相应的方法

每一个Mapper.xml都需要在mybatis核心配置文件mybatis.xml中注册
```xml
<mappers>  
    <mapper resource="com/chico/dao/UserMapper.xml"/>  
</mappers>
```
而且需要再pom.xml文件中配置resources 防止资源导出失败
```xml
<!--在build中配置resources，防止资源导出失败的问题-->  
<build>  
    <resources>        <resource>            <directory>src/main/resources</directory>  
            <includes>                <include>**/*.properties</include>  
                <include>**/*.xml</include>  
            </includes>        </resource>        <resource>            <directory>src/main/java</directory>  
            <includes>                <include>**/*.properties</include>  
                <include>**/*.xml</include>  
            </includes>            <filtering>true</filtering>  
        </resource>    </resources></build>
```

```java
public class UserDaoTest {  
    @Test  
    public void test(){  
//        获得SqlSession对象  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
//        方式一执行  
        UserDao userDao = sqlSession.getMapper(UserDao.class);  
        List<User> list = userDao.getUserList();  
        for(User user : list){  
            System.out.println(user);  
        }  
        sqlSession.close();  
    }  
}
```