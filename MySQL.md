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

## 第十九章 插入数据
- INSERT INTO tableName (columnName1, columnName2) VALUES (val1, val2);

## 第二十章 更新和删除数据
- UPDATE tableName SET columnName1 = newval1, columnName2 = newval2 WHERE  // 注意添加WHERE限制，否则全部更新
- UPDATE tableName SET columnName = null WHERE // 删除某行的一列
- DELETE FROM tableName WHERE // 删除某一行
- TRUNCATE tableName // 删除所有行

## 第二十一章 创建和操纵表
- 




















