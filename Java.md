# 疯狂的Java

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











