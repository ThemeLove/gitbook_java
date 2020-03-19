####一.SpringMVC 中常用注解  
	
	@RequestMapping    
	 属性： 
	 path:指定请求路径 
	 method:指定请求方法
	
	@RequestParam:当请求字段名称和接受参数名称不一致时，可以通过RequestParam配置绑定参数名称

	@PathVariable：路径参数   

####二.SpringMVC  类型转换器   
	
	1.SpringMVC中当前请求为日期格式的字符串时，Controller 用Date接收时，  
	  需要配置日期类型转换器，否则会转换失败，报错。   

	  
``` java

	public class StringToDateConverter implements Converter<String,Date>{
		
		@Override
		public Date convert(String source){
			SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-DD");
			Date date = null;
			try{
				date = sdf.parse(source);
			}catch(ParseException e){
				e.printStackTrace();
			}
			return date;
		}
	}

```   

	2.在spring-mvc.xml中配置类型转换工厂   
	  
```xml

	<!--类型转换工厂-->
	<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
		<property name="converters">
			<set>
				<bean class="自定义类型转换器全限定类名"/>
			</set>  
		</property> 
	</bean>

	<!--注解驱动-->  
	<mvc:annotation-driven conversion-service="conversionService"></mvc:annotation-driven>
```   

####三.编码过滤器   
	
```xml  

	<!--拦截所有请求，不包含静态资源-->
	<filter>
		<filter-name>CharactorEncoding</filter-name> 
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
			<init-param> 
				<param-name>encoding</param-name>  
				<param-value>utf-8</param-value>  
			</init-param>    
		</filter-class>
	</filter>   

	<filter-mapping>  
		<filter-name>CharactorEncoding</filter-name>
		<!--拦截所有请求，不包含静态资源-->
		<url-pattern>/*</url-pattern>
	</filter-mapping>  
```  


####三.引入静态资源后，必须静态资源放行   
	
	<!--对静态资源放行，把js下的静态资源映射到js目录下-->  
	
```xml  
	
	<mvc:resources mapping="/js/*" location="/js/"></mvc>

```

 
####四.SpringMVC文件的上传     

	1.引入依赖  
	
```xml

	<dependency>
		<groupId>commons-fileupload<groupId>  
		<artifactId>commons-fileupload</artifactId>
		<version>1.3.1</version>
	</dependency>

```   

JMX