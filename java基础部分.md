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
	
	JVM:是java虚拟机，不同平台有不同平台的Java虚拟机，从而实现跨平台


####10.逻辑运算符

|符号|作用|说明|
|---|---|:---|  
|&| 逻辑与|a&b，a和b都是true，结果为true，否则为false|
|I| 逻辑或|aIb，a和b都是false，结果为false，否则为true|
|^| 逻辑异或|a^b，a和b结果不同为true，相同为false|
|!|逻辑非|！a，结果和a的结果正好相反


####11.变量和常量的运算（类型转换） 
例子1：  

```java
	
	public static void main(String[] args){
		byte b1=1;
		byte b2=2;
		byte b3=1+2;
		byte b4=b1+b2;  //编译失败
		System.out.println(b3);
		System.out.println(b4);
	}

	分析： b3 = 1 + 2 ， 1 和 2 是常量，为固定不变的数据，在编译的时候（编译器javac），  
	已经确定了1+2的结果并没有超过byte类型的取值范围，可以赋值给变量b3 ，因此b3=1 + 2 是正确的。   
	反之， b4 = b2 + b3 ， b2 和 b3 是变量，变量的值是可能变化的，在编译的时候，编译器javac不确定b2+b3    
	的结果是什么，因此会将结果以int类型进行处理，所以int类型不能赋值给byte类型，因此编译失败。

```  

例子2：  

```java 

	public static void main(String[] args){
		short s = 1;
		s+=1;
		System.out.println(s);
	}

	分析： s += 1 逻辑上看作是s = s + 1 计算结果被提升为int类型，再向short类型赋值时发生错误，  
	因为不能将取值范围大的类型赋值到取值范围小的类型。    
	但是， s=s+1进行两次运算， += 是一个运算符，只运算一次，并带有强制转换的特点，   
	也就是说s += 1 就是s = (short)(s + 1) ，因此程序没有问题编译通过，运行结果是2
```

####12.构造方法 
	1.如果不提供构造方法，系统会给出无参数构造方法  
	2.如果提供了构造方法，系统将不再提供无参数构造方法
	3.构造方法是可以重载的，既可以定义参数，也可以不定义参数     

####13.键盘录入 Scanner 
	构造方法：
	public Scanner(InputStream inputStream) 构造一个新的 Scanner，它生成的值是从指定的输入流扫描的 

	成员方法： 
	String nextLine = scanner.nextLine();
	int nextInt = scanner.nextInt();    

####14.随机数  Random  
	构造方法： 
	public Random()  创建一个新的随机数生成器，seed默认为系统时间System.currentTimeMillis()
	public Random(long seed) 指定seed创建一个随机数生成器  

	成员方法： 
	int nextInt2 = random.nextInt(); 随机返回一个int范围内的值
	int nextInt3 = random.nextInt(10);返回一个伪随机数，范围在 0 （包括）和指定值 n （不包括）之间的 int 值。

	boolean nextBoolean = random.nextBoolean();

####15.String 
	字符串是常量，它们的值在创建后不能被更改,所以每当进行字符串拼接时，总是会在内存中创建一个新的对象

	String类的构造方法：  
	public String():初始化新创建的String对象，以使其表示空字符序列  
	public String(char[] value):通过当前参数中的字符数组来构造新的String  
	public String(byte[] bytes):通过使用平台的默认字符集解码当前参数中的字节数组来构造新的String  
	直接赋值的方法创建字符串对象  

	字符串的底层就是字节数组byte[]

	常用方法： 
	比较：
	public boolean equals(Object anObject): 将此字符串与指定对象进行比较  
	public boolean equalsIgnoreCase(String anotherString):将此字符串与指定对象进行比较，忽略大小写。  
	
	获取：
	public int length():返回此字符串的长度  
	public String concat(String str):将指定的字符串连接到该字符串的末尾   
	public char charAt(int index):返回指定索引出的char值  
	public int indexOf(String str):返回指定子字符传第一次出现在该字符串内的索引  
	public String substring(int beginIndex):返回一个子字符串，从beginIndex开始截取字符串到字符串结尾
	public String substring(int beginIndex, int endIndex):返回一个字符串，从beginIndex到endIndex截取字符串。含beginIndex,不含endIndex   
	
	转换：    
	public char[] toCharArray():将此字符串转换为新的字符数组 
	public byte[] getBytes():使用平台的默认字符集将该String编码转换为新的字节数组   
	public String replace(CharSequence target,CharSequence replacement):将与target匹配的字符串使用replacement字符串替换  

	分割：   
	public String[] split(String regex):将此字符串按照给定的regex(规则)拆分为字符串数组  

####16.StringBuilder     
	StringBuilder又称为可变字符序列，它是一个类似于 String 的字符串缓冲区，通过某些方法调用可以改变该序列的长度和内容  
	它的内部拥有一个数组用来存放字符串内容，进行字符串拼接时，直接在数组中加入新内容。   
	StringBuilder会自动维护数组的扩容。原理如下图所示：(默认16字符空间，超过自动扩充)  

	构造方法： 
	public StringBuilder():构造一个空的StringBuilder容器 
	public StringBuilder(String str):构造一个StringBuilder容器，并将字符串添加进去   

	常用方法： 
	public StringBuilder append(...):添加任意类型数据的字符串形式   
	public StringBuilder reverse():返回反转的字符序列  
	public String toString():将当前StringBuilder对象转换为String对象   

####17.StringBuilder 和 String 相互转换  
	1.StringBuilder 转换为 String 
	public String toString() : 通过toString()就可以实现把StringBuilder 转换为String   
	
	2.String转换为 StringBuilder  
	public StringBuilder(String s):通过构造方法就可以实现把 String 转换为 StringBuilder    
  
####18.String 和 StringBuilder的区别  
	String 类：内容是不可变的  
	StringBuilder类：内容是可变的    

####19.ArrayList  
	ArrayList 是大小可变的数组的实现，只能存储引用类型的数据，不能存储基本类型。    
	
	构造方法： 
	public ArrayList();  构造一个内容为空的集合       
	
	常用方法和遍历   
	public boolean add(E e):将指定的元素添加到此集合的尾部    
	public void add(int index, E element):在此集合中的指定位置插入指定的元素   
	public boolean remove(Object o):删除指定的元素，返回删除是否成功  
	public E remove(int index):移除此集合中指定位置上的元素。返回被删除的元素 
	public E set(int index,E element):修改指定索引处的元素，返回被修改的元素  
	public E get(int index):返回此集合中指定位置上的元素，返回获取的元素    
	public int size():返回此集合中的元素数，遍历集合时，可以控制索引范围，防止越界   

####20.各种数据类型占用字节大小  

|数据类型|占用空间大小(字节) 1 byte = 8 bit ,千字节就是平时所说的1kb|
|:---:|:---|
|byte|1|
|char|2|
|short|2|
|int|4|
|long|8|
|float|4|
|double|16|
|boolean|true/false(理论上占用1bit，即1/8字节，实际处理按1byte处理)  

####21.字符流只能对文本文件使用，不能对音视频等非字符格式的文件操作，否则可能会丢失内容。    
	

