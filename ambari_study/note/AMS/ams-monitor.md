# ams-monitor

ambari-metrics-monitor 在集群中每台服务器上收集系统级的度量值，并发送到 ambari-metrics-collector 。

## 收集的度量值

monitor 收集度量值包括：

1. CPU相关信息
    1. cpu_num: cpu数量
    2. cpu_user: 用户态使用的cpu时间比
    3. cpu_system: 系统态使用的cpu时间比
    4. cpu_idel: 从时间的角度衡量 cpu 的空闲程度
    5. cpu_nice: 用做 nice 加权的进程分配的用户态 cpu 时间比
    6. cpu_wio: cpu 等待磁盘写入完成时间
    7. cpu_intr: 硬中断消耗时间
    8. cpu_sintr: 软中断消耗时间
    9. cpu_steal: 虚拟机偷取时间
    10. boottime
2.  进程监控
    1. process run
    2. process total
3.  内存监控
    1. mem_total
    2. mem_used
    3. mem_free
    4. mem_shared
    5. mem_buffered
    6. mem_cached
    7. swap_free
    8. swap_used
    9. swap_ total
    10. swap_in
    11. swap_out
4. network
    1. bytes_out
    2. bytes_in
    3. pkts_out
    4. pkts_in
5. disk_usage
    1. disk_total
    2. dosk_used
    3. disk_free
    4. disk_percent
6. host_static_info
    1. cpu_num
    2. swap_total
    3. boottime
    4. mem_total
7. hostname
8. ip_address

## 数据采集

数据采集在 host_info.py 中实现，通过 python 的 psutil 模块采集系统数据。在 metrics_collector 中将数据组装。如果需要添加新的 host 级别的监控指标可以在这里添加 。

## 安装 ambari-monitor 后文件目录  

安装 ambari-monitor 后，会在 python 的 ```site-packages``` 中添加 ```resource_monitoring``` 目录, 内容为 ```ambari-metrics-host-monitor``` 中 ```src/python``` 的内容。

配置文件路径: ```/etc/ambari-metrics-monitor/conf/```




