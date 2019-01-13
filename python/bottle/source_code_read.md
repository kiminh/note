# bottle 使用及源码阅读笔记

参考:
[Python微型Web框架Bottle源码分析](https://mp.weixin.qq.com/s/mN2S1y_ukvWPbHFfBj9kHg)
[微型 Python Web 框架： Bottle](http://blog.csdn.net/huithe/article/details/8087645)

## bottle 简介

Bottle 是一个非常小巧但高效的微型 Python Web 框架, 它被设计为仅仅只有一个文件的 python 模块, 并且除Python标准库外, 它不依赖于任何第三方模块。 不建议在生产环境中使用, 可以用 flask 替代。

阅读原因:
- [Python微型Web框架Bottle源码分析](https://mp.weixin.qq.com/s/mN2S1y_ukvWPbHFfBj9kHg) 这篇博客引发的兴趣。
- Bottle 从发布至今一直贯彻的微型 Web 框架的理念。
- Bottle 一直坚持单文件发布，也就是只有一个 ```bottle.py``` 文件。
- 除了 Python 标准库之外没有依赖关系。
- 与 Flask、Django 都遵循 PEP-3333 的 WSGI 协议。
- 0.4.10 版本代码量小，加上大量注释也只有不到 1000 行的代码。

代码下载: [bottle](https://github.com/bottlepy/bottle)

安装:
    1. ```sudo easy_install -U bottle```  
    2. ```pip install bottle```  

## 使用

### base use

最简例子:

```python
from bottle import route,run

@route('/hello/:name')
def index(name='World'):
    return '<strong>Hello {0}!'.format(name)

run(host='localhost', port='8080')
```

运行后直接访问 ```http://localhost:8080/hello/<imput>``` 即可查看结果。

- 路由器(Request Routing)  

bottle 应用会有一个路由器, 将 URL 请求地址绑定到毁掉函数上, 每请求一次 URL, 其对应的回调函数就会运行, 回调函数的返回值将被发送到浏览器。可以通过 route() 方法添加不限数目的路由器。

- 动态路由(Dynamic routes)  

bottle 有特有的 URL 语法, 可以很容易的在 URL 中加入通配符, 将一个 route 映射到多个 URL 上。动态路由常常被用来创建一些有规律性的内容页面的地址。

- HTTP 请求方法(Request Methods)  

HTTP 协议为不同的需求定义了许多不同的请求方法, 在 Bottle 中, GET方法将是所有未指明请求访问的路由会默认使用的方法, 这些未指明方法的路由都将只接收 GET 请求, 要处理如 POST,  PUT 或者 DELETE 等等的其它请求, 你必须主动地在 route() 函数中添加 method 关键字, 或者使用下面这些 decorators: @get()@ , post() , put() , delete()。POST 方法在经常被用来处理 HTML 的表单数据。

- 自动回退(Automatic Fallbacks)  

特殊的 HEAD 方法, 经常被用来处理一些仅仅只需要返回请求元信息而不需要返回整个请求结果的事务。

- 静态文件路由(Routing Static Files)

对于静态文件, Bottle 内置的服务器并不会自动的进行处理, 这需要定义一个路由,告诉服务器在哪些文件是需要服务的, 并且在哪里可以找到它们。  

```python
from bottle import static_file
@route('/static/:filename')
def server_static(filename):
    return static_file(filename, root='/path/to/your/static/files')
```

`static_file()` 函数是一个安全且方便的用来返回静态文件请求的函数。

如果我们想要 "/path/to/your/static/files" 目录的子目录下的文件也被处理, 那么我们可以使用一个格式化的通配符。

```python
@route('/static/:path#.+#')
from bottle import static_file
def server_static(path):
    return static_file(path, root='/path/to/your/static/files')
```

## 源码阅读  

在 Bottle 中关于创建一个标准的 WSGI Server 涉及的类或者方法只有 3 个。
