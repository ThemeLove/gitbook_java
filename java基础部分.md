####1.java中的比较: == 

	基本类型：比较的是数据值是否相同 
	引用类型：比较的是内存地址，注意内存地址不等同与hashCode方法的返回值

####2.字符串比较 
	
	equals:比较字符串值是否相等 
	==：比较的是地址hash值 

	特别注意：
	String a = new String("abc");
	String b = "abc";
	String c = "abc";
	a.equals(b) ----->true
	a==b  ----->false
	b.equals(c) ----->true
	b==c ----->true

	因为:  
		1.通过关键字new 出来的字符串会单独开辟一个内存空间，内存地址hash值不同(Object对象的hashCode（）方法的返回值)，每次都不一样；    
		2.直接赋值的形式的字符串都会进入字符串常量池，全局共享一个  

####3.数据类型转换 
	Java程序中要求参与的计算的数据，必须要保证数据类型的一致性，如果数据类型不一致将发生类型的转换。   

	1.将取值范围小的类型自动提升为取值范围大的类型 
	int i = 1;
	byte b =2; 
	运算结果，变量的类型将是int类型，byte 的范围小，自动提升为int类型    
	
	2.范围小的类型向范围大的类型提升，byte、short 、char 运算时直接提升为int 
	byte、short 、char ----->int----->long----->float----->double
	
####4.java.lang包下的所有类无需导入，默认导入      
####5.方法参数： 
	基本数据类型作为方法参数，形式参数的改变不影响实际参数；  
	引用类型作为方法参数，形式参数的改变会影响实际参数。    
####6.Java虚拟机的内存划分  
|--|--|
|区域名称|作用|
|寄存器|给cpu使用，和我们无关|
|本地方法栈|JVM在使用操作系统功能的时候使用，和我们无关|
|方法区|存储可以运行的class文件|
|堆内存|存储对象或者数组，new来创建的，都存储在堆内存|
|方法栈|方法运行时使用的内存，比如main方法运行，进入方法栈中执行|  

####7.水仙花数： 
	水仙花数：是一个三位数（100~999），个位数的立方、十位数的立方、百位数的立方等于该数本身  

代码示例：
```java

	for(int k=100;k<1000;k++){
		int ge=k%10;
		int shi=k/10%10;
		int bai=k/10/10%10;
		if (ge*ge*ge+shi*shi*shi+bai*bai*bai==k) {
			System.out.println("水仙花数="+k);
		}
	}

	水仙花数=153
	水仙花数=370
	水仙花数=371
	水仙花数=407

```

####8.反转一个int数组  
```java 

	int [] intArr = new int[]{1,2,3,4,5,6,7,8};
	for(int start=0,end=intArr.length-1;start<=end;start++,end--){
		int temp = intArr[start];
		intArr[start]=intArr[end];
		intArr[end]=temp;
	}
```   

####9.JDK(Java Development Kit) 和 JRE(Java Runtime Environment)  和 JVM(Java Virtual Machine)
	JDK:是java程序开发工具包，包含JRE 和开发人员使用的工具(比如java.exe、javac.exe) 
		
	JRE:是java的运行时环境，包含JVM 和 核心内库   
	
	JVM:是java虚拟机，不同平台有不同品台的Java虚拟机，从而实现跨平台


