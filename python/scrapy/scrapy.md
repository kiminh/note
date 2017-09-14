# scrapy 笔记

## info

1. 安装

```
pip install scrapy
```

2. 创建 scrapy project

```shell
scrapy startproject tutorial # tutorial 为 project 名称
```

项目结构:

```
tutorial
│──scrapy.cfg                                 
└──tutorial                                    
    ├──download_dependencies.py               
    ├──items.py                               
    ├──middlewares.py                         
    ├──pipelines.py                           
    ├──settings.py                            
    ├──__init__.py                            
    └──spiders                                 
       ├──quotes_spider.py                    
       └──__init__.py                         
```

文件作用:  

3. 启动 scrapy project 

```shell
scrapy crawl tutorial #tutorial 为 project 名称
```

4. scrapy shell 模式

```
python shell "<web_site>"
```

