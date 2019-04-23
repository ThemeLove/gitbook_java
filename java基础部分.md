####1.java中的比较: == 

	基本类型：比较的是数据值是否相同 
	引用类型：比较的是地址的hash值是否相同 

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


		
