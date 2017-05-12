# MySQL必知必会

## 第一章 了解SQL
- 表，列，行，主键，外键

## 第三章 使用MySQL
- USE databaseName;
- SHOW databases;
- SHOW tables;
- DESCRIBE tableName;

## 第四章 检索数据
- SELECT columnName1, columnName2 FROM tableName;
- DISTUNCT // 只返回不同的值，需要直接放在列名的前面，而且是作用于所有列
- LIMIT num; LIMIT num1,num2  //从num1开始的num2个,检索的第一行是0
	- SELECT columnName1 FROM tableName LIMIT 5,5

## 第五章 排序检索数据
- ORDER BY
	- SELECT columnName1 FROM tableName ORDER BY columnName1;
	- SELECT columnName1,columnName2 FROM tableName ORDER BY columnName1, columnName2 //先columnName1排序，再columnName2排序
	- 默认是升序，DESC则为降序，只应用于直接位于其前面的列名
		- SELECT columnName1,columnName2 FROM tableName ORDER BY columnName1 DESC, columnName2;

## 第六章 过滤数据
- WHERE
	- 操作符 = != > < <= >= BETWEEN..AND
	- AND OR 
	- IS NULL 
		- WHERE columns IS NULL;
	- IN (val1, val2)
		- WHERE id IN (100, 101);
	- NOT

## 第八章 用通配符进行过滤
- LIKE 
	- WHERE columnName LIKE '%jet%';
- 下划线_，只匹配一个字符
	- WHERE columnName LIKE 'jet_';

## 第九章 用正则表达式搜索
- REGEXP
	- SELECT prod_name FROM products WHERE prod_name REGEXP '1000|2000'; // 包含1000或者2000
	- SELECT prod_name FROM products WHERE prod_name REGEXP '[0-9] Ton'; // 0-9可选，^表示非，[a-z]
	- 如果需要匹配特殊字符，比如.、|、[]等，用\\为前导，例如\\-,就是查找-
	- \\f 换页 \\n 换行 \\r 回车 \\t 制表
	- 匹配字符类
		- [:alnum:] // 任意字母数字
		- [:alpha:] // 任意字母
		- [:digit:] // 任意数字
	- 重复元字符 *(0或多) +(1或多) ？(0或1) {n} {n,} {n,m} 
		- sticks? // ?前面的字符出现0次或者1次 即匹配出sticks和stick
		- [[:digit:]]{4} // 4位数字
	- 定位符	^ 文本开始， $ 文本结尾，[[:<:]] 词的开始，[[:>:]] 词的结尾
		- REGEXP '^[0-9\\.]' // 以0-9和.开头
	- LIKE 匹配整个字符串，而REGEXP匹配的是字符子串
	


## 第十章 创建计算字段
- 拼接字段
	- SELECT Concat(columnName1, '(', columnName2, ')') FROM tableName;
	- RTrim(columnName1) // 去掉值右边的所有空格，同理LTrim()去掉左空格，Trim()去掉左右空格
- 使用别名
	- AS
		- SELECT Concat(columnName1, '(', columnName2, ')') AS columnNewName FROM tableName;
- 执行算术计算
	- 常用的算术运算符 + - * /

## 第十一章 数据处理函数
- 文本处理函数
	- Left() //返回串左边的字符
	- Length() 
	- Locate() //找到串的子串
	- Lower()
	- Upper()
	- RTrim() LTrim() Trim()
	- SubString() //返回子串的字符
- 日期与时间处理函数
	- 日期格式为yyyy-mm-dd '2017-05-08'
	- Date()
	- DateDiff() //返回日期之差
	- Day()
	- DayOfWeek()
	- Year()
	- Month()
	- Time() //返回时间
	- Hour()
	- Minute()
	- Second()
- 数值处理函数
	- Abs()
	- Mod() //余数
	— Rand()

## 第十二章 汇总数据
- 聚集函数
	- AVG() //某列的平均值
	- COUNT() //某列的行数，会忽略NULL，如果COUNT(*)则会统计所有行
	— MAX()
	- MIN()
	- SUM() //某列之和

## 第十三章 分组数据
- GOURP BY
	- SELECT vend_id, COUNT(*) FROM tableName GROUP BY vend_id;
	- GROUP BY子列中的列必须是检索列或者表达式，不能是聚合函数
— HAVING
	- WHERE过滤的是行而不是分组，使用HAVING替代;WHERE是数据分组前过滤，HAVING是数据分组后过滤
	- SELECT vend_id, COUNT(*) AS num_prods FROM tableName GROUP BY vend_id HAVING num_prods > 3;
	- SELECT vend_id FROM products WHERE prod_price >= 10 GROUP BY vend_id HAVING COUNT(*) >=2;
- 分组与排序
- SELECT子句顺序
	- SELECT
	- FROM
	- WHERE
	- GROUP BY
	- HAVING
	- ORDER BY
	- LIMIT

## 第十四章 使用子查询
- 嵌套在其他查询中的查询

## 第十五章 联结表
- 外键 某个表的一列，它包含另一个表的主键
- 内部联结 
	- SELECT vend_name, prod_name, prod_price FROM vendors INNER JOIN products ON vendors.vend_id = products.vend_id;
	- SELECT vend_name, prod_name, prod_price FROM vendors,products WHERE vendors.vend_id = products.vend_id;
- 使用表别名
	- SELECT vend_name, prod_name, prod_price FROM vendors AS a,products AS b WHERE a.vend_id = b.vend_id;
- 自联结
	- SELECT p1.prod_id, p1.prod_name FROM products AS p1, products AS p2 WHERE p1.vend_id = p2.vend_id AND p2.prod_id = 'DTNTR';
- 自然联结
	- 列只能返回一次，需要人工进行筛选
- 外部联结
	- SELECT customers.cust_id, orders.order_num FROM customers LEFT OUTER JOIN orders ON customers.cust_id = orders.cust_id  // customers表中的所有行都包括
- 带聚集函数的联结
	- SELECT customers.cust_name, customers.cust_id,COUNT(order.order_num) AS num_ord FROM customers LEFT OUTER JOIN orders ON customers.cust_id = orders.cust_id GROUP BY customers.cust_id

## 第十七章 组合查询
- UNION
	- SELECT vend_id, prod_id, prod_price FROM products WHERE prod_price <= 5 UNION SELECT vend_id, prod_id, prod_proce FROM products WHERE vend_id IN (1001,1002);  //两个SELECT查询的结果，组合成单个查询结果返回
	- UNION中的每个查询必须包含相同的类，表达式或聚集函数
	- UNION会默认去重
	- 排序输出时，ORDER BY加在最后一个SELECT

## 第十八章 全文本搜索

## 第十九章 插入数据
- INSERT INTO tableName (columnName1, columnName2) VALUES (val1, val2);

## 第二十章 更新和删除数据
- UPDATE tableName SET columnName1 = newval1, columnName2 = newval2 WHERE  // 注意添加WHERE限制，否则全部更新
- UPDATE tableName SET columnName = null WHERE // 删除某行的一列
- DELETE FROM tableName WHERE // 删除某一行
- TRUNCATE tableName // 删除所有行

## 第二十一章 创建和操纵表
- 创建表
```
CREATE TABLE tableName 
(
	columnName1   int   NOT NULL  AUTO_INCREMENT,
	columnName2   char(50) NULL ,
	columnName3   int   NOT NULL   DEFAULT 1,
	...
	PRIMARY KEY(olumnName1)
)
```
- 更新表
	- ALTER TABLE tableName ADD columnName char(50); //添加新列
	- ALTER TABLE tableName DROP COLUMN columnName; //删除新列
	- 定义外键
	```
		ALTER TABLE tableName1 
		ADD CONSTRAINT fk_tableName1_tableName2
		FOREIGN KEY (columnName) REFERENCES tableName2 (columnName);

	```
- 删除表
	- DROP TABLE tableName
- 重命名表
	- RENAME TABLE tableName TO newtableName
	






















