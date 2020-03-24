####一.Mybatis的xml方式的常用配置 
```xml

	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE configuration
	        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
	        "http://mybatis.org/dtd/mybatis-3-config.dtd">
	<configuration>
	    <!--
	    具体更加详情的配置请查看mybatis官网
	    https://mybatis.org/mybatis-3/zh/getting-started.html
	    -->
	
	    <!--引入外部属性文件-->
	    <properties resource="jdbc.properties"/>
	
	    <!--核心配置文件的全局的设置-->
	    <settings>
	        <!--开启全局的延迟加载-->
	        <setting name="lazyLoadingEnabled" value="true"/>
	        <!-- 如果是true，则立即加载-->
	        <!-- 如果是false，则按需加载-->
	        <!--如果小于3.4.1 则必须配置,大于：3.4.1. 可以省略-->
	        <setting name="aggressiveLazyLoading" value="false"/>
	        <!--开启二级缓存： 默认是开启状态，可以不配置-->
	        <setting name="cacheEnabled" value="true"/>
	    </settings>
	
	    <typeAliases>
	        <!--配置包的别名，配置好之后，该包中的类可以直接使用类名配置
	            比如com.mybatis.dao.User 配置的时候就可以直接写User
	        -->
	        <package name="com.mybatis.dao"/>
	    </typeAliases>
	
	    <!--default 可以指定用哪个环境的配置-->
	    <environments default="development">
	
	         <!--数据库的环境:
	        default； 指定默认的环境
	        -->
	        <environment id="development">
	
	            <!--事务管理： jdbc-->
	            <transactionManager type="JDBC"/>
	            <!--
	               dataSource: 数据源（数据库连接池）配置
	               type="POOLED" ： 数据源的类型配置
	                   POOLED :使用mybatis的自带数据源配置
	                   UNPOOLED: 不使用数据源配置， 使用Connection操作数据库
	                   JNDI:JNDI服务 数据源配置
	            -->
	            <dataSource type="POOLED">
	                <property name="driver" value="${jdbc.driver}"/>
	                <property name="url" value="${jdbc.url}"/>
	                <property name="username" value="${jdbc.username}"/>
	                <property name="password" value="${jdbc.password}"/>
	            </dataSource>
	        </environment>
	    </environments>
	    
	    <mappers>
	            <!--该种方式不推荐，因为要配置多个-->
	<!--        <mapper resource="AccountDao.xml"/>-->
	
	        <!-- mapper配置了package之后，对应的mapper文件要和接口在统计目录下
	            比如java代码中的com.mybatis.dao.UserDao中定义接口，那么对应的UserDao.xml的
	            mapper文件必须要和接口类名同名，且在resources中的路径也必须一致：com.mybatis.dao.UserDao.xml
	            这样UserDao.java和UserDao.xml在编译之后会在同级目录下，而mybatis动态代理方式正式通过这种方式
	            根据接口的全限定类名寻找Mapper.xml文件的
	         -->
	        <package name="com.mybatis.dao"/>
	    </mappers>
	
	</configuration>

```

####二.一对一方式查询  

xml 方式
AccountDao.xml 一条sql,没有引用UserDao.xml中的查询sql

```xml

    <resultMap id="accountUserResultMap" type="account">
        <id column="id" property="id"/>
        <association property="user" javaType="user">
            <result column="user_id" property="id"/>
            <result column="user_name" property="username"/>
            <result column="birthday" property="birthday"/>
            <result column="sex" property="sex"/>
            <result column="address" property="address"/>
        </association>
    </resultMap>

    <select id="findById" parameterType="int" resultMap="accountUserResultMap">
        select * from account as a inner join user as u on a.uid=u.user_id where a.id = #{accountId}
    </select>

```


####三.一对多 

> 1.xml方式  

UserDao.xml

```xml

	    <resultMap id="userResultSplit" type="user">
	        <id property="id" column="user_id"/>
	        <result column="user_name" property="username"/>
	        <result column="birthday" property="birthday"/>
	        <result column="sex" property="sex"/>
	        <result column="address" property="address"/>
	        <collection property="accountList" column="user_id" select="com.mybatis.dao.AccountDao.findByUid"/>
	    </resultMap>
	
	    <select id="findByIdSplit" parameterType="int" resultMap="userResultSplit">
	        select * from user where user_id = #{id}
	    </select>
```

AccountDao.xml

```xml1

    <select id="findByUid" parameterType="int" resultType="account">
        select * from account where uid = #{id}
    </select>
```

> 2.注解方式 

UserDao 

```java

    @Results(id="userResult", value={
            @Result(id = true,column = "user_id",property = "id"),
            @Result(property = "username",column = "user_name"),
            @Result(property = "accountList",column = "user_id",
                    many = @Many(select = "com.mybatis.dao.AccountDao.findByUid",fetchType = FetchType.LAZY)),
    })
    @Select("select * from user where user_id = #{userId}")
    User findById(Integer userId);

```
AccountDao 

```java 

    @Select("select * from account where uid = #{uid}")
    List<Account> findByUid(Integer uid);

```

####四.@SelectKey 和 @Option 实现主键回显   
	主键回显用于在插入数据时获取新插入数据的主键   

>1. SelectKey 方式，该方式比较通用，可以用在Mysql中或oracle中

	//  保存获取主键(主键回显):
	// @SelectKey   在oracle中会使用
	// keyProperty: 主键的属性名
	// keyColumn ：主键列名
	// resultType: 主键的类型
	// before=false : 不是在添加之前， 添加之后查询
	// before=true : 在添加之前查询
	// statement: 需要执行的sql语句
	// select last_insert_id() :最后一次执行添加生成的主键 

	xml方式：

	<selectKey keyColumn="uid" keyProperty="id" resultType="int" order="AFTER">
		select last_insert_id()
	</selectKey>  

	注解方式：
	@SelectKey(statement = "select last_insert_id()",keyColumn = "user_id",keyProperty = "id",resultType = Integer.class, before=false)
    @Insert("insert into user values(null ,#{username},#{password},#{sex},#{address},#{birthday})")
    public void insert(User user);

>2. Option 方式 

	该方式可适用于自增主键的数据库，比如mysql. oracle就不行

	注解方式
    @Options(useGeneratedKeys = true, keyProperty = "ID")
    void insertOperateLog(OperateLog operateLog);


####五.动态在注解中的使用

	动态sql在xml和主机方式中都可以使用
> 1.注解中使用动态sql方式一：

使用 @SelectProvider 注解
	
定义提供类 

```java

	public class UserDaoProvider {
	
	    public String findByNameAndAddressLike(String name, String address){
	        StringBuffer sb = new StringBuffer();
	        return sb.append("select * from user where 1 = 1 ")
	        .append(name.isEmpty()? "":" and user_name=#{param1}")
	        .append(address.isEmpty()?"":" and address=#{param2}").toString();
	    }
	}

```
UserDao 中使用 

```java

    @SelectProvider(type=UserDaoProvider.class,method = "findByNameAndAddressLike")
    List<User> findByNameAndAddressLike(String name, String address);
```

> 2.注解中使用动态sql方式二  

动态拼接sql，和在xml中使用一样 

```java

	  @Select("<script>" +
	            "select * from CoursewareAssociation where `ROOMID` = #{roomId} " +
	            "<if test='cid != 0'>" +
	            "and `CID` = #{cid}" +
	            "</if>" +
	            "<if test='vid &gt;= 0'>" +
	            "and `VID` = #{vid}" +
	            "</if>" +
	            "</script>")
	    List<CoursewareAssociation> selectCoursewareAssociation(CoursewareAssociation coursewareAssociation);

```









####六. Mybatis 的一级缓存 

```xml

	/**
	 * 一级缓存
	 *  1,在同一sqlSession对象范围下，两次执行同一个sql语句，第二层没有执行sql语句，说明缓存的存在
	 *  2.如果执行了增删改,提交操作，会清空缓存
	 *  3. sqlSession.clearCache();清空缓存
	 *  4. 一级缓存是SqlSession级别的, 必须是在一个sqlsession对象范围下才可以的得到一级缓存
	 *
	 * 一级缓存运行流程
	 *  第一次执行sql语句，查询到数据，在会在一级缓存中存储sql语句和数据，以 sql语句为key值, 以数据为value值
	 *  在第二层执行sql语句时，会先从缓存中查询 ，以sql为key查询，得到数据，直接返回，如果没有相应的sql语句，则查询数据库
	 *
	 */

```

####七.Mybatis 的二级缓存 

```java

	/**
	 * 二级缓存
	 *   1. 是sessionFactory级别的, 可以在多个SqlSession对象共享缓存数据
	 *   2. 默认的是开启的
	 *      <setting name="cacheEnabled" value="true"/>
	 *   3. 在需要使用二级缓存的配置映射文件中开启
	 *      <cache/>
	 *   4. 需要在二级缓存中保存的pojo对象必须实现序列化接口
	 *       User  implements Serializable
	 *   5. 在同一namespace范围下，执行提交操作，会清空该namespace的缓存
	 *
	 * 二级缓存的工作流程
	 *     1，在任意一个sqlSession对象中执行了sql查询语句，当关闭sqlSession对象时,在二级缓存中保存数据：以 namespace.sql语句为key值
	 *      以对象为value存储
	 *     2. 当其他sqlSession对象执行时， 需要根据namespace.sql 查询是否存在缓存
	 *
	 *
	 */
```
####八.Mybatis 的分页助手 PageHelper


####九.联合主键和外键的创建方式 

```xml

	create table sys_user_role(
	    userId int unsigned,
	    roleId int unsigned,
	    primary key(userId,roleId),
	    foreign key(userId) references sys_user(id),
	    foreign key(roleId) references sys_role(id)
	);
```


