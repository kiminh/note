# git

git是一个版本控制系统。

特点：
TODO

## 使用

### 安装

- linux

首先输入git命令查看有没有安装git

```
# 未安装
-bash: git: command not found

# 已安装
usage: git ...
```

CentOS下使用yum命令安装git，其他Linux系统可以使用相应的命令安装git。

```
yum install -y git
```

- Mac OS

等买了Mac以后再更新 :(

- Windows

推荐 git for windows。

下载地址： [git for windows](https://git-for-windows.github.io/)

### 基本指令

1. git config --global

这台机器的所有git仓库都会使用这个参数。
通过```git config --global```配置远程仓库、用户等信息。

2. 创建版本库

进入一个目录执行```git init```，通过git init将该目录改变成git可以管理的仓库。  
执行后生成一个```.git```目录，该目录用来跟踪管理版本库。该目录为隐藏目录，通过```ll -a```可以查看到。

3. git add

在仓库中添加一个文件使用```git add```命令。将仓库目录中的文件添加到git。

```
touch README.md
git add README.md
```

4. git commint

通过```git comnmint```提交，-m后面输入的是本次提交的说明。

```
git commint -m "add the file README.md"
```

5. git remote add oragin <远程仓库地址\>

通过```git remote add oragin <远程仓库地址\>```将本地分支与远程分支连接。

```
git remote add origin https://github.com/user_name/project_name.git
```

6. git push

通过```git push -u <local_branch\> <branch_url\>```将本地的commit提交到远程仓库。


```
git push -u <local_branch\> https://github.com/user_name/project_name.git
```

7 git clone

从远程仓库clone一个版本库。该命令会在本地主机生成一个目录，与远程主机的版本库同名。如果要指定不同的目录名，可以将目录名作为git clone命令的第二个参数。

```
git clone https://github.com/user_name/project_name.git
git clone https://github.com/user_name/project_name.git new_deritory
```

### 分支操作

TODO

1. 创建本地分支关联远程分支及分支切换:

```bash
# 创建本地分支, 关联远程分支
git branch add origin <git_url>
# 切换分支
git checkout <branch>
# 查看所有分支
git branch -a
# 删除本地分支
git branch -d <branch>
```




## notice

commit 时避免将 project 配置文件 commit 。如 ```.idea``` ， ```.metadata``` 等。
