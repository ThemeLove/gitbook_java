####一.SpringDataJpa的原理 

	底层还是使用jpa的原生api来实现 
	实现封装在SimpleJpaReposity类中，由此类提供dao的支持 
####二.继承JpaRepository的Dao，自带的常用的方法

>1.继承JpaRepositity的Dao，自带的常用的方法
 
- findOne:根据id查询，即时加载 
- getOne：根据id查询，懒加载，底层调用的entityManager.getRefrence方法  
- findAll:查询全部，不带参数默认查询全部 
	-  带参数Pageable（接口）,进行分页查询，实现类有PageRequest    
		- Page page = new Page(pageIndex,size) ,pageIndex:页面从0开始，size:每页的行数  
		- Page对象中有totalElements:总记录数、totalPages:总页数、content:结果列表
- 排序：Sort	
	- Sort sort = new Sort(Sort.Direction.Desc,"custId"); 参数1：排序规则、参数2：排序字段		  
- count：总记录数
	count()方法，比如：customerDao.count();  
- exists:是否存在  
	exists(long id)，比如：customerDao.exists(2) 

####三.使用jpql
  
>1.使用jpql查询

- 使用步骤：  
	- 1.在到接口中定义一个方法，使用方法的参数设置jpql的参数，使用方法的返回值接收查询结果  
	- 在方法上添加一个@Query注解 
	- 在注解中编写jpql 

- 查询全部：(以Customer表为例) 

		@Query("from Customer") 
		List<Customer> getAllCustomer();  

- 分页查询：同样使用Pageable,添加到定义方法中  
		
		@Query("from Customer") 
		List<Customer> getAllCustomerByPage(Pageable pageable); 

- 带参数查询:参数定义到接口方法里，多个参数依次对应jpql中的占位符，参数传递需要调用者保证
		
		注意：占位置 ?1 指定第一个参数，？2 指定第二个参数，一次类推？n 代表第n个参数

		@Query("from Customer where custName like ? and custAddress like ？") 
		List<Customer> getCustomerById(String custName, String custAddress);  
 
>2.使用jpql更新  

- 使用方法： 
	- 1.在dao接口中定义一个方法  
	- 2.在方法上添加@Query注解，在注解中编写jpql语句 
	- 3.在方法上添加@Modifing注解 
	- 4.在方法上添加@Transactional     

- 更新: 

		@Query("update Customer set custSource=?1 where custId=?2")
		@Modifying
		@Transactional
		void updateSource(String source, long id);	  
 
>3.删除操作同更新  

>4.jpql不支持插入操作  


####四.使用原生sql(一般不推荐)  

- 使用方法 
	- 1.在dao中定义一个方法 
	- 2.在方法上添加@Query注解 
	- 3.在注解中添加原生的sql语句，添加一个属性nativeQuery=true      
		
	例： 

		@Query(value="select * from customer where custname like ? and address like ?2")
		List<Customer> getCustomerListByNative(String custname, String address);
	
####五.方法命令规则查询  
	通过一定的规则定义一个方法，框架就可以根据方法名生成一个sql语句进行查询 

- 规则  
	- 1.在dao中定义一个方法，不用写添加@Query注解
	- 2.应该使用findBy开头 
	- 3.查询某个字段，findBy后跟实体类的属性的名称，默认是等于 
	- 4.如果有多个条件，就在方法加And+实体类的属性名 
	- 5.方法的参数对应查询定义 
	- 6.返回值根据返回的数据类型定义 
	- 7.如果需要分页，在方法中添加Pageable参数 

	例： 

	Pageable<Customer> findByCustNameLikeAndCustAddressLike(String name, String address,Pageable pageable);  

     
####六.使用Specification方式进行查询 
	说明书方式查询，最强大的查询方式，最复杂的查询方式  

- 使用方法：  
	- 1.需要dao继承JpaSpecificationExecutor接口 
	- 2.使用JpaSpecificationExecutor接口中提供方法查询，每个方法都需要使用specification对象作为参数
    
- JpaSpecificationExecutor接口介绍  

```java

	public interface JpaSpecificationExecutor<T> {
	    Optional<T> findOne(@Nullable Specification<T> var1);
	
	    List<T> findAll(@Nullable Specification<T> var1);
	
	    Page<T> findAll(@Nullable Specification<T> var1, Pageable var2);
	
	    List<T> findAll(@Nullable Specification<T> var1, Sort var2);
	
	    long count(@Nullable Specification<T> var1);
	}

```	

- Specification接口介绍 
	- toPredicate方法：返回一个查询条件，在方法中应该创建一个查询条件并返回  
		- 返回值：Predicate:查询条件 
		- 参数：  
			- Root:代表sql语句的根节点，相当于sql语句中的from后的表名  
			- CriteriaQuery: 
				- 查询对象：sql语句中的关键字包含在其中 Where、order by、group by、having  
			- CriteriaBuilder:工具对象 
				- 所有条件的生成都是使用CriteriaBuilder，只要涉及判断都需要使用CriteriaBuilder对象

	 

	
		 
	
	  
	    

