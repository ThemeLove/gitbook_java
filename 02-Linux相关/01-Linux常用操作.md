###1.Ubuntu上修改计算机名称 
	步骤：
		1.打开终端，输入sudo gedit /etc/hostname,输入密码，打开hostname文件，修改计算机名称，之后保存退出。 
		2./etc/hosts中也用到了主机名，修改计算机名的时候最好一并修改，输入sudo gedit /etc/hosts,打开hosts文件， 
		修改计算机名称，之后保存退出。 
		3.重启系统之后才能生效。 
###2.设置终端Terminal的光标形状
	在终端里右键----->Preferences----->Unnamed找到Cursor更改Cursor shape即可   

####3.Linux命令ipconfig不同的参数可以对网卡做不同的操作，比如
|	参数		|	作用		|	例子		|
|	:--:	|   :--:	|	:--	|
|	up		|	开启网卡	|	ifconfig ens40 up (ens40 是通过ifconfig查看的网卡名)
|	down	|	关闭网卡	|	ifconfig ens40 down (ens40 是通过ifconfig查看的网卡名)
####4.ip分为ipv4和ipv6，现在常用的是ipv4
    ip其中的一种分类为网络号和主机号来分的话分为A类、B类、C类、D类,(以ipv4为例)

    1． A类IP地址
    一个A类IP地址由1字节的网络地址和3字节主机地址组成，网络地址的最高位必须是“0”， 地址范围从1.0.0.0 到126.0.0.0。可用的A类网络有126个，每个网络能容纳1亿多个主机。
    
    2． B类IP地址
    一个B类IP地址由2个字节的网络地址和2个字节的主机地址组成，网络地址的最高位必须是“10”，地址范围从128.0.0.0到191.255.255.255。可用的B类网络有16382个，每个网络能容纳6万多个主机 。
    
    3． C类IP地址：（一般常见的局域网都是这种）
    一个C类IP地址由3字节的网络地址和1字节的主机地址组成，网络地址的最高位必须是“110”。范围从192.0.0.0到223.255.255.255。C类网络可达209万余个，每个网络能容纳254个主机。
    
    4． D类地址用于多点广播（Multicast）。
    D类IP地址第一个字节以“lll0”开始，它是一个专门保留的地址。它并不指向特定的网络，目前这一类地址被用在多点广播（Multicast）中。多点广播地址用来一次寻址一组计算机，它标识共享同一协议的一组计算机。
	
	
	私有ip
	在这么多网络ip中，国际规定有一部分ip地址是用于我们的局域网使用，也就是属于私网ip,不在公网中使用，它们的范围是：
		10.0.0.0 ~ 10.255.255.255
		172.16.0.0 ~ 172.31.255.255 
		192.168.0.0 ~ 192.168.255.255 
		
	注意:
		192.168.0.1或者192.168.0.0 就可以进入路由器配置
		ip地址 127.0.0.1 ~ 127.255.255.255 用于回路测试，比如127.0.0.1 可以代表本机ip地址，用http://127.0.0.1 就可以测试本机中配置的web服务器
####5.端口号，TCP/IP协议中的端口号，范围是0~65535，通常主机上的一个应用程序对应一个进程，一个进程对应一个端口。
	1.0~1024之间通常称为常用端口，比如常见的Http服务端口号为80，FTP服务端口为21
	 