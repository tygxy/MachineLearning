# Python

## 文件与流
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