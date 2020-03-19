##常用鉴权方式    

- 1.Http Basic Auth    
- 2.Cookie Auth   
- 3.SSO  
- 4.Token Auth  JWT (json web token)   
- 5.Auth     

> 1.Http Basic Auth 
 
	用户每次请求都携带用户名和密码，服务器校验用户名密码是否正确,服务端不保存登录状态；  
	这种每次请求都携带用户名密码，容易引发账号安全，很少使用

> 2.Cookie Auth   
 
	传统非Restful风格，用户登录时服务端会将用户信息存入session并设置过期时间,  
	将sessionId 返回，后续用户请求只需携带sessionId即可，由服务端保存用户状态。    
	
	优点：  
		1.信息存储在服务端，相对安全  
	    2.实现简单，session技术属于servlet规范  
	
	缺点：  
		1.不服务Restful风格，在服务端存储状态了；  
		2.在分布式环境中需要处理session共享问题（session可以用redis中存储来解决） 
	    3.维护session需要消耗资源     

> 3.SSO  单点登录    
	
	优点：
		1.多个相互信任的系统间，只需登录一次，通常用于大的集团内部各个系统之间   
   
	缺点：  
		1.认证状态保持在了服务端，不符合Restfual风格；  
		2.成本高，需要额外的认证服务器

> 4.Token Auth  JWT (json web token)     
  
	用户登录成功时，服务端将使用加密算法将用户信息加密（需要加盐，密钥特别重要），得到token,并且token中
	包含过期时间字段，返回给客户端，由客户端保存，后续其他请求都带上该token  

	优点：  
		1.服务端不保存登录状态，符合Restful风格；  
	    2.在分布式环境中适用             
  