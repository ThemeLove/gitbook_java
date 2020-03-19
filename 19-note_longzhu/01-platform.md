####1.RoomHandler.java   
	getLiveGameId(long roomId) ：	根据roomId获取GameId(开播类型)  

####2.DataSource  SqlSessionFactory SqlSessionFactoryTemplate 
	包装过程
	DataSource----->SqlSessonFactory----->SqlSessionFactoryTemplate  

	其中：SqlSessionFactory sqlSessionFactory=SqlSessionFactoryBean.getObject()
	
例如：

```java

	@Bean(name = "longzhuLiveDataSource")
    public DataSource longzhuLiveDataSource() {
        DynamicDataSource dynamicDataSource = new DynamicDataSource();
        // 默认数据源
        dynamicDataSource.setDefaultTargetDataSource(longzhuLive());

        // 配置多数据源
        Map<Object, Object> dsMap = new HashMap<>(5);
        dsMap.put(DatabaseServer.LongzhuLive, longzhuLive());
        dsMap.put(DatabaseServer.LongzhuLiveReadOnly, longzhuLiveReadOnly());
        dynamicDataSource.setTargetDataSources(dsMap);
        return dynamicDataSource;
    }

    @Bean("longzhuLiveSqlSessionFactory")
    public SqlSessionFactory longzhuLiveSqlSessionFactory() throws Exception{
        SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
        bean.setDataSource(longzhuLiveDataSource());
        return bean.getObject();
    }

    @Bean("longzhuLiveSqlSessionTemplate")
    public SqlSessionTemplate longzhuLiveSqlSessionTemplate() throws Exception{
        return new SqlSessionTemplate(longzhuLiveSqlSessionFactory());
    }

```