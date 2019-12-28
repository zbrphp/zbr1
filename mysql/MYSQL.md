## 使用命令修改用户root的密码为123 ##

	set password for 'root'@'localhost' = password('123');
**==》警告:**
**> 数据类型（int,,,,,,等）不能随便改，一旦修改，原数据将被破坏，若要修改先备份数据库；**

## 建表常用数据类型: ##

	例：
	自增id 	int unsigned
	登录名  	varchar(255)
	密码	char(32)
	头像	varchar(255)
	性别		enum('0','1','2')
	邮箱		varchar(255)
	地址		varchar(255)13:57 2019/12/21
	年龄		int unsigned
	电话		char(11)
	等级garde	tinyint unsigned
	状态status  tinyint undigned
	时间		int unsigned	

**查看建表语句:**
	
	show creat table `表名`; 

**MySQL 最主要的四种索引:（一张表索引尽量少于3个）**
	
	1.primary key 主键索引
	2.unique      唯一索引（可多个，账号等唯一数值的字段使用便于搜索联合查询）
	3.index       普通索引或常规索引（可多个，，尽量选择数据长度固定的字段例如手机号，但多占用空间，减慢插入，删除，修改等操作）
	4.fulltext    全文索引 不支持中文（不识别中文怎么办?使用-中文分词，主页搜索框要用，会将一句话拆分成词语和字来搜索）

**查看表索引:**
	
	show indexes from `表名`\G;(加\G竖着看)

**删除唯一、普通索引:**

	drop `索引名` on `表名`;
**删除主键索引:**
   
	1.去除自增属性
	alter table `表名` modify `字段名` 数据类型 约束条件;
   
	2.删除主键
	alter table `表名` drop primary key;  
**添加主键索引:**
	
	1.添加主键
	alter table `表名`add primary key('id') comment"主键添加";
	
	2.添加自增
	alter table `表名` modify `id` int(10) auto_incremment not null unsigned comment"添加自增"； 

**添加唯一索引**
	
	alter table `表名` add  unique  字段名别名(`字段名`) comment"添加唯一索引";
**添加普通索引**
	
	alter table `表名` add index 字段名别名('字段名') comment"添加普通索引";

**查询识别null:**
	
	使用'='符号无效,需要用到<=>符号；

**条件大小及范围：**
	
	查询空格:is null 或is not null
	不等于号:!= 或<>  (无区别)
    范围:between and 和 not between and
    条件:and 和 or
    范围集合:in 和 not in（例:id为1，2，3的需要查询则 in(1,2,3)）



===========================================================================================
# 增: #


   	1.insert into `表名`(`字段名`,``,``,)value(值,,,,),(值,,,,,)
    
    
    2.insert into `表名`value(值，，，),(值，，，)
    
    
    3.insert into `表名` set `字段名`=`值`,,,,;

# 删： #
    1.delete from `表名` where `?`=`?`;

    2.alter table `表名` drop `字段名`;(删除字段，即是列) 


<font color=#00ffff size=6>改：</font>

**(注意：update主要对数据修改，alter主要对字段数据类型表名字符集修改)**
  
    1. update `表名` set `字段名`=`新值`,,, where `?`=`?`;

    2. alert table `表名` change `原字段` `新字段` 数据类型;（修改字段名或数据类型）

    3. alert table `表名` add '字段' 数据类型 约束条件 first/after `字段名`; （添加字段）【使用了first/after 决定了字段插入的位置】

    4. alert table `表名` modify `字段` 新的数据类型 （修改字段数据类型）

    5. alert table `表名` rename to `新表名`;(修改表名)

    6. alert table `表名` default  character set 字符集;(修改表的字符集)

    7. alter tabls `表名` 引擎(engine=innodb)（修改表引擎）

# 查: #
    1.select * from `表名`   (查询表的所有数据)

    2.select `字段名`,``,``,  from `表名` where `?`=`?`;

    3.select distinct 字段名 from `表名` where `?`=`?`; (查询字段的值，避免重复，过滤重复)



## 排序（关键字order by）[asc升序  desc降序] ##
select * from `表名` order by 列名 asc
 
------------------------------------------------------------------


## 分页（关键字limit） ##

    select * from `表名` where `?`='?' limit (pageNo-1)*pagesize,pagesize ;
备注：(pagesize每页显示的记录数，pageNo为查询页数为变量由get传输)

## 模糊查询(关键字like）一般项目不允许用 ##
	% 表示任意长度
		李%    表示李字开头
		%结    表示结结尾的
		%宝强% 表示包含  包含宝强的
	_ 表示一位长度
		_白    表示白字前面可以有一位 李白，立白等
		__白   表示白字前面只可以有两位 莉莉白 没明白等
		_大_   表示大字前面有一位后面有一位  马大哈 
		白__   表示白字后面只能有两位  白晶晶

**表达式:**
	
	select id,name,age+20 from age where id = 1;
	
	updata `表名` set click_num=click_num+1 where `id`<100;点击量


## 使用mysql内置函数 ##
	concat() 字符连接
		例: select `id`,concat(`name`,'大特卖')from `表名`; 结果为name大特卖

	count() 计算总数
		例:select count(*) from `表名` ---表的总数据条数
	sum()计算和
		例:select sum(`click_num`)from `表名`;
	avg()平均值
	max()最大值
	unix_timestamp()时间戳
	select unix_timestamp；查询当前时间戳


起别名(关键字as):(为表字段或表名起别的名字)
	例:	1.   select `id`as"自增id" from `表名`;
		2.   select `id`  "自增id" from `表名`;(可不用as，但要空格)
     


多表查询:(.符号表示"的")
	内连接:
		select 表名.要查的字段名,,,,from `表名1` `表名2` where `表名1`.id` = `表2`.id; 
  
  	外连接:
		左连接(left join 以左边表为主的查询，会查询出左表的所有字段数据并拼接另一个表的字段数据):  
		select * from    表1 left join 表2     on     表1.字段 =表2.字段；（表1为左表）
		
		左连接(right join 以右边表为主的查询，会查询出右表的所有字段数据并拼接另一个表的字段数据):  
		select * from    表1 right join 表2    on     表1.字段 =表2.字段；（表2为右表）

	子查询:
		子查询就是以一条语句的为另一条语句的值的查询

	    包括:
		1.带in关键字的子查询（in相当于=）
			语句1 = (语句2);
			语句1 in(语句2);
			
		2，比较运算中的子查询（ < 或 >、<= 等,,,,）

	子查询的作用:把一张表的数据复制到另一张表(前提是表结构相同)
	例:insert into 表名 where 条件 查询语句




##复制表结构:##
	create table 新表 like 旧表;

## 复制数据： ##
	使用子查询

## 清空表数据，保留表结构: ## 
	
	Truncate table 表名; 

**备注：网站维护的主要任务就是数据备份,通过写脚本定时备份数据Linux+mysql**



# 权限分配与用户管理 #
	创建新用户并授权:
		grant 权限 on 数据库.数据表 to 用户名@登录主机 IDENTIFIED BY "密码"; (若用户存在则更新权限)
	%只能远程登录
	localhost 本地登录
	例: grant select,update,delete,insert on abc.ceshi to aaa@`%` identified by '123123';

	 删除用户
		Delete FROM user Where User='test' and Host='localhost';
		
		flush privileges;更新权限
		
		drop database testDB; //删除用户的数据库

	删除账户及权限：>drop user 用户名@'%';
				  >drop user 用户名@ localhost; 


 

## 修改指定用户密码 ##

  
	update mysql.user set password=password('新密码') where User="test" and Host="localhost";
	
	再更新一下权限：	flush privileges;

# 忘记root密码怎么办？ #
	1.关闭黑窗口

	2.打开黑窗口，输入mysqld --skip-grant-tables（不要关闭窗口）

	3.使用mysql -u root -p不用密码登录

	4.use mysql    用语句改root密码 更新权限

	5.退出安全模式，重启mysql


# 备份与还原 #
	
	库:
 	1.备份一个数据库  mysqldump -u root -p 库名 > 路径/文件名.sql

	2.还原数据库（数据库必须先创建好）mysqldump -u root -p 库名 < 路径/文件名.sql

	3.备份多个数据库  mysqldump -u root -p --databases库名1 库名2 > 路径/文件名.sql

	4.还原多个数据库（数据库无需创建） source 直接拖文件到黑窗口（可建立一个新空库，但备份后不会保存到空库）
	
	
	表:
	1.备份一个表  mysqldump -u root -p 库名 表名 >路径/文件名.sql

	2.还原一个表或多个表  先选库use 库 然后source 拖文件

	3.备份多个表   mysqldump -u root -p 库名 表名1 表名2 >路径/文件名.sql
