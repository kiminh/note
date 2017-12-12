# Hue

依赖安装:

```
yum install cyrus-sasl-gssapi cyrus-sasl-deve libxml2-devel
libxslt-devel mysql mysql-devel openldap-devel python-devel
python-simplejson sqlite-devel libffi-devel openssl-devel
gmp-devel gcc gcc-c++ mysql*
```

```
yum install -y ant gcc gcc-c++ krb5-devel mysql mysql-devel
openssl-devel cyrus-sasl-gssapi cyrus-sasl sqlite-devel
libacl-devel libtidy libxml2-devel libxslt-devel python-devel
python-simplejson python-setuptools rsync saslwrapper gmp
gmp-devel openldap-devel openldap-devel
```

编译过程出现的依赖缺少:

```
# 安装 openldap-devel
yum install openldap-devel
```

## 编译 hue

```
make install
```

## 集成到 ambari  

使用 github 上的 ambari-hue-service 完成。

```
git clone https://github.com/EsharEditor/ambari-hue-service.git /var/lib/ambari-server/resource/stack/HDP/<version>/services/HUE
```

增加 hue service 的前提: 在 HDP 的 repo 中添加编译好的 hue 。安装地址为: ```/usr/local/hue```。  

重启 ambari-server , 增加 hue 。

可能遇到问题:

```
UnicodeEncodeError: 'ascii' codec can't encode character u'\u201c' in position 3354: ordinal not in range(128)
```

作者给出的解决方案是: 为 ambari-common 中的 sudo , py 增加编码格式:

```python
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
```

实际解决方案: 修改 setup.py , 增加生成 hue 配置文件时, 指定编码格式为 ```utf-8```。

```python
File(format("{hue_conf_dir}/pseudo-distributed.ini"),
  content = InlineTemplate(params.hue_pseudodistributed_content),
  owner = params.hue_user,
  # 添加
  encoding = 'utf-8'
)
```

完成后可以正常集成, 但是启动后的界面为错误信息。

- 错误信息:

```
[12/Jul/2017 21:55:26 -0700] file_reporter ERROR    failed to write metrics to file
Traceback (most recent call last):
  File "/usr/local/hue/desktop/core/src/desktop/lib/metrics/file_reporter.py", line 51, in report_now
    json.dump(self.registry.dump_metrics(), f)
  File "/usr/local/hue/desktop/core/src/desktop/lib/metrics/registry.py", line 107, in dump_metrics
    metrics = self._registry.dump_metrics()
  File "/usr/local/hue/build/env/lib/python2.6/site-packages/pyformance-0.3.2-py2.6.egg/pyformance/registry.py", line 215, in dump_metrics
    metrics[key] = self.get_metrics(key)
  File "/usr/local/hue/build/env/lib/python2.6/site-packages/pyformance-0.3.2-py2.6.egg/pyformance/registry.py", line 199, in get_metrics
    metrics.update(getter(key))
  File "/usr/local/hue/build/env/lib/python2.6/site-packages/pyformance-0.3.2-py2.6.egg/pyformance/registry.py", line 132, in _get_gauge_metrics
    return {"value": gauge.get_value()}
  File "/usr/local/hue/build/env/lib/python2.6/site-packages/pyformance-0.3.2-py2.6.egg/pyformance/meters/gauge.py", line 36, in get_value
    return self.callback()
  File "/usr/local/hue/desktop/core/src/desktop/metrics.py", line 110, in <lambda>
    callback=lambda: User.objects.count(),
  File "/usr/local/hue/build/env/lib/python2.6/site-packages/Django-1.6.10-py2.6.egg/django/db/models/manager.py", line 136, in count
    return self.get_queryset().count()
  File "/usr/local/hue/build/env/lib/python2.6/site-packages/Django-1.6.10-py2.6.egg/django/db/models/query.py", line 294, in count
    return self.query.get_count(using=self.db)
  File "/usr/local/hue/build/env/lib/python2.6/site-packages/Django-1.6.10-py2.6.egg/django/db/models/sql/query.py", line 390, in get_count
    number = obj.get_aggregation(using=using)[None]
  File "/usr/local/hue/build/env/lib/python2.6/site-packages/Django-1.6.10-py2.6.egg/django/db/models/sql/query.py", line 356, in get_aggregation
    result = query.get_compiler(using).execute_sql(SINGLE)
  File "/usr/local/hue/build/env/lib/python2.6/site-packages/Django-1.6.10-py2.6.egg/django/db/models/sql/compiler.py", line 786, in execute_sql
    cursor.execute(sql, params)
  File "/usr/local/hue/build/env/lib/python2.6/site-packages/Django-1.6.10-py2.6.egg/django/db/backends/util.py", line 53, in execute
    return self.cursor.execute(sql, params)
  File "/usr/local/hue/build/env/lib/python2.6/site-packages/Django-1.6.10-py2.6.egg/django/db/utils.py", line 99, in __exit__
    six.reraise(dj_exc_type, dj_exc_value, traceback)
  File "/usr/local/hue/build/env/lib/python2.6/site-packages/Django-1.6.10-py2.6.egg/django/db/backends/util.py", line 53, in execute
    return self.cursor.execute(sql, params)
ProgrammingError: relation "auth_user" does not exist
LINE 1: SELECT COUNT(*) FROM "auth_user"
```

- 解决方法:

参考 ambari-hue-service 的 README.md , 运行集成的 custom command 解决, 即执行 migrate。

## 使用问题  

1. 使用 hive 问题:

```
relation "beeswax_session" does not exist LINE 1: ...application", "beeswax_session"."properties" FROM "beeswax_s... ^

There are currently no rules defined. To get started, right click on any table column in the SQL Assist panel.
```

2. Metastore Manager 问题:

```
relation "beeswax_session" does not exist LINE 1: ...application", "beeswax_session"."properties" FROM "beeswax_s... ^

hive
  |-Databases
  |-Error loading databases.
```

3. HBase

```
Api Error: TSocket read 0 bytes

Api Error: ('Connection aborted.', BadStatusLine('\x00\x00\x00\x8b\x89\x01\x08\xff\xff\xff\xff\x0f\x12\x80\x01\n',))

Api Error: ('Connection aborted.', BadStatusLine('\x00\x00\x00\x8b\x89\x01\x08\xff\xff\xff\xff\x0f\x12\x80\x01\n',))
```

## hue 集成 NameNode HA

参考:

[Hadoop High Availability](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_hadoop-ha/content/ch05s04.html)

在 hue.ini 中, ha 的 NameNode 和 普通状态的 NameNode 需要配置的 webhdfs_url 不同:

```ini
[hadoop]

  [[hdfs_clusters]]

    [[[default]]]

      # Enter the filesystem uri
      fs_defaultfs=hdfs://localhost:8020

      # Use WebHdfs/HttpFs as the communication mechanism.
      # Domain should be the NameNode or HttpFs host.
      webhdfs_url=http://localhost:50070/webhdfs/v1
```

**需要完成的准备工作如下:**

1. 在 hue server 的节点上安装 ```Hadoop HttpFS```

```shell
yum install hadoop-httpfs
```

2. 配置 httpfs-env.sh:

  - 添加 java 环境。

3. 在 ```/etc/hadoop-httpfs/conf/httpfs-site.xml``` 中添加以下配置:

```xml
<property>
 <name>httpfs.proxyuser.hue.hosts</name>
 <value>*</value>
</property>

<property>
 <name>httpfs.proxyuser.hue.groups</name>
 <value>*</value>
</property>
```

4. 启动 HttpFS service

5. 在 core.xml 中添加以下配置:

```xml
<property>
 <name>hadoop.proxyuser.httpfs.groups</name>
 <value>*</value>
</property>

<property>
 <name>hadoop.proxyuser.httpfs.hosts</name>
 <value>*</value>
</property>
```

配置 hue.ini :  

```ini
[hadoop]

  [[hdfs_clusters]]

    [[[default]]]

      # Enter the filesystem uri
      # 使用 NameNode service ID
      fs_defaultfs=hdfs://mycluster

      # Use WebHdfs/HttpFs as the communication mechanism.
      # Domain should be the NameNode or HttpFs host.
      webhdfs_url=http://localhost:14000/webhdfs/v1
```

重启 hue。

## 守护进程启动 hue

```
{hue_path}/build/env/bin/supervisor -d
```

## hue 管理员用户丢失

问题描述:

通过 ambari 安装 hue 后, 管理员用户无法登录。  
问题排查: hue 的数据库中无管理员用户。

解决方法:

进入hue shell 进行添加用户。

```bash
$HUE_DIR/build/env/bin/hue shell
```

Then set these properties to true:

```python
# hue shell
from django.contrib.auth.models import User
user = User.objects.create(username='admin')
user.set_password('admin')
user.is_superuser = True
user.save
```

```python
# 忘记密码时修改密码
from django.contrib.auth.models import User
user = User.objects.get(username='example')
user.set_password('some password')
user.save()
```

## troubleshooting

### hive 无法获取 database, table 信息, 无法执行sql

环境描述: 管理工具使用 ambari, Hadoop 使用 HDP2.5 发行版, hue版本为 3.11.0。

问题描述: 通过合法的用户登陆 hue, 进入 hive 的界面, 无法加载 database 信息和 table 信息。

定位问题思路:

  1. 怀疑是 hive 连接问题, 通过 hive 提供的两种方式 `hive` 命令和 `beeline` 能够正常连接到 hive 执行 sql, 排除 hive 引起问题;
  2. 查看日志, 无异常日志，未定位到问题原因;
  3. Google 查找类似现象, 找到和 hue 的用户缓存相关, 新建用户后能够使用。 经尝试, 该方式能够暂时解决问题。

相似现象:
https://community.hortonworks.com/questions/48834/hivebeewax-on-hue-time-out-error-while-i-run-simpl.html
https://stackoverflow.com/questions/16247073/how-to-refresh-clear-the-distributedcache-when-using-hue-beeswax-to-run-hive-q

配置 check:
