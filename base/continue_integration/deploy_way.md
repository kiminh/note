# 自动化部署 可行的部署方式

1. 通过分布式的 jenkins 来部署, 即在需要部署的机器上通过 slave.jar 来完成
2. ansible  进行部署, 即部署的步骤为通过 ansible 将编译好的可执行文件 copy 至发布的服务器上, 直接通过 ansible 来执行运行。
3. 通过 pipeline 的模式进行配置,  即编写 groovy 脚本, 部署模块即 `stage('Deploy') {}`, 
