# HIVE

## 1.HIVE SQL

### 1.1 DQL操作:数据查询SQL

- 基本select操作
```
SELECT [ALL | DISTINCT] select_expr, select_expr, ...
FROM table_reference
[WHERE where_condition]
[GROUP BY col_list [HAVING condition]]
[   CLUSTER BY col_list
  | [DISTRIBUTE BY col_list] [SORT BY| ORDER BY col_list]
]
[LIMIT number]
```
	- DISTRIBUTE BY 是会到一个reducer去处理
	- ORDER BY 是全局排序，SORT BY 是本机做排序

- 查询结果输出
	- 写入HDFS中
	```
	hive> INSERT OVERWRITE DIRECTORY '/tmp/hdfs_out' 
			SELECT a.* FROM invites a WHERE a.ds='<DATE>';
	```
	- 写入本地
	```
	hive> INSERT OVERWRITE LOCAL DIRECTORY '/tmp/local_out' 
			SELECT a.* FROM pokes a;
	```
- 删除hive drop table tableName;
- 清空hive内部表 truncate table tableName;
- 清空hive外部表 



## 2.HIVE-MYSQL

- mysql-To-hive
	- 创建hive分区外部表(分区属性不能在创建表的属性集合中重复出现)
	```
	hive >CREATE EXTERNAL TABLE qcdq.appuser(
       	 >dvid STRING,
		 >appkey STRING,
		 >chan STRING
	     >)  
	     >PARTITIONED BY(dt STRING)
	     >ROW FORMAT DELIMITED
	     >FIELDS TERMINATED BY '\t'
	     >STORED AS TEXTFILE;  
	```
	- 【补充】hive内部表导入数据方法
	```
	hive >load data inpath '/qcdq/middledata/pagecateinfo' into table qcdq.pagecateinfo
	```
	- 导入数据，建立分区
	```
	/opt/cloudera/parcels/CDH-5.7.5-1.cdh5.7.5.p0.3/lib/sqoop/bin/sqoop import --connect 'jdbc:mysql://192.168.104.2:3306/dealer?characterEncoding=utf8&useSSL=false' --username 'java_write_pc' --password 's#()Grermsfl,<DrWcelPSeWe,' --target-dir '/qcdq/middledata/dealerOrderAllInfo/'$yesterday'' --query 'SELECT deviceid,create_time,carid,dealer_order_id,productid FROM dealer_order_all where (deviceid is not null and deviceid != "") and Date(create_time)= "'$yesterday'" and $CONDITIONS' --split-by deviceid 

	/opt/cloudera/parcels/CDH-5.7.5-1.cdh5.7.5.p0.3/lib/hive/bin/hive -e "
	alter table qcdq.dealerOrderAll add partition(dt='$yesterday') location '/qcdq/middledata/dealerOrderAllInfo/$yesterday';
	"
	```

- hive-To-mysql
	- hive查询结果存入HDFS
	```
	/opt/cloudera/parcels/CDH-5.7.5-1.cdh5.7.5.p0.3/lib/hive/bin/hive -e "
	insert overwrite directory '/qcdq/sqoop2/serialDayStat/$yesterday'
	select collect_set(dt)[0] , serialid, svid, count(1) as pv, count(distinct uid) as uv
	from(
	select b.serialid as serialid, svid, uid, dt
	from qcdq.webspv a join qcdq.carparam b on a.exv = b.id
	where (
	       a.pageid = 1104 or a.pageid = 1106 or a.pageid = 1109 or a.pageid = 1110 or 
	       a.pageid = 1206 or a.pageid = 2106 or a.pageid = 2107 or a.pageid = 2109 or 
	       a.pageid = 2203 or a.pageid = 3111) 
	       and dt = '$yesterday'
	       
	union all

	select  b.id as serialid, svid, uid, dt
	from qcdq.webspv a join qcdq.carserial b on a.exv = b.urlspell
	where (a.pageid = 1102 or a.pageid = 1103 or a.pageid = 1105 or a.pageid = 1107 or 
	       a.pageid = 1112 or a.pageid = 1202 or a.pageid = 1203 or a.pageid = 2102 or 
	       a.pageid = 2108 or a.pageid = 2201 or a.pageid = 2202)
	       and dt = '$yesterday'

	union all

	select b.id as serialid, svid, uid, dt
	from qcdq.webspv a join qcdq.carserial b on a.exv = b.id
	where (a.pageid = 3102 or a.pageid = 3103 or a.pageid = 3104 or a.pageid = 3105 or
	       a.pageid = 3106 or a.pageid = 3107 or a.pageid = 3108 or a.pageid = 3109 or 
	       a.pageid = 3110 or a.pageid = 3112 or a.pageid = 3113 or a.pageid = 3200 or
	       a.pageid = 3201 or a.pageid = 2203)
	       and dt = '$yesterday'
	) serialDayStat
	Group by svid, serialid
	Sort by serialid
	"
	```
	- HDFS存入mysql
	```
	/opt/cloudera/parcels/CDH-5.7.5-1.cdh5.7.5.p0.3/lib/sqoop/bin/sqoop export --connect 'jdbc:mysql://192.168.120.2/xy_log_analytics' --username 'java_write_pc' --password 's#()Grermsfl,<DrWcelPSeWe,' --table serial_day_stat --columns day,serialid,platform,pv,uv --export-dir /qcdq/sqoop2/serialDayStat/$yesterday --fields-terminated-by '\001' --input-null-string '\\N' --input-null-non-string '\\N' -m 1  --update-key day,serialid,platform --update-mode allowinsert
	```




