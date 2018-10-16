tdw sql语法：

1.数据类型http://www.w3school.com.cn/sql/sql_datatypes.asp

​    1).整型尽量用bigint，浮点型尽量用double，避免在隐式转换的时候出现一些小问题。

​    2).强制类型转换可以使用cast 例如 cast (a as double)

2.[Hive文件格式](https://www.cnblogs.com/Richardzhu/p/3613661.html)

​    hive文件存储格式包括以下几类：

​       1、TEXTFILE

​       2、SEQUENCEFILE

​       3、RCFILE

​       4、ORCFILE(0.11以后出现)

其中TEXTFILE为默认格式，建表时不指定默认为这个格式，导入数据时会直接把数据文件拷贝到hdfs上不进行处理；

SEQUENCEFILE，RCFILE，ORCFILE格式的表不能直接从本地文件导入数据，数据要先导入到textfile格式的表中， 然后再从表中用insert导入SequenceFile,RCFile,ORCFile表中。

https://www.cnblogs.com/Richardzhu/p/3613661.html

相比TEXTFILE和SEQUENCEFILE，RCFILE由于列式存储方式，数据加载时性能消耗较大，但是具有较好的压缩比和查询响应。数据仓库的特点是一次写入、多次读取，因此，整体来看，RCFILE相比其余两种格式具有较明显的优势。 

3.alter table :该语句用于在已有的表中添加、修改或删除列。

4.count(1)、count(*)与count(列名)的执行区别https://blog.csdn.net/iFuMI/article/details/77920767

5.[Oracle 中 decode 函数用法](https://www.cnblogs.com/ZHF/archive/2008/09/12/1289619.html)

含义解释： 

```
**decode**(条件,值1,返回值1,值2,返回值2,...值n,返回值n,缺省值)

该函数的含义如下：
IF 条件=值1 THEN
　　　　RETURN(翻译值1)
ELSIF 条件=值2 THEN
　　　　RETURN(翻译值2)
　　　　......
ELSIF 条件=值n THEN
　　　　RETURN(翻译值n)
ELSE
　　　　RETURN(缺省值)
END IF
```

**decode**(字段或字段的运算，值1，值2，值3）

​       这个函数运行的结果是，当字段或字段的运算的值等于值1时，该函数返回值2，否则返回值3
 当然值1，值2，值3也可以是表达式，这个函数使得某些sql语句简单了许多

使用方法： 
1、比较大小
select **decode**(sign(变量1-变量2),-1,变量1,变量2) from dual; --取较小值
sign()函数根据某个值是0、正数还是负数，分别返回0、1、-1
例如：
变量1=10，变量2=20
则sign(变量1-变量2)返回-1，**decode**解码结果为“变量1”，达到了取较小值的目的。

 

2、此函数用在SQL语句中，功能介绍如下：

 

 

 

Decode函数与一系列嵌套的 IF-THEN-ELSE语句相似。base_exp与compare1,compare2等等依次进行比较。如果base_exp和 第i 个compare项匹配，就返回第i 个对应的value 。如果base_exp与任何的compare值都不匹配，则返回default。每个compare值顺次求值，如果发现一个匹配，则剩下的compare值（如果还有的话）就都不再求值。一个为NULL的base_exp被认为和NULL compare值等价。如果需要的话，每一个compare值都被转换成和第一个compare 值相同的数据类型，这个数据类型也是返回值的类型。

 

 

 

Decode函数在实际开发中非常的有用

**结合Lpad函数，如何使主键的值自动加1并在前面补0**select LPAD(decode(count(记录编号),0,1,max(to_number(记录编号)+1)),14,'0') 记录编号 from tetdmis

 

select decode(dir,1,0,1) from a1_interval

dir 的值是1变为0，是0则变为1

通常我们这么写:

select count(*) from 表 where 性别 ＝ 男；

select count(*) from 表 where 性别 ＝ 女；

要想显示到一起还要union一下，太麻烦了

用decode呢，只需要一句话

select decode(性别，男，1，0），decode(性别，女，1，0） from 表



