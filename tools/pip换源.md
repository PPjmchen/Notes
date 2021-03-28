# PIP源

# 一、换源

## 比较常用的国内镜像包括：

（1）阿里云 http://mirrors.aliyun.com/pypi/simple/
（2）豆瓣http://pypi.douban.com/simple/
（3）清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/
（4）中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/
（5）华中科技大学http://pypi.hustunique.com/

## 设置方法：（以清华镜像为例，其它镜像同理）

### （1）临时使用：

可以在使用pip的时候，加上参数-i和镜像地址(如https://pypi.tuna.tsinghua.edu.cn/simple)

例如：

```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pandas
```

这样就会从清华镜像安装pandas库。

### （2）永久修改 一劳永逸：

（a）Linux下，修改 ~/.pip/pip.conf (没有就创建一个文件夹及文件。文件夹要加“.”，表示是隐藏文件夹)
内容如下：

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host = https://pypi.tuna.tsinghua.edu.cn
```

(b) windows下，直接在user目录中创建一个pip目录，如：C:\Users\xx\pip，然后新建文件pip.ini，即 %HOMEPATH%\pip\pip.ini，在pip.ini文件中输入以下内容（以豆瓣镜像为例）：

```
[global]
index-url = http://pypi.douban.com/simple
[install]
trusted-host = pypi.douban.com
```

# 二、Memory Error

install前面加--no-cache-dir

