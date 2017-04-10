# Spark快速大数据分析

## 第一章 Spark数据分析导论

- Spark各组件的组成 
	- SparkSQL/Spark Streaming/MLib/GraphX

## 第二章 Spark下载与入门

- Spark应用通过驱动器程序(driver program)发起集群上的操作，管理多个执行器(executor)节点，一般是SparkContext
- bin/spark-sumbit 运行相关jar包
- 初始化SparkContext的方法(python版)
	- from pyspark import SparkConf, SparkContext
	- conf = SparkConf().setMaster("local").setAppName("MyApp") # 设置集群URL和应用名

## 第三章 RDD编程

- 对RDD的操作为转换操作(transformation)和行动操作(action)
- 创建RDD 
	- 读取外部数据
		- text = sc.text("file:///usr/local/spark/mycode/word.txt") # 本地
		- text = sc.text("/user/hadoop/word.txt") # HDFS
	- 创建集合传入sc.parallelize()中
	- 写入外部
		- text.saveAsTextFile('/user/hadoop/outputFile')
- RDD的转换操作
	- RDD.map(fun)
	- RDD.filter(fun)
	- RDD.faltMap(fun) 
	- RDD1.union(RDD2) # 必须类型数据相同
	- RDD1.intersection(RDD2) # 交集，必须类型数据相同
	- RDD.distinct() # 去重
- RDD的行动操作
	- RDD.reduce(fun)
	- RDD.fold(zero)(fun)
	- RDD.aggregate(zeroValue)(seqOp,combOp)
	- RDD.collect() # 收集
	- RDD.take(num) 
	- RDD.foreach(fun)
	- RDD.count()
- RDD的持久化
	- RDD.persist() # 缓存在内存中

## 第四章 键值对RDD的操作

- 创建PairRDD
	- RDD.map(lambda x: (x.split(" ")[0], x.split(" ")[1])) # 通过map创建键值对
- 常见操作
	- reduceByKey(func)
	- groupByKey()
	- mapValues(func)
	- keys()
	- values()
	- sortByKey()
	- combineByKey()
	- join(otherRDD)

- 数据分区
- example
```python

# wordCount
text = sc.textFile('test.md')
word = text.flatMap(lambda x: x.split(" "))
count = word.map(lambda x: (x, 1)).reduceByKey(lambda x, y: x + y)
count.saveAsTextFile('/user/hadoop/outputFile1')

# 其他操作
text = sc.textFile('test1')
pairRDD = text.map(lambda x: (x.split(",")[0], int(x.split(",")[1])))

# mapValues reduceByKey
result = pairRDD.mapValues(lambda x: (x, 1)).reduceByKey(lambda x, y: (x[0] + y[0], x[1] + y[1]))

# groupByKey
result = pairRDD.groupByKey().map(lambda x: (x[0], list(x[1]))).collect()

result.saveAsTextFile('/user/hadoop/outputFile2')

# 计算平均值
data = sc.parallelize(Array(("spark",2), ("hadoop",6), ("hadoop",4), ("spark",6)))

data1 = data.mapValues(lambda x: (x, 1)).reduceByKey(lambda x, y: x[0] + y[0], y[1] + y[1])

result = data1.mapValues(lambda x: x[0]/ x[1]).collect()

for line in result:
	print line
```









