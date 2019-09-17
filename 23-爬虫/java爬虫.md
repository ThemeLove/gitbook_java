####一.java爬虫常用的基础框架 
	HttpClient:用于请求网络发送http请求
	Jsoup:用于解析网页  
####二.webmagic框架  
	1.webmagic 是java的一个爬虫框架，底层是httpclient 和jsoup;
	  webmagic 四大组件：
	
	  downloader:下载器组件，从互联网下载html页面，并封装成Page对象，传递给PageProcess组件，底层是HttpClient实现。
	
	  pageProcesser:爬虫的核心组件，必须自定义
	    Site对象：站点参数的配置：抓取的间隔时间，超时时间，代理，编码格式，重试次数，添加header等
	    Page对象：Html:Downloader组件抓取的页面封装成的Html对象；
	    Selectable对象转化成字符串：
	        toString()、get():如果是列表的话，只取第一个元素
	        all():可以把Selectable转化成字符串，转换成一个List<String>
	        links():取当前节点中所有的链接地址
	        nodes():返回一个List<Selectable>对象
	
	  pipeline:负责存储持久化解析的数据
	    框架默认提供三个实现
	    ConsolePipeline:框架默认使用，向控制台输出
	    FilePipeline:向磁盘输出，可以把抓取到的结果保存到磁盘
	    JsonFilePipeline:向磁盘输出保存到json数据
	
	  scheduler:维护的待爬去的url列表
	    url队列
	    实现方法：
	        QueueScheduler:java内存队列（默认使用）
	        FileCacheQueueScheduler:使用文件做缓存的队列
	        RedisSechduler:使用redis做队列，解决分布式爬虫问题
	    向队列中添加url时去重
	        1) 使用java中的hashset去重（默认使用）
	        2) 使用redis的set去重
	        3) 使用布隆过滤器去重
	            有点：速度快
	            缺点：可能未去重
	
	    布隆过滤器使用方法：
	        1) 创建一个QueryScheduler对象
	        2) 创建一个布隆过滤器的对象
	        3) 给Scheduler设置布隆过滤器
	        4) 布隆过滤器需要guava支持
	2.Spider 工具类 
		初始化爬虫，装配各个组件，同时设置参数    

####三.定时器 
	1.Timer：java中的定时器，功能简单 
	2.Quartz：定时框架，功能强大，使用繁琐 
	3.@Scheduled:SpringBoot中使用的定时器 
		a.底层也是Quartz 
		步骤：1).在SpringBoot的引导类中添加@EnableScheduling  
			 2).在需要使用的方法上添加@Scheduled,支持cron表达式 
	  @Scheduled常用属性： 
		fixedDelay:固定延迟，固定延迟多长时间执行,long类型 
		fixedDelayString:固定延迟执行，同fixedDelay,数据类型是String类型	
		fixedRate:固定周期执行
		fixedRateString:固定周期执行，同fixedRateString,数据类型是String类型 
		复杂周期执行应该使用cron表达式  
		cron在线表达式生成：http://cron.qqe2.com/     
		
	  
####四.代理的使用
	1.应用场景 
	  防止服务器识别出爬虫，封停ip  
	2.两个免费代理ip的服务器网站： 
	  米扑代理：https://proxy.mimvp.com/free.php
	  西刺代理：http://www.xicidaili.com   
	3.再webmagic中使用代理，在downloader中设置 
```java

    downloader = new HttpClientDownloader();
    SimpleProxyProvider proxyProvider = SimpleProxyProvider.from(new Proxy("140.143.152.93", 1080),
            new Proxy("140.143.152.93", 1080),
            new Proxy("140.143.152.93", 1080),
            new Proxy("140.143.152.93", 1080)
    );
    downloader.setProxyProvider(proxyProvider);
```
	     
####五.selenium+无头浏览器 
	1.selenium:前段测试框架：能通过代码控制浏览器  
	  有java、.net、python、node.js 等各个版本     
	2.无头浏览器：没有图像的浏览器        
      phantomjs:无头浏览器   
	  普通浏览器的的无头浏览模式： 
	  chrome 
	  firefox 
	  edge   
	  