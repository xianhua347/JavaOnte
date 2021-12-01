# Mysql
## 1.DML语言(Data Manipulation Language)

### 作用：存储数据,管理数据

#### insert
```sql
insert into `表名`(`属性名`)
values('值') -- 基本格式 --
```
 - 注意事项：
	- 如果不使用属性值，sql会挨个匹配值
	- 如果要插入多行数据 values('值'),() 直接跟逗号和括号 

#### update

```sql
UPDATA `表名` set colum_name = value,[next colnum_name = value]
where[条件]
```

#### delete

```sql
DELETE FROM `表名`
WHERE [条件]
```

#### TRUNCATE

清空数据库 表的结构和索引不会变

- 特点：
  - 重新设置自增列，计数器会归零
  - 不会影响事务

## 2.DQL(Data Query Language)
### 作用：查询表

**关键字 select**

去重使用 distinct

```sql
SELECT DISTINCT `studentno`
FROM `student`
```

### join子句

![u=3535404185,579235274&fm=26&gp=0](C:\Users\Administrator\OneDrive\onte\u=3535404185,579235274&fm=26&gp=0.jpg) 


| 操作       | 结果                                               |
| :--------- | -------------------------------------------------- |
| inner join | 至少有个值匹配就返回值                             |
| RIGHT JOIN | 会从右表返回所有要查询的值，即使左表没有相对于的值 |
| LEFT JOIN  | 会从左表返回所有要查询的值，即使右表没有相对应的值 |

常用的查询题目
```sql
-- 查询学生所在的年级
SELECT s.studentno,s.studentname,g.gradename
FROM student AS s
INNER JOIN grade AS g
ON s.`gradeid` = g.`gradeid`

-- 查询参加Java程序设计或者c语言的学生的信息
SELECT
	st.studentno,
	st.studentname,
	sub.subjectname,
	re.studentresult 
FROM
	student AS st
	INNER JOIN result AS re ON st.studentno = re.studentno
	INNER JOIN `subject` AS sub ON re.subjectno = sub.subjectno 
WHERE
	sub.subjectname = '数据库结构-1' OR sub.subjectname = 'C语言-1'
```

### 排序子句（order by）

```sql
ORDER BY `columnname` [DESC OR ASC] -- DESC是升序 asc是降序
```

### 分页子句（limit）

```sql
limit ['起始页','页面大小']
limit[5,10] -- 从第五条数据显示，一共显示10条数据
```

### 聚合函数

| **函数** | **作用** |
| ------- | ---- |
| count() | 计数 |
| sum()   | 求和 |
| avg()   | 平均值 |
| max() | 最大值 |
| min() | 最小值 |

COUNT() 注意事项：

```sql
SELECT COUNT(`字段`) from table -- 会忽略null值
SELECT COUNT(*) FROM TABLE -- 不会忽略null值，本质是在计行数
SELECT COUNT(1) FROM TABLE -- 不会忽略所有的null
```

## 3.MD5加密

```sql
UPDATE testmd5 SET `passworld` = MD5(passworld) -- 使用md5
INSERT INTO `talbename` VALUES(MD5(str)) -- 插入时候加密

```

DM5加密比对直接用明文比对

```sql
`passworld`=md5('明文密码')
```



## 4.事务

### 4.1事务原则（acid）

#### 原子性（**A**tomicity）

​	要么全完成，要么全不完成.

#### 一致性（**C**onsistency）
  	在事务开始之前和事务结束以后，数据库的完整性没有被破坏

#### 隔离性（**I**solation）

  	数据库允许多个并发事务同时对其数据进行读写和修改的能力

#### 持久性（**D**urability）

 	事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。

```sql
set autocommit = 0; -- 关闭自动提交
START TRANSACTION -- 开启事务

UPDATE account set money = money - 200 WHERE `name`='a' 
UPDATE account set money = money + 500 WHERE `name`='b'
-- 一组事务
COMMIT; -- 提交 （提交之后无法回滚）
ROLLBACK;

set autocommit = 1; -- 开启自动提交
```

### 5.JDBC

#### 连接数据库步骤

1. 加载数据库驱动

2. 填写用户信息和连接数据库

3. 使用statement对象 执行sql

4. 返回结果集

5. 释放连接🔗

```java
   package com;
   
   import java.sql.*;
   
   public class JDBC_first {
       public static void main(String[] args) throws SQLException {
           try {
               Class.forName("com.mysql.cj.jdbc.Driver");
           } catch (ClassNotFoundException e) {
               e.printStackTrace();
           }
           //用户信息
           String url = "jdbc:mysql://localhost:3306/school?useSSL=true&serverTimezone=UTC";
           String username="root";
           String password="1";
           //连接数据库 connection是数据库对象
           Connection connection = DriverManager.getConnection(url, username, password);
   
           //执行sql对象
           Statement statement = connection.createStatement();
           //查询
           String sql = "SELECT * FROM `student`";
           ResultSet resultSet = statement.executeQuery(sql);
           //返回结果的集合
           while (resultSet.next()){
               System.out.println("id"+resultSet.getObject("Student_id"));
               System.out.println("login_pwd"+resultSet.getObject("login_pwd"));
               System.out.println("name"+resultSet.getObject("Student_name"));
               System.out.println("grade_id"+resultSet.getObject("grade_id"));
               System.out.println("phone"+resultSet.getObject("phone"));
               System.out.println("address"+resultSet.getObject("address"));
               System.out.println("borndate"+resultSet.getObject("borndate"));
               System.out.println("email"+resultSet.getObject("email"));
               System.out.println("identitycard"+resultSet.getObject("identitycard"));
               //打印数据
           }
           resultSet.close();
           statement.close();
           connection.close();
   
       }
   }
   
```

####    sql对象

1. DriverManager是驱动管理对象(加载驱动)

2.  connection 数据库对象(可以用户数据库级别的操作)

3. Statement sql对象(操作数据)

   1. ```java
      statement.execute() //操作数据 增 删 改 查询 
      ```

4. ResultSet 结果集对象(返回值)
