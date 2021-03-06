####1.java对象的布局---对象的组成  

1.1 对象在内存中的组成

- 对象头、实例数据、对齐填充（数据对齐）   

注意：对齐填充是，java中规定对象要是8个字节的倍数，如果一个对象不是8个字节的倍数，就会进行对齐填充。 数据对齐的理由：比如64位的计算机一次可以处理64位也就是8个字节的数据，如果可以保证每个对象都是8个字节的倍数，可以提供效率   

1.2 打印一个对象在内存中的组成形式 

- 可以添加jol（OpenJDK 开源的一个工具）的maven依赖，调用api来打印      





####2.Java类的加载过程   

参考博客：[https://www.cnblogs.com/wangsen/p/10838733.html](https://www.cnblogs.com/wangsen/p/10838733.html)
 
1 加载 (loading)   

	- 1.1 通过类的全限定名获取该类的二进制字节流。
	- 1.2 将二进制字节流所代表的静态结构转化为方法区的运行时数据结构。
	- 1.3 在内存中创建一个代表该类的 java.lang.Class 对象，注意类对象是放在堆中，  
		  作为方法区这个类的各种数据的访问入口 
	   
怎样获取类的二进制字节流，JVM 没有限制。除了从编译好的 .class 文件中读取，还有以下几种方式：

    从 zip 包中读取，如 jar、war 等
	从网络中获取
	通过动态代理生成代理类的二进制字节流
	从数据库中读取
	
	加载阶段与连接阶段的部分内容交叉进行，但这两个阶段的开始仍然保持先后顺序。

2 连接 (linking)  

	2.1 验证 (Verification）
	确保 Class 文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全

	2.2 准备 (Preparation) 
	为类变量（静态成员变量）分配内存并设置初始值的阶段。这些变量（不包括实例变量）所使用的内存都在方法区中进行分配   

	对于 byte 类型，默认值为零，即（byte）0。
	对于 short 类型，默认值为零，即（short）0。
	对于 int 类型，默认值为零，即 0。
	对于 long 类型，默认值为零，即 0L。
	对于 float 类型，默认值为正零，即 0.0f。
	对于 double 类型，默认值为正零，即 0.0d。
	对于 char 类型，默认值为空字符，即 '\u0000'。
	对于 boolean 类型，默认值为 false。
	对于所有引用类型，默认值为 null。

	2.3 解析 (Resolution)  
	虚拟机将常量池内的符号引用替换为直接引用。会把该类所引用的其他类全部加载进来（ 引用方式：继承、实现接口、域变量、方法定义、方法中定义的本地变量）
	
	https://www.cnblogs.com/shinubi/articles/6116993.html
	
	符号引用：一个 java 文件会编译成一个class文件。在编译时，java 类并不知道所引用的类的实际地址，因此只能使用符号引用来代替。
	
	直接引用：直接指向目标的指针（指向方法区，Class 对象）、指向相对偏移量（指向堆区，Class 实例对象）或指向能间接定位到目标的句柄

3 初始化 (Initialization)  

	执行静态变量的初始化和静态Java代码块，并初始化程序员设置的变量值

4 使用 (Using) 

    使用阶段包括主动引用和被动引用

5 销毁 (Unloading)    

    java堆中不存在该类的任何实例。
    加载该类的ClassLoader已经被回收
    该类对应的java.lang.Class对象没有任何地方被引用，无法在任何地方通过反射访问该类的方法

####3.JMM (java memory model)  

	线程从主内存读取变量值到更新主内存值的过程，对其他线程还有可见性的问题，volicate关键字
- 1. lock  （锁定）
- 2. read（读取）
- 3. load  （载入）
- 4. use （使用）
- 5. assign  （赋值）
- 6. store  （存储）
- 7. write （写入）
- 8. unlock   （解锁）     

####4.可见性

	要实现共享变量的可见性，必须保证两点：  
	
- 线程修改后的共享变量能够及时从工作内存刷新到主内存中  
- 其他线程能够及时把共享变量的最新值从主内存更新到自己的工作内存中   

####5.Java 语言层面支持的可见性实现方式  

- synchronized  
- volatile    
- ReentrantLock

####6.synchronized的功能   
	
- 原子性（同步）  
- 可见性      

JMM 关于synchronized的两条规定： 

- 线程解锁前（线程退出synchronized代码块前），必须把共享变量的最新值刷新到主内存中   
- 线程加锁时，如果工作内存中共享变量的值被标记为失效，需要从主内存中重新读取最新的值（注意：加锁与解锁需要是同一把锁）  

这样就实现了线程解锁前对共享变量的修改在下次加锁时对其他线程可见

####7.指令重排序  
	重排序：代码书写的顺序与实际执行的顺序不同，指令重排序是编译器或处理器为了提高程序性能而做的优化，经过重排序的代码可能更符合cpu执行的特点。  

- 1. 编译器优化的重排序 （编译器优化）
- 2. 指令级并行重排序（处理器优化），双核或多核  
- 3. 内存系统的重排序（处理器优化），内存系统读写缓存做的优化

特点：as-if-serial  
	
- 无论如何重排序，程序执行的结果应该与代码顺序执行的结果一致（Java编译器、运行时和处理器都会保证Java在单线程下遵循as-if-serial语意）    
- 重排序不会给单线程带来内存可见性问题   
- 多线程中程序交错执行时，重排序可能会造成内存可见性问题     


####8. volatile 实现可见性   

深入来说：通过加入内存屏障和禁止重排序优化来实现的 

- 对volatile变量执行写操作时，会在写操作后加入一条store屏障指令   
- 对volatile变量执行读操作时，会在读操作前加入一条load屏障指令  

通俗的讲：volatile变量在线程内每次被访问时，都强迫从主内存中重读该变量的值，   
而当该变量发生变化时，又会强迫线程将最新的值刷新到主内存，这样任何时刻，不同的线程总能看到该变量的最新值    

线程写volatile变量的过程：

- 1.改变线程工作内存中volatile变量副本的值   
- 2.将改变后的副本的值从工作内存刷新到主内存   

线程读volatile变量的过程  

- 1.从主内存中读取volatile变量的最新值到线程的工作内存中   
- 2.从工作内存中读取volatile变量的副本     

####9.volatile 不能保证变量复合操作的原子性   

```java

	// synchronized 代码块里的复合操作可以保证一个线程完整的执行的完毕之后，
	// （即使执行过程中丢失cpu的执行权，暂停之后也会继续执行）  
	// 再由其他获取锁的线程执行。
	synchronized(this){
		number++;
	}

	//但是volatile 修饰的变量，类似于number++ 复合操作，却不能保证一个线程完整  
	//的执行完成，执行过程中可能丢失cpu执行权，其他线程也可以修改number的值，当该
	//线程再次获取到cpu执行权时会接着继续执行，但是由于被volatile修饰，其他线程的  
	//改动该线程可以立刻看到，所以会用最新的number值继续后续的操作
	public static volatile nummber = 0;

	number++;

	//1.读取number最新值 
	//2.将number的值加1  
	//3.写入最新的number的值

```   

总结：volatile 保证可见性，不保证原子性   

####10.volatile 使用场景   

要在多线程中安全的使用volatile变量，必须同时满足：  

1.对变量的写入操作不依赖其当前值  
 
- 不满足：bumber++ 、 count = count*5;   

2.该变量没有包含在具有其他变量的不变式中  

- low 和 up 都是volatile 修饰的值  ，那么low<up 就不满足     
	  

####11.synchronized 和 volatile 比较  

- volatile 不需要加锁，比synchronized 更轻量级，不会阻塞线程  
- 从内存可见性角度讲，volatile读相当于加锁，volatile写相当于解锁  
- synchronized 技能保证可见性，又能保证原子性，而volatile只能保证可见性，无法保证原子性    

####12.synchronized 和 reentrantLock 的区别   

- jdk1.6 之前，synchronized 是重量级锁,它会调用操作系统内核完成线程的同步，  
- jdk1.8 之后，synchronized 经过优化，提升了效率，和reentrantLock不相上下  

注意： 

	1.reentrantLock 不涉及调用操作系统，底层是通过AQS来完成的，是轻量级锁，比较快;     
	  如果多个线程交替执行，其实reentrantLock 和队列无关，没用到队列，  
	  在jdk级别就解决了同步问题,只有存在线程竞争时才用到了底层的AQS队列  
   
	2.将一个线程进行挂起是通过park方法实现的，调用 park后，线程将一直阻塞直到超时 
	  或者中断等条件出现.unpark可以终止一个挂起的线程，使其恢复正常。整个并发框架中 
	  对线程的挂起操作被封装在 LockSupport类中，LockSupport类中有各种版本pack方法， 
	  但最终都调用了Unsafe.park()方法  

	3. ReentrantLock中有个private volatile int state 的变量，就是锁的同步状态标志位

reentrantLock实现的主要技术点：自旋、park--unpark、 CAS(compare and set) 原子操作

