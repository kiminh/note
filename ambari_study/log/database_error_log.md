# 数据库相关错误

## 服务相关

1. 场景描述

  集群中有自定义服务, 在执行自定义服务的删除时, 配置相关内容未被删除, 无法添加新服务。

2. 报错信息

```log
ClusterImpl:2398 - Config inconsistency exists: unknown configType=xxx
```

3. 解决思路

  - 源码中报错方法:
```Java
public Config getDesiredConfigByType(String configType) {
  loadConfigurations();
  clusterGlobalLock.readLock().lock();
  try {
    for (ClusterConfigMappingEntity e : clusterDAO.getClusterConfigMappingEntitiesByCluster(getClusterId())) {
      if (e.isSelected() > 0 && e.getType().equals(configType)) {
        return getConfig(e.getType(), e.getTag());
      }
    }

    return null;
  } finally {
    clusterGlobalLock.readLock().unlock();
  }
}

```

  - 相关表: `clusterconfigmapping`, `confgroupclusterconfigmapping`, `clusterconfig`, `hostcomponentdesiredstate`, `hostcomponentstate`, `servicecomponentdesiredstate`, `servicedesiredstate`, `clusterdesiredstate`, `clusterconfigmapping`, `serviceconfighosts`, `serviceconfig`

  - 查询脚本

```sql
SELECT FROM ambari.clusterconfigmapping WHERE type_name LIKE '%error_service%';
DELETE FROM ambari.confgroupclusterconfigmapping WHERE config_type LIKE '%error_service%';
DELETE FROM ambari.clusterconfig WHERE type_name LIKE '%error_service%';


-- 控制台中无效(或安装失败的或状态异常的 service), 通过清除数据库记录彻底清除 service
SELECT FROM ambari.hostcomponentdesiredstate WHERE service_name LIKE '%error_service';
SELECT FROM ambari.hostcomponentstate WHERE component_name LIKE '%error_service';
SELECT FROM ambari.servicecomponentdesiredstate WHERE component_name LIKE '%error_service%';
SELECT FROM ambari.servicedesiredstate WHERE service_name LIKE '%error_service';
SELECT FROM ambari.clusterservice WHERE service_name LIKE '%error_service%';

SELECT * FROM ambari.clusterconfig WHERE service_name LIKE '%error_service';
SELECT FROM ambari.serviceconfigmapping WHERE service_config_id IN (
  SELECT ambari.service_config_id FROM serviceconfig WHERE service_name LIKE '%error_service'
);
SELECT FROM ambari.serviceconfighosts WHERE service_config_id IN (
  SELECT ambari.service_config_id FROM serviceconfig WHERE service_name LIKE '%error_service%'
);

SELECT FROM ambari.clusterconfig WHERE type_config LIKE '%error_service%';
```

  - 删除脚本

```sql
-- 通过 ambari 界面删除 service 后, 如果新添加其他版本的该 service, 可能导致错误:
-- ERROR [ambari-heartbeat-monitor] HostImpl:1374 - Config inconsistency exists: unknown configType=<error_config_set>
DELETE FROM ambari.clusterconfigmapping WHERE type_name LIKE '%error_service%';
DELETE FROM ambari.confgroupclusterconfigmapping WHERE config_type LIKE '%error_service%';
DELETE FROM ambari.clusterconfig WHERE type_name LIKE '%error_service%';


-- 控制台中无效(或安装失败的或状态异常的 service), 通过清除数据库记录彻底清除 service
DELETE FROM ambari.hostcomponentdesiredstate WHERE service_name LIKE '%error_service';
DELETE FROM ambari.hostcomponentstate WHERE component_name LIKE '%error_service';
DELETE FROM ambari.servicecomponentdesiredstate WHERE component_name LIKE '%error_service%';
DELETE FROM ambari.servicedesiredstate WHERE service_name LIKE '%error_service';
DELETE FROM ambari.clusterservice WHERE service_name LIKE '%error_service%';

SELECT * FROM ambari.clusterconfig WHERE service_name LIKE '%error_service';
DELETE FROM ambari.serviceconfigmapping WHERE service_config_id IN (
  SELECT ambari.service_config_id FROM serviceconfig WHERE service_name LIKE '%error_service'
);
DELETE FROM ambari.serviceconfighosts WHERE service_config_id IN (
  SELECT ambari.service_config_id FROM serviceconfig WHERE service_name LIKE '%error_service%'
);

DELETE FROM ambari.clusterconfig WHERE type_config LIKE '%error_service%';


```
