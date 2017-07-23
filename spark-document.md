# Spark Official Document

## 1. 快速入门

## 2. 编程指南

### 2.1 spark的初始化
- 必须创建SparkContext对象
```
import org.apache.spark.SparkContext
import org.apache.spark.SparkConf

val conf = new SparkConf().setAppName(appName).setMaster(master) 
val sc = new SparkContext(conf)
```
### 2.2 RDDs

- 并行集合
	- sc.parallelize()创建并行集合
	```
	val data = Array(1, 2, 3, 4, 5)
	val distData = sc.parallelize(data)
	```
- 外部数据集
	- sc.textFile()
	```
	val distFile = sc.textFile(URLPath)
	```
- RDD操作
	- 基础
		- 两种类型操作，transformations和actions
		- RDDs.cache()持久化到内存中
		- RDDs.collect()集中在一个节点上
		- 


