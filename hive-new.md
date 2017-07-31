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

alter table databaseName.tableName add partition(dt='2017-07-07') location '/qcdq/middledata/webspv/2017-07-07';
```
- 中文别名 `列名`，如果在shell中执行， \`列名\`

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

## 4.删除/清空表
```
drop table databaseName.tableName

truncate table databaseName.tableName
```

## 5.创建函数
```
create temporary function functionName as 'udfs.DistTimeId';
```

## 6.加载数据

### 6.1 向数据表中加载文件
```
// 地址指定分区名(内部表)
load data inpath "/xx/xx/2017-06-05" into table databaseName.tableName partition (dt = '2017-06-05')
```
### 6.2 将查询结果插入hive表
```
insert overwrite table databaseName.tableName partition (dt = '2017-06-05')
select * from 
```

### 6.3 查询结果写入文件系统
```
// 写入HDFS
insert overwrite directory '/xx/xx'
select * from

// 写入本地
insert overwrite local directory '/xx/xx'
select * from

```
### 6.4 分区外部表怎么加载数据
```
alter table qcdq.webspv add partition(dt='2017-07-07') location '/qcdq/middledata/webspv/2017-07-07';
```

## 7.Hive,mysql,hbase互导数据

### 7.1 mysql-to-hive
- 创建分区外部表
- sqoop导入数据
```
 /opt/cloudera/parcels/CDH-5.7.5-1.cdh5.7.5.p0.3/lib/sqoop/bin/sqoop import --connect 'jdbc:mysql://192.168.104.2:3306/ dealer?characterEncoding=utf8&useSSL=false' --username 'java_write_pc' --password 's#()Grermsfl,<DrWcelPSeWe,' --target-dir '/qcdq/middledata/dealerOrderAllInfo/'$yesterday'' --query 'SELECT deviceid,create_time,carid,dealer_order_id,productid FROM dealer_order_all where (deviceid is not null and deviceid != "") and Date(create_time)= "'$yesterday'" and $CONDITIONS' --split-by deviceid 

 /opt/cloudera/parcels/CDH-5.7.5-1.cdh5.7.5.p0.3/lib/hive/bin/hive -e "
 alter table qcdq.dealerOrderAll add partition(dt='$yesterday') location '/qcdq/middledata/dealerOrderAllInfo/$yesterday';
 "
```

### 7.2 hive-to-mysql
- hive结果存入HDFS
- HDFS存入mysql
```
/opt/cloudera/parcels/CDH-5.7.5-1.cdh5.7.5.p0.3/lib/sqoop/bin/sqoop export --connect 'jdbc:mysql://192.168.120.2/xy_log_analytics' --username 'java_write_pc' --password 's#()Grermsfl,<DrWcelPSeWe,' --table serial_day_stat --columns day,serialid,platform,pv,uv --export-dir /qcdq/sqoop2/serialDayStat/$yesterday --fields-terminated-by '\001' --input-null-string '\\N' --input-null-non-string '\\N' -m 1  --update-key day,serialid,platform --update-mode allowinsert
```

### 7.3 hive-to-hive
```
// 建表
hive >CREATE EXTERNAL TABLE qcdq.newsinfo(
       	 >uid STRING,
		 >newsid STRING,
	     >)  
	     >PARTITIONED BY(dt STRING)
	     >LOCATION '/qcdq/middladata/newsinfo'

// 查询数据到HDFS

/opt/cloudera/parcels/CDH-5.7.5-1.cdh5.7.5.p0.3/lib/hive/bin/hive -e "
insert overwrite directory '/qcdq/middledata/newsinfo/$yesterday'
select distinct uid,split(exv,'\\\\|')[1] as newsid from qcdq.webspv where dt = '$yesterday' and pageid = 1405 order by uid 
"  

// 导入表，并建立分区
/opt/cloudera/parcels/CDH-5.7.5-1.cdh5.7.5.p0.3/lib/hive/bin/hive -e "
load data inpath '/qcdq/middledata/newsinfo/$yesterday' into table qcdq.newsinfo partition(dt='$yesterday')
"
```

### 7.4 hive to hbase
```
// 建立外部表
hive >CREATE EXTERNAL TABLE qcdq.newsrecommend(
       	 >uid STRING,
		 >recommend STRING,
	     >)  
	     >FIELDS TERMINATED BY ','
	     >LOCATION '/qcdq/recommend/newsrecommend/output'

// 建立hive和hbase映射表
CREATE TABLE qcdq.hive_hbase_newsrecommend(key string, value string)     
STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'   
WITH SERDEPROPERTIES ("hbase.columns.mapping" = ":key,cf1:val")   
TBLPROPERTIES ("hbase.table.name" = "qcdq_newsrecommend");

// 利用hive导入数据
insert overwrite table qcdq.hive_hbase_newsrecommend select * from qcdq.newsrecommend;
```

## 8.Hive SQL
- 取每组的前50
```
select t6.dt,t6.pageid,t6.type,t6.pv,t6.uv,t6.svid from 
(select *,rank() over(partition by svid order by pv desc) as num from(
select collect_set(dt)[0] as dt,pageid,1 as type,count(1) as pv,count(distinct uid) as uv,svid from qcdq.webspv where dt = '2017-07-30' and (svid = 'xywww' or svid = 'xym') and campsrc != 'one_net-pc' and seq = 1 group by pageid,svid) t5 ) t6 where t6.num <= 50
```






