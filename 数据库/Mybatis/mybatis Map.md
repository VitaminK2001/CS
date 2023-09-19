UserMapper.xml文件中如果参数类型prameterType后面跟的事User
则会存在一个问题
`#{}`中的名字需要和User类的属性名一一对应
而且`#{}`参数的数量要和实体类的属性数量一致
假设我们实体类，或者数据库中的表参数过多，我们应当考虑用map

为了避免这个问题
可以将参数类型设置为**一个**数据类型，比如int，传递的参数名就是#{id}，可以将多个参数类型改为map，这个map是自定义的

```xml
<delete id="deleteUser" parameterType="int">  
    delete from mybatis.user where id = #{userid};  
</delete> 如果只有一个参数的话，上忙都可以不写
#{}中的参数名都可以随便写 因为是直接获取到的
```
```java
public void deleteUser(){  
    SqlSession sqlSession = MybatisUtils.getSqlSession();  
    UserMapper userMapper = sqlSession.getMapper(UserMapper.class);  
    int res = userMapper.deleteUser(3);  
    sqlSession.commit();  
    sqlSession.close();  
}
```

```xml
<insert id = "addUser2" parameterType="map">  
    insert into mybatis.user(id, name, pwd) values (#{userid}, #{username}, #{password});  
</insert>
其中的#{}中的参数名字是和map的键值对应的
```
```java
public void addUser2(){  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
//        注意map的类型是前面是String后面是Object因为String是map的key值  
        Map<String, Object> map = new HashMap<>();  
        map.put("username", "eason");  
        map.put("userid", 5);  
        map.put("password", 123);  
        mapper.addUser2(map);  
        sqlSession.commit();  
        sqlSession.close();  
}
```


