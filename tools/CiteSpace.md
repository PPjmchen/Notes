# CiteSpace用法

## 一. 基于Web Of Science的文献关键词提取

### 1. 在WOS上导出所需文献

检索时数据库选择Web of Science核心合集：

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210329180733416.png" alt="image-20210329180733416" style="zoom: 67%;" />

选择要到处的文献：

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210329191915266.png" alt="image-20210329191915266" style="zoom: 67%;" />

其中，**记录内容选择全记录与引用的参考文献，文件格式选择纯文本**

![image-20210329191953998](https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210329191953998.png)

保存文件时，文件名以download开头，后缀随意：

![](https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210329192123936.png)

### 2. 在CiteSpace上做关键词提取

刚进入CiteSpace，需要点最下方的Agree才能进入主面板：

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210329192519266.png" alt="image-20210329192519266" style="zoom:50%;" />

New一个新项目，其中需要填写参数有以下四个：

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210329193010099.png" alt="image-20210329193010099" style="zoom:67%;" />

- Title：给当前项目起一个名字
- Project Home：给当前项目指定一个路径（先提前新建好文件夹）
- Data Directory：存放当前项目所需的导出文献数据（先建一个data文件夹，再把第一步导出的文献文本数据放在该目录里）
- Data Source：选择WoS

填写完毕以后点击save即可

![image-20210329192700624](https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210329192700624.png)

回到主面板，填写文献分析相关参数：

- Time Slicing：筛选文献时间区间
- Text Processing：选择需要分析的文献内容，可选Title，Abstract等
- Node Types：选择keyword，其他根据需要多选

> 在Node Types中，**Author**，**Institution**以及**Country**是用来进行Co—authorship分析的，他们之间的差异仅仅是在分析合作上的主体粒度不同而已（可以分别理解为微观合作、中观合作和宏观合作），在分析时它们是可以多项选择的。
>
> **Term**分析的功能是对文献中*名词性术语*的提取，主要*从文献的标题、摘要、关键词和索引词位置提取*；**Keyword**主要是对作者的原始关键词的提取。Term和Keyword常常用来对文本主题进行共词（co—words）的挖掘分析。**Category**是对文献中标引的科学领域的共现分析（co—occurrence）,这种分析有助于了解对象文本在科学领域的分布情况。
>
> **Reference**是文献的共被引，**Author**是作者的共被引，**Journal**是期刊的共被引。
>
> **Ariticle**是文献的耦合分析功能，**Grant**则是对研究基金的分析（Web of Science的数据从2008年才增加了资助基金数段内容，因此不要对2008年之前的数据进行基金分析）。
>
> 在使用CiteSpace生成的各种图谱中，作者的合作图谱中的节点大小表示作者、机构或国家/地区发表论文的数量，之间的连线反应合作关系的强度。论文的主题、关键词以及科学类别的网络图谱中，节点的大小代表它们出现的频次，之间的连线表示共现强度。共被引分析图谱中，节点的大小反映了被引用的次数。文献的共被引反映了单个文献的引用次数，作者的共被引网络反映了作者被引用的次数，期刊的共被引网络中节点大小反映期刊被引用的次数，之间的连线反映了共被引的强度。在文献耦合网络中，一个节点代表一篇论文，节点的大小可以按照被引频次显示，节点之间的连线反映了耦合强度。
>
> 原文链接：https://blog.csdn.net/weixin_37938228/article/details/104628154

![image-20210329193214028](https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210329193214028.png)

设置好文献分析参数以后，点击主面板的**GO**，若分析成功，则弹出窗口：

![image-20210329194433644](https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210329194433644.png)

在可视化窗口中可以看到生成的可视化结果：

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210329194524019.png" alt="image-20210329194524019" style="zoom:67%;" />

点击Export：Network Summary Table

![image-20210329194544938](https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210329194544938.png)

导出关键字列表：

<img src="https://raw.githubusercontent.com/PPjmchen/Notes-Imgs/main/image-20210329194941605.png" alt="image-20210329194941605" style="zoom:67%;" />

**更多用法及细节待补充**

****

