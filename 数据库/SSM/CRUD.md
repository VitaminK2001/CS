# 新增商品
welcome-file-list在web.xml中自己设定
jsp出现的原因，在第一个欢迎页上点击button出现的结果
![[截屏2022-12-03 16.25.12.png]]
admin下的product.jsp 79行 -> 77行
href自链接，点击后跳转到77行对应的/admin/addproduct.jsp页面
![[截屏2022-12-03 16.27.50.png]]
123行，提交按钮点击后表示要执行form的action动作，也就对应49行的productsave.action动作
![[截屏2022-12-03 16.27.05.png]]
49行的/prod/save.action这个动作是有Controller分发的
![[Pasted image 20221203163446.png]]
在Controller包下找到对应的RequestMapping对应的/prod/save方法
![[Pasted image 20221203163540.png]]
save是service包中ProductInfoService接口定义的方法
在ProuctInfoServiceImpl中被实现
![[截屏2022-12-03 16.37.23.png]]
通过productInfoMapper对象的insert方法实现save方法
insert是ProductInfoMapper接口中定义的方法
在ProductInfoMapper.xml文件中被实现
这是被spring封装的mybatis负责的内容
![[Pasted image 20221203163932.png]]
![[Pasted image 20221203164245.png]]
一个UserMapper对应的接口被一个UserMapper.xml文件实现
这个步骤是在mybatis-condig.xml核心配置文件中做的
通过
```xml
<mappers>  
    <mapper resource="com/chico/dao/UserMapper.xml"/>  
</mappers>
```
注册

# 删除商品
product.jsp文件中第87行表单界面上的button“删除”的onclick
跳转到del方法
![[截屏2022-12-03 16.51.34.png]]
![[Pasted image 20221203165254.png]]
![[Pasted image 20221203165337.png]]
del方法中对应的url 先跳转到controller 处理用户的请求
![[Pasted image 20221203165410.png]]
ProductInfoServiceImpl.java