namespace中的报名要和Dao / mapper包下的接口名一致
```xml
<mapper namespace="com.chico.dao.UserMapper"> 这里是UserMapper
<!--    select查询语言的id对应方法名-->  
<!--    返回结果要写全路径名-->  
    <select id = "getUserList" resultType="com.chico.pojos.User">  
            select * from mybatis.user;  
    </select>  
</mapper>
```
![[截屏2022-11-30 21.42.17.png]]
id 是UserMapper接口中的方法名
resultType是返回值
parameterType是参数的类型
根据ID查询用户
若方法User getUserById(int id)，则在xml文件中：
![[截屏2022-11-30 21.45.50.png]]

首先编写UserMapper接口中的方法
```java
public interface UserMapper {  
    List<User> getUserList();  
    int addUser(User user);  
    int updateUser(User user);  
}
```
编写UserMapper.xml文件实现UserMapper接口
```xml
<insert id="addUser" parameterType="com.chico.pojos.User">  
    insert into mybatis.user (id, name, pwd) values (#{id}, #{name}, #{pwd});  
</insert>  
  
<update id="updateUser" parameterType="com.chico.pojos.User">  
    update mybatis.user set id = #{id}, pwd = #{pwd} where name = #{name};  
</update>
```
最后编写测试类中的方法
```java
 @Test  
    public void addUser(){  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);  
        int res = userMapper.addUser(new User(3, "Eason", "123455"));  
        if(res > 0) System.out.println("插入成功");  
  
//        提交事务  
        sqlSession.commit(); //如果没有提交事务依然会显示插入成功，但是实际上没有插入  
        sqlSession.close();  
    }  
    @Test  
    public void updateUser(){  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);  
        int res = userMapper.updateUser(new User(1, "chico", "123"));  
        sqlSession.commit();  
        sqlSession.close();  
    }
```
注意点
增删改需要提交事务
`sqlSession.commit();  `

注意parameterType是可以跟别的类型的，但是同样需要用双引号括起来
```xml
<delete id="deleteUser" parameterType="int">  
    delete from mybatis.user where id = #{id}  
</delete>
```