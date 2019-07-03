## 《Python for data anaysis》

### numpy

1. 创建数组
	```
	- np.array([1,2,3])
	- np.zeros((2,3))
	- np.ones((3))
	- np.random.randn(2,3)
	- np.arange(-5,5,0.1)
	```

2. 基本操作
	- 对数组的改变，是直接操作数组，所以如果需要复制的话，需要显示声明
	```
	arr_copy = arr.copy()
	```
	- reshape和转置
	```
	arr.reshape((3,5))
	arr.T
	```

3. 统计方法
	- sum, mean, std, max/min等
	```
	arr.mean()
	arr.sum(axis=0) # 列相加
	arr.sum(axis=1) # 行相加
	arr.mean(1)
	```

4. 排序和唯一
	```
	arr.sort()
	arr.sort(1) # 按行排序
	np.unique(arr)
	```

5. 随机性
	- np.random.XX

### pandas入门

1. Series和DataFrame
	- Series描述一维数组的，用一维数组+索引来表示；可以当字典用，pd.Series(dictName)
	```
	obj = Series([4, 7, -5, 0])
	obj.index(['a','b','c','d'])
	obj.isnull()
	```
	- DataFrame就是DF
	```
	df = pd.DataFrame(data)
	df = pd.DataFrame(data, columns=['year','name'])
	df.head()
	df[['year','name']]
	df.loc[0] # 取第一行
	del df['year']
	new_df = df.drop('pop',axis=1)
	```

2. 读取数据
	- csv读操作
	```
	data = pd.read_csv('/Users/guoxingyu/Documents/work/python/jupyter/info.csv', encoding='gb2312')
	data = pd.read_table('/Users/guoxingyu/Documents/work/python/jupyter/info.csv', encoding='gb2312', sep=',', names=['id','name','age','job'])
	```
	- csv写操作
	```
	data.to_csv('/Users/guoxingyu/Documents/work/python/jupyter/out.csv', encoding='gb2312')
	```

3. 数据预处理
	- 删除缺失值
	```
	#	操作Series
	data.isnull()	# 查找缺失值
	data.dropna()   # 过滤缺失值
	#	操作DataFrame
	data.dropna()   # 只要有nan的行就过滤
	data.dropna(how='all') # 过滤全部为nan的行
	data.dropna(axis=1, how='all') # 过滤全部为nan的列
	data.dropna(subset=['name','age'])
	```
	- 填充缺失值
	```
	data.fillna(0)  # 0去填充缺省值
	```
	- 

