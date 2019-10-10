####1.redis 苏宁passport等登录注册url、是否支持代理等都配置在苏宁scm系统里，通过jar包来获取
####2.@Transactional  
	1.该注解加在方法上，只要方法内部发生RuntimeException异常，就会回滚，数据库里面的数据也会回滚  
	2.@Transactional(rollbackFor = Exception.class),该注解加在方法上，遇到非运行时异常也回滚  

####3.登录注册流程  
	1.收集用户信息调用passport注册接口  
	
####4.登录注册相关javaBean  
	UserInfoDTO:登录注册对外用户信息接口 
	UserInfoEntity:随见用户数据库对应javaBean:对应表qry_user_info

####5.苏宁自研jpa框架 
	1.@ShardMapping(shardRef = "spray-router-username") 的用法

	2.redis.clients.util.Hashing方法
	 Hashing.MURMUR_HASH.hash(value) 方法   hash一致性算法   
	
	3.ehcache是配置缓存框架  
	
	4.