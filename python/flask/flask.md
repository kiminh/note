# flask

- 介绍

[Welcome to Flask](http://flask.pocoo.org/docs/0.12/)

## use

- install  

```
pip install flask
```

- usage  

- 项目结构  

```
project_directory
  ├──<code_and_configurations>
  ├──static  
  |  └──<static_file>
  ├──templates
     └──hello.html
```

- start project

```shell
export FLASK_APP=<main_py_file>
flask run # Running on http://127.0.0.1:5000/
flask run --host=0.0.0.0 # Running on http://0.0.0.0:5000/
```

```shell
# Debug mode
export FLASK_DEBUG=1
flask run
```

- methods
    - GET: 浏览器从服务器中获取数据(大多数的形式, just get the information)  
    - HEAD: 浏览器从服务器获取 head 中的数据
    - POST: 浏览器向服务器发送数据(并且服务器必须确保数据被存储并且只存储一次)
    - PUT: 和 POST 类似, 相比 POST 的优点是: 考虑在传输期间连接丢失：在这种情况下，浏览器和服务器之间的系统可能会安全地接收请求，而不会破坏任何事情。 使用POST是不可能的，因为它只能被触发一次。
    - DELETE: 删除服务器上的信息
    - OPTIONS: 提供一些选项

- template
