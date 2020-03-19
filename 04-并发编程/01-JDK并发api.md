##jdk中的并发包  
	java.util.concurrent  作者：Doug Lea   道格·利     

参考博客：  

[https://juejin.im/entry/5e53e5c1f265da5735504fb6](https://juejin.im/entry/5e53e5c1f265da5735504fb6)   

[https://juejin.im/entry/5b6bec4e6fb9a04fe54916c4](https://juejin.im/entry/5b6bec4e6fb9a04fe54916c4)

####一.ThreadPoolExecutor:创建线程池的最全构造方法  

    public ThreadPoolExecutor(int corePoolSize,//核心线程数
                          int maximumPoolSize,//最大线程数
                          long keepAliveTime,//非核心线程保活时间
                          TimeUnit unit,//时间单位
                          BlockingQueue<Runnable> workQueue,//任务工作队列
                          ThreadFactory threadFactory,//创建线程工程
                          RejectedExecutionHandler handler)//达到最大线程数并且任务工作队列也满时，再提交任务时的处理策略）   

	
####二.jdk自带的集中线程池 
Executors通过静态方法提供4种类型的线程池,他们都是调用ThreadPoolExecutor构造方法创建的 

Executors.newFixedThreadPool(5) 
Executors.newCachedThreadPool()   
Executors.newSingleThreadExecutor()  
Executors.newScheduledThreadPool(5) 
	
	FixedThreadPool:固定线程池
	public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
    }
	
	CachedThreadPool: 缓存线程池
    public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    } 

	ScheduledThreadPoolExecutor : 可调度线程池，提供定时、定期方法
	public ScheduledThreadPoolExecutor(int corePoolSize) {
        super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
              new DelayedWorkQueue());
    }

	SingleThreadExecutor:单线程线程池
	public static ExecutorService newSingleThreadExecutor() {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>()));
    }

	
	注意：1.阿里巴巴java开发手册建议线程池不允许使用Executors去创建，而是通过ThreadPoolExecutors的方式，   
	     这样的处理方式会更加明确线程池的运行规则，避免规则资源耗尽的风险。  
		 说明Executors各个方法的弊端  
	     newFixedThreadPool 和 newSingleThreadExecutor  
		 主要问题是堆积的请求处理队列可能会耗尽非常大内存，甚至OOM  
	
		 newCachedThreadPool和newScheduleThreadPool   
		 主要问题是线程最大数是Integer.MAX_VALUE,可能会创建数量非常多的线程，甚至OOM        

  		 2.根据ThreadPoolExecutors手动创建线程池的方式，各个参数的选择：   
		   核心线程数：可以根据cpu的核心数考虑：Runtime.getRuntime().availableProcessors()   
		   如果应用场景是cpu密集型可以考虑：核心线程数=cpu核数   
		   如果应用场景是io密集型可以考虑：核心线程数=cpu核数*2+1
	 


####三.当一个线程池里的线程异常后，jdk会怎么处理   
	
	1.当执行的是execute时，可以看到堆栈异常信息的输出  
	
	2.当执行的是submit时，堆栈异常没有输出。但是调用Future.get()方法时，可以捕获到异常。  
	
	3.不会影响线程池里面其他线程的正常执行   
	
	4.线程池会把异常的线程移除掉，并创建一个新的线程放到线程池中      

####四.使用线程池的好处   
	
	1.线程的重用  
	  java中线程的创建和销毁开销是巨大的，线程池的使用可以减少线程的创建    

	2.控制线程池的并发数    
	  线程池可以设置核心线程数和最大线程数，所以可以控制最大并发数。   
	
	3.线程池可以对线程进行管理  
	  线程池可以提供定时、定期、单线程、并发数控制等功能，比如通过
	  ScheduledThreadPool线程池来执行S秒后，没隔N秒执行一次的任务。

