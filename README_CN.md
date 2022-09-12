# 文档  

- [English](/README.md)  
- [简体中文](/README_CN.md)  

# 仓库概览

这个仓库包含四个部分

1. `./Full Paper.pdf 展示了Leopard论文的完整版本。
1. `./Bugs List.pdf` 包含Leopard发现的代表性代码缺陷。
2. `./Interesting Bugs.pdf` 演示在TiDB中发现的几个代码缺陷。
3. `./BugReporducer` 是在TiDB中发现的几个错误的详细重现步骤。

# 代码缺陷复现器（Bug Reproducer）

## 使用方法
```
java -jar BugReproducer.jar ${CONFIG_PATH}
```

这里的JAR包是用OpenJDK8编译的，如果使用时遇到什么问题，可以尝试从<https://github.com/lpypl/BugReproducer>获取源码，编译教程见该项目主页。


## 配置文件  
配置文件描述了BUG复现的流程。    

配置文件示例如下：

```yml
initUrl: "jdbc:mysql://127.0.0.1:3306?serverTimezone=UTC&useServerPrepStmts=true&cachePrepStmts=true"
connUrl: "jdbc:mysql://127.0.0.1:3306/reproduceDB?serverTimezone=UTC&useServerPrepStmts=true&cachePrepStmts=true"
user: "root"
password: "root"
create: true
load: true
createDatabaseList:
  - "drop database if exists reproduceDB;"
  - "create database reproduceDB;"
createTableList:
  - "create table table_7_2(a int primary key, b int, c double);"
loadList:
  - "insert into table_7_2 values(676, 5012153, 2240641.4);"
operationList:
  - trxId: "739"
    sql: "update table_7_2 set b=-5012153, c=2240641.4 where a=676;"
  - trxId: "723"
    sql: "update table_7_2 set b=852150 where a=676;"
  - trxId: "739"
    sql: "commit;"
  - trxId: "1000"
    sql: "select * from table_7_2;"
  - trxId: "1000"
    sql: "commit;"
```

注释:  

`initURL`: 不包含数据库名称的JDBC_URL，用于（重新）创建数据库  

`connURL`: 包含数据库名称的JDBC_URL，用于创建表，导入初始数据，执行测试  

`user`: 用户名  

`password`: 密码  

`create`: 是否执行 `createDatabaseList` and `createTableList`  

`load`: 是否执行 `loadList`  

`createDatabaseList`: 用于创建测试数据库的SQL序列  

`createTableList`: 用于创建测试数据表的SQL序列

`loadList`: 用于导入初始数据的SQL序列  

`operationList`: 测试语句列表，`trxId` 是事务ID, `sql` 就是要执行的 SQL  


## TiDB 安装方法  

请参考PingCAP官方项目： <https://github.com/pingcap/tiup>  

## Bug 描述  

详见各个配置文件，其中展示了复现BUG的SQL序列，并描述了BUG。  