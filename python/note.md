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

8. 字典推倒式

```python
d = {key: value for (key, value) in iterable}
```

9. \*args and \*\*kwargs

当你不确定你的函数里将要传递多少参数时你可以用*args。例如, 它可以传递任意数量的参数:

```python
def print_everything(*args):
  for count, thing in enumerate(args):
    print '{0}. {1}'.format(count, thing)

print_everything('apple', 'banana', 'cabbage')
# console output
# 0. apple
# 1. banana
# 2. cabbage
```

\*\*kwargs允许你使用没有事先定义的参数名

```python
def table_things(**kwargs):
  for name, value in kwargs.items():
    print '{0} = {1}'.format(name, value)

table_things(apple = 'fruit', cabbage = 'vegetable')
# console output
# cabbage = vegetable
# apple = fruit
```

也可以混着用。命名参数首先获得参数值然后所有的其他参数都传递给 \*args 和 \*\*kwargs 。命名参数在列表的最前端。例如:

```python
def table_things(titlestring, **kwargs)
```
