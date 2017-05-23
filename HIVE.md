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





