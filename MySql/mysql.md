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
> sql分类
>> - DDL(Data Definition Language) : 数据定于语言，定义数据库对象：库、表、列等；
>> - DML(Data Manipulation Language) : 数据操作语言，操作数据库*表中*的记录（数据）
>> - DQL(Data Query Language) : 数据查询语言，用来查询数据
>> - DCL(Data Control Language) ： 数据控制语言，用来定义访问权限和安全级别
