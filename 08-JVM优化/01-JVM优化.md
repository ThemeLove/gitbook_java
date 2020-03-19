###JVM优化    

<a href="#part1"/>
**1.为什么要学习JVM优化**

<a href="#part2"/>
**2.JVM的运行参数**  

<a href="#part3"/>
**3.JVM的内存模型**   

<a href="#part4"/>
**4.jstat命令的使用**     

<a href="#part5"/>
**5.jamp命令的使用以及通过MAT工具进行分析** 

<a href="#part6"/>
**6.jstack的使用查看线程状态**   

<a href="#part7"/>
**7.VisualVM工具的使用**

      

<a id="part1"/> 
**1.为什么要学习JVM优化**

在本地开发环境中我们很少会遇到需要对jvm进行优化的需求，但是到了生产环境，我们可能有下面的需求  

- 运行的应用"卡住了",日志不输出，程序没有反应，怎么解决？
- 服务器的CPU负载突然升高，怎么办？
- 在多线程应用下，如何分配线程的数量?
- 如何调试远程运行的tomcat?   


<a id="part2"/>
**2.JVM的运行参数**    

JVM的参数类型分为三类，分别是：  

- 标准参数 
 - -help
 - -version  
 
- -X参数（非标准参数）  
 - -Xint 
 - -Xcomp

- -XX参数（使用率较高） 
 - -XX:newSize 
 - -XX:+UseSerialGC    



> 2.1标准参数

jvm的标准参数，一般都是很稳定的，在未来的JVM版本中不会改变，可以使用  
  
- -help：命令检索出所有的标准参数  
- -version: 查看JVM版本   
- -Dxxxx=值 ： 设置属性xxxx=值，代码中System.getProperty(xxxx)获取值   

可以通过java -server、java -client设置JVM的运行参数  

- server模式JVM的初始化堆空间会大一些，默认使用的是并行垃圾回收器，启动慢运行快。  
- client模式JVM初始化堆空间会小一些，使用串行的垃圾回收器，它的目标是为了让JVM的启动速度更快，但运行速度会比server模式慢些  
- JVM在启动的时候会根据硬件和操作系统自动选择使用server还是client
	- 32位操作系统：
		- 如果是windows系统，不论硬件配置如何，都默认使用client类型的JVM
		- 如果是其他操作系统，机器配置有2GB以上的内存同时有2个以上CPU的话默认使用server模式，否则使用client模式
	- 64位操作系统：只有server模式，不支持client类型

> 2.2 -X 非标准参数

-X参数是jvm的非标准参数，在不同版本的jvm中，参数可能会有所不同，可以通过java -X 查看非标准参数  

- -Xint：解释模式，-Xint标记会强制JVM执行所有的字节码，当然这会降低运行速度，通常10倍或更多

- -Xcomp：编译模式，与 -Xint相反，JVM在第一次使用时会把所有的字节码编译成本地代码，从而带来最来最大程度的优化，但是第一次执行的时候会慢一些  

- -Xmixed：混合模式，将解释模式与编译模式进行混合使用，由jvm自己决定，这是jvm默认的模式，也是推荐的模式    

- Xms:设置jvm堆内存的初始大小 
	- 如：-Xms512m等价于-XX:InitialHeapSize=字节大小,设置jvm初始堆内存为512m  
- Xmx:设置jvm堆内存的最大大小  
	- 如：-Xmx2018m等价于-XX:MaxHeapSize=字节大小,设置jvm最大堆内存为2048m

> 2.3 -XX 非标准参数  

-XX参数也是非标准参数，主要用于jvm的调优和debug操作  
 
-XX参数的使用有2种方式，一种是boolean类型，一种是非boolean类型   

- boolean类型  
	- 格式：-XX:[+-]属性名:表示启用或禁用指定属性名的属性  
	- 如：-XX:+DisableExplicitGC 表示禁用手动调用gc操作，也就是说调用System.gc()无效   
- 非boolean类型 
	- 格式:-XX:属性名=属性值：指定属性名和属性值 
	- 如：-XX:NewRatio=1 表示新生代和老年代的比值    

> 2.4 查看jvm的运行参数 

- 运行java命令时打印参数：需要添加-XX:+PrintFlagsFinal参数即可  
	- 如：java -XX:+PrintFlagsFinal -version  
	- 注意：该种方式打印的参数如果有boolean类型和数字类型，值的操作符是=或:=，分别代表默认值和被修改的值    

- 查看正在运行的jvm参数
	- jinfo -flags <进程id>  
	- linux下查看进程命令：ps -ef|grep tomcat 
	- jps 或者添加jps -l:jdk提供的查看运行的java进程  

- 查看某一个具体的参数
	- jinfo -flag MaxHeapSize 5512  

<a id="part3"/>
**3.JVM的内存模型**      

>3.1 内存划分  

- 线程共享区： 方法区、堆内存   
- 线程独享：本地方法区、程序计数器、栈内存  

>3.2 各区存储内容 

- 方法区：保存的是类信息、常量、静态变量、JIT（即时）编译时代码;  
   
		Class对象是存放在堆区的，不是方法区，这点很多人容易犯错。 
		类的元数据（元数据并不是类的Class对象。Class对象是加载的最终产品， 
		类的方法代码，变量名，方法名，访问权限，返回值等等都是在方法区的） 
		才是存在方法区的

		jvm为每个已加载的类型都维护一个常量池  

		注意：方法区jvm团队在设计上其实当成了永久区来设计，在jkd1.8之后，  
		永久区被废弃了之后，改为metaSpace（元数据区），方法区也被移动到该区域，  
		并且将原来方法区中的常量池移到了堆中

> 3.3 jdk1.7堆内存模型  

- Young年轻区（代） 
    
	Young区被划分为三个部分，Eden区和两个大小严格相同的Survivor区，   
	其中，Survivor区间中，某一时刻只有其中一个是被使用的，另外一个  
	留做垃圾收集时复制对象用，在Eden区间变满的时候，GC就会将存活的  
	对象移动到空闲的Survivor区间中，根据JVM的策略，在经过几次垃圾  
	收集后，依然存活于Survivor的对象将被移动到Tenured区间。  
	Edon和Survivor0、Survivor1 内存大小比为8:1:1  

- Tenured年老区（代）  
  
    Tenured区主要保存生命周期长的对象，一般是一些老的对象，当一些  
	对象在Young复制转移一定次数以后，对象就会被转移到Tenured区，  
	一般如果系统中用了application级别的缓存，缓存中的对象往往会  
	被转移到这一区间    

- Perm永久区    

	Perm代主要保存class、method、field对象，这部分的空间一般不会    
	溢出，除非一次性加载了很多的类，不过在涉及到热部署的应用服务器   
	的时候，有时候会遇到java.long.OutOfMemoryError:PermGen space   
	的错误，造成这个错误的很大原因就有可能是每次都重新部署，但是重新   
	部署后，类的class没有被卸载掉，这样就造成了大量的class对象保存    
	在了perm中，这种情况下一般重新启动应用服务器可以解决问题    


> 3.4 jdk1.8堆内存模型    

- jdk1.8的堆内存模型由2部分组成，年轻代+年老代   
	年轻代：Eden+2*Survivor 
	年老代：OldGen  
	在jdk1.8中变化最大的Perm区，用Metaspace(元数据空间)进行了替换   
	需要特别说明的是：Metaspace所占用的内存空间不是虚拟机内部的，    
	而是在本地内存中，这也是与jdk1.7的永久代最大的区别所在
 
- 为什么jdk1.8要废弃1.7中的永久区？    
	移除永久代是为了融合HotSpot JVM与JRockit VM而做出的努力，  
	因为JRockit没有永久代，不需要配置永久代        

>3.5 总结： 
	一般用new新创建的对象会放到年轻代的Eden区，Eden区满的时候会进行GC,   
	将存活的对象转移到Survivor区，几次GC之后，依然存活于Survivor的对象  
	将被移动到年老代，年老代区间的对象也会被回收。这点在jdk1.7、jdk1.8中都是一样的   

>3.6 本地方法区：就是那些native方法，注意这些也是线程私有的     
 
>3.7 程序计数器：可以理解为指向当前线程调用的指针，是线程私有的    
 
>3.8 方法栈： 保存局部变量、引用等，线程私有  

<a id="part4"/>
**4.jstat命令的使用**          

通过jstat命令进行查看堆内存各部分的使用量，以及加载类的数量 
  
命令格式如下： jstat [-命令选项][vmid][间隔时间/毫秒][查询次数]   

> 4.1 查看class加载统计     

jstat -class 6219    

说明：   

- Loaded:加载class的数量  
- Bytes:所占用空间大小  
- Unloaded:未加载数量  
- Bytes:未加载占用空间  
- Time:时间   

> 4.2 查看编译统计    

jstat -compiler 6219   

说明：  

- Compiled:编译数量  
- Failed:失败数量   
- Invalid:不可用数量  
- Time:时间  
- FailedType:失败类型  
- FailedMethod:失败的方法   

> 4.3 垃圾回收统计  

jstat -gc 6219     

也可以指定打印的间隔和次数，每1秒中打印一次，共打印5次
jstat -gc 6219 1000 5

说明：

- S0C：第一个Survivor区的大小（KB）   
- S1C：第二个Survivor区的大小（KB）   
- S0U：第一个Survivor区使用大小（KB）  
- S1U：第二个Survivor区使用大小（KB）  
- EC：Eden区的大小（KB）
- EU：Eden区使用大小（KB）  
- OC：Old区的大小（KB）
- OU：Old使用大小（KB）   
- MC：方法区大小（KB）  
- MU：方法区使用大小（KB）  
- CCSC：压缩类空间大小（KB）  
- CCSU：压缩类空间使用大小（KB）
- YGC：年轻代垃圾回收次数  
- YGCT：年轻代垃圾回收消耗时间  
- FGC：老年代垃圾回收次数  
- FGCT：老年代垃圾回收消耗时间  
- GCT：垃圾回收消耗总时间      

<a id="part5"/>
**5.jamp命令的使用以及通过MAT工具进行分析**    

前面通过jstat可以对jvm堆的内存进行统计分析，而jmap可以获取更加详细的内容，如：内存使用情况的汇总，对内存溢出的定位于分析  

> 5.1 查看内存使用情况  

jmap -heap pid     

pid 可以使用jps命令查看

说明：  

- Heap Configuration:堆内存配置信息  
- Heap Useage:堆内存的使用情况     
- PS Young Generation: 年轻代
- Eden Space:年轻代eden区  
- From Space: Survivor 使用情况
- To Space:Survivor 使用情况  
- Ps Old Generation:年老代     

> 5.2 查看内存中对象数量及大小   

- 查看所有对象，包括活跃以及非活跃的   	
	- 命令格式：jmap -histo pid | more    注：| more 管道命令，表示分页查看，pid可以通过jps命令查看   
- 查看活跃对象  
	- 命令格式：jmap -histo:live pid | more      注：| more 管道命令，表示分页查看，pid可以通过

对象说明：  

B ：byte  
C ：char  
D ：double  
F ：float  
I ：int  
J ：long  
Z ：boolean  
[ ：数组 ，如[I 表示int[]   
[L+类名：   其他对象数组        

> 5.3将内存使用情况dump到文件中  

有些时候我们需要将jvm当前内存中的情况dump到文件中，然后对它进行分析，jmap也支持dump到文件中。  

- 命令格式：jmap -dump:format=b,file=dumpFileName pid   
	- 说明： pid 可以通过jps命令查看，format=b 表示二进制格式 
	- 示例：jmap -dump:format=b,file=/tmp/dump.dat 6219       

> 5.4 通过jhat对dump文件进行分析   

- 命令格式：jhat -port <port> <file>  
	- 示例：jhat -port 9999 /temp/dump.dat  
	- 说明：解析完成之后，会开启一个服务监听9999端口，可通过ip:9999查看解析结果,并支持OQL 查询   
	
> 5.5 通过MAT（Memory Analyzer Tool）工具对dump文件进行分析     

<a id="part6"/>
**6.jstack的使用查看线程状态**    

有些时候我们需要查看下jvm中的线程执行情况，比如：比如发现服务器的CPU的负载   
突然增高了，出现了死锁、死循环等，我们该如何分析呢？   

由于程序是正常运行的，没有任何输出，从日志方面也看不出什么问题，   
所以就需要看下jvm的内部线程的执行情况，然后再进行分析查找出原因   

这个时候，就需要借助jstack命令了，jstack的作用是将正在运行的jvm  
的线程情况进行快照，并且打印出来    

> 命令格式：jstack pid   注：pid 可以通过jps命令查看 


<a id="part7"/>
**7.VisualVM工具的使用**  
VisualVM，能够监控线程，内存情况，查看方法的CPU时间和内存中的对象，已被GC的对象，  
反向查看分配的堆栈  

> 7.1 启动，在jdk的安装目录的bin目录下，找到jvisualvm.exe，双击打开即可显示图像化界面  

> 7.2 VisualVM也可以查看远程JVM的进程运行状态   
> 
- 借助JMX（java Management Extensions,即java管理扩展）实现  
- 需要现在远程Tomcat服务器修改配置使之支持，在tomcat的bin目录下修改catalina.sh的配置，具体可百度  
- 之后就可以在VisualVM的可视化工具上连接远程服务器






