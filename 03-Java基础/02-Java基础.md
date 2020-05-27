#### 一.ajax中的跨域问题  

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
			  调用方解决：  
						调用方隐藏跨域，将需要跨域的请求配置代理进行转发（具体可参考以上博客链接）;  
						比如可以请求自己服务器，由服务器请求跨域资源，之后再返回给前端，因为服务端不存在跨域问题  
			  被调用方解决：  
						可以在http服务器（apache或者nginx等）或者web服务器（tomcat,weblogic等） 进行设置，
						比如Spring中可以在Controller方法上添加@CrossOrigin注解；或者配置过滤器拦截器统一处理；  
						或者SpringBoot中添加同源Cors(配置类中实例化org.springframework.web.filter.CorsFilter) 
						主要原理就是在responseHeader中添加允许跨域的头信息（请求头：Access-Control-Allow-Origin）， 
						告诉浏览器该请求是允许跨域的，这也间接说明了另一个问题：跨域请求浏览器是可以正常请求，  
						并且服务端正常响应的，但是浏览器接收到响应由安全机制判定该请求是跨域请求时，报出跨域请求问题          
		（3）：XmlHttpResquest的解决办法： 
			  只要改变请求的类型不为XmlHttpResquest即可，浏览器就不会报跨域错误；  
			  其中典型的解决方案就是jsonp方式，其原理就是动态创建script标签，然后在script标签中进行跨域请求，script标签中是允许跨域请求的  
			  然后将动态创建的script标签添加到html的header中，请求完毕再移除。   
	
	具体详情可参考以上视频连接   

#### 二.java中正则使用的注意事项 

```java
	//在使用matcher.group（）之前，一定要先调用matcher.find()方法。否则会抛出No match found异常
	String pattern="\\d+";
	Pattern r = Pattern.compile(pattern);
	Matcher matcher = r.matcher(versionStr);
	if (matcher.find()) {
		String result = matcher.group(0);
		System.out.println("result="+result);
	}
```

#### 三.命令行执行可执行jar包时指定编码：-Dfile.encoding=utf-8

​	1.在编译机器中运行都没问题，但是将代码导出成jar包在执行时，一些读写IO文件操作可能会报错
​	com.sun.org.apache.xerces.internal.impl.io.MalformedByteSequenceException: 3 字节的 UTF-8 序列的字节 3 无效。	

	经排查是编码问题，window下命令行执行jar包时，默认是gbk编码，所以需要制定编码。
	
	java -Dfile.encoding=utf-8 -jar jenkins_vastool.jar "[{'channels': ['3'], 'gameId': '1556'}]"     
	
	命令： java -D属性名=属性值   
	解释： 该命令可以设置java程序启动时的系统属性值；  
		  何为系统属性值呢？也就是在System类中通过getProperties()得到的一串系统属性

#### 四.Java获取环境变量：

​		Java提供了System类的静态方法getenv()getProperties()用于返回系统相关的变量与属性，  
​		getenv方法返回的变量大多于系统相关，getProperties方法返回的变量大多与java程序有关。 

		System.getenv()： 
				方法是获取所有系统环境变量的值，比如JAVA_HOME路径
		System.getenv(String str)： 
				接收参数为任意字符串，当存在指定环境变量时即返回环境变量的值，否则返回null。
	
		System.getProperties()： 
				是获取jvm运行时相关属性，包括文件编码、操作系统名称、区域、用户名等，
				此属性一般由jvm自动获取，不能设置。
		System.getProperty(String str)：
				接收参数为任意字符串，当存在指定属性时即返回属性的值，否则返回null  
		
		注意：System.getProperties()、System.getProperty() 也可以获取用户通过java -D属性名=属性值 设置的参数                          

#### 五. Process java.lang.Runtime.exec(String command, String[] envp, File dir)   

​		该方法会生成一个新的进程去运行调用的程序,此方法返回一个java.lang.Process对象，  
​	    该对象可以得到之前开启的进程的运行结果，还可以操作进程的输入输出流。		

		Process对象有以下几个方法：
			1、destroy()			杀死这个子进程
			2、exitValue()		得到进程运行结束后的返回状态
			3、waitFor()			得到进程运行结束后的返回状态，如果进程未运行完毕则等待知道执行完毕
			4、getInputStream()	得到进程的标准输出信息流
			5、getErrorStream()	得到进程的错误输出信息流
			6、getOutputStream()	得到进程的输入流
		
		现在来讲讲exitValue()，当线程没有执行完毕时调用此方法会抛出IllegalThreadStateException异常，  
		最直接的解决方法就是用waitFor()方法代替。     

#### 六.java代码加载顺序

​	静态代码块：  

```
1.最早执行，类被载入内存时执行，只执行一次  
2.没有名字、参数和返回值，有关键字static   
3.多个静态代码块顺序执行 
```


​	构造代码块：  

```
1.执行时间比静态代码块晚，比构造函数早，和构造函数一样，只在对象初始化的时候运行  
2.没有名字、参数和返回值 
3.多个构造代码块顺序执行    
```


​	构造函数：

```
1.执行时间比构造代码块时间晚，也是在对象初始化的时候运行
2.没有返回值，构造函数名称和类名一致      
```

#### 七.java8中的新特性    StreamAPI   提供了各种快捷操作数据和流的操作

​	 	博客链接：[https://blog.csdn.net/u010425776/article/details/52344425](https://blog.csdn.net/u010425776/article/details/52344425 "博客链接")   

#### 八.Collections.shuffle(List<?> list) 

​		打乱集合中元素的顺序，场景：洗牌场景      

#### 九.Java中处理大数字(超过16位有效位)的操作类   

```
java.math.BinInteger 类,  是针对大整数的处理类
java.math.BigDecimal 类,  用于高精度计算.是针对大小数的处理类  
```

#### 十.BigDecimal 类可以处理java中的数字精度问题，如果在做支付相关需求时可以用该类

```java
BigDecimal b1 = new BigDecimal("25.00");
BigDecimal b2 = new BigDecimal("4.00");

BigDecimal add = b1.add(b2);           //加
BigDecimal subtract = b1.subtract(b2); //减
BigDecimal multiply = b1.multiply(b2); //乘
BigDecimal divide = b1.divide(b2);     // 除

/**
 * 求余数
 * 返回值为 (this % divisor) 的 BigDecimal
 */
BigDecimal remainder = b1.remainder(b2);

/**
 * 求相反数
 * 返回值是 (-this) 的 BigDecimal
 */
BigDecimal negate = b1.negate();

/**
 * 将此 BigDecimal 与指定的 BigDecimal 比较
 * 根据此方法,值相等但具有不同标度的两个 BigDecimal 对象（如，2.0 和 2.00）被认为是相等的;
 * 相对六个 boolean 比较运算符 (<, ==, >, >=, !=, <=) 中每一个运算符的各个方法,优先提供此方法;
 * 建议使用以下语句执行上述比较：(x.compareTo(y) <op> 0), 其中 <op> 是六个比较运算符之一;
 * 指定者：接口 Comparable<BigDecimal> 中的 compareTo
 * 返回：当此 BigDecimal 在数字上小于、等于或大于 val 时，返回 -1、0 或 1
 */
int i = b1.compareTo(b2);
```

#### 十一.ResourceBundle与Properties   

```
1.两者都可以便捷的读取配置文件，    
2.区别在于ResourceBundle通常是用于国际化的属性配置文件读取，Properties则是一般的属性配置文件
```

#### 十二.Collections.unmodifiableMap()

```bash
#方法会返回一个“只读”的map，当你调用此map的put方法时会抛错
public static final Map<String,String> TYPE ;
static {
    Map<String,String> tempMap  = new HashMap();
    tempMap.put(MYSQL,"mysql");
    tempMap.put(SQLSERVER,"sqlserver");
    TYPE = Collections.unmodifiableMap(tempMap);
}
#如果向转化后的map中put数据会报throw new UnsupportedOperationException()错误
```

#### 十三.Collections中常用静态方法

```shell
#Collections中有些常用的静态方法
Collections.emptySet();//创建一个空set
Collections.emptyList();//创建一个空list
Collections.emptyMap();//创建一个空map
Collections.unmodifiableMap();//使一个map变为不能修改，即为只读
Collections.empty....();
```

#### 十四.java文件操作之Path,Paths,Files

```bash
Java7中文件IO发生了很大的变化，专门引入了很多新的类：
import java.nio.file.DirectoryStream;
import java.nio.file.FileSystem;
import java.nio.file.FileSystems;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.attribute.FileAttribute;
import java.nio.file.attribute.PosixFilePermission;
import java.nio.file.attribute.PosixFilePermissions; 
在以前的操作中，主要通过File构造一个文件，然后将File作为入参，获取输入流等操作，Api的操作不是很流畅
新api的引入改变了这一点
```

> 参考博客：![https://unnue.com/article/5207]()

#### 十五. jdk1.7新特性 try-with-resource

```bash
# 用法
try(){
	//todo
}catch (Exception e){
	//todo
}
# 把代码写进try()括号里的作用:
# 不用再写finally块
# try括号内的资源会在try语句结束后自动释放，前提是这些可关闭的资源必须实现java.lang.AutoCloseable
# 接口。InputStream 和OutputStream 父类中一定实现了AutoCloseable接口

例如：
try (InputStream fis = new FileInputStream(source);
     OutputStream fos = new FileOutputStream(target)){
        byte[] buf = new byte[8192];
        int i;
        while ((i = fis.read(buf)) != -1) {
            fos.write(buf, 0, i);
        }
    }catch (Exception e) {
        e.printStackTrace();
    }
```

#### 十六.



