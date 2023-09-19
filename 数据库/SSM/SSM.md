## Broswer and Tomcat
Tomcat是web容器，servlet是web组件，组件放在容器中
通过Tomcat的WEB-INFO目录下的web.xml进行web容器注册
web容器中有三个组件 Servlet、Jsp、JavaBean
>javaBean是从servlet中抽离出来的，为了减少Servlet代码冗余
>Tomcat不需要识别javaBean，只有Servlet和JavaBean打交道
>Tomcat 识别的是Jsp和Servlet
>web.xml 文件中容器注册只有servlet，jsp自注册

Browser --> Servlet (RequestMapping)

## servlet和JavaBean解耦
servlet 调用 javabean接口（一个和数据库连接的工具）
>不同类的javabean被调用时候，Servlet不需知道调用谁，只需一个factory。通过Factory.getModelInstance() 方法，屏蔽返回的是m1 还是 m2。

但是通过工厂，对象数量变多了
>servlet <--> javabean (1 : 1)
>servlet <--> factory <--> javabean (1 : 1 : 1)

而Factory和业务无关，可以抽离
> 由spring 提供工厂 factory
> 出口的分发和入口的分发，都被spring dispatch servlet接管
> 出口：servlet -> jsp *(路由表分发)*
> 入口: browser -> servlet *(浏览器查询)*
> dispatch servlet是基类，为的是减少重复代码量
> servlet是继承dispatch servlet的子类

# 关于项目框架
[[项目框架]]

# mybatis框架配置文件
## SqlMapConfig.xml配置文件

SqlMapConfig.xml在mybatis中是核心配置文件
在Spring框架中也有
但是在Spring框架中不需要像mybatis一样写很多
基本上该有的配置都被spring框架整合了，包括jdbc.properties文件的引用和数据库连接池等等。
所以这里只写了一个分页插件的配置
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-config.dtd">  
<configuration>  
    <settings>        
    <setting name="logImpl" value="STDOUT_LOGGING"/>  
    </settings>    <!--    分页插件的配置-->  
<!--    mybatis插件都被封装到SpringMVC和Spring中-->  
<!--    只用配置一个分页插件-->  
    <plugins>  
        <plugin interceptor="com.github.pagehelper.PageInterceptor">
        </plugin>  
    </plugins>  
</configuration>
```

# 关于mybaits
MyBatis的持久化解决方案是将用户从原始的JDBC访问中解放出来,用户只需要定义需要操作的SQL语句,无须关注底层的JDBC操作,就可以以面向对象的方式来进行持久化层操作.底层数据库连接的获取,数据访问的实现,事务控制等都无须用户关心,从而将应用层从底层的JDBC/JTA API抽取出来.通过配置文件管理JDBC连接,让MyBatis解决持久化的实现.在MyBatis中的常见对象有SqlSessionFactory和SqlSession.
类比
SqlSession ==> Connection 
SqlSession对象完全包含以数据库为背景的所有执行SQL操作的方法,它的底层封装了JDBC连接,可以用SqlSession实例来直接执行被映射的SQL语句.
SqlSessionFactory是创建SqlSession的工厂.


# Spring框架配置文件
## applicationContext_dao.xml

spring接管了mybatis后的配置信息
1. jdbc.properties 属性文件的读取
2. 创建数据源
	1. 需要使用数据库连接池
	2. 传统的jdbc需要频繁地建立与数据库的连接 , 使用完毕之后需要断开连接(释放资源) , 会消耗大量的时间以及资源
	3. 数据源建立多个数据库连接, 这些连接保存在连接池中 , 当应用需要连接数据库时, 直接从池中获取空闲的连接对象 , 使用完毕之后, 将连接放回池中
3. 创建Mybatis中的SqlSessionFactory
4. 创建Mapper文件的扫描器
	1. Mapper扫描器的作用就是自动的根据定义的Mapper接口，让Mybatis创建其实现类

## 登录业务逻辑实现 Admin & AdminExample

@service后面跟着的是类
表示在spring启动的时候创建对象

@autowired后面跟着的是对象
表示spring会完成对象的实例化，这样达到解耦合的目的

根据传入的用户名到DB中**查询**对象
若查询到对象，在**进行密码的比对**

AdminExample是查询条件的封装类，如果查询语句带条件，则一定要创建adminExample对象用来封装条件
方法andANameEqualTo()完成的是条件的拼接

AdminMapper.xml完成数据库表的列名和java类中成员变量名的映射

ProductInfoMapper.xml完成sql语句和sqlid的映射

注意在进行密码对比是要进行将传入的密码进行md5加密再和数据库中的密文对比`String miPwd = MD5Util.getMD5(pwd)`


### controller和service包的开发

界面层的开发都在controller包中
业务逻辑的开发都在service包中
开发的时候是先开发业务逻辑，然后开发界面
在界面层有一个@RequestMapping表示页面发出的请求会跳转到业务逻辑的实现

#### 实现登录判断并进行相应的跳转
接口
	controller.AdminAction

@controller 表示交给spring创建controller对象

切记在所有的界面层，一定会有业务逻辑层的对象
这里用AdminService表示业务逻辑层，同样是Spring注入（IOC）

##### @RequestMapping
是将用户在浏览器URL中输入的请求，跳转到对应的*.action.jsp文件
也就是通俗意义上的**打注解**
而且存在有多层RequestMapping的嵌套，例：/prod/update表示对商品进行更新，对应在update.jsp中就是

```jsp
action="${pageContext.request.contextPath}/prod/update.action"
```

##### public String login(String name, String pwd)
调用adminservice的login方法
如果admin不为空说明登录成功
else 登录失败 

登录成功
	return "main"表示登录如果成功会跳转到main.jsp的页面上
	在springmvc.xml文件中有一个视图解析器
	它会拼接路径的前缀和后缀，实现跳转的功能

登录失败
	return "login"

#### 商品界面不分页 业务逻辑层开发
接口
	service.ProductInfoService
实现类
	service.ProductInfoServiceImpl
同样，切记业务逻辑层中一定有数据访问层的对象
	 这里是spring注入的productInfoManager
 
 因为这里是空的查询
 所以只需要new 一个ProductiInfoExample即可

#### 商品界面层不分页 控制器开发
controller.ProductInfoAction
切记
	在界面层中一定有业务逻辑层的对象
spring自动注入
	productInfoService

#### 商品界面分页 业务逻辑层开发
##### 接口：service.productInfoService
首先用pageHelper设置当前页和每一页的记录个数

pageInfo splitPage(int pageNum, int pageSize)
	分页插件使用pageHelper工具类

注意展示的时候按照Pid降序排序
	这样好处是新插入的商品会显示在第一列
	不用翻到最后一页才能找到

设置完排序后取集合

将查到的集合封装到PageInfo对象中
下面是pageInfo对象所在的类
![[截屏2022-11-27 21.19.47.png]]
pageinfo中所有的成员变量都会被赋值


#### 商品界面分页 控制器开发
controller.ProductInfoAction

在product.jsp中有翻页的设置
第127行
```jsp
<a href="javascript:ajaxsplit(${info.prePage})" aria-label="Previous">
```

a表示的是超链接
```jsp
<a href="javascript:ajaxsplit(${info.nextPage})" aria-label="Next">  
    <span aria-hidden="true">»</span></a>
```
表示如果有点击的动作ajax会发出跳转页面的请求，先调用业务逻辑层，再调用数据访问层，获取后一页的所有数据，返回到名称为Next的页面做显示

页码的显示通过遍历实现

#### ajax提交分页请求

#### 新增商品功能分析
图片上传到服务器，上传的图片添加到当前商品的图片的属性中，然后将这个信息写到数据库中去（服务器端的地址）

##### 商品类别信息的添加
###### 商品图片的上传
当前数据库商品表中type_id是外键引用了类型表的主键
接口
	service.ProductTypeService
	listener.ProductTypeListener

异步ajax不但完成图片的上传到服务器，而且会回显

###### 商品的添加

#### 商品的更新
当点击编辑按钮后
	将商品的信息显示在当前的页面上
	update.jsp作用是完成如何回显
所以需要return “update”，表示回传到update页面做展示

回显的内容中
	一开始就有一个被选中的类别是因为
	遍历了整张类别表，其中和当前商品属性相等的类别会被选中![[截屏2022-11-28 10.13.16.png]]
	

#### 单个商品的删除
删除的时候必须要弹窗提示
	同样弹窗写在`product.jsp`中

在service.ProductInfoServiceImpl中实现delete方法
因为在product.jsp中有
`url: "${pageContext.request.contextPath}/prod/delete.action"`
所以RequestMapping("/delete")

#### 批量删除
##### 全选按钮的实现
##### 底层mapper
ProductInfoMapper.xml
```jsp
delete from product_info where p_id in  
<foreach collection="array" item="pid" separator="," open="(" close=")">  
    #{pid}  
</foreach>
```

##### 业务逻辑层的实现（service）
1. ProductInfo接口中一个deleteBatch()
2. 在ProductInfoImpl中实现deleteBatch()

##### 界面控制实现（controller）
@RequestMapping 
	对应ProductInfoMapper.xml中的sql语句
	也对应客户在url中输入的请求

#### 多条件查询功能
创建封装查询条件的类
	页面传送条件到后台去，一般会将这种放在vo包中
ProductInfoMapper 多条件查询接口开发
	Mapper->Mapper.xml->service->controller
ProductInfoMapper.xml将实体类中的成员变量和数据库中的列名有一个绑定，这个绑定的名称叫做ResultMap，所以我们查询结果的返回值是ResultMap


