####一.Cookie的过期时间   
```java
	
	/**
	 *  1.cookie.setMaxAge(-1)：会话级别：为默认值，Cookie 类成员变量中默认值为-1
	 *    HttpServeletResponse.addCookie(Cookie cookie)方法后，浏览器接收到MaxAge的值<0 时，
	 *    cookie即为会话级别，将cookie保存到内存中，所以关闭浏览器后，cookie从内存中消失，再次代开浏览器即为新的一次会话。
	 */
	specifyCookie.setMaxAge(-1);
	
	/**
	 * 2.cookie.setMaxAge(0): 立即删除： 代表服务器想立刻清除该cookie
	 *   HttpServeletResponse.addCookie(Cookie cookie)方法后，浏览器接收到MaxAge的值=0时，
	 *   浏览器将立刻删除该cookie，无论是内存当中还是文件系统中。
	 */
	specifyCookie.setMaxAge(0);
	
	/**
	 * 3.cookie.setMaxAge(大于0)：指定时间后过期：
	 *   HttpServeletResponse.addCookie(Cookie cookie)方法后，浏览器接收到MaxAge的值>0时，
	 *   将cookie存储于本地磁盘, 过期后删除
	 */
	specifyCookie.setMaxAge(60*5);
```  

####二.Cookie的路径  
```java

	/**cookie.setPath()
	 * 1.设置cookie的有效访问路径。有效路径指的是cookie的有效路径保存在哪里，那么浏览器在有效路径下访问服务器时就会带着cookie信息，否则不带cookie信息
	 * 2.默认为当前servletPath路径的所在目录
	 *   比如urlPatterns = "/expireDate" 时，path默认为 /  ; servletPath此时为/expireDate
	 *   urlPatterns = "/aaa/expireDate时，path为/aaa/ ; servletPath此时为/aaa/exprieDate
	 */
```  

####三.浏览器中的限制  
	1.浏览器一般只允许存放300个Cookie，每个站点最多存放20个Cookie，每个Cookie的大小限制为4KB  
	2.cookie一般情况下不支持中文，但是可以在服务端用URLEncoder类中的encode方法编码先将中文编码再设置，  
	  取值时，先用URLDecoder.decode方法解码成中文     

####四.Cookie和Session的存储  
	1.Cookie 是服务端设置，浏览器接收，由浏览器在客户端存储维护的 
	2.Session 是保存在服务端保存的，不同的服务器可以有不同的保存策略，可以维护在内存、文件、数据库、或者redis中  
	3.Session 是依赖cookie的，通过唯一id值依赖cookie维护在浏览器端的（java中默认为JSESSIONID,php中为PHPSESSID），   
      如果存在每次浏览器请求时携带给服务端；服务器拿到sessionID 查找session;java中通过HttpServletRequest.getSession()  
      获得session()，如果第一次请求则创建，如果查找到，则可以刷新过期时间，session在服务端会一直保留， 
	  直到主动调用session.invalidate()方法，或者session到达过期时间，java中session.setMaxInactiveInterval（30*60）可以设置过期时间；  
	  Tomcat默认session超时时间为30分钟
    
	参考blog:https://blog.csdn.net/qq_28986619/article/details/81109935
        
	 


