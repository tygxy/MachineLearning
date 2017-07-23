# Hive

## 1.创建表

### 1.1 创建外部表
```
create external table databaseName.tableName(
uid string,
sid string
)
PARTITIONED BY(dt STRING)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '\t'
location '/xx/xx'
```

## 2.修改表结构

### 2.1 增加列
```
// 在当前列的末尾，分区列之前增加新的列
alter table databaseName.tableName add columns (sdt string)
```

## 3.表分区操作

### 3.1 创建分区见1.1

### 3.2 增加/删除分区
```
alter table databaseName.tableName add partition(dt = '2017-07-06') location '/xx/xx/2017-07-06'

alter table databaseName.tableName drop partition(dt = '2017-07-06')
```

## 4.删除表
```
drop table databaseName.tableName
```

## 5.创建函数
```
create temporary function functionName as 'udfs.DistTimeId';
```

## 6.加载数据

### 6.1 向数据表中加载文件
```
// 地址指定分区名
load data inpath "/xx/xx/2017-06-05" into table databaseName.tableName partition (dt = '2017-06-05')
```
### 6.2 将查询结果插入hive表
```
insert overwrite table databaseName.tableName partition (dt = '2017-06-05')
select * from 
```

### 6.3 查询结果写入文件系统
```
insert overwrite directory '/xx/xx'
select * from
```
### 6.4 分区外部表怎么加载数据
```
alter table qcdq.webspv add partition(dt='2017-07-07') location '/qcdq/middledata/webspv/2017-07-07';
```






