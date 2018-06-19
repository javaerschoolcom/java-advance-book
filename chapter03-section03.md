# JDBC

### 什么是JDBC

> 它是JAVA中制定的操作数据库的一种技术标准

### JDBC开发步骤

* 导入数据库的驱动mysql-connector-java-5.1.18-bin.jar
* 注册驱动

```  java
//对于不同数据库驱动不一样
Class.forName("com.mysql.jdbc.Driver");
```

* 获取连接对象Connection

``` java
//Oracle写法：jdbc:oracle:thin:@localhost:1521:name
//SqlServer写法：jdbc:microsoft:sqlserver://localhost:1433; DatabaseName=name
//MySql写法：jdbc:mysql://localhost:3306/name
//如果连接的是本地的Mysql数据库，并且连接使用的端口是3306，那么的url地址可以简写为： //jdbc:mysql:///name
Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/student", "root", "admin");
```

* 获取语句对象进行CRUD操作


* 关闭资源

### JDBC核心接口

> JDBC中提供了一系列接口用于操作数据库

##### DriverManager

> 驱动管理器，通过它获取连接对象

``` java
Class.forName("com.mysql.jdbc.Driver");
Connection DriverManager.getConnection(url,username,password);
```

##### Connection

> 获取JAVA操作数据库的连接对象，有了连接对象才能操作数据库

``` java
Statement statement = connection.createStatement() //获取普通的语句对象
PreparedStatement preparedStatement = connection.prepareStatement(sql)//经常使用    
```

##### Statement

> 核心操作数据的对象，可以进行CRUD操作，更多的是使用它的子接口PreparedStatement操作

``` java
//statement对象中方法
statement.executeUpdate(sql);//新增，修改，删除
statement.executeQuery(sql);//查询
statement.execute(sql);//通用

//preparedStatement对象中方法
preparedStatement.setObject(index,object); //index从1开始
preparedStatement.executeUpdate()；
preparedStatement.executeQuery()；
preparedStatement.execute()；
```

##### ResultSet

> 主要用于查询操作，封装返回结果集

``` java
resultSet.next();//是否有下一个
resultSet.getObject(int); //获取查询结果对应的列顺序对应的值
resultSet.getObject(String); //获取查询结果对应的列名对应的值
```

### Statement&PreparedStatement关系及区别

##### 关系

PreparedStatement继承自Statement,都是接口

##### 区别

PreparedStatement可以使用占位符，是预编译的，批处理比Statement效率高

PreparedStatement可以防止SQL注入攻击

### 事务

> 数据库操作中一组最小的单元，这组工作单元由一系列操作组成，要么完全地执行，要么完全地不执行

### 事务特点

* 原子性：一个事务是一个不可分割的工作单位，事务中包括的诸操作要么都做，要么都不做
* 一致性：事务必须是使数据库从一个一致性状态变到另一个一致性状态。一致性与原子性是密切相关的
* 持久性：指一个事务一旦提交，它对数据库中数据的改变就应该是永久性的。接下来的其他操作或故障不应该对其有任何影响
* 隔离性：保证每个事务是独立的执行

### JDBC中事务

> JDBC中处理事务，都是通过Connection对象完成的,同一事务中所有的操作，都在使用同一个Connection对象

编写事务常用方法如下：

``` java
connection.setAutoCommit(false) //表示开启事务
connection.commit（）//提交结束事务  
connection.setSavepoint()//设置事务回滚点
connection.rollback（)//回滚到开启事务状态
connection.rollback（savepoint)//回滚到指定的回滚点
```

##### 隔离性

脏读：一个事务读取了另一个事务还没提交的数据

不可重复读：一个事务读取了另一个事务update的数据（对同一记录的两次读取不一致）

幻读：一个事务读取了另一个事务insert操作（对同一张表的两次查询不一致）

###  数据源连接池

> 对于创建数据库连接对象是一个非常耗性能功能，有必须把耗性能功能提前去做或者降低开销

> 常用的数据库连接池有dbcp,c3p0,Druid等

##### dbcp使用

1. 导入第三方依赖commons-dbcp-1.4.jar&commons-pool-1.6.jar
2. ​编写核心代码








