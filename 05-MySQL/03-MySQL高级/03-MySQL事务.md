##事务
####1.为什么要有事务？
事务广泛的运用于订单系统、银行系统等多种场景    
 
例如：

> A用户和B用户是银行的储户，现在A要给B转账500元，那么需要做以下几件事：

> 1.检查A的账户余额>500元；  
> 2.A 账户中扣除500元;  
> 3.B 账户中增加500元;  

正常的流程走下来，A账户扣了500，B账户加了500，皆大欢喜。  
那如果A账户扣了钱之后，系统出故障了呢？A白白损失了500，而B也没有收到本该属于他的500。  
以上的案例中，隐藏着一个前提条件：A扣钱和B加钱，要么同时成功，要么同时失败。 
 
事务的需求就在于此  
所谓事务,它是一个操作序列，这些操作要么都执行，要么都不执行，它是一个不可分割的工作单位。   

例如，银行转帐工作：从一个帐号扣款并使另一个帐号增款，这两个操作要么都执行，要么都不执行。  
所以，应该把他们看成一个事务。事务是数据库维护数据一致性的单位，在每个事务结束时，都能保持数据一致性   

####2.事务四大特性(简称ACID)
* 原子性(Atomicity)
* 一致性(Consistency)
* 隔离性(Isolation)
* 持久性(Durability)  

一个很好的事务处理系统，必须具备这些标准特性：
原子性（atomicity）  
> 一个事务必须被视为一个不可分割的最小工作单元，整个事务中的所有操作要么全部提交成功，要么全部失败回滚，对于一个事务来说，不可能只执行其中的一部分操作，这就是事务的原子性

一致性（consistency） 
> 数据库总是从一个一致性的状态转换到另一个一致性的状态。（在前面的例子中，一致性确保了，即使在执行第三、四条语句之间时系统崩溃，支票账户中也不会损失200美元，因为事务最终没有提交，所以事务中所做的修改也不会保存到数据库中。）

 隔离性（isolation）
>通常来说，一个事务所做的修改在最终提交以前，对其他事务是不可见的。（在前面的例子中，当执行完第三条语句、第四条语句还未开始时，此时有另外的一个账户汇总程序开始运行，则其看到支票帐户的余额并没有被减去200美元。）

持久性（durability）
> 一旦事务提交，则其所做的修改会永久保存到数据库。（此时即使系统崩溃，修改的数据也不会丢失。）   

    以下内容出自《高性能MySQL》第三版，了解事务的ACID及四种隔离级有助于我们更好的理解事务运作。
    
    下面举一个银行应用是解释事务必要性的一个经典例子。假如一个银行的数据库有两张表：支票表（checking）和储蓄表（savings）。   
	现在要从用户Jane的支票账户转移200美元到她的储蓄账户，那么至少需要三个步骤：
    
    检查支票账户的余额高于或者等于200美元。
    从支票账户余额中减去200美元。
    在储蓄帐户余额中增加200美元。
    上述三个步骤的操作必须打包在一个事务中，任何一个步骤失败，则必须回滚所有的步骤。
    
    可以用START TRANSACTION语句开始一个事务，然后要么使用COMMIT提交将修改的数据持久保存，要么使用ROLLBACK撤销所有的修改。事务SQL的样本如下：
    
    start transaction;
    select balance from checking where customer_id = 10233276;
    update checking set balance = balance - 200.00 where customer_id = 10233276;
    update savings set balance = balance + 200.00 where customer_id = 10233276;
    commit;   

####3.事务命令  
表的引擎类型必须是innodb类型才可以使用事务，这是mysql表的默认引擎  

    1.开启事务，命令如下：
      开启事务后执行修改命令，变更会维护到本地缓存中，而不维护到物理表中
	    begin;
	    或者
	    start transaction; 

    2.提交事务，命令如下
      将缓存中的数据变更维护到物理表中
      commit; 

    3.回滚事务，命令如下：
      放弃缓存中变更的数据
      rollback; 

    注意:
      修改数据的命令会自动的触发事务，包括insert、update、delete
      而在SQL语句中有手动开启事务的原因是：可以进行多次数据的修改，如果成功一起成功，否则一起会滚到之前的数据   

####4.隔离级别   
-   Read uncommitted（读未提交）：最低级别，任何情况都无法保证 
-   Read committed（读已提交）：可避免脏读的发生
-   Repeated read（可重复读）ge：可避免脏读、不可重复读的发生
-   Seriable（串行化）：可避免脏读、不可重复读、幻读的发生。就相当于单线程串行化，效率低
	
视频教程地址（鲁班学院老师实操演示数据库隔离级别带来的问题）：[https://www.bilibili.com/video/av60449909?from=search&seid=9472250175177092868](https://www.bilibili.com/video/av60449909?from=search&seid=9472250175177092868)   

####5.mysql 数据库引擎
> 1 mysql 中的数据库引擎有InnoDB、MyISAM类型   
> 2 InnoDB 和MyISAM 的区别和联系  
> 
- InnoDB 支持数据库事务，支持行级锁、实现外键约束,写多的时候用InnoDB
- MyISAM不支持数据库事物，不支持行级锁和外键，支持表级锁；查询快增删慢，读多的时候用MyISAM
- 
- MySql 在5.1之前默认存储引擎是MyISAM,在此之后默认引擎是InnoDB

####6.mysql 排他锁  
	3.1 查询时添加排他锁，事物结束才会释放锁，多线程并发操作时，其他事物会阻塞，可以用来做分布式锁。 

	会给符合查询条件的行都加锁
	
	使用前请确保开启了事物，如果用框架默认开启了事物

	select * from user for update;    

	3.2 事物中insert、update、delete 也会默认加锁    


	
	 
	