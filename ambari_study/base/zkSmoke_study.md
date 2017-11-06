## zlSmoke.sh usage

检测 zookeeper 状态

```bash
<path>/zkSmoke.sh {zk_cli_shell} {smokeuser} {config_dir} {client_port} {security_enabled} {kinit_path_local} {smokeUserKeytab} {smokeUserPrincipal} {zk_smoke_out}
# zk_cli_shell = "/usr/lib/zookeeper/bin/zkCli.sh"
# smokeuser = <os_user_execute_zkCli.sh>
# config_dir = "/etc/zookeeper/conf"
# client_port = /configurations/zoo.cfg/clientPort
# security_enabled = False
# kinit_path_local =
# smokeUserKeytab
# smokeUserPrincipal
# zk_smoke_out

sh zkSmoke.sh /usr/local/lng/zookeeper/bin/zkCli.sh root /usr/local/lng/zookeeper/conf /usr/local/lng/zookeeper/conf/zoo.cfg/clientPort False a a a /root/Documents/log.out

# 删除安全相关操作后
sh zkSmoke.sh /usr/local/lng/zookeeper/bin/zkCli.sh root /usr/local/lng/zookeeper/conf /usr/local/lng/zookeeper/conf/zoo.cfg/clientPort False out.log
```

## read the script

通过向 zkCli.sh 执行 zookeeper 指令, 新建测试 znode, 删除新建的 znode 来判断 zookeeper 的状态。
