####一.HttpServletRequest 中 getParameter 和 getAttribute 方法  
	1.getParameter()方法  
	  该方法是用于客户端传递过来的参数，它的返回值类型永远是字符串类型，这个赋值动作是服务端容器完成的
	  特别的，当我们新建一个Servlet 继承自 HttpServlet 后，HttpServletRequest中并没有setParameter方法   
	2.getAttribute()方法
      该方法是在请求传到服务器端后，在去使用其进行存取一些附加数据。它的返回类型永远是Object.  
	  注：
	  所以如果需要在服务器端进行跳转，并需要想下个页面发送新的参数时，由于没有setParameter方法，   
	  只可以通过setAttribute()，将值放入到request对象，然后在其他页面使用getAttribute获取对应的值， 
	  这样就达到一次请求可以在多个页面共享一些对象信息，并且setAttribute可以设置引用类型对象   

	总结：获取客户端参数时用req.getParameter;服务端设置值时用req.setAttribute方法。   

####二.RequestContextHolder 
	在Spring MVC中，为了随时都能取到当前的request对象，其内部也是通过ThreadLocal实现，将request和当前线程绑定完成；  
	可以通过RequestContextHolder的静态方法getRequestAttributes() 获取Request相关的变量，如 request, response等。  
	例： 
	RequestContextHolder.getRequestAttributes().getRequest();
	RequestContextHolder.getRequestAttributes().getResponse();   

####二.Filter（过滤器） 和 HandlerInterceptor（spring中的拦截器） 的区别 
	Filter： 是javax.servlet.Filter里的包，需要配置在web.xml里或者用WebFilter注解;   
			 由服务器容器创建，比如Tomcat，不能使用框架里的资源，比如Spring中的Service 

	HandlerInterceptor： 是spring 框架里的拦截器，实现原理是aop方式，可以拦截到方法（即Controller中的响应方法）   
						 是Spring容器创建的，所以配置时也要用spring的配置文件或者注解，  
						 拦截器中可以使用spring里的其他资源，比如Service等  
	
	总结：1.Filter 和 HandlerInterceptor 都可以起到拦截、处理Request 和 response，比如权限校验、  
		   统一添加修改header、设置编码、登录校验等；   
		 2.如果是用框架的话（比如spring）,建立用拦截器，因为可以更全面的拦截和定制请求和响应。其他框架struts也有类似拦截器
		 
####三.ResponseBodyAdvice 
	可以实现增强返回值，就是拦截Controller中的返回值，然后统一处理。比如统一添加一个requestID 字段、加密      
	@ControllerAdvice和ResponseBodyAdvice接口统一处理返回值 
	
	1.定义一个类实现ResponseBodyAdvice接口，重写supports 和 beforeBodyWrite方法。  
	  supports 返回值为boolean，表示是否拦截；beforeBodyWrite方法中可以添加增强response的方法。  
	
	2.添加@ControllerAdvice注解，可以指定basePackages 参数，指定拦截规则。 
 
```java 

	例如：
	@ControllerAdvice(basePackages = "com.test")
	public class EncryptResponseAdvice implements ResponseBodyAdvice<Object> {
	    @Override
	    public boolean supports(MethodParameter returnType, Class<? extends HttpMessageConverter<?>> converterType) {
	        return true;
	    }
	    @Override
	    public Object beforeBodyWrite(Object body, MethodParameter returnType,
	            MediaType selectedContentType, Class<? extends HttpMessageConverter<?>> selectedConverterType,
	            ServerHttpRequest request, ServerHttpResponse response) {
	        //先做版本判断，然后加密处理
	       
	        return body;
	    }
	}
```     

####四.SpringMVC 中的 CommonsMultipartResolver 可以实现文件的上传  

####五.Objects 类 
	jdk1.7后出现，提供静态方法操作对象 
	1.public static boolean equals(Object a,Object b):比较对象a和对象b是否相等.
  	  比较2个对象是否相等,底层依赖对象重写的equals的方法,如果没有重写,则使用Object的equals()

	2.public static <T> T requireNonNull(T obj):检查对象obj不为null,如果为null,则抛出空指针异常,否则返回obj本身.
	  可以判断对象是否是空对象.限制参数不能为空.
	
	3.public static boolean nonNull(Object obj):判断对象是否为null,不为返回true,否则返回false
	
	4.public static boolean isNull(Object obj):和nonNull()相反.   

####六.JDK1.8 中新增时间日期类，Instant(时间戳类)、Duration类、Period类   
```java 

	例子：
	//      获取当前时间
	        Instant ins = Instant.now();
	        System.out.println(ins);
	//      设置偏移量
	        OffsetDateTime offsetDateTime = ins.atOffset(ZoneOffset.ofHours(8));
	        System.out.println("offsetDateTime="+offsetDateTime);
	//      获取系统默认时区时间
	        ZonedDateTime zonedDateTime = ins.atZone(ZoneId.systemDefault());
	        System.out.println("zonedDateTime="+zonedDateTime);
	//      getEpochSecond()：获取从1970-01-01  00：00：00到当前时间的秒值
	        long epochSecond = ins.getEpochSecond();
	        System.out.println("epochSecond="+epochSecond);
	//      getNano():把获取到的当前时间的描述换算成纳秒
	//      注意这里的getNano() 并不是从1970-01-01  00：00：00 开始的，而是从Instant.now()实例时置为0,重新计算,注意是纳秒
	        int nano = ins.getNano();
	        System.out.println("nano="+nano);  

	输出： 
	2019-08-22T11:00:42.911Z
	offsetDateTime=2019-08-22T19:00:42.911+08:00
	zonedDateTime=2019-08-22T19:00:42.911+08:00[Asia/Shanghai]
	epochSecond=1566471642
	nano=911000000 
```   


