# zookeeper

参考: [zookeeper 基本原理](http://www.cnblogs.com/wade-luffy/p/5767811.html)

ACL分为两个维度，一个是属组，一个是权限，子目录/文件默认继承父目录的ACL。而在Zookeeper中，node的ACL是没有继承关系的，是独立控制的。Zookeeper的ACL，可以从三个维度来理解：一是scheme; 二是user; 三是permission，通常表示为scheme:id:permissions。  
sasl: sasl的对应的id，是一个通过sasl authentication用户的id，zookeeper-3.4.4中的sasl authentication是通过kerberos来实现的，也就是说用户只有通过了kerberos认证，才能访问它有权限的node.
