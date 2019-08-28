####一.redis 是内存型缓存数据库，属于nosql  
####二.redis 官网默认只提供linux 版本，window版本可以通过微软github上下载 
	  windows版链接：https://github.com/microsoftarchive/redis/releases  

####三.windows 版本redis 常用命令  
window 版redis 解压即用，截图如下：
 
![](/16-redis/images/redis_dir.png)    

	1.直接命名行进入redis解压目录: 
	启动redis：
	redis-server.exe redis.windows.conf
  
	2.添加到window系统服务中，然后启动
	安装服务，添加到window的系统服务中，使其开机自启动：
	redis-server --service-install redis.windows.conf   

	卸载服务，从window系统服务中移除：
	redis-server --service-uninstall 

	启动服务： 
	redis-server --service-start 
	
	停止服务： 
	redis-server --service-stop    

	3.命令行客户端 
	双击redis-cli.exe 可以进入redis命令行客户端    


####四.redis可存储的五种数据类型  
	String  	字符串
	Hash 		哈希
	List  		字符串列表 （类似java中的LinkedList）
	Set 		字符串集合
	SoredSet 	有序字符串集合 （类似java中的TreeSet）

####五.redis 命令行下五种数据类型的存、取、删除 
	1.String 类型 
	存：set key value 
	例：set name zhangsan  

	取：get key  
	例： get name      

	删除：del key 
	例：del user

	如果没有key,则返回nil,java代码中实现为null 
![](/16-redis/images/redis_string.png)       

	2.List 类型  元素允许重复
	存：lpush key value1 value2 ....  
	例：lpush mylist a b c d e 
	
	范围取：lrange key startIndex endIndex  
	特别：lrange key 0 -1 表示取全部
	例：lrange mylist 0 1
	
	指定索引取：lindex key index  
	例：lindex mylist 3  
![](/16-redis/images/redis_list_lpush_lrange_lindex.png)     

	从头部弹出：lpop key  
	例：lpop mylist 
	
	从尾部弹出：rpop key 
	例如：rpop mylist  
![](/16-redis/images/redis_list_lpop_rpop.png)   

	3.Hash 类型  
	存：hset key name value 
	例：hset myhash user wangwu 

	取指定key指定键：hget key name 
	例：hget myhash user 

	取指定key的所有键值：hgetall key 
	例：hgetall myhash 

	删除指定key指定键：hdel key name 
	例：hdel myhash age
![](/16-redis/images/redis_hash_set_get_del.png)    

	4.Set 类型 元素不允许重复
	存：sadd key value1 value2.... 
	例：sadd myset a b c d e 

	取所有：smembers key 
	例：smembers myset 

	删除指定key中指定元素：srem key name 
	例：srem myset e
![](/16-redis/images/redis_set_add_members_rem.png)    
  
	5.SoredSet 类型 有序集合，不允许重复  
	存：zadd key score1 value1 score2 value2....
	例：zadd mysort 1 zhangsan 2 lisi 3 wangwu 
	
	范围取：zrange key startIndex endIndex 
	特别：zrange key 0 -1 表示取全部 
	例：zrange mysort 0 3 
	   zrange mysort 0 -1   

	删除：zrange key value  
	例如：zrange mysort wangwu
![](/16-redis/images/redis_sortedset_add_range_rem.png)    

	
####六.redis 其他命令  
	 1.自增1： incr key  
	   例如： incr num  
	
	 2.自减1：decr key 
	   例如： decr num  
![](/16-redis/images/incr_decr.png)  	  

	 3.自增步数：incrby key step
	   例如：	incrby num 5  
	 4.自减步数：decrby key step
	   例如:		decrby num 5
![](/16-redis/images/incrby_decrby.png)     
	
	 5.查看所有键：keys * 
	   例如：keys *  
	
	 6.判断key是否存在,1代表存在，0代表不存在  exists key	 
	   例如：exist mylist  
	
	 7.删除指定key : del key  
	   例如：del user  
	
	 8.判断key的类型： type key 
	   例如：type num   
![](/16-redis/images/redis_common_command.png)       

####七.redis 的持久化  
	redis.windows.conf配置文件中可配置
  
	1.有RDB 的默认持久化机制
	  可配置备份策略，会生成dump.rdb文件备份redis里的数据     
	2.AOF 持久化 
	  可配置，类似于mysql的备份，可以记录每次操作的命令     
      若开启，会生成appendonly.aof 文件   
   
	






	

