# MySql
> 数据库：存储、维护、管理数据的集合
> 数据库（DateBase,DB) 数据集合（文件系统）
> 数据管理系统（DateBase Management System,DBMS）,建立、使用、维护数据库
> 行：记录 列：字段
> 常见数据库:
>> - Oracle 甲骨文
>> - DB2:IBM的产品（大笨象）
>> - SQL Server:Microsoft的产品
>> - PostgreSQL
>> - MySQL
**************
#### 下载
> Download MySQL Community Server
*****************
#### mysql服务启动与关闭
> - net start mysql57
> - net stop mysql57
> 
> 若出现错误：System error 5 has occurred.则打开管理员权限
****************
#### mysql登录与退出
> - mysql -u root -p
> - quit;
> - mysql -h 127.0.0.1 -u root -p 访问其他数据库
#### 修改密码
> 1.停止mysql服务：运行 service.msc 或者 net stop mysql
> 2.cmd下 输入 `mysqld --skip-grant-table` 启动服务 光标不动（不要关闭改窗口）
> 3.新打开 cmd 输入 mysql -u root -p 不需要密码
> 4.use mysql;
> 5.update user set password=password('abc') WHERE User='root';
> 6.关闭两个cmd窗口 在任务管理器结束 mysqld 进程
> 7.重启 mysql
**************
#### SQL
> SQL:Structure Query Language（结构化查询语言）
> 继续所有重要的数据库管理系统都支持SQL
> 各数据库厂商都支持ISO的SQL标准（普通话）
> 各数据库厂商在标准的基础上做了自己的扩展（方言）
> **sql分类**：
>> - `DDL`(Data Definition Language) : 数据定义语言，定义数据库对象：库、表、列等；create、alter、drop
>> - `DML`(Data Manipulation Language) : 数据操作语言，操作数据库*表中*的记录（数据）;inster、update、delete
>> - DQL(Data Query Language) : 数据查询语言，用来查询数据
>> - `DCL`(Data Control Language) ： 数据控制语言，用来定义访问权限和安全级别;select
>
> *sql语句以；结尾*
*****
### DDL create alter drop
> 数据定义语言
> 对 *数据库 表 列*进行操作
#### 创建数据库
> 语法
>> - create {database|schema}[if not exists] dbname [create_specification[,create_specification]...]
> 
> creater_specification
> - [default]character set charset_name 指定字符集
> - [default]collate collation_name 指定数据库字符集的比较方式

    mysql> create database mydb1;
    mysql> create database mydb2 character set gbk;
    
    mysql> show create database mydb2; 
    
    mysql> show character set //展示字符集
    mysql> create database mydb3 character set utf8 collate utf8_general_ci;
****************  
 #### 查询数据库
 > 查看当前数据库服务器中的所有数据库
 >> - show databases;
 >
 > 查看前面创建的 mydb2 数据库的定义信息
 >> - show create database mydb2;
 >
 ************
 #### 删除数据库
 > 删除前面创建的 mydb3 数据库
 >> - drop database mydb3;
 ***************
 #### 修改、备份、恢复数据库
 > 语法
 >> - alter database db_name [alter_specification];
 > 
 > alter_specification:
 >> - character set charset_name;
 >> - collate collation_name;
 
    mysql> alter database mydb2 character set utf8 collate utf8_general_ci;
****************
#### 查看当前使用的数据库
> select database();
****************
#### 切换数据库
> use mydb2;
*****************
#### 创建数据表
> 语法：
  
        create table 表名(
          字段1 字段类型，
          字段2 字段类型，
          ...
          字段n 字段类型
        );
>  常用的数据类型：
>> - int : 整形 tinyint(1字节) smallint(2字节) int(4字节) bigint(8字节)
>> - float 
>> - doublie : 浮点型，eg:double(5，2)表示最多5位，其中必须有小数2位，即最大值为999.99
>> - char : 固定长度字符串类型;  eg: char(10) 'abc       '
>> - varchar : 可变长度字符串类型; eg: varchar(10) 'abc'
>> - text : 字符串类型，大数据量的数据（4M）（不建议使用）
>> - blob : 字节类型，二进制数据（图片、视频、音频） longblob
>> - data : 日期类型，格式为: yyy-mm-dd
>> - time : 时间类型，格式为: hh:mm:ss
>> - timetamp : 时间戳类型 yyy-mm-dd hh:mm:ss *会自动赋值* （不好用2023年？）
>> - datetime : 日期时间类型 yyy-mm-dd hh:mm:ss

    mysql> use mydb1;
    mysql> create table emp(
        -> id int,
        -> name varchar(50),
        -> gender varchar(10),
        -> birthday date,
        -> entry_date date,
        -> job varchar(100),
        -> salary double,
        -> resume varchar(200)
        -> );
******************
#### 查询当前数据库中的所有表 
> show tables;
****************
#### 查看表的字段信息 
> desc table_name;
#### 表中添加列 add
> alter table emp add column image blob; 默认是列
#### 修改列中元素 modify
> alter table emp modify job varchar(60);
#### 删除列
> alter table drop image;
#### 给表改名
> rename table old_name to new_name;
#### 查看创建表的细节
> show create table table_name;
> show create table table_name\G;
#### 修改表的字符集
> alter table table_name character set gbk;
#### 修改列名 change
> alter table table_name change old_name new_name varchar(100); 
#### 删除表
> drop table table_name;
***************
### DML 操作（重要）insert update delete
> 数据操作语言 
> 对*表中数据*进行增、删、改、查
> 小知识：
> - 在mysql中，字符串类型和日期类型都要用单引号括起来。 eg : 'tom' '2015-09-04'
> - 空值 : null 
#### 查询表中所有数据
> `select` * from 表名;
#### 插入操作 ： insert
> `insert into` table_name(列名1，列名2，...) `values`(列值1，列值2，...)；
> 注意:
>> - 列名与列值类型、个数、顺序要一一对应
>> - 可以把列名当作java中的形参，把列值当作实参
>> - 值不要超出列定义的长度
>> - 如果插入空值，请用 null
>> - 插入的日期和字符一样，都使用引号括起来

    mysql> insert into emp
        -> values(1,'tom','男','2015-09-24',10000,'2015-09-25','good boy');
#### 修改操作
> `update` 表名 `set` 列名1=列值1，列名2=列值2...where 列名=值；
> update emp set salary=5000;
> update emp set salary=3000 where name='tom'; 相当于 if( name == 'tom' ）即使不存在也不会报错
> update emp set salary=4000,gender='m' where name='jerry';
> update emp set `salary=salary+1000` where name='菜10';
#### 删除操作 delete
> `delete from` 表名 [where 列名=值];
> delete from emp where name='tom';
> 删除表中所有记录:
>> delete from emp;  可以根据日志恢复
>> turncate from emp; 删除表再给你创建一个，不能恢复，执行速度快
********************************
### DQL 操作 select
> 数据查询语言(重要)
> 数据库执行 DQL 语句不会对数据进行改变，而是让数据库发送结果集给客户端
> 语法：
>> - select 列名 from 表名 
>> [where -> group by -> having -> order by]

    select selection_list /\*要查询的列名称\*/
     from table_list /\*要查询的表名称\*/
     where condition /\*行条件\*/
     group by grouping_columns /\*对结果分组*\/
     having condition /\*分组后的行条件\*/
     order by sorting_columns /\*对结果分组\*/
     limit offset_start,row_cow_count /\*结果限定,mysql独有的，分页用的\*/
#### 基础查询
> 查询所有列
>> - select * from table_name;
> 查询指定列
>> - select sid,sname,age from stu;
#### 条件查询
> 条件查询就是在查询时给出 where 子句
> 在  where 子句中可以使用如下运算符及关键字：
>> - =、!=、<>、<、<=、>、>=；
>> - between ... and ...;
>> - in(set,...); 在固定的范围内
>> - is null ; is not null
>> - and
>> - or
>> - not
>
> select * from stu where sid in('s_1001','s_1002','s_1003');
> select * from stu where sid not in('s_1001','s_1002','s_1003');
> select * from stu where age is null;
>
> select * from stu  where age >=20 and age<=40;
> select * from stu  where age between 20 and 40; 与上语法一样，数据反向则不行
>
> select * from stu where gender<>'male';
#### 模糊查询 like
> 通配符：
>> -\_ : 任意一个字符
>> -% : 任意 0~n 个字符
>
> eg: '%张%' '姜\_'
>
