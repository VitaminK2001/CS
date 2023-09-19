在java执行的时候 传递通配符 
```java
List<User> userList = mapper.getUserLike("%李%");
```

在sql拼接的时候，传递通配符 （在xml文件中）
```xml
<select id = "getUserLike" parameterType = "com.chico.pojos.User">
	select * from User where name like "%"#{value}"%";
</select>
```

