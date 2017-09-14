# tiny note

1. user/group 相关

user 相关可以使用 pwd 模块, group 相关可以使用 grp 模块

```python
# pwd 模块
import pwd
# 返回 uid 的用户信息
pwd.getpwuid(uid)
# 返回 name 用户的信息
pwd.getpwnam(name)

# 返回所有用户信息
pwd.getpwall():
```

```python
# grp 模块
# 返回对应gid的组信息
grp.getgrgid(gid)
# 返回对应group name的组信息
grp.getgrname(name)
# 返回所有组信息
grp.getgrall()
```

2. virtualenv

3. pexpect  

一个自动控制的 Python 模块,可以用来ssh、ftp、passwd、telnet 等命令行进行自动交互  

4. APScheduler  

python 定时任务框架

5. Windows 下执行 scrapy 需要 win32api, 通过下载 pypiwin32 模块解决依赖问题。
```
pip install pypiwin32
```

6. 用 ```requirements.txt``` 安装类库

```shell
pip install -r requirements.txt
```

    request 文件格式: 

```
requests==1.2.0
Flask==0.10.1
numpy
```

7. Sphinx

制作简介美观的文档, usage: django 文档使用该模块。

```
pip install Sphinx
```
