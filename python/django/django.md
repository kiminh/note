# django


## usage  

- quick start project 

```
django-admin startproject mysite
```

- run server

在 project 根目录下执行: 

```bash
# python manage.py runserver <port/host:server>
# just localhost can visit, default port is 8000
python manage.py runserver
# just localhost can visit, assign port
python manage.py runserver <port>
# all host can visit, assign port
python manage.py runserver <0:port>

# Run python manage.py makemigrations to create migrations for those changes
python manage.py makemigrations
# 执行 python manage.py migrate 来讲数据库完成对数据库修改
python manage.py migrate

# 进入 project 命令行模式
python manage.py shell
```


- create a new app

```bash
python manage.py startapp polls
```

1. views.py

主要用来返回一个 HTTP Response

2. urls

需要在 ```/<project>/urls.py``` 中添加 ```<new_app>``` 的路由, 同时在 ```<new_app>/urls.py``` 中添加相应的路由。

- 数据库相关

```bash
# 生成 sqlite 数据库和数据库表
python manage.py migrate
```

