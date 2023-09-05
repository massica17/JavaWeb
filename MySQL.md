## 1. DDL 操作数据
1. 创建 create
   ```sql
   create database [ if exists ] db_name [ character utf8 ]; 创建数据集
2. 查询 retrieve
   ```sql
    show databases; 查询所有数据库名称
    show create database db_name; 查看创建数据库的创建和字符集

3. 修改 update
    ```sql
    alter database db_name character utf8; 修改字符集, 如果表已经创建，并不会改变现有表的字符集，只影响之后加入的表
4. 删除 delete
   ```sql
   drop database [ if exists ] db_name;
5. 使用 use
   * use db_name;
   * select database(); 查询正在使用的数据库

## 2. DDL 操作表
1. 创建 create
   * create table table_name(列名 数据类型, .....); 注意最后一列不需要加逗号
   * create table t_name like t_name2; 复制表结构并创建新表
   * 常用的数据库类型 
     * int
     * double
     * date (yyyy-MM-dd)
     * datetime (yyyy-MM-dd HH:mm:ss)
     * timestamp (格式同datetime, 若赋值为null， 则默认为当前时间)
     * varchar
2. 查询 retrieve 
3. 修改 update
   * 修改表名 alter table t_name rename to new_t_name;
   * 修改表的字符集 alter table character set utf8;
   * 添加列 alter table t_name add 列名;
   * 修改列名称，类型 alter table t_name change 列名 新列名 数据类型
   * 修改列数据类型 alter table t_name modify 列名 新数据类型
4. 删除 delete
   * drop table [ if exists ] t_name; 

## 3. DML 增删改表中的数据

1. 添加数据  
   * insert into table_name (col1, col2, ...) values (val1, val2, val3 ...); 
2. 删除数据
   * delete from table_name [ where ? ]; 注意，如果不加条件则删除所有的表数据,有多少数据就是执行多少次，效率低
   truncate table table_name; 删除表并创建一个一样的表，相当于删除所有的数据 
3. 修改数据
   * update table_name set col1=val1, ... [ where ? ];

## 4. DQL 查询表中的数据
1. 语法
    select 字段列表 
    from 表名列表 
    where 条件列表 
    group by 分组字段 
    having 分组之后的条件 
    order by 排序 
    limit 分页限定
2. 基础查询
   * 去除重复 select [ distinct 去除重复数据 ] 列名
   * 选择并计算，注意若数据中有null则计算结果必为null 需要使用 ifnull select math, english, math + ifnull(english, 0) from t_name;
   * 起别名 as 或者 空格 别名
3. 条件查询
   * where 
   * 条件运算符 <>(不等于), between ... and, in, like, is null, and(&&), or(||), not(!)
   * 判断 = null 是错误的，需要用 is null 判断， != null 用 is not null 判断
   * 模糊查询 like
     * select * from t_name where name like '正则表达式';
     * _ 一个任意字符， % 任意多个字段
4. 排序查询
   * 排序关键字 select * from table order by 列名 ( 备选列名 当第一个条件一样的时候 ) asc, 升序排序， desc，降序排序
5. 聚合函数
   * count 计算个数排除null select count(ifnull(name, 0)) from student; 
   * max 计算最大值 select max(id) from student;
   * min 计算最小值 select min(id) from student;
   * sum 和 select sum(id) from student;
   * avg
6. 分组查询
   * select ( 分组字段，聚合函数 ) from student group by sex; 
   * having， where 和 having 区别，where 在分组之前限定，不满条件不参与分组， having在分组后限定，不满足结果不查询出来
   * where 后不可以跟聚合函数，having可以;
   * select sex, avg(math), count(id) 人数 from student where math > 70 group by sex having 人数 > 2;
7. 分页查询
   * limit 开始的索引，每页的条数
   * select *from student limit 0,3; (索引 = （当前页码 - 1）* 每页条数);
8. 约束
   * 保证表数据的准确性，有效性，完整性。
   * 分类：
     * 主键约束 primary key
     * 非空约束 not null 加载创建表的语句后面
     * 唯一约束 unique (唯一约束可以有null 但是只有一条记录可以null)
     * 外键约束 foreign key
   * 非空约束
     * 创建表时添加
     * alter table student modify name varchar(20) not null;
     * alter table stu modify name varchar(20);
   * 唯一约束 ( 可以只有一个值为null )
     * 创建表时添加
     * alter table student modify name varchar(20) unique;
     * alter table stu drop index name;
   * 主键约束 ( 非空且唯一, 一张表只能有一个主键 )
     * 创建表时添加
     * alter table stu modify name int primary key;
     * alter table student drop primary key;(特点：没有指定主键，因为只有一个主键)
     * 自动增长，与上一行的数值加一，当某一列是数值类型，auto_increment，此时主键可以为null;
   * 外键约束
     * 创建表时添加外键列：constraint 外键名称 foreign key 表内列名 references 外主表名(主表列名)
     * 删除外键 alter table t_name drop foreign key 外键名称;
     * 添加外键 alter table t_name add constraint 外键名称 foreign key 表内列名 references 外主表名(主表列名);
     * 级联操作
       * 添加级联操作 alter table t_name add constraint foreign key (外键名称) references 主表名称(主表列名称) on update cascade on delete cascade;
       * 级联更新: on cascade update
       * 级联删除: on cascade delete
       
## 5. 数据库设计

1. 多表关系
2. 数据库范式

## 6. 多表查询

1. 内连接查询
   1. 显式内连接 
       ```sql
       select 字段列表 from t_name1 inner join t_name2 on t_name1.col1 = t_name2.col2; 
   2. 隐式内连接 使用where条件消除无用数据
2. 外连接查询
   1. 左外连接（查询左表的所有记录和交集部分） 
        ```sql
        select 字段列表 from t_name1 left join t_name2 on t_name1.col1 = t_name2.col2;
   2. 右外连接 类似左外连接
3. 子查询 (查询中嵌套查询)
   1. 单行单列：
        ```sql
        select max(sal) from emp; select * from emp where sal = 9000; 
        可以合并elect * from emp where sal = (select max(sal) from emp);
   2. 多行多列 in， 也可以作为虚拟表
   3. 多行单列

## 7. 事务
1. 事务的基本介绍
   1. 概念：如果一个事务包含多个流程，要么同时成功，要么同时失败
   2. 操作：
      * 开启事务 start transaction
      * 回滚 rollback
      * 提交 commit
       ```sql
       start transaction;
       事件代码
       commit;
       rollback;
   3. 自动提交 mysql自动提交，一条DML语句就会自动提交
   4. 手动提交 要自己start transaction
2. 事务的四大特征
   * 原子性，不可分割的最小操作单元
   * 持久性，事务提交或回滚后，数据库会持久化的保存数据
   * 隔离性，多个事务之间相互独立
   * 一致性，事务操作前后，数据总量不变
3. 事务的隔离级别
   * 存在问题 脏读， 不可重复读，幻读
   * 隔离级别；
     * read uncommitted：读未提交，脏读，不可重复读，幻读
     * read committed：不可重复读，幻读
     * repeatable read：幻读
     * serializable：解决所有问题
   * 设置隔离级别 set global transaction isolation level "隔离级别"

## 8. DCL 管理用户、授权
1. 创建用户
    ```sql
    create USER '用户名'@'主机名' identified by '密码'
2. 删除用户
    drop user '用户名'@'主机名'
3. 修改密码
    update user set password = password('新密码') where user = '用户名';
    set password for '用户名'@'主机名' = password('新密码');
4. 忘记root密码
   1. 管理员 cmd：net stop mysql
   2. mysql --skip-grant-tables; 无验证登录
   3. 打开新的cmd，输入mysql，update修改密码
   4. 关闭两个窗口，进程管理器关闭mysql.exe进程
   5. 使用新密码登录
5. 权限管理
   1. 查询权限 show grant for '用户名'@'主机名'
   2. 授予权限 grant 权限列表(select, update, delete, all) 数据库.表名(\*.*) to '用户名'@'主机名'
   3. 撤销权限 revoke on 数据库名.表名 from '用户名'@'主机名'
   

     
