####一.dependencyManagement:依赖管理  
	作用不是让所有的子项目依赖这里的配置，不是配置jar包的依赖关系，而是配置锁定jar包的版本号   

####二.classpath 和 classpath* 的区别
    classpath:加载本项目下的指定目录的配置文件
    classpath*:加载本项目下和本项目所依赖的所有项目的指定目录下的配置文件

####三.soa架构：也叫分布式架构，给互联网项目使用   
	
	互联网项目主要解决的三高问题：高并发、高性能、高可用
	
	定义：soa架构就是分布式架构，是一种设计，主要实现就是将传统项目中的一个模块拆分成一个一个项目；
	   这样可以降低模块间代码的耦合度，利于维护扩展，利于维护。
	
	优点：低耦合，利于扩展，利于维护
	缺点：结构复杂，对于小型项目成本比较高    

####四.dubbo
	1) 什么是dubbo:dubbo是阿里巴巴公司产生的一个rpc实现框架
	     dubbox是当当网维护的dubbo代码的版本
	2) dubbo作用：跨项目调用方法，例如从A项目中的controller调用B项目中的service方法
	3) dubbo同类型的技术有哪些：
	rpc协议实现框架：dubbo、dubbox、springCloud
	优点：传输效率快;
	缺点：controller 和 service 两个项目必须是java语言实现     

####五.idea 将项目打包成.war包，将来可以放到tomcat 中运行   
	1.方式1，直接用maven的package命令，可以在maven视图上点击项目的Lifecycle中的package 命令     
  
方式2.参考博客：[https://blog.csdn.net/zhang135687/article/details/84101524](https://blog.csdn.net/zhang135687/article/details/84101524)    

####六.linux 下安装zookeeper
	1) 解压zookeeper压缩包: sudo tar -zxvf xxxxxx.zookeeper.tar.gz -C ./
	2) copy配置文件:进入解压目录后，sudo cp conf/zoo_simple.cfg zoo.cfg
	3) 启动zookeeper:sudo ./bin/zkServer.sh start
	4) 查看启动状态:sudo ./bin/zkServer.sh status
	5) 关闭zookeeper:sudo ./bin/zkServer.sh stop

	注意：1. 原来从目前的最新版本3.5.5开始，带有bin名称的包    
			才是我们想要的下载可以直接使用的里面有编译后的二进制的包，而之前的普通的tar.gz的包   
			里面是只是源码的包无法直接使用    
 
		 2. zookeeper需要java运行时环境，需要先安装jdk，并配置环境变量，如果设置好了启动报错：not set JAVA_HOME.....,
		    这时可以尝试修改zkServer.sh 中手动添加export JAVA_HOME=jdk路径

####七.ubuntu下防火墙设置   
	1.查看防火墙状态    	sudu ufw status  
	2.开启防火墙       	sudo ufw enable   
	3.关闭防火墙		  	sudo ufw disable 
     
####八.mybatis 逆向工程   
	mybatis官方推出的逆向工程，用来连接数据库根据表结构生成pojo实体类，生成映射文件和接口文件，但只有单表的增删改查；  
	如果多表增删改查还是需要自己手动写。  
	注意：mybatis生成这些文件的方式是追加而不是覆盖，如果想重新生成需要将原来文件全部删除再重新运行。   

####九.在npm项目命令中安装插件
	npm install --save xxx  : npm 安装插件   
	--save 参数是默认将插件配置到package.json当中 
####十.jwt (json web token)     

####十一.docker 常用命令     
	docker run:创建并启动容器   
	-di:开后台进程运行容器    
	--name:指定容器名称（不应重复） 
	-p:指定端口映射  当前端口：容器端口	
	-e: 指定容器环境变量  	       

	docker rm 容器id/容器名	：根据容器id/容器名 删除容器	

	docker images ： 查看下载的所有镜像   
	docker search 镜像名 ： 去中央仓库搜索镜像   
	docker pull 镜像名 ：下载镜像

	docker ps： 查看当前启动的容器
	docker ps -a：查看所有创建的容器

	docker start 容器id/容器名称   ： 启动容器  
	docker restart 容器id/容器名称   ： 重启容器   
	docker stop  容器id/容器名称    ： 停止容器

	docker logs 容器id/容器名称 : 查看容器的log    

####十二.分布式id生成器  
	雪花算法（时间戳+机器id+...）    	   

####十三.SpringCache    spring自带的缓存框架
      

####十四.monogo 命令     

	1.选择和创建数据库 
	  use 库名  ：如果库名已经存在则直接使用，如果不存在则创建再使用
	  例如：use articledb  创建一个articledb数据库   
   
	
	2.插入文档：在当前使用的库中操作    
	  db.集合名.insert(json)  
	  例如：db.article.insert({name:"tom",age:NumberInt(18)});   

	3.查询
	  查询所有：db.集合名.find()      
	  例如：db.article.find(); 
	  
	  条件查询：db.集合名.find(json类型的查询条件)   
	  例如：查询文章访问数为1023的文档
	  db.article.find({visits:NumberInt(1023)})
		
	4.指定查询条数:返回查询结果的前n条文档
	  db.集合名.find(条件).limit(查询条数)   
	  例如：db.article.find().limit(2)     
	
	5.修改文档:修改整条记录
	  db.集合名.update(查询json,修改json);
	  例如：修改id为999的文档整体为{name:"满分作文"}
	  db.article.update({_id:"999"},{name:"满分作文"});   

	6.局部修改:只改变修改的字段，其他字段不变
	  db.集合名.update(查询json,{$set:修改json})
	  例如：修改id为999的文档name字段为"满分作文"  
	  db.article.update({_id:"999"},{$set:{name:"满分作文"}});  

	7.删除文档 
	  db.集合名.remove([查询json]) 条件为空时表示删除所有
 	  例如：删除id为999的文章
	  db.article.remove({_id:"999"}) 
 
	  删除所有article文档：db.article.remove({});   

	8.统计条数
	  db.集合名.count([查询json]) 条件为空时表示统计所有
	  例如：
	  查询所有数量：db.article.count({});   
	  查询文章访问数大于1000的数量：db.article.count({visits:{$gt:1000}});
	     
	9.模糊查询：本质是调用find方法，查询条件为正则表达式
	  db.集合名.find(带正则表达式的查询条件)   
	  例如：查询content属性中包含手机的文档  
	  db.article.find({content:/手机/});    

	  查询content属性以"完美"结尾的文档
	  db.article.find({content:/完美$/});   

	  查询content属性以"2020"开头的文档   
	  db.article.find({content:/^2020/});
	
	10.查询中的逻辑条件  
       大于：$gt   : db.article.find({visits:{$gt:1000}});
       大于等于：$gte : db.article.find({visits:{$gte:1000}});
	   小于：$lt : db.article.find({visits:{$lt:1000}});
       小于等于：$lte : db.article.find({visits:{$lte:1000}});
       不等于：$ne : db.article.find({visits:{$ne:1000}});
	
	   包含：$in : db.article.find({_id:{$in:[1000,2000]}});
       不包含：$nin : db.article.find({_id:{$nin:[1000,2000]}});   
	
	11.条件连接  
	   $and  : 交集条件   ： 
		例如 : 查询id在等于1或等于2并且visits大于1000的文档
		db.article.find({$and:[{_id:{$in:["1","2"]}},{visits:{&gt:1000}}]});
	   $or	 : 并集条件
		例如 : 查询id在等于1或等于2或者visits大于1000的文档
		db.article.find({$or:[{_id:{$in:["1","2"]}},{visits:{&gt:1000}}]});
	12.列值增长 
	   $inc
	   可以使某个文档的字段在每次查询的时候自增，是monoge的一个快捷功能
	   例如：db.article.update({_id:"1"},{${inc:{visits:NumberInt(1)}}});

	   

