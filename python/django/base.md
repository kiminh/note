# django note

## 规则

一个URL模式就是一个正则表达式

- 编码次序
    1. URLpattern;
    2. views

## start 生成文件及说明

生成文件

```
mysite
  __init__.py
  manage.py
  setting.py
  urls.py
```

- manage.py
    命令行工具, 允许以多种方式与 Django project 进行交互。 ```python manage.py help``` 查看帮助。

- setting.py
    该 Django project 的设置或配置。

- urls.py
    Django 项目的 URL 设置, 可视为 django 网站目录。

启动服务器: 

```bash
python manage.py runserver
```

该指令将在 8000 端口启动一个本地服务器, 不要在正式的部署环境中使用这个服务器, 在同一时间, 该服务器只能可靠的处理一次单个的请求。

## 执行流程

- setting.py

所有的 Django project 开始于 setting.py , 脚本在 manage.py 同一个目录下查找名为 setting.py 的文件。这个文件包含所有关于这个 Django 项目的配置信息, 均大写: ```INSTALLED_APPS```, ```MIDDLEWARE```. ```ROOT_URLCONF```, ```TEMPLATE_DIRS``` ,```WSGI_APPLICATION``` , ```DATABASE_NAME```, ```AUTH_PASSWORD_VALIDATORS```, ```LANGUAGE_CODE```, ```TIME_ZONE```, ```USE_I18N```, ```USE_L10N```, ```USE_TZ```, ```STATIC_URL``` 。最重要的是 ```ROOT_URLCONF```， 它将作为 ```URLconf``` 告诉 Django 在这个站点哪些 Python 模块将被用到。

Django 时区

Django 有时区意识, 默认时区为 America/Chicago, 使用其他时区需要在 setting.py 中修改。请参见它里面的注释, 以获得最新世界时区列表。

```python
ROOT_URLCONF = 'mysite.urls'
```

对应的文件是 ```<project_dir>/urls.py```

一个视图功能必须返回一个 HttpResponse， 完成后, Django 将完成剩余的转换 Python 对象到一个合适的带有 HTTP 请求头和 body 的 Web Respond。

## Django 的错误界面

访问一个包含错误 Python 错误的界面, 将在界面上看到包含大量信息的错页。

1. 在页面顶部, 可以获取关键异常信息, 包括:
    - 异常数据类型
    - 异常参数
    - 哪个文件引发了异常
    - 出错行号
2. 关键信息的下方, 显示了对该异常的完整追踪信息。有助于在 Python 命令行解释器中获得追溯信息。 对栈中的每一帧, Django 都显示了其文件名, 函数(方法)名称, 行号, 该行源代码。
3. 点击改行可以看到出错的前后几行, 获取上下文。
4. 栈中的每一帧的 ```Local vars``` 可以看到一个所有局部变量的列表, 以及在出错那一帧时的值(方便调试)。
5. 注意 ```Traceback``` 下面的 ```Switch to copy-and-paste view``` 文字, 这些文字链接了另一个追溯视图,  方便复制错误信息。
6. "Share this traceback on a public Web site" 按钮传回追溯信息到 [Paste a new item](http://www.dpaste.com), 可以得到一个单独的 URL 并与其他人分享追溯信息。
7. ```Request information``` 部分包含了有关生产错误的 Web 请求的大量信息: ```GET``` 和 ```POST```, ```cookie``` 值, 元数据(如 CGI 请求头)。
8. ```Request``` 信息下面, ```settings``` 列出了 Django 使用的具体配置信息。

- 使用技巧

通过临时插入 ```assert False``` 触发一个调试页, 通过调试页查看局部变量和程序语句。

- 禁用错误页

错误页信息包含 project 的 Python 代码的内部结构, 以及 Django 的配置, 因此不应将该页在 Internet 上公开。因此 Django 的出错信息只在 debug 模式才会出现, 在实际应用中应当禁用 debug 模式

```python
# settings.py
# 设置是否开启 Debug 模式
DEBUG = True
# 设置能够访问的 hosts 
ALLOWED_HOSTS = []
```

最精简的达到两个配置环境设定的方案是使用一个配置文件, 在此配置文件中根据不同的环境进行设置。 一个达到这个目的的方法是检查当前的主机名。

```python
# settings.py

import socket

if socket.gethostname() == 'my-laptop':
    DEBUG = TEMPLATE_DEBUG = True
else:
    DEBUG = TEMPLATE_DEBUG = False
```

### 设置错误警告

当使用 Django 制作的网站运行中出现了异常, 你会希望去了解以便于修正它。 默认情况下, Django在代码引发未处理的异常时, 将会发送一封 Email 至开发者团队。但你需要去做两件事来设置这种行为。改变你的 ADMINS 设置用来引入你的 E-mail 地址，以及那些任何需要被注意的联系人的 E-mail 地址。 这个设置采用了类似于(姓名, Email)元组。

```python
ADMINS = (
    ('John Lennon', 'jlennon@example.com'),
    ('Paul McCartney', 'pmacca@example.com'),
)
```

## 数据库配置

数据库配置也是在 Django 的配置文件 ```setting.py``` 中。 内容为: 

```python
DATABASES = {
    'default': {
        'ENGINE': '',
        'NAME': '',
        'USER': '',
        'PASSWORD': '',
        'HOST': '',
        'PORT': '',
    }
}
```

配置纲要: 

1. DATABASE_ENGINE 配置数据库类型, 内容如下: 

配置 | 数据库 | 所需要的适配器
---|---|---
```postgresql``` | PostgreSQL | psycopg 1.x
```postgresql_pycopg2``` | PostgreSQL | psycopg 2.x
mysql | MySQL | MySQLdb
sqlite3 | SQLite | 如果使用 Python 2.5+ 则不需要适配器。 否则就使用 pysqlite
oracle | Oracle | cx_Oracle

配置示例: 
```python
DATABASE_ENGINE = 'postgresql_psycopg2'
DATABASE_NAME = 'mydb'
# 如果使用 SQLite, 需要配置数据库文件的完整路径
DATABASE_NAME = '/home/django/mydata.db'
# 此处的 MySQL 是一个特例。 如果使用的是 MySQL 且该项设置值由斜杠（ '/' ）开头，MySQL 将通过 Unix socket 来连接指定的套接字
DATABASE_HOST = '/var/run/mysql'
```

**<center>数据库配置错误信息</center>**

错误信息 | 解决方法
---|---
```You haven’t set the DATABASE_ENGINE setting yet.``` | 不要以空字符串配置`` DATABASE_ENGINE`` 的值。
```Environment variable DJANGO_SETTINGS_MODULE is undefined.``` | 使用```python manager.py shell``` 命令启动交互解释器, 不要以 ```python``` 命令直接启动交互解释器。
```Error loading _____ module: No module named _____.``` | 未安装合适的数据库适配器。
```_____ isn’t an available database backend.``` | 把 ```DATABASE_ENGINE``` 配置成前面提到的合法的数据库引擎。
```database _____ does not exist``` | 设置 ```DATABASE_NAME``` 指向存在的数据库, 或者先在数据库客户端中执行合适的 ```CREATE DATABASE``` 语句创建数据库。
```role _____ does not exist``` | 设置 ```DATABASE_USER``` 指向存在的用户, 或者先在数据库客户端中执创建用户。
```could not connect to server``` | 查看 ```DATABASE_HOST和DATABASE_PORT``` 是否已正确配置, 并确认数据库服务器是否已正常运行。

在 setting.py 中配置 models :

```python
INSTALLED_APPS ={
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'mysite.<app>',
}
```

在 models.py 中描述实体类, 通过以下方式检测和生成数据库表。

```bash
# 验证有效性
python manage.py validate
# 生成 CREATE TABLE 语句
python manage.py sqlall <>
```

2. 



