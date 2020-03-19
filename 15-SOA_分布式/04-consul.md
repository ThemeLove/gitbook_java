####1. 什么是consul?  

	Consul 是 HashiCorp 公司推出的开源产品，用于实现分布式系统的服务发现、服务隔离、服务配置，  
	这些功能中的每一个都可以根据需要单独使用，也可以同时使用所有功能。  
	Consul 官网目前主要推 Consul 在服务网格中的使用。
	
	与其它分布式服务注册与发现的方案相比，Consul 的方案更“一站式”——内置了服务注册与发现框架、 
	分布一致性协议实现、健康检查、Key/Value 存储、多数据中心方案，不再需要依赖其它工具。 
	Consul 本身使用 go 语言开发，具有跨平台、运行高效等特点，也非常方便和 Docker 配合使用  

####2. 常见注册中心   

	在 Spring Cloud 体系中，几乎每个角色都会有两个以上的产品提供选择，比如在注册中心有： 
	Eureka、Consul、zookeeper、etcd 、nacos （阿里）等；网关的产品有 Zuul、Spring Cloud Gateway 等。  
	在注册中心产品中，最常使用的是 Eureka 和 Consul，两者各有特点，企业可以根据自述项目情况来选择  

####3. 龙珠直播中注册中心选择的是consul  
	
龙珠项目里使用步骤： 

	1.Java 后端选择的是将mysql、redis、kafka、mongoDB、grpc等配置节点信息配置在consul 里，  
	  然后将consul的ip和端口号通过环境变量配置在web容器里  
	2.项目启动通过指定key获取要连接的consul节点的信息，拿到consul 节点信息之后，再通过consul 拿到  
	  mysql、redis、kafka、mongoDB、grpc等配置节点信息，初始化项目基本组件             