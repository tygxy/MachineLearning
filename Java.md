# 《疯狂的Java》

## 第四章 数组

- 创建 int[] array = new int[5] {1，2，3，4，5};
- Arrays类
	- 方法
	```java
	int binarySearch(type[] a, type key); //使用二分法查找key在a数组中的索引，要求a必须升序排列
	type[] copyOf(type[] original,int length); //复制original数组的length个元素给新数组
	void sort(type[] a);
	boolean equals(type[] a, type[] b); //a,b数组长度相等，元素也相同

	//使用方法
	Arrays.sort(nums);
	Arrays.copyOf(nums, 10);
	```

## 第五章 面向对象(上)

### 5.1 类与对象

- 区分栈内存和堆内存
- 类包括成员变量，方法，构造器
```java
public class 类名 {
	//成员变量
	//方法
	//构造器
}
```
- static可以修饰成员变量和方法，属于类本身，不属于该类的某个对象，需要通过类调用
	- static修饰的成员，不能访问没有static修饰的成员，即静态成员不能访问非静态成员
- this总是指向调用该方法的对象
	- 最大作用是让类中的一个方法，访问该类中的另外一个方法
	- 如果普通方法中有个局部变量与成员变量重名，程序需要访问这个被覆盖掉的成员变量，则需要使用this

### 5.2 方法详解

- 方法的基本类型参数传递是值传递，将实际参数的副本传入方法，参数本身不受影响
	- 引用类型的参数传递，也是值传递。但是参数由于是引用变量，类似指针，实参和形参都指向同一个对象，所以还是会改变对象本身
- 方法重载，方法名相同，形参列表不同

### 5.3 成员变量与局部变量

- 成员变量包括实例变量和类变量；局部变量包括形参，方法局部变量，代码块局部变量

### 5.4 隐藏与封装
- 封装是指将对象的状态信息隐藏在对象内部，通过类提供的方法实现对内部信息的访问和操作
- package,import,import static

### 5.5 深入构造器
- 构造器的重载
```java
public class Apple{
	String name;
	String color;
	double weight;
	// 无参数构造器
	public Apple(){}
	// 两个参数构造器
	public Apple(String name, String color){
		this.name = name;
		this.color = color;
	}
	// 三个参数构造器
	public Apple(String name, String color, double weight){
		this(name, color);
		this.weight = weight;
	}
}
```
### 5.6 类的继承
- 重写，子类对父类方法的覆盖
- 子类方法中调用父类被覆盖的实例方法或者实例变量，使用super关键字
```java
public void callOverrideedMethod(){
	super.fly();
	super.name = "xxx";
}
```
- 子类不会获得父类的构造器，但是可以调用父类的构造器，使用super，而且至少(隐式)调用一次
```java
class Base {
	public double size;
	public String name;
	public Base(double size, String name) {
		this.size = size;
		this.name = name;
	}
}
public class Sub extends Base {
	public String color;
	public Sub (double size, String name, String color) {
		super(size, name);
		this.color = color;
	}
}
```
- 如果父类方法需要被外部调用，用public修饰，但是又不希望子类重写，可以使用final

### 5.7 多态
- 相同类型的变量、调用同一个方法时呈现出多种不同的行为特征，这就是多态
	- 父类的引用指向子类对象
- instanceof，用于判断前面的对象是否是后面的类，或者子类
```java
Object hello = "hello";
System.out.println(hello instanceof Object)
```
- (type)variable 可以强制类型转换


### 5.8	继承与组合
- 组合是把旧类对象作为新类的成员变量组合进来

### 5.9 初始化块
- 在构造器前执行
- 静态块在类被加载时就会被调用


## 第六章 面向对象(下)

### 6.1 Java增强的包装类
- 基本类型转换成字符串
```java
String dbStr = String.valueOf(2.345);
String boolStr = String.valueOf(ture);
```
- 字符串转换成基本类型
```java
String intStr = "123";
// 方法一
int it1 = new Interger(intStr);
// 方法二
int it2 = Interger.parseInt(intStr);
```

### 6.2 处理对象
- toString()
- ==和equals()
	- 对于两个引用类型变量，只有他们指向同一个对象时，才会返回true
	- str1.equals(str2) // 如果str1和str2值相同，可以判断为true，因为String的equals()已经重写了

### 6.3 类成员
- 类成员包括类变量，类方法，静态初始化块，内部类

### 6.4 final修饰符
- 可以修饰类，变量，方法，用于表示它修饰的类，变量，方法不可变
- final修饰基本类型，不能对基本类型重新赋值，但是final修饰引用类型时，只保证引用类型变量引用的地址不改变，但是引用对象可以改变
- final修饰的方法，不可重写；子类不可重写父类的final方法；但是可以重载
- final修饰的类不可有子类

### 6.5 抽象类
- 抽象类和抽象方法用abstract修饰，有抽象方法的类必须定义成抽象类
- 抽象类不能实例化，只能当成父类被子类继承
- 抽象方法只有方法签名，没有方法实现的方法，用abstract修饰，方法体为空，最后增加;
- 抽象类举例
```java
public abstract class Shape{
	private String color;
	public abstract double calPerimeter();
	public Shape(){

	}
	public Shape(String color){
		this.color = color;
	}
}

public class Triangle extends Shape {
	private double a;
	private double b;
	private double c;
	public Triangle(String color, double a, double b, double c){
		super(color);
		this.a = a;
		this.b = b;
		this.c = c;
	}
	public double calPerimeter(){
		return a + b + c;
	}	
}
```

### 6.6 接口
- 定义了某一批类所需要遵循的规范，接口包括成员变量(静态变量)，方法(抽象实例方法，类方法或默认方法)，内部类
```java
public interface Output {
	void out();
	void getData(String message);
}

public class machine implements Output {
	
}
```

### 6.7 内部类
- 非静态内部类
	- 可以访问外部类的私有变量，但是反过来不行；外部类必须显式的创建非静态内部类对象，才能使用内部类的成员
	- 如果外部类变量，内部类变量重名，则用外部类类名.this和this区分
- 静态内部类
- 使用内部类
	- 外部类内部使用内部类
	- 外部类之外使用非静态内部类
	- 外部类之外使用静态内部类

### 6.8 Lambda表达式

### 6.9 枚举类
- 关键字enum定义枚举类

### 6.10 对象与垃圾回收


## 第七章 Java基础类库

### 7.1 与用户互动
- main函数中public static void main(String[] args){}
	- 执行java 类名 参数，就可以将参数输入到args中
- Scanner 获取输入
```java
// 读取键盘输入
Scanner sc = new Scanner(System.in);
while(sc.hasNext()) {
	System.out.println(sc.next());
}

// 读取文件内容
Scanner scFile = new Scanner(new File("Test.java"));
while(scFile.hasNextLine) {
	System.out.println(scFile.nextLine());
}
```

### 7.2 系统相关

### 7.3 常用类
- Object类
	- equals() 
		- Object中的equals()是比较两个对象是否同一内存地址，即是否引用同一对象
		- 但是实际是对两个对象进行内容的比较，不是引用的比较，所以需要重写
- String类和StringBuffer
	- String创建之后字符串序列不可变，但是StringBuffer创建后字符串可以改变
	- String的方法
	```java
	// 构造器String()
	String s = new String("我是字符串");

	// char charAt(int index) 获取指定位置的字符
	System.out.println(s.charAt(5));

	// boolean equals(Object anObject) 比较字符串与Object对象的字符串内容是否相等

	// == 判断内容与地址是否相同

	// int indexOf(String str) 找出str子串在该字符串第一次出现的位置

	// String substring(int beginIndex, int endIndex) 返回从beginIndex到endIndex的子字符串

	// static String valueOf(X x) 将基本类型转换成String
	String dbStr = String.valueOf(2.345);

	// String[] arr = s.split(" "); String拆分

	// s.toCharArray()  把String拆成char数组

	// s.toUpperCase()  把String全部大写

	// s.toLowerCase()  把String全部小写
	```
	- null是String没有new,引用没有创建，没有内存;""和new String()是已经new了，只不过内部为空
	- 推荐直接赋值,String s = 'aa'，除非有必要才会创建String对象，String s = new String("aa")
	- 字符串的拼接
		- concat(); append(); "+"
	- StringBuffer的方法
	```java
	StringBuffle sb = new StringBuffle();

	sb.append("java");
	sb.insert("hello ");
	sb.replace(5, 6, ",");
	sb.delete(5, 6);
	sb.reverse();
	sb.setCharAt(i,'1'); 
	sb.setLength(10); //返回10位的sb
	```
- Math类
- Random类
- BigDecimal类
- Calendar类
- Integer类
	- 进制之间的转换
	 	- 十进制转二进制 Integer.ToBinaryString(int i)
	 	- 二进制转十进制 Integer.valueOf(String str,2).toString() 

### 7.5 正则表达式


## 第八章 Java集合

### 8.1 Java集合概述
- 集合主要由Collection和Map两个接口派生而来
	- Collection包括Set，Queue，List三个子接口，已经派生出来的实现类
	- Map接口派生众多实现类

### 8.2 Collection和Iterator接口
- Collection的接口方法
```java
boolean add(Object o);
boolean addAll(Collection c);
void clear();
boolean contains(Object o);
boolean containsAll(Collection c);
boolean isEmpty();
boolean remove(Object o);
boolean removeAll(Collection c); //删除集合c中的所有元素
boolean retainAll(Collection c); //删除集合c中不包含的元素
int size();
Iterator iterator(); //返回Iterator对象
Object[] toArray(); //集合转换成数组，所有集合元素变为对应的数组元素,变成了Object
T[] toArray(T[] a); //集合转换成类型为T的数组 eg:res.ToArray(new String[0])
```
- Iterator的方法
```java
boolean hasNext();
Object next();
void remove(); //删除集合上一次next方法返回的元素

Collection books = new ArrayList();
books.add("Think in Java");
books.add("python");
Iterator it = books.iterator();
while (it.hasNext()) {
	System.out.println((String)it.next()); //next()返回的对象是Object，需要强制类型转换
}
```
### 8.3 Set集合
- 不可重复，没有顺序，基本操作与Collection相同
- HashSet类
	- 是Set接口的典型实现，使用Hash算法存储集合元素
- TreeSet类
	- 是SortedSet接口的实现类，是有序的
	- TreeSet的额外方法
	```java
	Object first() //返回集合第一个元素
	Object last() 
	Object lower(Object e) // 返回小于e的最大元素
	Object higher(Object e) 
	SortedSet subSet(Object fromElement, Object toElement) //返回从fromElement(包含)到toElement(不包含的子序列
	```
	- 如果将一个对象插入到TreeSet中，则该对象的类必须实现Comparable接口
- EnumSet类

### 8.4 List集合
- List接口方法
```java
void add(int index, Object element);
Object get(int index); //返回索引为index的对象
Object remove(int index);
int indexOf(Object e); //返回e第一次出现的索引
Object set(int index, Object e); 将index处的元素替换成e
List subList(int fromIndex, int toIndex);  
sort();
replaceAll();
```
- ListIterator
	- 提供了反向迭代的能力
	```java
	boolean hasPrevious()
	Object Previous()
	```
- ArrayList和Vector实现类

### 8.5 Queue集合
- Queue接口的方法
```java
void add(Object e) //添加到尾部
Object peek()  //返回头部元素，但不删除
Object poll()  //返回头部元素，删除
```
- PriorityQueue实现类
	- 保存队列顺序是按照大小顺序排列，这点已经不是队列了

- Deque接口
	- 是Queue的子接口，双端队列

- ArrayDeque是Deque的实现类，可以当栈和队列使用
	- 当栈使用
	```java
	ArrayDeque stack = new ArrayDeque();
	stack.push(Object e);
	stack.peek();
	stack.pop();
	```
	- 当队列使用
	```java
	ArrayDeque queue = new ArrayDeque();
	queue.offer(Object e);
	queue.peek();
	queue.poll();
	```
- LinkedList实现类
	- LinkedList是基于链表，而ArrayList和ArrayDeque是基于数组
	- LinkedList 可以作为List集合，双端队列，栈使用

### 8.6 Map集合
- Map接口的方法
```java
boolean containsKey(Object key)
boolean containsValue(Object Value)
Object get(Object key)
Set keySet() //返回Map中所有key组成的Set集合
Object put(Object key, Object value)
Object remove(Object key)
Collection values() //返回Map中所有value组成的Collection集合
//遍历
for (Object key : map.keySet()) {
	map.get(key);
}
```
- HashMap实现类
	- 作为Key的对象必须实现hashCode()和equals()方法
	- LinlkedHashMap子类，使用双向链表维护key-value的次序
- SortedMap接口和TreeMap实现类

### 8.7 操作集合的工具类Collections
- 排序操作
```java
void reverse(List list);
void sort(List list); 
void shuffle(list);
// 使用方法
Collections.reverse(list);
```
- 查找,替换操作
```java
int binarySearch(List list, Object key); //使用二分查找搜索list,获得指定对象key在list集合中的索引
int frequency(Collection c, Object o); //返回指定集合中指定对象的出现次数
Object max(Collection c);
// 使用方法
Collections.frequency(list, -5);
```
- 同步控制
	- 保证线程安全
	```java
	List list = Collections.synchronizedList(new ArrayList());
	```

## 第九章 泛型
- 泛型入门
	- 允许程序在创建集合时指定集合元素的类型
	```java
	List<String> list = new ArrayList<>();
	Map<Integer, String> map = new HashMap<>();
	```
- 深入泛型
	- 可以为任何类，接口增加泛型声明

- 泛型方法


## 第十章 异常处理
- 异常处理机制
	- try...catch捕获异常,先捕获小异常，再捕获大异常
	```java
	try{
		// 业务实现代码
	}catch (SubException e1) {
		// 异常处理块1
	}catch (SubException e2) {
		// 异常处理块2
	}finally {
		// 资源回收
	}
	```
	- 访问异常信息
	```java
	getMessage() //返回异常的详细描述信息
	printStactTrace() //返回异常的跟踪栈信息
	```
	- 使用finally回收物理资源
		- 垃圾回收机制回收内存，但是如数据库连接，网络连接，磁盘文件等物理资源必须显示回收
		- finally一定会执行

- Checked异常和Runtime异常体系

- 使用throw抛出异常
	- 与throws有区别，throw是自行抛出的异常
	- throw new Exception()

## 第十五章 输入/输出

### 15.1 File类

- 访问文件名相关的方法
```java
String getName(); //返回File对象所表示的文件名或路径名
String getPath(); //返回路径名
String getAbsulutePath(); //绝对路径名
boolean renameTo(File newName); //重命名
```
- 文件检测相关的方法
```java
boolean exists();
boolean isFile();
boolean isDirectory();
```
- 文件操作相关的方法
```java
boolean createNewFile(); //如果File对象对应的文件不存在，就创建一个File对象指定的新文件
boolean delete();
```
- 目录操作相关的方法
```
boolean mkdir();  //创建File目录
String[] list(); //返回File对象所有子文件名
File[] listFile(); //返回File数组
```

### 15.2 理解Java的IO流
- InputStream/Reader 所有输入流的基类，前者是字节输入流，后者是字符输入流
  OutputStream/Writer 所有输出流的基类，前者是字节输出流，后者是字符输出流

### 字节流和字符流
- InputStream/Reader是抽象基类，不能创建实例
	- InputStream方法，实例是FileInputStream
	```java
	int read() //读取单个字节
	int read(byte[] b) //读取b.length个字节的数据，并存储在b中
	```
	- Reader方法，实例是FileReader
	```java
	int read()
	int read(char[] chuf)

	// example
    FileReader reader = new FileReader("/Users/guoxingyu/Documents/work/java/CrazyJava/Ch04/test/hello.txt");
    char[] chuf = new char[100];  // 每次只取这么多字符
    int hasRead = 0;
    while ((hasRead = reader.read(chuf)) > 0) {
        System.out.println(new String(chuf, 0 , hasRead));
    }
    reader.close()
	```
- OutputStream和Writer
	- Writer方法，实例是FileWriter
	```java
	void write(String str);

	// example
    FileWrite wr = new FileWriter("/Users/guoxingyu/Documents/work/java/CrazyJava/Ch04/test/test.txt");
    wr.write("我是郭大宝");
    wr.close() // 必加，否则写不进去
	```

## 总结

### Java集合框架

#### 总体框架
![](raw/1.jpg?raw=true)

#### List
- ArrayList
- LinkedList

#### Set
- HashSet
- LinkedHashSet
- TreeSet
	- 是一种排序二叉树，有序

#### Map
- HashMap
- TreeMap



# 《Java程序员面试宝典》

## Java基础知识
- Java程序初始化的顺序
	- 父类静态变量、父类静态代码块、子类静态变量、子类静态代码块、父类非静态变量、父类非静态代码块、父类构造函数、子类非静态变量、子类非静态代码块、子类构造函数
	- 针对一个类来说，静态变量、静态代码块、非静态变量、非静态代码块、构造函数
- Java中的作用域
	- 成员变量与实例化的对象作用范围相同;静态变量被所有实例共享，与类作用范围相同;局部变量在花括号内有效
	- public当前类，同一package，子类，其他package都可用;private只有第一个;protected可在前三个;default可在前两个
- clone方法的作用 
	- Java在处理基本数据类型时，使用的是值传递(传递输入参数的复制)，其他类型都是按引用传递(传递是对象的一个引用)的方式执行;对象除了函数调用是引用传递外，在使用“=”赋值时也是引用传递
	- 针对上述情况，如果需要对已有对象A创造出相同状态的B，且对B修改不影响A，则使用obj类中的clone方法
	- 浅复制和深复制，判断依据是类中是否存在非基本类型(即对象)的数据成员
	- 浅复制：被复制对象的所有变量都与原来对象相同，但是对象的引用仍指向原来的对象；深度复制则把引用其他对象的变量指向被复制的新对象
- 反射机制 可以在运行时动态的创建类的对象
- 组合 在新类里面创建原有类的对象，重复利用类的功能
- 多态两种表现形式，编译时的多态通过方法的重载实现；运行时的多态通过方法的重写实现












