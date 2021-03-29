# 逐行读写txt文件

## 1. 直接以这个字符串的形式读取txt文件

```python
with open(label_path, 'r') as f:
     gt = f.read()
```

## 2. 逐行读txt文件

```python
with open(label_path, 'r') as f:
     gt = f.read().splitlines()
```

## 3. 逐行写txt文件

```python
with open('test.txt', 'w') as f:
	arr=['a','b','c']
	iLen=len(arr)

	for x in arr:
    	f.write(x+'\n')
```


