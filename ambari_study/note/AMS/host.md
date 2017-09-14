# 为 host 增加监控项  

1. 监控每个 host 的状态在 ambari-metrics-host-monitoring 中。编译后为 ambari-metrics-monitor 。  

2. 监控指标包括 :  
    1. cpu: ```cpu_num, cpu_user, cpu_system, cpu_idle, cpu_nice, cpu_wio, cpu_intr, cpu_sintr, cpu_steal, boottime```  
    2. memory: ```mem_total, mem_used, mem_free, mem_shared, mem_buffered, mem_cached```, 和 ```swap_free, swap_used, swap_total, swap_in, swap_out```  
    3. disk:  
    4. network:```bytes_out, bytes_in, pkts_out, pkts_in```  
    5. processes:```proc_run, proc_total```  
    6. 
以上监控内容通过 python 的 psutil 模块获取, 监控内容在 ```host_info.py``` 中。  

3. 组装后发送到 ambari-metrics-collector(也就是 timeline server) , ambari-metrics-collector 接收到上报数据后将其存储到 hbase 中, 通过 REST Api 将监控数据发布。  

4. ambari-server 通过 metrics-collector 发布的 REST Api 读取监控数据, 并将其显示。

## host monitoring

- 通过修改 host_info.py 可以修改监控内容。通过 application_metric_map.py 中的 ApplicationMetricMap 类的 put_metric() 方法组装通过 MetricsCollector 类的 process_host_collection_event 获取的上报数据。通过修改这两部分可以修改上报监控指标。

## 发布 REST Api  

1. 通过读取配置 metrics_def 目录下的所有 .dat 文件获取监控指标。为该目录下所有文件中的监控项发布 REST 接口, ambari-server 通过读取该接口获取监控数据。  

2. ambari-server 读取到的监控信息通过配置 ganglia_properties.json 来发布 REST 接口, ambari-web 获取数据将其展示在界面上。




















