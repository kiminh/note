# hue 无法从自带的 sqlite 中将数据同步到配置的 MySQL 数据库中

- 背景介绍

    - 通过 Ambari 安装, 配置 Hue(Hue 的数据库配置为 MySQL )。 启动后需要执行自定义 command ```metastoresync``` 来将 Hue 自带的 sqlite 中的数据同步至 MySQL 中。执行自定义 command ```usersync``` 将系统用户同步到 MySQL 中。
    自定义命令介绍:
    1. metastoresync: ```metastoresync``` 命令首先判断使用的数据库类型, 如果不是 Sqlite, 将数据同步至相应的数据库中; 如果是 Sqlite, 不执行同步。同步的过程即执行 ```<hue_install_path>/build/env/bin/hue syncdb --noinput``` 和 ```<hue_install_path>/build/env/bin/hue migrate```。这两个命令的作用是将 Hue 的数据同步到配置好的外部数据库中。
    2. usersync: ```usersync``` 命令的作用是将已有用户同步到 hue 的数据库中。实际执行的指令为: ```<hue_install_path>/build/env/bin/hue useradmin_sync_with_unix``` 或 ```<hue_install_path>/build/env/bin/hue sync_ldap_users_and_groups```  

- 问题描述  

    - 安装好 Hue 后, 没有启用 Hue 的所有 Module, 选择需要启用的 Module, 执行自定义 command ```metastoresync``` 来同步数据库, 执行自定义 command ```usersync``` 同步系统用户, 完成后, 打开界面观察, 能够查看到 Hue 正常的 Hue 的界面。添加新的 Module, 重新执行自定义 command ```metastoresync```, 无法将新的数据同步到 MySQL, 导致相应功能无法使用。

已提交 issue 到 GitHub, 目前的做法是在第一次同步数据到 MySQL 中前, 先将所有用到的 Module 打开, 然后再同步数据。

- hue migrate

hue migrate 实际执行的操作为 django 的 ```python manage.py migrate```

PS: hue <command> 实际执行的操作为 ```python manage.py <command>```\

- hue migrate 解释:

<install_path>/build/env/bin/hue 即 Django 框架中的 manage.py。
```
hue syncdb -> python manage.py syncdb
hue migrate -> python manage.py migrate
```

- Django 中使用 South

[django-south 使用](http://www.weiguda.com/blog/2/)

  - South 介绍:  
  django-south 是为 Django 应用程序提供一致、易于使用和数据库不可知的迁移的工具。在 django-admin 命令中有 syncdb 指令, 其目的是根据 model.py 创建相应的数据库表. 但我们在开发的过程中, 经常会需要更改 model, 删除或者增加Field, 这时, syncsb 命令就不那么好用了, 因为 syncsb 无法自动更改数据库表结构. 因此, 我们时常需要手动删除数据库表, 再运行syncdb。

  - 使用:  
  在 django 的 setting.py 的 INSTALLED_APPS 中添加 'south', 并运行syncdb, 创建south所需要的数据表。执行指令导入数据库: ```python manage.py syncdb```
