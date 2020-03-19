####一.AOP(面向切面)  
	spring中的aop底层是jdk和cglib的动态代理实现，结合注解使用  


####二.xml中开启aop自动代理注解 
	
	<aop:aspect-autoproxy/>     

	 

####三.AOP 中的概念   
	
####四.数据源持久层技术  
	jdbc----->dbutils---->jdbcTemplate(Spring提供)----->ibatis----->mybatis(主流)----->spring data jpa(趋势)   

####五.数据源  
	c3p0
	dbcp 
	spring jdbc 自带数据源  
	druid 阿里数据源  

####六.各个数据源xml配置

```

	a. c3p0数据源
			<!--c3p0数据源-->
	        <dependency>
	            <groupId>c3p0</groupId>
	            <artifactId>c3p0</artifactId>
	            <version>0.9.1.2</version>
	        </dependency>
		 <!--引入外部属性文件-->
	    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
	    <!--配置c3p0数据源-->
	    <bean id="c3p0DataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
	        <property name="driverClass" value="${jdbc.driver}"></property>
	        <property name="jdbcUrl" value="${jdbc.url}"></property>
	        <property name="user" value="${jdbc.user}"></property>
	        <property name="password" value="${jdbc.password}"></property>
	    </bean>
	b. dbcp 数据源
		<!--dbcp数据源-->
	        <dependency>
	            <groupId>commons-dbcp</groupId>
	            <artifactId>commons-dbcp</artifactId>
	            <version>1.4</version>
	        </dependency>
			<!--dbcp数据源-->
	    <bean id="dbcpDataSource"  class="org.apache.commons.dbcp.BasicDataSource">
	        <property name="driverClassName" value="${jdbc.driver}"></property>
	        <property name="url" value="${jdbc.url}"></property>
	        <property name="username" value="${jdbc.user}"></property>
	        <property name="password" value="${jdbc.password}"></property>
	    </bean>
	c. spring jdbc 自带数据源，包含了JdbcTemplate对象
		<!--spring自带数据源-->
	        <dependency>
	            <groupId>org.springframework</groupId>
	            <artifactId>spring-jdbc</artifactId>
	            <version>5.0.2.RELEASE</version>
	        </dependency>
	d. <!--spring自带的数据源-->
	    <bean id="springDataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
	        <property name="driverClassName" value="${jdbc.driver}"></property>
	        <property name="url" value="${jdbc.url}"></property>
	        <property name="username" value="${jdbc.user}"></property>
	        <property name="password" value="${jdbc.password}"></property>
	    </bean>
```   


####六.事务管理的方式  
	1.编程式事务管理： 在业务层中写了很多事务技术代码。  
	  优点：控制的更精准；缺点：大量重复代码
	  比如先获取数据库连接，手动开启事务代码等，connection.setAutoCommit(false)  
	  begin、commit、rollback等
	  
	2.声明式事务管理：xml 事务管理器配置、注解事物管理器配置。
	优点：解耦，aop实现无侵入业务层代码；缺点：不够精准
####六.
	问题：
	一个数据库事务必须用同一个connection对象吗？ 
	动态代理怎么和aop结合的？
	jdk动态代理和cglib动态代理   
    dbutils、c3p0、dbcp、spring_temp数据源xml方式配置   

	事物的隔离级别   

	事物的传播  