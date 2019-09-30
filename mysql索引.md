## MySql索引学习 ##


### 1.表设计
    
	  CREATE TABLE `test_user` (
	  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键',
	  `account` varchar(20) DEFAULT '' COMMENT '账号',
	  `pass_word` varchar(32) DEFAULT '' COMMENT '密码',
	  `name` varchar(20) DEFAULT '' COMMENT '姓名',
	  `age` tinyint(4) unsigned DEFAULT '0' COMMENT '年龄',
	  `sex` tinyint(4) unsigned NOT NULL DEFAULT '0' COMMENT '0-男，1-女，默认为0',
	  `email` varchar(60) DEFAULT '' COMMENT '邮件',
	  `create_id` bigint(20) NOT NULL DEFAULT '0' COMMENT '创建人',
	  `create_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
	  `update_id` bigint(20) NOT NULL DEFAULT '0' COMMENT '更新人',
	  `update_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
	  `is_delete` tinyint(4) unsigned NOT NULL DEFAULT '0' COMMENT '是否删除，0-未删除，1-删除，默认为0',
	  PRIMARY KEY (`id`),
	  UNIQUE KEY `account` (`account`)
	) ENGINE=MYISAM  AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='测试用户表';




### 2.存储过程

MySql5.7版本
	
	    -- 如果存储过程已存在，先删除 
	DELIMITER $$ 
	DROP PROCEDURE
	IF EXISTS test ; CREATE PROCEDURE test (IN num INT)
	BEGIN
		DECLARE
			n INT DEFAULT 1 ;
		WHILE n <= num DO
			INSERT INTO test_user (
				`account`,
				`pass_word`,
				`name`,
				`age`,
				`sex`,
				`email`,
				`create_id`,
				`update_id`
			)
		VALUES
			(
				concat('account', n),
				MD5(num),
				concat('nick', n),
				FLOOR(rand() * 50),
				FLOOR(RAND() * 2),
				concat('nick', n, '@qq.com'),
				0,
				0
			) ;
		SET n = n + 1 ;
		END
		WHILE ;
	END $$ 
	
	DELIMITER ;


调用

    CALL test(1000000); 


### 3.修改表

修改存储引擎

    ALTER TABLE `test_user` ENGINE=INNODB;


增加唯一索引

   
     ALTER TABLE `test_user` ADD unique(`account`);


普通索引

     ALTER TABLE test_user ADD INDEX index_name (name )


联合索引

     ALTER TABLE test_user ADD INDEX index_age_sex ( age, sex)


资料：https://www.jianshu.com/p/4de94cb1e32e

### 3.测试

测试语句

    SELECT * FROM test_user  WHERE account='account100000'


    SELECT * FROM test_user  WHERE id=100

    SELECT * FROM test_user WHERE `name`='nick18'

     -- 取10条数据
    select * from test_user where account like 'account2%'  limit 5,10

     --不走索引
    select id,account from test_user where account like '%account2%'  

     --走索引
     select id,account from test_user where account like '%account2%'  

     ---走索引
    SELECT * FROM test_user  WHERE age>=10  AND age<=11

      ---走索引
    SELECT * FROM test_user  WHERE age=10
    
     ---不走索引
    SELECT * FROM test_user  WHERE  age<=11

     
    
更新语句
    
    UPDATE test_user SET create_time='2019-09-18 11:15:09' WHERE  id>=1000 AND id<=3000


时间范围查询
  
      --走索引
    SELECT * FROM test_user WHERE  create_time>='2019-09-16 0:00:00'  AND create_time<='2019-09-18 0:00:00'

     --走索引
    SELECT * FROM test_user WHERE create_time BETWEEN  '2019-09-16 0:00:00' and '2019-09-18 0:00:00'

     --不走索引
    select * from test_user  where DATE_FORMAT(create_time,'%Y-%m-%d')='2019-09-16';

分组

    SELECT COUNT(*) FROM test_user WHERE  create_time>='2019-09-16 0:00:00'  AND create_time<='2019-09-18 0:00:00' GROUP BY sex



