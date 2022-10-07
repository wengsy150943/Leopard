# Bug Reproducer

## Usage
```
java -jar BugReproducer.jar ${CONFIG_PATH}
```

The jar file was built and packaged with OpenJDK-8.
You can get source file from <https://github.com/lpypl/BugReproducer>.


## Configuration
Configuration is a description of the bug reproduction process.  

Here is an example.

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

Comments:  

`initURL`: A JDBC_URL **without** database name for (re)creating test database  

`connURL`: A JDBC_URL **with** database name for creating test tables, loading test data, executing test operations  

`user`: Username  

`password`: Password  

`create`: Whether to execute `createDatabaseList` and `createTableList`  

`load`: Whether to execute `loadList`  

`createDatabaseList`: SQL list for creating test database  

`createTableList`: SQL list for creating test tables

`loadList`: SQL list for loading test data  

`operationList`: A sequence of sqls used to test the database. The `trxId` is transaction ID, and the `sql` is just a excutable SQL  


## TiDB Installation

Please refer to <https://github.com/pingcap/tiup>


## Bug Description  

See each configuration file for details, where the SQL sequence used to reproduce the bug is shown and the bug is described.  