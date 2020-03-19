####1. SpringCloud 与 dubbo  

- dubbo 只是做服务与发现（Zookeeper），远程调用（RPC-部分底层使用的是netty）框架，是一种具体的实现

- SpringCloud 是一套分布式解决方案，收集了包括服务发现（Netflix Eureka）、服务调用（Netflix Feign）、熔断器（Netflix Hystrix）、服务网关（Netflix Zuul）、分布式配置（Spring Cloud Config）、消息总线（Spring Cloud Bus） 等几大模块    

- SpringCloud 需要配合SpringBoot使用

####2.分布式理论（CAP）  

任何分布式架构都不能同时严格满足这三个特性，分区容错性 必须满足，  
一致性和可用性在设计的分布式框架的时候需要选择更倾向哪个

- Consistency(一致性) 
- Availability(可用性) 
- Partition Tolerance(分区容错性)  

zookeeper 和 Eureka 对比 

	zookeeper 在设计的时候更倾向于一致性，比如多个zookeeper集群，只有一个主节点，  
    一致性由主节点保证，所有从节点从主节点同步。当主节点出现故障的时候，会通过选举从从节点中尽快选出新的主节点。  
    这个过程可能会导出zookeeper不可用，直到新的主节点出现  

	Eureka 在设计的时候更倾向于可用性，Eureka集群每个节点之间是对等的，相互同步服务列表。   
	过程当中可能服务列表不一致，但是当其中任何一个节点挂掉的时候，其他节点 
	正常工作，可以保证服务的可用性。Eureka客户端会每隔30秒向服务端发送心跳。Eureka的客户端也会缓存服务列表      

	Eureka 特点
	1.客户端会向服务端发送心跳（30秒/s）,服务端通过客户端心跳来注册微服务以及维护可用微服务列表  

	2.服务端之间通过相互复制（同步）方式保证服务列表一致性  

	3.如果服务端发现85%以上微服务都没有发送心跳，将不会删除列表中的为服务，不会向其他服务器同步服务列表（保护模式），也是可用性体现

	4.即使全部Eureka服务器挂掉，客户端会使用缓存在本地的服务列表（客户端缓存）   

####3.Eureka-Server 搭建 
	
1.添加依赖   

```xml

	<dependency>
	    <groupId>org.springframework.cloud</groupId>
	    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
	    <version>2.2.2.RELEASE</version>
	</dependency>

``` 

2.添加配置

```xml

	server:
		port:6868    # 服务端口  
	eureka:
		client:
			registerWithEureka:false  # 是否将自己注册到Eureka服务中，本身就是服务无需注册  
			fetchRegistry:false		# 是否从Eureka 中获取注册信息  
			serviceUrl:
				defaultZone:http://127.0.0.1:${server.port}/eureka/  #Eureka客户端与Eureka服务端进行交互的地址
	enable-self-preservation: false       # 关闭Eureka 的保护机制


```

3.@EnableEurekaServer 注解加在启动类上 


####4.Eureka-Client 搭建   

1.添加依赖  

```xml

	<dependency>
	    <groupId>org.springframework.cloud</groupId>
	    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
	    <version>2.2.2.RELEASE</version>
	</dependency>

```

2.添加配置

```xml
 
	eureka:
		client:
			registerWithEureka:true  # 是否将自己注册到Eureka服务中，本身就是服务无需注册  
			fetchRegistry:true		# 是否从Eureka 中获取注册信息  
			serviceUrl:
				defaultZone:http://127.0.0.1:${server.port}/eureka/  #Eureka客户端与Eureka服务端进行交互的地址
		instance: 
			prefer-ip-address:true  # 注册服务时，注册ip地址

```

3.@EnableEurekaClient 注解加在启动类上     


####5.Netflix-Fegin ( 远程调用RPC )

1.添加依赖  

```xml

	<dependency>
	    <groupId>org.springframework.cloud</groupId>
	    <artifactId>spring-cloud-starter-openfeign</artifactId>
	    <version>2.2.2.RELEASE</version>
	</dependency>

```  

2. 定义远程调用接口

```java

	# 指定远程调用服务器为tensquare-base,备用方案为LabelClientImpl.class
	@FeignClient(value="tensquare-base", fallback=LabelClientImpl.class)	
	public interface LabelClient{
		
		# 以下为远程服务接口	
		@RequestMapping(value="/user/{userId}", method="RequestMethod.GET")
		public Result findById(@PathVariable("labelId") String labelId);
	}

```

3.启动类需要添加 
	
	@EnableDiscoveryClient   //开启远程服务发现客户端
	@EnableFeignClients		//开启Feign远程调用

4.自带负载均衡，同一个服务启动多个实例，会均衡调用    




####6.SpringCloud 中的熔断  

	现象：假如为服务A 调用B----->B调用C,如果C出现故障，将会导致B故障，B节点故障又会导致A节点故障，  
	      由此引发分布式节点的故障雪崩  

熔断机制：

	1.SpringCloud 熔断器默认是关闭状态，默认有一个阀值，多次调用失败，  
	  达到阀值，熔断器打开，如果有备用方案则用备用方案。    
	2.熔断器有一个时间窗口，当时间窗口结束，熔断器变为半开状态，这时会熔断器  
	  会再次尝试调用，如果失败，熔断器会再次打开，进入下一个时间窗口。  
	  如果多次调用成功，则熔断器关闭。
	3.节点A远程调用节点B时，考虑到节点B出现故障，给A调用B一个提供一个备用方案。   
	  再熔断器打开的情况下，如果A调用B失败，会自动使用备用方案完成调用   

		
启用熔断器：

```xml

	feign: 
		hystrix: 
			enabled:true  # 启用hystrix 熔断器

```	   

备用方案： 
 
	1.提供一个远程调用接口的本地默认实现即可，为服务架构首先会进行远程RPC调用；   
	  如果远程调用失败，熔断器打开，就会调用本地的备用方案，注意备用方案已定要可行  
	2.在接口上添加fallback参数，值为默认实现类 
	
```java

	# 指定远程调用服务器为tensquare-base,备用方案为LabelClientImpl.class
	@FeignClient(value="tensquare-base", fallback=LabelClientImpl.class)	
	public interface LabelClient{
		
		# 以下为远程服务接口	
		@RequestMapping(value="/user/{userId}", method="RequestMethod.GET")
		public Result findById(@PathVariable("labelId") String labelId);
	}

```

####7.Spring-cloud 微服务网关 

常用作用：  

- 1. 网关项目也需要注册到Eureka服务     
- 2. 作为多个微服务项目的统一入口，转发请求            
- 3. 对多个微服务项目做统一登录鉴权             

1.添加依赖  

```xml

	<dependency>
	    <groupId>org.springframework.cloud</groupId>
	    <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
	    <version>2.2.2.RELEASE</version>
	</dependency>
```  

2.添加配置  

```xml

	zuul:
	  routes:
		tensquare-base:	#项目名称，自由填写  
			path:/gathering/**	#配置请求URL的请求规则   
			serviceId:tensquare-gathering	#指定Eureka注册中心的服务id
		xxx....  #其他多个微服务项目  
```  
 
3.启动类添加@EnableZuulProxy     //开启zuul网关   


4.网关过滤器  

添加私有请求头 网关过滤器

```java
	
	/**
	 * zuul网关只会转发标准的http协议头，业务如果有自定义的协议头需要这里手动添加
	 */
	@Component
	public class WebFilter extends ZuulFilter{
	
		// 指定过滤器类型
		@Overvide
		public String filterType(){
			// pre:可以请求被路由之前调用
			// route:
			// error: 处理请求时发生错误时调用
			// post: 在route 和 error 过滤器之后被调用
			
			return "pre";	
		}
	
		// 定义过滤器执行优先级
		// 返回0就是最高优先级
		@Override
		public int filterOrder(){
			return 0;
		}
	
		//指定当前过滤器是否启用
		@Override
		public boolean shouldFilter(){
			
			// 也可以通过业务代码来动态启用过滤器
			return true;
		}
	
		// 网关过滤器的业务逻辑代码
		// zuul网关只会转发标准的http协议头，业务如果有自定义的协议头需要这里手动添加
		@Override
		public Object run() ZuulException{
			// 
			// 1.获取请求头 
			RequestContext requestContext = RequestContext.getCurrentContext(); 
			HttpServletRequest request = requestContext.getRequest(); 
			String authorization = request.getHeader("Authorization");
			// 2.获得Authorization头 
			if(!StringUtils.isEmpty(authorization)){
				// 3. 将Authorization 头添加到网关转发请求中 
				requestContext.addZuulRequestHeader("Authorization", authHeader);
			}
			// return null 继续放行
			return null;
		}
	
	}

```   

权限处理 网关过滤器  

```java
	
	/**
	 * 判断登录和权限
	 */
	@Component
	public class WebFilter extends ZuulFilter{
	
		// 指定过滤器类型
		@Overvide
		public String filterType(){
			// pre:可以请求被路由之前调用
			// route:
			// error: 处理请求时发生错误时调用
			// post: 在route 和 error 过滤器之后被调用
			
			return "pre";	
		}
	
		// 定义过滤器执行优先级
		// 返回0就是最高优先级
		@Override
		public int filterOrder(){
			return 0;
		}
	
		//指定当前过滤器是否启用
		@Override
		public boolean shouldFilter(){
			
			// 也可以通过业务代码来动态启用过滤器
			return true;
		}
	
		// 网关过滤器的业务逻辑代码
		// 判断登录和权限
		@Override
		public Object run() ZuulException{
			// 1.获取请求头 
			RequestContext requestContext = RequestContext.getCurrentContext();  
			HttpServletRequest request= requestContext.getRequest();  
			if(request.getMethod().equals("OPTIONS")){
				return null;
			}
			String url = request.getRequestURL().toString();  
			if(url.indexOf("/admin/login")>0){
				// 登录页面，直接放行
				return null;
			}
			String authHeader = request.getHeader("Authorization");
			if(authHeader!=null && authHeader.startsWith("Bearer ")){
				String token = authHeader.substring(7);  
				Claims claims = JwtUtil.parseJWT(token);
				if(claims!=null){// 如果有管理员权限放行
					if("admin".equals(claims.get("role"))){
						requestContext.addZuulRequestHeader("Authorization", authHeader);
						return null;
					}
				}
			}
			
			requestContext.setSendZuulResponse(false); //终止  
			requestContext.setResponseStatusCode(401);//修改状态码  
			requestContext.setResponseBody("no privilege");
			requestContext.getResponse().setContentType("text/html;charset=UTF-8");
			return null;
		}
	
	}

```   


####8.Spring-Cloud 配置中心      

使用git 来统一管理众多微服务项目中的配置文件，方便管理     

> 1.新建配置中心项目步骤

1.添加依赖  

```xml

	<dependency>
	    <groupId>org.springframework.cloud</groupId>
	    <artifactId>spring-cloud-config-server</artifactId>
	    <version>2.2.2.RELEASE</version>
	</dependency>

```

2.启动类添加@EnableConfigServer 注解   

3.编写application.yml  

```xml

	server: 
		port:12000  
	spring: 
		application:
			name:tensquare-config  
		cloud: 
			config: 
				server: 
					git: //指定使用从git仓库加载配置
						uri:https://gitee.com/chuanzhiliubei/tensquare-config.git    
```   

4.启动项目，浏览器测试 http://localhost:12000/base_dev.yml    

>2. 修改需要配置的微服务项目    

1.添加依赖  

```xml

	<dependency>
	    <groupId>org.springframework.cloud</groupId>
	    <artifactId>spring-cloud-config-server</artifactId>
	    <version>2.2.2.RELEASE</version>
	</dependency>

```  

2.删除原有的application.yml   

3.添加新的配置文件bootstrap.yml, 并配置    

```xml

	spring:
		cloud: 
			config: 
				name:base  #配置文件的名称前缀
				profile:dev   #配置文件名称后缀
				label:master  #从git的哪个分支加载配置文件
				uri:http://127.0.0.1:12000   # 指定从哪个微服务加载配置
```         


####9.Spring-Cloud-Bus 消息总线     

场景：
	spring-cloud支持配置中心，但是如果没有消息总线，配置中心的配置只会在  
	微服务项目启动的时候拉取一次，当配置中心的配置改变的时候，微服务项目不能  
	及时同步最新配置；消息总线就是解决这个问题的 

git仓库的webhook 机制 
 
	当git仓库中的内容改变时，仓库会自动发送一个请求访问指定路径  

大致流程  

1. git仓库配置webhook通知的url,一般为配置中心项目的地址;  
	配置中心和微服务项目事先配置好订阅关系，配置中心相当于消息生产者，  
	微服务项目相当于消费者。配置中心默认用的是rabbitMQ
2. 当配置文件改动，触发webhook，自动发送请求通知配置中心  
3. 配置中心接受到请求，发送一条消息 
4. 微服务项目接收到消息，重新拉取配置文件，实现同步    

注意： 
	消息中间件部分需要自己搭建，配置中心配置mq的地址，充当消息生产者； 
	微服务项目配置监听的mq地址，充当消费者 


>1 配置中心改动 

1.添加依赖

```xml  

	<dependency>
	    <groupId>org.springframework.cloud</groupId>
	    <artifactId>spring-cloud-bus</artifactId>
	    <version>2.2.1.RELEASE</version>
	</dependency>

	<dependency>
	    <groupId>org.springframework.cloud</groupId>
	    <artifactId>spring-cloud-stream-binder-rabbit</artifactId>
	    <version>3.0.3.RELEASE</version>
	</dependency>

```     

2.添加配置  

```xml
	spring:
		rabbitmq:
			host:192.168.199.146  
	management:
		endpoints:
			web:
			  exposure: 
				include:buf-refresh  #暴露触发消息总线的地址

```

>2 消息总线微服务客服端配置  

1.添加依赖  

```xml  

	<dependency>
	    <groupId>org.springframework.cloud</groupId>
	    <artifactId>spring-cloud-bus</artifactId>
	    <version>2.2.1.RELEASE</version>
	</dependency>

	<dependency>
	    <groupId>org.springframework.cloud</groupId>
	    <artifactId>spring-cloud-stream-binder-rabbit</artifactId>
	    <version>3.0.3.RELEASE</version>
	</dependency>

	<dependency>
	    <groupId>org.springframework.boot</groupId>
	    <artifactId>spring-boot-starter-actuator</artifactId>
	    <version>2.2.5.RELEASE</version>
	</dependency>
```     

2.添加配置  
	
```xml

	spring:
		rabbitmq:
			host:192.168.199.146  
```  

3.微服务项目中用到配置的地方需要添加@RefreshScope 注解，使配置生效  

	原因： 因为Spring容器只在启动的时候，实例化了各种组件Bean对象；  
	虽然重新获取了最新的配置，但是Spring中的实例还是原来的，所以需要  
	@RefreshScope 注解，相当于重新实例化该注解的类实例，使新配置重新生效
	
	






	
