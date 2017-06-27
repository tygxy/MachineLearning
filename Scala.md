# scala

## 1.scala基础
- range
```
1 to 10 by 2
1 until 5
```
- for循环
```
// for (变量<-表达式) 语句块
for (i <- 1 to 5) 
	println(i)

for (i <- 1 to 5 if i%2==0; j <- 1 to 3 if j!=i) 
	println(i*j)

// 生成一个集合
for (i <- 1 to 5 if i%2==0) yield i 
```


## 2.数据结构
- 数组Array
```
val myStrArr = new Array[String](3) // 声明一个长度为3的字符串数组，每个数组元素初始化为null
myStrArr(0) = "BigData" // 在Scala中，对数组元素的应用，是使用圆括号

val intValueArr = Array(12,45,33) // 简洁初始化和赋值

```
- 列表List,各个元素相同类型
```
val intList = List(1,2,3)
```
- 元组，各个元素可以不同类型
```
val tuple = ("BigData",2015,90.0)
println(tuple._1)  // 输出‘BigData’

```
- 集Set,不重复元素结合,包括可变集和不可变集
 	- 可变集合
	```
	import scala.collection.mutable.Set
	val myMutableSet = Set("Database","BigData")
	myMutableSet += "Cloud Computing"

	```
- 

