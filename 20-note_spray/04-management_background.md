## 管理后台  

####1.操作日志表 sql 
	create table `mgt_operation_log` (
		id bigint unsigned not null primary key auto_increment comment '主键id',
		module_name varchar(100) default null comment '操作模块',
		target_id bigint unsigned not null comment  '操作目标id', 
		content varchar(500) default null comment '变更内容',
		ext varchar(500) default null comment '扩展字段', 
		create_user varchar(50) default null comment '创建人',
		create_time timestamp null default current_timestamp comment '创建时间',
		update_user varchar(50) default null comment '修改人',
		update_time datetime default null on update current_timestamp comment '修改时间'
	);    

----------------------------------------------------------------------------------- 

	创建mgt_operateion_log sql: 

	CREATE TABLE `mgt_operation_log` (
	  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键id',
	  `module_name` varchar(100) DEFAULT NULL COMMENT '操作模块',
	  `target_id` bigint(20) unsigned DEFAULT '0' COMMENT '操作目标id',
	  `content` varchar(500) DEFAULT NULL COMMENT '变更内容',
	  `ext` varchar(500) DEFAULT NULL COMMENT '扩展字段',
	  `create_user` varchar(50) DEFAULT NULL COMMENT '创建人',
	  `create_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
	  `update_user` varchar(50) DEFAULT NULL COMMENT '修改人',
	  `update_time` datetime DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间',
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;                                 