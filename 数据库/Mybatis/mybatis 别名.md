```xml
mybatis-config.xml
别名有三种方式
第一种用typeAlias
<typeAliases>  
<!--        可以给pojo中的类起别名-->  
        <typeAlias type="com.chico.pojo.User" alias="User"/>  
  
</typeAliases>

第二种用package
<package name="com.chico.pojo"/>
在没有注解的情况下默认是用包下的类的首字母小写名作为别名 user
首字母大写也行，但是建议用小写表示是通过扫描包的方式
第一种可以自定义类名
但是第二种不行

第三种是用注解，在第二种的基础上，如果有注解，则别名为其注解值
@Alias("Hello")  
public class User {}

<select id = "getUserList" resultType="hello">  
        select * from mybatis.user;  
</select>

注意对于上面的起别名，xml需要全部更新
```

