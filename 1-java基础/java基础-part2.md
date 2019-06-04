####一.ajax中的跨域问题  
慕课网视频讲解链接：[https://www.imooc.com/learn/947](https://www.imooc.com/learn/947 "慕课网视频讲解链接")  

	1.跨域问题产生的原因： 
		（1）：浏览器的限制
			  跨域限制只有在浏览器环境中才有，是浏览器为了安全考虑对跨域请求做了限制 
		（2）：跨域 
			  请求域名和本站域名 必须是跨域才会触发浏览器的跨域问题 
		（3）：XmlHttpResquest  
			  请求的类型必须是XmlHttpResquest才会引发浏览器的跨域问题  
 			  
			  比如说img、script标签中发起的请求就不是XmlHttpRequest类型的，所以可以跨域  
	
	2.解决思路：根据跨域问题产生的原因解决思路可以从三方面入手 
		（1）：浏览器禁止检查：可以命令行启动浏览器：chrome --disable-web-security  
		（2）：跨域方面解决：
			  调用方解决：调用方隐藏跨域，将需要跨域的请求配置代理进行转发（具体可参考以上博客链接）  
			  被调用方解决：可以在http服务器（apache或者nginx等）或者web服务器（tomcat,weblogic等） 进行设置，主要原理就是在responseHeader中添加允许跨域的头信息  
			  告诉浏览器该请求是运行跨域的          
		（3）：XmlHttpResquest的解决办法： 
			  只要改变请求的类型不为XmlHttpResquest即可，浏览器就不会报跨域错误；  
			  其中典型的解决方案就是jsonp方式，其原理就是动态创建script标签，然后在script标签中进行跨域请求。  
			  然后将动态创建的script标签添加到html的header中，请求完毕再移除。   

	具体详情可参考以上视频连接   
		
	