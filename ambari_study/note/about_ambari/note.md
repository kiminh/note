# tiny note

1. Execute

执行 shell 命令可以通过 resource_manager 模块的 Execute 来执行, 可以通过返回来判断是否执行成功。

```
Execute(format('<command>'))
# shell 的返回值为 0, 表示执行成功, 返回值为 1, 表示执行失败。 可以添加 retry 参数来设置重复次数。
```

DB chech 步骤
Checking for configs not mapped to any cluster
Checking for configs selected more than once
Checking for hosts without state
Checking host component states count equals host component desired states count
Checking services and their configs
