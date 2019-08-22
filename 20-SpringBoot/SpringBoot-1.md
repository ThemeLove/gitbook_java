###1.lombok
	 1.Lombok能通过注解的方式,在编译时自动为属性生成构造器、getter/setter、equals、hashcode、toString方法,通常用于javabean  
	 2.需要在IDEA中单独安装插件，不然代码中报错
###2.spring-data-mongodb 
	Spring Data for MongoDB是Spring Data的一个子模块。 目标是为MongoDB提供一个相近的一致的基于Spring的编程模型 
	有些常用的注解
	@Document：把一个java类声明为mongodb的文档，可以通过collection参数指定这个类对应的文档
	@Indexed:声明该字段需要索引，建索引可以大大的提高查询效率。
	@CompoundIndex:复合索引的声明，建复合索引可以有效地提高多字段的查询效率。
	@Field:给映射存储到 mongodb 的字段取别名
	@Id:文档的唯一标识，在mongodb中为ObjectId，它是唯一的
	@Transient:默认情况下所有的私有字段都映射到文档，该注解标识的字段从存储在数据库中的字段列中排除（即该字段不保存到 mongodb）  

###3.ResponseBodyAdvice 
	实现ResponseBodyAdvice拦截Controller方法默认返回参数，统一处理返回值/响应体   

###4.Spring相关工具类  
	StringUtils			:字符串相关工具类    

###5.@PostConstruct 和 @PreDestory注解  
	从Java EE5规范开始，Servlet增加了两个影响Servlet生命周期的注解（Annotation）：@PostConstruct和@PreConstruct。    
	这两个注解被用来修饰一个非静态的void()方法.而且这个方法不能有抛出异常声明。  

	1.@PostConstruct说明
     被@PostConstruct修饰的方法会在服务器加载Servlet的时候运行，并且只会被服务器调用一次，类似于Serclet的init()方法。   
	 被@PostConstruct修饰的方法会在构造函数之后，init()方法之前运行。

	2.@PreDestroy说明
     被@PreDestroy修饰的方法会在服务器卸载Servlet的时候运行，并且只会被服务器调用一次，类似于Servlet的destroy()方法。    
	 被@PreDestroy修饰的方法会在destroy()方法之后运行，在Servlet被彻底卸载之前     
####6.项目中获取配置文件读取流的方法  
	1.jdk中的方法
	InputStream resourceAsStream = getClass().getClassLoader().getResourceAsStream("SqlMapperConfig.xml");   
	2.myBatis中的  
    InputStream inputStream = Resources.getResourceAsStream("SqlMapperConfig.xml");    

####7.mybatis中的配置文件开头一定要声明mybatis dtd 约束，否则mybatis框架解析配置文件失败  
	1.config 约束    
例如：

```xml

	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE configuration
	        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
	        "http://mybatis.org/dtd/mybatis-3-config.dtd">
	<configuration>
	    <environments default="development">
	        <environment id="development">
	            <transactionManager type="JDBC"/>
	            <dataSource type="POOLED">
	                <property name="driver" value="com.mysql.jdbc.Driver"/>
	                <property name="url" value="jdbc:mysql://10.200.202.158:3306/mybatis?characterEncoding=utf-8"/>
	                <property name="username" value="root"/>
	                <property name="password" value="root"/>
	            </dataSource>
	        </environment>
	    </environments>
	
	    <mappers>
	        <mapper resource="UserMapper.xml"/>
	    </mappers>
	
	</configuration>

```
	2.mapper约束   
例如：

```xml

	<?xml version="1.0" encoding="utf-8" ?>
	<!DOCTYPE mapper
	        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	<mapper namespace="UserMapper">
	    <select id ="findAllUsers" resultType="com.mybatis.domain.User">
	        select * from user;
	    </select>
	</mapper>
```

####8.打包工程成jar  
	使用idea自带的打包和安装功能将当前工程打成jar包安装到本地Maven仓库，只需要双击install 命令即可  


