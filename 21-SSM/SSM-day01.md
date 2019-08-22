####1.搭建项目 
####2.配置pom.xml
####3.配置      
####4.IDEA中服务器配置  
	1.IDEA只是一个开发工具,并没有自带tomcat
	2.springboot框架中集成了tomcat  
	3.如果使用idea工具，springboot框架开发的话，不需要配置tomcat 
	4.如果使用idea工具，SSM(Spring+SpringMVC+Mybatis)框架开发的话，则需要配置tomcat    
####5.Tomcat
	tomcat是web应用服务器，是一个Servlet/JSP容器。Tomcat作为servlet容器，负责处理客户请求，  
	把请求传送给servlet，并将servlet的响应传送回给客户。而servlet是一种运行在支持Java语言的服务器上的组件      

####6.window下启动tomcat时，控制台乱码 
	解决：修改tomcat安装目录/conf/logging.properties 文件中
		 java.util.logging.ConsoleHandler.encoding = GBK ，重启服务器即可   

####7.创建产品表（product）  
	create table product(
		`id` int unsigned primary key auto_increment,
		`productNum` varchar(50) not null unique,
		`productName` varchar(50) not null,
		`cityName` varchar(50) default null,
		`departureTime` varchar(50) default null,
		`productPrice` numeric(8,2) default null,
		`productDesc` varchar(500) default null,
		`productStatus` tinyint default 1  
	);

####8.插入数据 
	insert into product values(null,'1001','北京城一日游','廊坊','2019-07-01',100.00,'感受京城之美',1);  
	insert into product values(null,'1002','上海一日游','苏州','2019-07-02',300.0,'探秘魔都',1);    

####9.创建订单表  
	 create table orderInfo (
	  `id` int unsigned primary key auto_increment,
	  `orderNum` varchar(50) not null unique,
	  `orderTime` varchar(50) not null,
	  `orderDesc` varchar(500),
	  `orderStatus` int unsigned,
	  `payType` int unsigned,
	  `peopleCount` int unsigned,
	  `productId` int unsigned,
	  foreign key (`productId`) references product(`id`)
	 );  
####10.插入数据  
	insert into orderInfo values(null,'A1000000','2019-07-01','orderDesc',1,1,5,1);