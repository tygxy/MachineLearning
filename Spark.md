# Spark快速大数据分析

## 执行spark程序
- scala
```
spark-submit --master yarn-cluster  --driver-memory 8G --num-executors 100 --executor-cores 5 --executor-memory 4G  --class NewsRecommend  ./NewsRecommend.jar
```

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

# Spark技术内幕

## 第1章 spark简介
- 深入理解driver(主控程序，提交job,将job转换成Task,协调executor中Task调度),cluster manager,executor,job,stages,task的关系
- applicaiton(用户写的应用程序),SparkContext(向cluster manager申请资源)
## 第2章 spark学习环境的搭建
## 第3张 rdd实现详解
- 分布式数据集的容错性有两种方式：数据检查点和记录数据的更新（17页）
- RDD是只读，分区的集合；一个分区就是一个Task处理的基本单元，分区决定并行计算的颗粒，partition和task数量一致
- partitioner是RDD的分区函数，一种是基于哈希的HashPartitioner;一种是基于范围的RangePartitioner,并且分区函数只对key-value的RDD
- reduceByKey和groupByKey区别
	- ReduceByKey有本地merge，但是groupByKey是所有键值对都需要移动；此外groupByKey不能接受自定义函数，只能分组后再map,但是ReduceByKey后面可以接自定义函数
- 常见算子
	- map,filter,flatMap,union,distinct,groupByKey,reduceByKey,sortByKey,join
	- reduce,collect,count,take,saveAsTextFile,countByKey,foreach
- 根据计算逻辑生成DAG
- 宽依赖，窄依赖
	- 每个parent RDD的partition最多被子RDD的一个partition使用，即为窄依赖
	- 子RDD的partition依赖多个parent RDD的partition，即为宽依赖，需要shuffle
- 划分stage的依据 是否有shuffle过程(是否有宽依赖)，
- 划分job的依据 是否有action动作
## 第4章 Scheduler(任务调度)模块详解
- stage划分
	- 划分依据 shuffle
	- 划分过程从一个job的最后一个RDD开始，根据它的依赖关系，倒着往前推
- Task是集群运行的基本单元，有ShuffleMapTask和ResultTask
## 第5章 Deploy模块详解
- Cluster Manager部署方式有standalone,local,yarn,EC2,Mesos等
- yarn Cluster模式，yarn Client模式，区别在于用户提交的application的spark Context在本机运行，适合application与本地有交互的场景
## 第6章 Executor模块详解
- 对于同一个application，在一个worker上只有一个executor,带不代表一个物理节点只有一个executor，可以在一个物理节点部署多个worker
- 参数设置 spark.executor.memory 最多使用内存大小，每个executor上支持的任务数量取决于executor持有的CPU core的数量
- 一个Executor内同一时刻可以并行执行的Task数由总CPU数／每个Task占用的CPU数决定，即spark.executor.cores / spark.task.cpus
## 第7章 Shuffle模块详解
- spark.shuffle.manage
	- 两种shuffle方式，hash based shuffle和sort based shuffle
- spark.shuffle.spill
	- 默认是true，指shuffle过程中如果内存中数据超过阈值，是否将部分数据临时存入外部存储
- spark.shuffle.memoryFraction
	- 当shuffle过程中内存达到总内存的多少比例时开始spill，相当于设置shuffle站内存大小
## 第8章 Storage模块详解
- 存储级别
	- disk_only,memory_only,memory_only_ser,memory_and_disk,memory_and_disk_ser
- spark.storage.memoryFraction
	- 决定内存中有多少用于RDD cache，默认0.6








