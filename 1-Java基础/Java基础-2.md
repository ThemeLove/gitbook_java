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
		

####二.java中正则使用的注意事项 
	在使用matcher.group（）之前，一定要先调用matcher.find()方法。否则会抛出No match found异常
 
```java

	String pattern="\\d+";
	Pattern r = Pattern.compile(pattern);
	Matcher matcher = r.matcher(versionStr);
	if (matcher.find()) {
		String result = matcher.group(0);
		System.out.println("result="+result);
	}

```      

####三.命令行执行可执行jar包时指定编码：-Dfile.encoding=utf-8
	1.在编译机器中运行都没问题，但是将代码导出成jar包在执行时，一些读写IO文件操作可能会报错
	com.sun.org.apache.xerces.internal.impl.io.MalformedByteSequenceException: 3 字节的 UTF-8 序列的字节 3 无效。	
 
	经排查是编码问题，window下命令行执行jar包时，默认是gbk编码，所以需要制定编码。
	
	java -Dfile.encoding=utf-8 -jar jenkins_vastool.jar "[{'channels': ['3'], 'gameId': '1556'}]"   

####四.Java获取环境变量：
		Java提供了System类的静态方法getenv()和getProperty()用于返回系统相关的变量与属性，  
		getenv方法返回的变量大多于系统相关，getProperty方法返回的变量大多与java程序有关。 

		System.getenv()： 方法是获取所有系统环境变量的值
		System.getenv(String str)： 接收参数为任意字符串，当存在指定环境变量时即返回环境变量的值，否则返回null。

		System.getProperty()： 是获取系统的相关属性，包括文件编码、操作系统名称、区域、用户名等， 
							   此属性一般由jvm自动获取，不能设置。
		System.getProperty(String str)： 接收参数为任意字符串，当存在指定属性时即返回属性的值，否则返回null                          

####五. Process java.lang.Runtime.exec(String command, String[] envp, File dir)   
		该方法会生成一个新的进程去运行调用的程序,此方法返回一个java.lang.Process对象，  
	    该对象可以得到之前开启的进程的运行结果，还可以操作进程的输入输出流。
		
		Process对象有以下几个方法：
		　　1、destroy()　　　　　　  杀死这个子进程
		　　2、exitValue()　　　 　 得到进程运行结束后的返回状态
		　　3、waitFor()　　　　 　　 得到进程运行结束后的返回状态，如果进程未运行完毕则等待知道执行完毕
		　　4、getInputStream()　　得到进程的标准输出信息流
		　　5、getErrorStream()　　得到进程的错误输出信息流
		　　6、getOutputStream()　得到进程的输入流
		
		现在来讲讲exitValue()，当线程没有执行完毕时调用此方法会跑出IllegalThreadStateException异常，  
		最直接的解决方法就是用waitFor()方法代替。     

####六.java代码加载顺序
	静态代码块：最早执行，类被载入内存时执行，只执行一次。没有名字、参数和返回值，有关键字static。   
	构造代码块：执行时间比静态代码块晚，比构造函数早，和构造函数一样，只在对象初始化的时候运行。没有名字、参数和返回值。     
	构造函数：执行时间比构造代码块时间晚，也是在对象初始化的时候运行。没有返回值，构造函数名称和类名一致。            

####七.java8中的新特性    StreamAPI   提供了各种快捷操作数据和流的操作
博客链接：[https://blog.csdn.net/u010425776/article/details/52344425](https://blog.csdn.net/u010425776/article/details/52344425 "博客链接")   

####八.Collections.shuffle(List<?> list) 
	打乱集合中元素的顺序，场景：洗牌场景



