####1.longzhu.platform.infrastructure.config.mysql下的配置类中 
	类注解 MapperScan（basePackages="longzhu.platform.data.mysql.xxxxx"）,这里在infrastructure引用了data模块的包名       

####2.GlobalConfigInit中的47行,其中给redis赋值相关写成了mysql????
	@Value("${consul.redis.prefix:conn/v2/mysql/}")
	private String redisPrefix;      

####3.

