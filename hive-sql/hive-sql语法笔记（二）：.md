### 一、常用基础语法：

#### 1.创建表

```
create table sy(//创建表sy
  id bigint,  //表头名及其数据类型
  name char(10),
  sex char(10)
)
```

#### 2.插入数据/导入数据

```
1.插入数据：
  insert into sy values(1,'张三','男')
  INSERT INTO TABLE tablename1 [PARTITION (partcol1=val1, partcol2=val2 ...)] select_statement1 FROM from_statement;
2.导入数据：
  load
```

