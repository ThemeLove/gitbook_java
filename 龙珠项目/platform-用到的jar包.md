####1.guawa: 
	google提供的一个功能非常齐全的工具类jar，里面包含对字符串、集合、数组等的快捷api   

####2.consul    
	可以用来做服务注册和发现
	1. java客户端 有orbitz 和 ecwid  可以选择
	   以orbitz 客户端为例   
	   添加依赖   
	   <dependency>
            <groupId>com.orbitz.consul</groupId>
            <artifactId>consul-client</artifactId>
            <version>1.2.6</version>
        </dependency>
	   
	   常用代码：
	   Consul client =  Consul.builder().withHostAndPort(HostAndPort.fromParts(host, port)).build()  
	   HealthClient healthClient = client.healthClient();
	   KeyValueClient keyValueClient = client.keyValueClient();  
	   List<String> valuesAsString = keyValueClient.getValuesAsString(String key)
####3.fastjson  阿里巴巴出品的json解析框架  
	  JSON.parseObject(String jsonStr,Class clazz)
  
####4.ElasticSearch  
	是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎
