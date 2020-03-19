####一.Spring的容器对象
	1.BeanFactory是spring容器的定层接口
	2.接口ApplicationContext是BeanFactory的子接口； 
	  实现类有：
	  ClassPathXmlApplicationContext-->从类路径下读取配置文件 
	  FileSystemXmlApplicationContext-->从绝对路径指定配置文件读取 
	  AnnotationConfigApplication-->纯注解配置实用的类     

代码示例：

```java

	ClassPathXmlApplicationContext ac = new ClassPathXmlApplicationContext("beans.xml"); 
	Object userDao = ac.getBean("userDao");
```

	3.BeanFactory与ApplicationContext的区别：
	  BeanFactory创建容器对象时，只是加载了配置文件，没有创建对象，获取对象时，才创建对象。  
	  ApplicationContext:在创建容器对象时，只创建单例模式的对象，多例模式的对象在获取时才创建对象
	  
####二.ClassLoader的api加载配置文件为输入流 

	//从类路径下读取配置文件 
	InputStream in= Class.forName("").getClassLoader().getResourceAsStream("");

	//从绝对路径指定配置文件读取
	InputStream in= Class.forName("").getClassLoader().getSystemResourceAsStream("");  

####三.SpringIOC容器默认创建的bean对象是单例的 
	
	   singleton:单例 默认值  
	   prototype:多例的   

	   可以通过xml或者注解配置其scope属性指定  

####四.实例化bean的三种方式   
	1.Spring 容器常用的创建对象
	1.<bean id="userDao" class="com.itcast.UserDao">  
	
	2.静态工厂创建对象（getUserDao 是BeanFactory的一个静态方法，直接调用，不用创建工厂实例）
	<bean id="userDao" class="com.itcast.BeanFactory" factory-method="getUserDao">
	   
	3.实例工厂创建对象（getUserDao 是BeanFactory的成员方法，不能直接使用，需要先创建工厂实例）
	<bean id="beanFactory" class="com.itcast.BeanFactory"> 
	<bean id="userDao" factory-bean="beanFactory" factory-method="getUserDao">   

####五.IOC (控制反转)  
	IOC:控制反转包含 依赖注入 和 依赖查找  
	
	控制反转理解：
	传统创建对象是用new关键字来创建，是需要调用者主动创建，
	但是现在如果使用IOC框架的话，比如Spring，Spring框架会通过工厂方法帮你把对象好，  
	创建对象的工作不再是调用者，反转过来了，即创建对象的控制行为反转了。  
	
	依赖注入：Dependency Injection  
	简单理解：就是把我所需要的依赖对象直接注入进来，不需要再手动创建对象 
	业务层需要持久层，通过配置文件或者注解配置传入持久层对象，就是依赖注入。

####六.Spring 常用注解
	
	@Component：标记在类上，不能用在方法上

	 作用：创建对象，只要标记了，扫描到该包，对象就会创建
	
	 衍生的三个注解  
	 @Controller	:一般用于web(控制层)层上
	 @Service		:一般用于业务层
	 @Repository  	:一般用于持久层
	
	如果没有指定value的值，默认约定为简单类名首字母小写  
 
	例如：
	UserDaoImpl----->userDaoImpl  

	@Autowired  --自动注入
    
	可以标记在属性和set方法上，如果标记在属性上，可以没有set方法  
	特点：默认自动按照类型注入  
	流程：当属性、set方法上标记了@Autowired,会自动在容器中查询该属性类型的对象，  
	     如果有且只有一个，则注入。   
	
	@Qualifier：必须与@Autowired结合使用  
		作用：如果自动注入按照类型注入失败，则按照指定的名称注入；   
		如果没有@Qualifier,类型注入失败，则按照属性名按照名称注入  

	@Resource  --自动注入
	 流程：当属性、set方法标记了@Resource,会自动按照名称注入，如果名称没有找到，   
		   则根据类型注入，如果类型有多个，则抛出异常   
	
	总结：@Autowired:默认按照类型注入，如果类型有多个，则按照名@Qualifier配置的名称注入，   
					如果没有@Qualifier注解，则按照属性名称注入，如果没有找到，则抛出异常。  
				    如果没找到，则抛出异常	-- spring提供的  
	     @Resource:默认按照名称注入，如果名称没有找到，按照类型注入，    
		  			 如果类型有多个（一般同类型有多个实现类）则抛出异常。 --jdk提供的  

	@Configuration:比较该类为配置文件类  
	 		       可以替换applicationContext.xml    
	
	@ComponentScan("com.themelove")  
	 相当于<context:component-scan base-package="com.themelove"/> 
	
	@Import:引入其他配置文件类  
	 相当于xml中<import resource="classpath:applicationContext.xml"/>

	@Bean:通过方法创建对象，并放入到spring容器中
	 相当于xml中的<bean>标签    
	
	@Scope("singleton|prototype")  
	 配置对象的范围：相当于bean标签中的属性 scope="singleton|prototype"  
	
	生命周期 
	@PostConstruct:相当于bean标签的属性 init-method,构造方法执行之后执行   
	@PreDestroy:相当于bean标签的属性：destroy-method,对象销毁之前执行  
	
	@Value 给属性赋值  只能赋值简单类型    
	@PropertySource:引入外部属性文件 
	 相当于xml中的<context:property-placeholder location="classpath:jdbc.properties"/>   
	 之后再xml中直接用${jdbc.driver}进行引用    

####七.Spring与junit的整合     
	引入依赖
	1.引入依赖 
	  <dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<versioin>4.12</version>
	  </dependency>  
	
	<!--引入spring-5的测试包：必须引用相应的junit包，junit的版本必须是4.12以上-->  
	<!--引入spring-4的测试包：必须引用相应的junit包，junit的版本必须是4.9以上-->    

	  <dependency>
		 <groupId>org.springframework</groupId> 
		 <artifactId>spring-test</artifactId>  
		 <version>5.0.2.RELEASE</version> 
	  </dependency>    

	2.配置测试环境  
	  a.替换junit的运行器，为spring与junit整合后的运行器  
	    @RunWith(SpringJunit4ClassRunner.class)    

	  b.指定配置文件路径，会自动创建容器对象   
		xml方式
		@ContextConfiguration({"classpath:applicationContext.xml"})   
	    @ContextConfiguration(classes={SpringConfiguration.class})  

	  c.测试:
	    直接用@Autowired从容器中直接获取某类型的对象   

####八.Spring的核心包  
	
	<dependency> 
		<groupId>org.springframework</groupId> 
		<artifactId>spring-context</artifactId>
	    <version>5.0.2.RELEASE</version>  
	</dependency>   
	
	其中子依赖为  
	spring-beans : javabean 工厂相关
	spring-aop  : aop相关
	spring-core :spring核心包
	spring-expression  :表达式相关：比如aop定义切入点的表达式  
						* com.themelove.service.impl.*.*(..) 
	
	