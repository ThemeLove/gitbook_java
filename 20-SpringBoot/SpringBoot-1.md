###1.lombok
	 Lombok能通过注解的方式,在编译时自动为属性生成构造器、getter/setter、equals、hashcode、toString方法,通常用于javabean  
###2.spring-data-mongodb 
	Spring Data for MongoDB是Spring Data的一个子模块。 目标是为MongoDB提供一个相近的一致的基于Spring的编程模型 
	有写常用的注解
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
     被@PostConstruct修饰的方法会在服务器加载Servlet的时候运行，并且只会被服务器调用一次，类似于Serclet的inti()方法。   
	 被@PostConstruct修饰的方法会在构造函数之后，init()方法之前运行。

	2.@PreDestroy说明
     被@PreDestroy修饰的方法会在服务器卸载Servlet的时候运行，并且只会被服务器调用一次，类似于Servlet的destroy()方法。    
	 被@PreDestroy修饰的方法会在destroy()方法之后运行，在Servlet被彻底卸载之前   

