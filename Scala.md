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
- Map
	- 可变映射
	```
	import scala.collection.mutable.Map
	val university2 = Map("XMU" -> "Xiamen University", "THU" -> "Tsinghua University","PKU"->"Peking University")
	university2("XMU") = "Ximan University" //更新已有元素的值
	university2("FZU") = "Fuzhou University" //添加新元素

	// 循环遍历操作
	// for ((k,v) <- 映射) 语句块

	for ((k,v) <- university) printf("Code is : %s and name is: %s\n",k,v)
	for (k<-university.keys) println(k)
	for (v<-university.values) println(v)

	```
- 迭代器(Iterator)
```
// while循环
val iter = Iterator("Hadoop","Spark","Scala")
while (iter.hasNext) {
    println(iter.next())
}

// for循环
val iter = Iterator("Hadoop","Spark","Scala")
for (elem <- iter) {
    println(elem)
}
```

## 3.类
- 创建类
```
class Counter {
    private var value = 0
    increment(step: Int): Unit = { value += step}
    def current(): Int = {value}
}
```
- 方法的返回值，不需要靠return语句，方法里面的最后一个表达式的值就是方法的返回值
- 创建对象
```
val myCounter = new Counter
myCounter.increment(5) //或者也可以不用圆括号，写成myCounter.increment
println(myCounter.current)
```

## 4.模式匹配
```
for (elem <- List(9,12.3,"Spark","Hadoop",'Hello)){
    val str  = elem match{
        case i: Int => i + " is an int value."
        case d: Double => d + " is a double value."
        case "Spark"=> "Spark is found."
        case s: String => s + " is a string value."
        case _ => "This is an unexpected value."
    }
println(str)    
}
```

## 5.针对集合的操作

### 5.1 遍历操作
- 列表的遍历
	- for循环遍历
	```
	val list = List(1, 2, 3, 4, 5)
	for (elem <- list) println(elem)
	```
	- foreach遍历
	```
	val list = List(1, 2, 3, 4, 5)
	list.foreach(elem => println(elem))
	```
- 映射的遍历
```
val university = Map("XMU" -> "Xiamen University", "THU" -> "Tsinghua University","PKU"->"Peking University")

// for
for ((k,v) <- university) printf("Code is : %s and name is: %s\n",k,v)

// foreach
university foreach {case(k,v) => println(k+":"+v)}
```

### 5.2 map和flatmap
```
val books = List("Hadoop", "Hive", "HDFS")
books.map(s => s.toUpperCase)
```


