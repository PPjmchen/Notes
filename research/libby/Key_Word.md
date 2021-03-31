# 关键字提取

## 1. 将Excel文件转换为json文件

1. 先使用Excel的**替换**功能将Excel中的所有双引号换成单引号

2. 打开网站https://www.bejson.com/json/col2json/，复制Excel所有的内容到面板上，点击转换：

   <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210331105138036.png" alt="image-20210331105138036" style="zoom:67%;" />

   先用记事本建一个txt文件，复制JSON代码到txt文件中，再另存为papers.json，打开后如下所示：

   <img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210331105601307.png" alt="image-20210331105601307" style="zoom:67%;" />

## 2. 运行代码

解压Get_Keywords.zip，把第二步的json文件放在目录中，在终端执行：

```bash
pip install pdftotext

python extract.py
```

在该目录下生成result.json

## 3. 导出excel

文件打开并复制所有内容，打开网站http://j2e.kpoda.com/，粘贴到面板并导出Excel2003文件.

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210331113351994.png" alt="image-20210331113351994" style="zoom:67%;" />

​	**跳出收费窗口后不用理他，过一会就会弹出下载窗口！**

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210331111355861.png" alt="image-20210331111355861" style="zoom:67%;" />

打开该文件时点击**是**打开就可以了

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210331111430022.png" style="zoom: 67%;" />

左侧栏是文章标题，右侧栏是keywords

![image-20210331111636576](https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210331111636576.png)