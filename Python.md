# Python

## 1.文件与流
- 常用方法
```python
# 打开文件,参数有r,w,a
file = open('filePath','r') 
# 读文件
file.read()
file.readline()

eg:
file = open(XX)
while True:
	line = file.readline()
	if not line:
		break
	process(lines)

# 写文件
file = open('filePath','w')
file.write(String)
file.close()
```

## 2.数据结构

### 2.1 list(列表)
- 可修改
- myList = [1, 2, 3, 4, 5]
- myList[1 : 3] // 第一个索引包含在分片内，第二个不包含在分片内
- list方法
```
- 复制列表
newList = mylist[:]
- lst.append(x)
- lst.count(xx)
- lst1.extend(lst2) //lst1连接了lst2,lst1会改变
- lst.index(x) // x第一个出现的索引
- lst.insert(3,'four')
- lst.pop()
- lst.remove(x)
- lst.reverse() // 改变原序列
- lst.sort(reverse=True)  // 改变原序列
```
### 2.2 Tuple(元组)
- 不可修改
- mySet = (1, 2, 3)

### 2.3 String(字符串)
- 不可修改
- String方法
```
- s.find('SubString') // 子串起点位置
- ‘.’.join(['www','baidu','com']) // split的逆操作，用.连接字符串
- s.lower()
- s.replace(st1,st2) // str2替换所有str1
- s.split()
- s.strip()
```

### 2.4 dict(字典)
- myDict = {}
- 方法
```
- myDict['001'] = '郭星宇'
- myDict.clear()
- myDict.copy() // 浅层复制
- myDict.deepcopy() // 深层复制
- myDict.get()
- myDict.has_key(k) 
- myDict.items() //返回一个列表，[(k1,v1),(k2,v2)...]
- myDict.keys() //返回一个列表
- myDict.valyes() //返回一个列表
- myDict.pop(k)
```

## 3.杂项 
- zip(a,b) //压缩序列，打包成一个元祖的列表
```
names = ['a','b','c']
ages = [21, 23, 41]
for name,age in zip(names,ages):
	print name + ':' + str(age)
```
- enumerate() //提供索引的地方迭代索引-值对
```
for index,string in enumerate(strings):
	if 'xxx' in string:
		strings[index] = '[censored]'
```
- 函数中，参数为可变的数据结构如列表，会改变实际参数

## 类
```
class Person:
	def setName(self, name):
		self.name = name
	def getName(self):
		return self.name
	// 私有方法
	def __printName(self):
		print self.name
	def greet(self):
		print 'hello' + self.name


foo = Person()
foo.setName("gxy")
print foo.getName()
foo.greet()
```

















