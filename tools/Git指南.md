## 1. 基本用法

在本地仓库文件夹下，先配置用户名及账号

```
git config --global user.name jamin
git config --global user.email ppjmchen@gmail.com
```

查看该仓库用户名及账号

```
git config user.name
git config user.email
```

对管理库进行git初始化

```
git init
```

查看当前管理库中的文件

```
git status
```

将文件添加到管理库中

```
git add <filename>
```

提交更改

```
git commit -m 'create file'
```

合并add和commit

```
git commit -am 'create file'
```

如果管理库中的文件发生了更改，则从Unmodified状态变为Modified状态，此时可以用`git diff`来查看更改内容（更改是与上一次commit的内容进行比较）

```
git diff
```

同时如果需要再次提交文件，则需要将Modified文件重新add，并commit

**注意：如果文件刚刚发生更改，则处于Modified状态，可以用`git diff`查看变化。如果文件已经被add到staged状态，则应该使用`git diff --cached`查看变化**

## 2. 回到过去

### 对整个管理库进行版本穿梭

如果想在上一次的commit中添加新的修改而不做新的commit（修改已经commit的版本）

```
git commit --amend --no-edit
```

如果文件发生了修改后已经add到了staged状态，想要回到Modified状态：

```
# 假设1.py发生了修改
git add 1.py
# 将1.py退回到Staged状态
git reset 1.py
```

如果文件发生了修改后已经add到了staged状态，想要回到commit的状态

```
# 回到commit的状态
git reset --hard HEAD
# 回到前一次commit的状态
git reset --hard HEAD^
# 回到前两次commit的状态
git reset --hard HEAD^^
git reset --hard HEAD~2
# 回到指定ID的commit 
git reset --hard e8f9eac
```

查看所有HEAD的变化过程

```
git reflog
```

### 对单个文件进行版本穿梭

指定单个文件回到指定ID的版本

```
git checkout e8f9eac -- 1.py
```

## 3. 分支

版本库中默认创建的分支为`master`，即稳定版本。同时，也可以从已有的版本中创建新的分支。

分支创建：

```
# 创建一个名为dev的分支
git branch dev
git checkout -b dev
# 切换到dev分支
git checkout dev
```

删除分支：

```
git branch -d dev
```

将分支合并到master

```
# 合并时保留merge信息
git merge --no-ff -m "keep merge info" dev
# 合并后不保留merge信息
git merge dev
```

如果分支修改后，master分支也发生了修改，在merge时就会发生冲突。

此时需要在冲突文件中手动修改冲突，然后在master分支中重新commit。



未完成的代码可以完整暂存到一个独立空间

```
git stash
```

需要时将独立空间的代码再读出



## 上传文件至github

### 配置github账号

```
git config --global user.email "542760094@qq.com"
```

### 关联github仓库

```
git remote add origin https://github.com/PPjmchen/torch_learning.git
```

### 查看修改

```powershell
git status
```

### 将修改的信息存入索引库

```
# 添加全部被修改信息
git add .
# 添加指定文件
git add <new_file>
```

### 提交修改至本地仓库

```
git commit -m 'update_massage'
```

### 上传至github

```
git push origin master
```

