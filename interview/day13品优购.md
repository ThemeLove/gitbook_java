####一.pom.xml dependency中<scope>属性的配置   
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <scope>provided</scope>
        </dependency>
	
	注意：
	1.<scope>compiled</scope>:
		1）：该项目在编译、测试、运行阶段都需要这个artifact对应的jar包在classpath中，	
			也就是说当把项目打包成jar包或war包时运行到tomcat中时，该包也会被编译包含在内。  
		2）：<scope>compiled</scope>是默认级别，不配置<scope>时默认为该级别   

	2.<scope>provided</provided>:    
		1）：该项目在编译、测试阶段都需要这个artifact对应的jar包在classpath中，   
			 也就是说当把项目打包成jar包或war包时运行到tomcat中时，该包不会被编译包含在内，  
			 一般容器中有相同的jar包时需要配置为provided，否则编译后的jar或者war项目会因为jar包冲突，导致项目运行失败。   
			 比如tomcat中已经包含了servlet-api的jar包，所以需要配置为provided属性。

	 
####二.ServerContextAware接口  
	   aware是感知的意思
	   在Spring中，凡是实现ServletContextAware接口的类，都可以取得ServletContext    

####三.消息中间件  JMS(Java Manager Service)
  	jms的全称是java manager service (Java 消息服务) jms 是jdk底层定义的规范，   
	各大厂商来实现该规范。类似于jpa、jndi 等底层规范。    
	
	作用：在soa分布式架构系统中，或者企业中的多个项目中，进行多个系统异步传递消息   

	使用场景：
	


