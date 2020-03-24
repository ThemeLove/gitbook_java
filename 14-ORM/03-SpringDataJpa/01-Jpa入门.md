####1.JPA(Java Persistence API)  
	定义：意即Java持久化API,是Sun官方在JDK5.0后提出的java持久化规范，这些接口在javax.persistence里定义。  
	JPA的出现主要是为了简化持久层开发以及整合ORM技术，结束Hibernate、TopLink、JDO等ORM框架各自为营的局面。  

	JPA包括以下3方面的技术： 
	ORM映射元数据：支持xml和注解两种元数据的形式，元数据描述对象和表之间的映射关系   
	API:操作实体对象来操作CRUD操作  
	查询语言：通过面向对象而非面向数据库的查询语言（JPQL）查询数据，避免程序的SQL语句紧密耦合      

	Jpa、Spring Data Jpa、 Hibernate 之间的关系： 
	总的来说JPA是ORM规范，Hibernate、TopLink、OpenJpa等是JPA规范的具体实现，  
	这样的好处是开发者可以面向JPA规范进行持久层的开发，而底层的实现则是可以切换的。  
	Spring Data Jpa则是在JPA之上添加另一层抽象（Repository层的实现），类似于Slf4j    

####2.SpringDataJpa 
	并不是jpa规范的实现，基于原生jpa的api进行再次的封装，提供更简单的使用方法，和更好的用户体验  
	一般底层的实现还是用选择Hibernate     

####3.jpa入门程序
	1.创建一个maven的java工程
	2.在resources/META-INF/persistence.xml创建jpa的配置文件，路劲有规定
	    常用配置：
	    persistence-unit:持久化单元，必须要有一个
	        name:持久化单元名称
	        transaction-type:事务类型
	            RESOURCE_LOCAL:但数据库事务
	            JTA:分布式事务，跨数据库事务，多数据库事务
	        property:
	            hibernate.show_sql:是否在控制台输出sql语句
	            hibernate.format_sql:是否格式化sql语句
	            hibernate.hbm2ddl.auto:是否自动创建表
	                create:自动创建表，先删除，再创建
	                update:自动创建表，如果表存在直接使用，不存在就创建
	                none:不自动创建表，如果表存在直接使用，如果不存在则报错
	3.创建一个Entity类，对应数据库表的字段
	  类上添加:
	  @Entity
	  @Table(name ="数据库表名")
	
	  属性上添加
	  @Id :指明主键对应的字段
	  @GeneratedValue:指明主键的生成策略，如果手动生成主键，可以没有此注解；如果主键需要框架生成需要配置此注解
	        GenerationType.IDENTITY:自增长主键，推荐在mysql下使用，不能在Oracle使用
	        GenerationType.SEQUENCE:使用序列生成主键，一般在Oracle下使用，不推荐在mysql使用
	        GenerationType.AUTO:有框架自动选择
	  @SequenceGenerator:序列生成器，Oracle下使用多
	  @TableGenerator:表生成器，会新建一张表来专门生成主键id
	
	4.编写测试程序，实现数据添加
	    1）创建一个EntityManagerFactory对象，单例
	    2）通过EntityManagerFactory获取EntityManager
	    3）EntityManager获得事务并开启
	    4）构造数据对象
	    5）使用EntityManager对象的persist方法向数据库插入数据
	    6）事务提交
	    7）关闭连接  

	
		增加：entityManager.persist();  
		删除：entityManager.remove();
		修改：entityManager.merge(); 

		根据id查询：
			 entityManager.find();//即时加载
			 entityManager.getReference();//延迟加载，用到的时候再加载
	
####4.jpql 

	jpql相当于sql语句的变种，只是把表名换成实体类的类名，字段名换成实体类的属性名 

	例如： 
	查询全部：
		sql: select * from customer  
		jpql:from Customer
	使用方法：
		1）：创建一个EntityManager对象 
		2）：使用entityManager创建一个Query对象 
		3）：使用Query查询  
		4）：关闭连接   

```java

	1.查询全部 
	  sql:select * from customer; 
	  jpql:from Customer  

	2.使用jpql分页  
	  使用Query对象的方法设置分页信息  
	  起始行号：query.setFirstResult 
	  每页的条数：query.setMaxResult   

	  Query query = entityManager.createQuery("from Customer"); 
	  query.setFirstResult(5);//设置起始行号
	  query.setMaxResult(5);//设置每一页查询个数
	  List<Customer> customerList = query.getResultList();  

	  jpa底层会根据数据库类型，自动转换成相应的sql语句

	3.带条件的查询 
	  sql:select * from customer where id = ?  
	  jpql:from Customer where id=? 

      query.setParameter(jpql中第几个参数的，对应的值)  
	  例如：query.setParameter(1,2L)

	4.查询带排序  
	  sql:select * from customer order by id desc  
      jpql:from Customer order by id desc 

	5.聚合查询:count sum max min avg
	  sql:select count(*) from customer  
	  jpql:select count(*) from Customer  

	总结：Query对象的常用方法
		  Query query = entityManager.createQuery("from Customer"); 
	  	  query.setFirstResult(5);//设置起始行号
	  	  query.setMaxResult(5);//设置每一页查询个数
		  query.setParameter(jpql中第几个参数的，对应的值)
		  query.getResultList 	 //获得结果集
		  query.getSingleResult  //获得单个结果
	  
```  

####5.


	
   

  