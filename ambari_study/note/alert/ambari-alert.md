# ambari-alert study

简介： Ambari 为了帮助用户鉴别以及定位集群的问题，实现了告警（Alert）机制。每个服务都能够通过提供一个 alerts.json 文件来定义 Ambari 应该跟踪哪些警报。

- [ambari服务配置及Alert详解(from ibm docs)](https://www.ibm.com/developerworks/cn/opensource/os-cn-bigdata-ambari3)
- [Monitoring and Alerts](https://docs.hortonworks.com/HDPDocuments/Ambari-2.4.1.0/bk_ambari-user-guide/content/ch_monitoring_and_alerts.html)
- [Administering Ambari and components(from ibm)](https://www.ibm.com/support/knowledgecenter/en/SSPT3X_4.2.5/com.ibm.swg.im.infosphere.biginsights.admin.doc/doc/admin_icnav_admin.html)

## alert 工作原理



## 查看alerts

Ambari 的 UI 提供 Alert 的查看，也可以通过 REST API 查看 Ambari 的 alert。

REST API：

```shell
# 此处 ambari-server 的 hostname 为 adm-test
# http://adm-test:8080/api/v1/clusters/test/alert_definitions

# 可以通过以下指令查看 alert
curl -u admin:admin -H "X-Requested-By: ambari" -X GET http://adm-test:8080/api/v1/clusters/test/alert_definitions

# 获取编号为 x 的 alert
curl -u admin:admin -H "X-Requested-By: ambari" -X GET http://adm-test:8080/api/v1/clusters/test/alert_definitions/x

# 获取某个 service 的 alerts
curl -u admin:admin -H "X-Requested-By: ambari" -X GET http://adm-test:8080/api/v1/clusters/test/alert_definitions?AlertDefinition/service_name=<service_name\>
```

## 管理 alerts

TODO

## alert 详细介绍

- alert 的属性  

alert 拥有已下属性：

1. id
2. name
3. label
4. cluster_name
5. service_name
6. component_name
7. source: 用来配置 alert 类型、阈值以及提示信息。
8. scope:
9. interval: 告警的检测时间间隔

- source 配置

- 告警类别  

告警类别配置在 source 中。

类型 | 用途 | 告警级别 | 阈值是否可配置 | 单位  
---|---|---|---|---  
PORT | 用来监测某台机器的某个端口是否可用 | OK, WARN, CRITICAL | 是 | 秒
METRIC | 用来监测 Metric 相关的p配置属性 | OK, WARN, CRITICAL | 是 | 变量
AGGREGATE | 聚合定义用于组合来自不同节点的另一个警报定义的结果。 | OK, WARN, CRITICAL | 是 | 百分比
WEB | 用于监测一个 WEB UI(URL) 是否可用 | OK, WARN, CRITICAL | 否 | 无
SCRIPT | Alert 监测逻辑由一个自定义的 python 脚本执行 | OK, CRITICAL | 否 | 无

### PORT

端口定义用于检查到远程端点的TCP连接。

需要在 source 中指定：

```json
"source": {
    "default_port": 2181,
    "reporting": {
        "ok": {
            "text": "TCP OK - {0:.3}s response on port {1}"
        },
        "warning": {
            "text": "TCP OK - {0:.3}s response on port {1}",
            "value": 1.5
        },
        "critical": {
            "text": "Connection failed: {0} to {1}:{2}",
            "value": 5
        }
    },
    "type": "PORT",
    "uri": "{{core-site/ha.zookeeper.quorum}}"
}
```

1. uri
2. default_port:一个URI，主机或整数，表示如果不能实现其他指定的属性，则使用的后备值
3. reporting

### METRIC

METRICS source 字段用于定义可以查询的 JMX 端点。

TODO

source 配置

```json
"source": {
    "jmx": {
        "property_list": [
            "java.lang:type=OperatingSystem/SystemCpuLoad",
            "java.lang:type=OperatingSystem/AvailableProcessors"
        ],
        "value": "{0} * 100"
    },
    "reporting": {
        "ok": {
            "text": "{1} CPU, load {0:.1%}"
        },
        "warning": {
            "text": "{1} CPU, load {0:.1%}",
            "value": 200
        },
        "critical": {
            "text": "{1} CPU, load {0:.1%}",
            "value": 250
        },
        "units": "%"
    },
    "type": "METRIC",
    "uri": {
        "http": "{{hdfs-site/dfs.namenode.http-address}}",
        "https": "{{hdfs-site/dfs.namenode.https-address}}",
        "https_property": "{{hdfs-site/dfs.http.policy}}",
        "https_property_value": "HTTPS_ONLY",
        "default_port": 0,
        "high_availability": {
            "nameservice": "{{hdfs-site/dfs.nameservices}}",
            "alias_key": "{{hdfs-site/dfs.ha.namenodes.{{ha-nameservice}}}}",
            "http_pattern": "{{hdfs-site/dfs.namenode.http-address.{{ha-nameservice}}.{{alias}}}}",
            "https_pattern": "{{hdfs-site/dfs.namenode.https-address.{{ha-nameservice}}.{{alias}}}}"
        }
    }
}
```

1. jmx:
2. reporting
3. uri

### addition uri

1. uri - 包含 URI 的定义类型可以取决于任何数量的有效子属性。在某些情况下，URI可能非常简单，只包含一个端口。在其他情况下，URI可能更为复杂，包括明文，SSL 和受 Kerberos 保护的安全端点的属性。

- 1. http
- 2. https
- 3. https_property
- 4. http_property_value
- 5. kerberos_keytab
- 6. kerberos_principal
- 7. default_port
- 8. high_availability

2. reporting - 定义确定警报状态时使用的阈值和文本的结构。 ok总是一个必需元素，但只需要一个警告或关键元素。 某些警报只能有两种状态（例如 OK 和 CRITICAL），并且将绕过警告元素的需要。

3. default_port - 表示如果不能实现其他指定属性的后备值的URI，主机或整数。


### AGGREGATE

聚合定义用于组合来自不同节点的另一个警报定义的结果。

source配置：

```json
"source": {
    "type": "PERCENT",
    "alert_name": "datanode_process",
    "reporting": {
        "ok": {
            "text": "affected: [{1}], total: [{0}]"
        },
        "warning": {
            "text": "affected: [{1}], total: [{0}]",
            "value": 10
        },
        "critical": {
            "text": "affected: [{1}], total: [{0}]",
            "value": 30
        }
    },
    "units": "%"
}
```

### WEB

Web 定义在功能上类似于端口定义。但是，它们不仅检查 TCP 连接，而且还会验证是否返回了适当的HTTP响应代码。

source 配置：

```json
"source": {
    "reporting": {
        "ok": {
            "text": "HTTP {0} response in {2:.3f} seconds"
        },
        "warning": {
            "text": "HTTP {0} response in {2:.3f} seconds"
        },
        "critical": {
            "text": "Connection failed to {1}: {3}"
        }
    }
},
"type": "WEB",
"uri": {
    "http": "{{hdfs-site/dfs.namenode.http-address}}",
    "https": "{{hdfs-site/dfs.namenode.https-address}}",
    "https_property": "{{hdfs-site/dfs.http.policy}}",
    "https_property_value": "HTTPS_ONLY",
    "kerberos_keytab": "{{hdfs-site/dfs.web.authentication.kerberos.keytab}}",
    "kerberos_principal": "{{hdfs-site/dfs.web.authentication.kerberos.principal}}",
    "default_port": 0,
    "high_availability": {
        "nameservice": "{{hdfs-site/dfs.nameservices}}",
        "alias_key": "{{hdfs-site/dfs.ha.namenodes.{{ha-nameservice}}}}",
        "http_pattern": "{{hdfs-site/dfs.namenode.http-address.{{ha-nameservice}}.{{alias}}}}",
        "https_pattern": "{{hdfs-site/dfs.namenode.https-address.{{ha-nameservice}}.{{alias}}}}"
    }
}
```

### SCRIPT

SCRIPT 类型需要在 source 中指定：

```json
"source": {
    "path": "HDFS/2.1.0.2.0/package/alerts/alert_ha_namenode_health.py",
    "type": "SCRIPT"
}
```

SCRIPT 例子：

```python
#!/usr/bin/env python
# coding=utf-8

import ambari_simplejson as json
import socket
import os
from resource_management import *

# 告警指标正常
RESULT_CODE_OK = 'OK'
# 告警指标异常
RESULT_CODE_CRITICAL = 'CRITICAL'
# 告警未知
RESULT_CODE_UNKNOWN = 'UNKNOWN'

# 定义 tokens ，可以是一个 configuration

def get_tokens():
    return()


def execute():
    '''
    please write what you want the alert do.
    '''

    # 判断这个 alert 需要的前置条件，如果不成立，则返回 ‘UNKNOW’
    if configurations = None:
        return(RESULT_CODE_UNKNOWN, configurations is none)

    if not alert:
        return(RESULT_CODE_OK, informations)
    else:
        return(RESULT_CODE_CRITICAL, informations)
```

## alert工作原理

TODO

## 增加一个alert

可以通过两种方法添加alert：

1. 通过 curl 添加 alerts  
2. 通过配置 alerts.json 添加 alerts（推荐使用）




## **注意**

1. 通过配置alerts.json 添加 alerts 时，不需要重启 ambari-server ，只需要将该 service 删除后重新安装。当 Service 安装完成之后，再修改 alert.json 是没有用的。Ambari 已经将 Service 相关的告警存入到数据库中了。这时候如果更新 alert.json，Ambari 便不会再读取里面的配置，即使重启也不行。
2. 手动添加好 alerts.json 后但是还未重新安装该 service ，重启 ambari-server 会失败，详情查看日志。

3. 添加邮件提醒
  > 修改配置文件 `/etc/ambari-server/conf/ambari.properties`, 添加一行 `alerts.template.file=/path/to/alert-templates.xml`
  > 在 ambari 界面的 alert 中配置邮箱提醒。
  > 注意, 需要 smtp 及 客户端授权码。
