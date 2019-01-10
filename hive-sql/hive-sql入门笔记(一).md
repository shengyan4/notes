#### 			hive sql 入门笔记（一）

##### 1.hive是什么？

- 官网解释：

  Apache Hive数据仓库软件提供对存储在分布式中的大型数据集的查询和管理，它本身是建立在Apache Hadoop之上，主要提供以下功能：
  （1）它提供了一系列的工具，可用来对数据进行提取/转化/加载（ETL）；
  （2）是一种可以存储、查询和分析存储在HDFS（或者HBase）中的大规模数据的机制；
  （3）查询是通过MapReduce来完成的（并不是所有的查询都需要MapReduce来完成，比如select * from XXX就不需要；
  （4）在Hive0.11对类似select a,b from XXX的查询通过配置也可以不通过MapReduce来完成

  **ps:提炼一下：**
  **1.hive是一个数据仓库**
  **2.hive基于hadoop。**
  **总结为一句话：hive是基于hadoop的数据仓库。**


- Hive是一种建立在Hadoop文件系统上的数据仓库架构，并对存储在HDFS中的数据进行分析和管理。（也就是说对存储在HDFS中的数据进行分析和管理，我们不想使用手工，我们建立一个工具把，那么这个工具就可以是hive）。

- 知识拓展：

  （1）Hadoop:

  [Hadoop](https://baike.baidu.com/item/Hadoop)是一个由Apache基金会所开发的[分布式系统](https://baike.baidu.com/item/%E5%88%86%E5%B8%83%E5%BC%8F%E7%B3%BB%E7%BB%9F/4905336)基础架构。

  用户可以在不了解分布式底层细节的情况下，开发分布式程序。充分利用集群的威力进行高速运算和存储。

  [1][ ]() Hadoop实现了一个[分布式文件系统](https://baike.baidu.com/item/%E5%88%86%E5%B8%83%E5%BC%8F%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F/1250388)（Hadoop Distributed File System），简称HDFS。HDFS有高[容错性](https://baike.baidu.com/item/%E5%AE%B9%E9%94%99%E6%80%A7/9131391)的特点，并且设计用来部署在低廉的（low-cost）硬件上；而且它提供高吞吐量（high throughput）来访问[应用程序](https://baike.baidu.com/item/%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F/5985445)的数据，适合那些有着超大数据集（large data set）的应用程序。HDFS放宽了（relax）POSIX的要求，可以以流的形式访问（streaming access）文件系统中的数据。

  Hadoop的框架最核心的设计就是：HDFS和MapReduce。HDFS为海量的数据提供了存储，而MapReduce则为海量的数据提供了计算。

![img](http://www.aboutyun.com/data/attachment/forum/201405/05/170634fayc3wpkgantknbb.png)

##### 2.hive 基本操作

- 创建表：

​	hive> CREATE TABLE pokes (foo INT, bar STRING); 

​        Creates a table called pokes with two columns, the first being an integer and the other a string

- 创建一个新表，结构与其他一样

​	hive> create table new_table like records;

- 创建分区表：

​	hive> create table logs(ts bigint,line string) partitioned by (dt String,country String);

- 加载分区表数据：

  ​	hive> load data local inpath '/home/hadoop/input/hive/partitions/file1' into table logs partition (dt='2001-	01-01',country='GB');


- 展示表中有多少分区：

  ​	hive> show partitions logs;


- 展示所有表：

  ​	hive> SHOW TABLES;

  ​        lists all the tables

  ​	hive> SHOW TABLES '.*s';

​	lists all the table that end with 's'. The pattern matching follows Java regular
​	expressions. Check out this link for documentation 

- 显示表的结构信息

  ​	hive> DESCRIBE invites;

   	shows the list of columns


- 更新表的名称：

  ​	hive> ALTER TABLE source RENAME TO target;


- 添加新一列

  ​	hive> ALTER TABLE invites ADD COLUMNS (new_col2 INT COMMENT 'a comment');


- 删除表：

  ​	hive> DROP TABLE records;

- 删除表中数据，但要保持表的结构定义

  ​	hive> dfs -rmr /user/hive/warehouse/records;


- 从本地文件加载数据：

  ​	hive> LOAD DATA LOCAL INPATH '/home/hadoop/input/ncdc/micro-tab/sample.txt' OVERWRITE INTO TABLE records;


- 显示所有函数：

  ​	hive> show functions;


- 查看函数用法：

  ​	hive> describe function substr;


- 查看数组、map、结构

  ​	hive> select col1[0],col2['b'],col3.c from complex;


- 
  内连接：

  ​	hive> SELECT sales.*, things.* FROM sales JOIN things ON (sales.id = things.id);


- 查看hive为某个查询使用多少个MapReduce作业

  ​	hive> Explain SELECT sales.*, things.* FROM sales JOIN things ON (sales.id = things.id);


- 外连接：

  ​	hive> SELECT sales.*, things.* FROM sales LEFT OUTER JOIN things ON (sales.id = things.id);

  ​	hive> SELECT sales.*, things.* FROM sales RIGHT OUTER JOIN things ON (sales.id = things.id);

  ​	hive> SELECT sales.*, things.* FROM sales FULL OUTER JOIN things ON (sales.id = things.id);


- in查询：Hive不支持，但可以使用LEFT SEMI JOIN

  ​	hive> SELECT * FROM things LEFT SEMI JOIN sales ON (sales.id = things.id);


- 
  Map连接：Hive可以把较小的表放入每个Mapper的内存来执行连接操作

  ​	hive> SELECT /*+ MAPJOIN(things) */ sales.*, things.* FROM sales JOIN things ON (sales.id = things.id);

​	INSERT OVERWRITE TABLE ..SELECT：新表预先存在
​	hive> FROM records2
  	  > INSERT OVERWRITE TABLE stations_by_year SELECT year, COUNT(DISTINCT station) GROUP BY year 
   	 > INSERT OVERWRITE TABLE records_by_year SELECT year, COUNT(1) GROUP BY year
   	 > INSERT OVERWRITE TABLE good_records_by_year SELECT year, COUNT(1) WHERE temperature != 							9999 AND (quality = 0 OR quality = 1 OR quality = 4 OR quality = 5 OR quality = 9) GROUP BY year;  

​	CREATE TABLE ... AS SELECT：新表表预先不存在
​	hive>CREATE TABLE target AS SELECT col1,col2 FROM source;

- 创建视图：

  ​	hive> CREATE VIEW valid_records AS SELECT * FROM records2 WHERE temperature !=9999;


- 查看视图详细信息：

  ​	hive> DESCRIBE EXTENDED valid_records;

···························································································································································································

- 传统数据库：

- 添加：

​	insert into 表名 values(); 

- 修改：

​	update 表名 set a=b where b=c; 

- 删除：

​	delete from 表名where a=b;

- 查询：

  select * from 表名 where a=b;