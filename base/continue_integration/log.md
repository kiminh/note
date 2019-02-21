# log & des

```log
STATUS | wrapper  | 2017/10/04 22:04:31 | --> Wrapper Started as Daemon
STATUS | wrapper  | 2017/10/04 22:04:31 | Launching a JVM...
INFO   | jvm 1    | 2017/10/04 22:04:31 | WrapperManager class initialized by thread: main  Using classloader: sun.misc.Launcher$AppClassLoader@5c647e05
INFO   | jvm 1    | 2017/10/04 22:04:32 | Wrapper (Version 3.2.3) http://wrapper.tanukisoftware.org
INFO   | jvm 1    | 2017/10/04 22:04:32 |   Copyright 1999-2006 Tanuki Software, Inc.  All Rights Reserved.
INFO   | jvm 1    | 2017/10/04 22:04:32 |
INFO   | jvm 1    | 2017/10/04 22:04:32 | Wrapper Manager: JVM #1
INFO   | jvm 1    | 2017/10/04 22:04:32 | Running a 64-bit JVM.
INFO   | jvm 1    | 2017/10/04 22:04:32 | Wrapper Manager: Registering shutdown hook
INFO   | jvm 1    | 2017/10/04 22:04:32 | Wrapper Manager: Using wrapper
INFO   | jvm 1    | 2017/10/04 22:04:32 | Load native library.  One or more attempts may fail if platform specific libraries do not exist.
INFO   | jvm 1    | 2017/10/04 22:04:32 | Loading native library failed: libwrapper-linux-x86-64.so  Cause: java.lang.UnsatisfiedLinkError: no wrapper-linux-x86-64 in java.library.path
INFO   | jvm 1    | 2017/10/04 22:04:32 | Loaded native library: libwrapper.so
INFO   | jvm 1    | 2017/10/04 22:04:32 | Calling native initialization method.
INFO   | jvm 1    | 2017/10/04 22:04:32 | Inside native WrapperManager initialization method
INFO   | jvm 1    | 2017/10/04 22:04:32 | Java Version   : 1.8.0_144-b01 Java HotSpot(TM) 64-Bit Server VM
INFO   | jvm 1    | 2017/10/04 22:04:32 | Java VM Vendor : Oracle Corporation
INFO   | jvm 1    | 2017/10/04 22:04:32 |
INFO   | jvm 1    | 2017/10/04 22:04:32 | Control event monitor thread started.
INFO   | jvm 1    | 2017/10/04 22:04:32 | Startup runner thread started.
INFO   | jvm 1    | 2017/10/04 22:04:32 | WrapperManager.start(org.tanukisoftware.wrapper.WrapperSimpleApp@77459877, args[]) called by thread: main
INFO   | jvm 1    | 2017/10/04 22:04:32 | Communications runner thread started.
INFO   | jvm 1    | 2017/10/04 22:04:32 | Open socket to wrapper...Wrapper-Connection
INFO   | jvm 1    | 2017/10/04 22:04:32 | Failed attempt to bind using local port 31000
INFO   | jvm 1    | 2017/10/04 22:04:32 | Opened Socket from 31001 to 32000
INFO   | jvm 1    | 2017/10/04 22:04:32 | Send a packet KEY : j2I0XvBZUDWpNk3c
INFO   | jvm 1    | 2017/10/04 22:04:32 | handleSocket(Socket[addr=/127.0.0.1,port=32000,localport=31001])
INFO   | jvm 1    | 2017/10/04 22:04:32 | Received a packet LOW_LOG_LEVEL : 2
INFO   | jvm 1    | 2017/10/04 22:04:32 | 2017.10.04 22:04:32 INFO  app[][o.s.a.AppFileSystem] Cleaning or creating temp directory /usr/local/sonarqube/temp
INFO   | jvm 1    | 2017/10/04 22:04:32 | 2017.10.04 22:04:32 INFO  app[][o.s.a.p.JavaProcessLauncherImpl] Launch process[es]: /usr/local/java/jre/bin/java -Djava.awt.headless=true -Xmx1G -Xms256m -Xss256k -Djna.nosys=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=75 -XX:+UseCMSInitiatingOccupancyOnly -XX:+HeapDumpOnOutOfMemoryError -Djava.io.tmpdir=/usr/local/sonarqube/temp -cp ./lib/common/*:./lib/search/* org.sonar.search.SearchServer /usr/local/sonarqube/temp/sq-process3503792944078908234properties
INFO   | jvm 1    | 2017/10/04 22:04:41 | 2017.10.04 22:04:41 INFO  app[][o.s.a.SchedulerImpl] Process[es] is up
INFO   | jvm 1    | 2017/10/04 22:04:41 | 2017.10.04 22:04:41 INFO  app[][o.s.a.p.JavaProcessLauncherImpl] Launch process[web]: /usr/local/java/jre/bin/java -Djava.awt.headless=true -Dfile.encoding=UTF-8 -server -Djava.io.tmpdir=/usr/local/sonarqube/temp -cp ./lib/common/*:./lib/server/*:/usr/local/sonarqube/lib/jdbc/mysql/mysql-connector-java-5.1.42.jar org.sonar.server.app.WebServer /usr/local/sonarqube/temp/sq-process2642504210563671582properties
INFO   | jvm 1    | 2017/10/04 22:04:45 | 2017.10.04 22:04:44 INFO  app[][o.s.a.SchedulerImpl] Process [web] is stopped
INFO   | jvm 1    | 2017/10/04 22:04:45 | 2017.10.04 22:04:45 INFO  app[][o.s.a.SchedulerImpl] Process [es] is stopped
INFO   | jvm 1    | 2017/10/04 22:04:45 | 2017.10.04 22:04:45 INFO  app[][o.s.a.SchedulerImpl] SonarQube is stopped
STATUS | wrapper  | 2017/10/04 22:04:47 | <-- Wrapper Stopped
```

```bash
 mvn --batch-mode verify org.sonarsource.scanner.maven:sonar-maven-plugin:3.0.1:sonar -Dsonar.host.url=http://192.168.1.88:9000/ -Dsonar.login=tangl -Dsonar.password=123456 -Dsonar.analysis.mode=preview -Dsonar.issuesReport.console.enable=true -Dsonar.gitlab.commit_sha=$CI_COMMIT_SHA -Dsonar.gitlab.ref=$CI_COMMIT_REF_NAME Dsonar.gitlab.project_id=$CI_PROJECT_PATH
```

```bash
# Personal Access Token
8YQ-gxbWupxJAGRWCVrc
```

```bash
# sonar 令牌
gitlab: 5f6f68885d1cf72e86bf223411adb9bb5fb50b3e
```

```bash
# 使用 Maven 执行 SonarQube 扫描
# 在正常的 mvn 编译命令后增加 sonar:sonar -D....
mvn sonar:sonar \
  -Dsonar.host.url=http://192.168.1.88:9000 \
  -Dsonar.login=5f6f68885d1cf72e86bf223411adb9bb5fb50b3e
```

```bash
# sonar.java.binaries
# Please provide compiled classes of your project with sonar.java.binaries property
mvn package sonar:sonar   -Dsonar.host.url=http://192.168.1.88:9000   -Dsonar.login=5f6f68885d1cf72e86bf223411adb9bb5fb50b3e -Dsonar.sources=src/main/java/ -Dsonar.libraries=./target/sonar-gitlab-plugin-2.2.0-SNAPSHOT/META-INF/lib/*.jar
```

```txt
# sonarqube's new token for jenkins
8fce20e507448c653d87174102db4b25e17b28db
```

```bash
mvn package sonar:sonar   -Dsonar.host.url=http://192.168.1.88:9000   -Dsonar.login=5f6f68885d1cf72e86bf223411adb9bb5fb50b3e -Dsonar.sources=src/main/java/ -Dsonar.libraries=./src/main/webapp/WEB-INF/lib/*.jar
```

## jenkins log

```log
Oct 25, 2017 2:25:10 PM hudson.XmlFile replaceIfNotAtTopLevel
WARNING: JENKINS-45892: reference to hudson.maven.MavenModuleSet@73486958[ssm] being saved from unexpected /var/lib/jenkins/jobs/ssm/builds/37/build.xml
java.lang.IllegalStateException
	at hudson.XmlFile.replaceIfNotAtTopLevel(XmlFile.java:210)
	at hudson.model.AbstractItem.writeReplace(AbstractItem.java:509)
	at sun.reflect.GeneratedMethodAccessor563.invoke(Unknown Source)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at com.thoughtworks.xstream.converters.reflection.SerializationMethodInvoker.callWriteReplace(SerializationMethodInvoker.java:89)
	at hudson.util.RobustReflectionConverter.marshal(RobustReflectionConverter.java:141)
	at com.thoughtworks.xstream.core.AbstractReferenceMarshaller.convert(AbstractReferenceMarshaller.java:69)
	at com.thoughtworks.xstream.core.TreeMarshaller.convertAnother(TreeMarshaller.java:58)
	at com.thoughtworks.xstream.core.AbstractReferenceMarshaller$1.convertAnother(AbstractReferenceMarshaller.java:84)
	at hudson.util.RobustReflectionConverter.marshallField(RobustReflectionConverter.java:265)
	at hudson.util.RobustReflectionConverter$2.writeField(RobustReflectionConverter.java:252)
	at hudson.util.RobustReflectionConverter$2.visit(RobustReflectionConverter.java:224)
	at com.thoughtworks.xstream.converters.reflection.PureJavaReflectionProvider.visitSerializableFields(PureJavaReflectionProvider.java:138)
	at hudson.util.RobustReflectionConverter.doMarshal(RobustReflectionConverter.java:209)
	at hudson.util.RobustReflectionConverter.marshal(RobustReflectionConverter.java:150)
	at com.thoughtworks.xstream.core.AbstractReferenceMarshaller.convert(AbstractReferenceMarshaller.java:69)
	at com.thoughtworks.xstream.core.TreeMarshaller.convertAnother(TreeMarshaller.java:58)
	at com.thoughtworks.xstream.core.TreeMarshaller.convertAnother(TreeMarshaller.java:43)
	at com.thoughtworks.xstream.core.AbstractReferenceMarshaller$1.convertAnother(AbstractReferenceMarshaller.java:88)
	at com.thoughtworks.xstream.converters.collections.AbstractCollectionConverter.writeItem(AbstractCollectionConverter.java:64)
	at com.thoughtworks.xstream.converters.collections.CollectionConverter.marshal(CollectionConverter.java:74)
	at com.thoughtworks.xstream.core.AbstractReferenceMarshaller.convert(AbstractReferenceMarshaller.java:69)
	at com.thoughtworks.xstream.core.TreeMarshaller.convertAnother(TreeMarshaller.java:58)
	at com.thoughtworks.xstream.core.AbstractReferenceMarshaller$1.convertAnother(AbstractReferenceMarshaller.java:84)
	at hudson.util.RobustReflectionConverter.marshallField(RobustReflectionConverter.java:265)
	at hudson.util.RobustReflectionConverter$2.writeField(RobustReflectionConverter.java:252)
	at hudson.util.RobustReflectionConverter$2.visit(RobustReflectionConverter.java:224)
	at com.thoughtworks.xstream.converters.reflection.PureJavaReflectionProvider.visitSerializableFields(PureJavaReflectionProvider.java:138)
	at hudson.util.RobustReflectionConverter.doMarshal(RobustReflectionConverter.java:209)
	at hudson.util.RobustReflectionConverter.marshal(RobustReflectionConverter.java:150)
	at com.thoughtworks.xstream.core.AbstractReferenceMarshaller.convert(AbstractReferenceMarshaller.java:69)
	at com.thoughtworks.xstream.core.TreeMarshaller.convertAnother(TreeMarshaller.java:58)
	at com.thoughtworks.xstream.core.TreeMarshaller.convertAnother(TreeMarshaller.java:43)
	at com.thoughtworks.xstream.core.TreeMarshaller.start(TreeMarshaller.java:82)
	at com.thoughtworks.xstream.core.AbstractTreeMarshallingStrategy.marshal(AbstractTreeMarshallingStrategy.java:37)
	at com.thoughtworks.xstream.XStream.marshal(XStream.java:1026)
	at com.thoughtworks.xstream.XStream.marshal(XStream.java:1015)
	at com.thoughtworks.xstream.XStream.toXML(XStream.java:988)
	at hudson.XmlFile.write(XmlFile.java:181)
	at hudson.model.Run.save(Run.java:1920)
	at hudson.maven.MavenModuleSetBuild.notifyModuleBuild(MavenModuleSetBuild.java:599)
	at hudson.maven.MavenBuild$ProxyImpl2.end(MavenBuild.java:618)
	at sun.reflect.GeneratedMethodAccessor655.invoke(Unknown Source)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at hudson.model.Executor$2.call(Executor.java:939)
	at hudson.util.InterceptingProxy$1.invoke(InterceptingProxy.java:23)
	at com.sun.proxy.$Proxy87.end(Unknown Source)
	at sun.reflect.GeneratedMethodAccessor655.invoke(Unknown Source)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at hudson.remoting.RemoteInvocationHandler$RPCRequest.perform(RemoteInvocationHandler.java:899)
	at hudson.remoting.RemoteInvocationHandler$RPCRequest.call(RemoteInvocationHandler.java:873)
	at hudson.remoting.RemoteInvocationHandler$RPCRequest.call(RemoteInvocationHandler.java:832)
	at hudson.remoting.UserRequest.perform(UserRequest.java:205)
	at hudson.remoting.UserRequest.perform(UserRequest.java:52)
	at hudson.remoting.Request$2.run(Request.java:356)
	at hudson.remoting.InterceptingExecutorService$1.call(InterceptingExecutorService.java:72)
	at org.jenkinsci.remoting.CallableDecorator.call(CallableDecorator.java:19)
	at hudson.remoting.CallableDecoratorList$1.call(CallableDecoratorList.java:21)
	at jenkins.util.ContextResettingExecutorService$2.call(ContextResettingExecutorService.java:46)
	at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)
```

```bash
# sh /home/jenkins/tomcat/bin/shutdown.sh
rm -rf /home/jenkins/tomcat/webapps/*
cp target/*.war /home/jenkins/tomcat/webapps/
sleep 2
sh /home/jenkins/tomcat/bin/startup.sh
```

```log
org.eclipse.jetty.io.EofException
        at org.eclipse.jetty.io.ChannelEndPoint.flush(ChannelEndPoint.java:292)
        at org.eclipse.jetty.io.WriteFlusher.flush(WriteFlusher.java:429)
        at org.eclipse.jetty.io.WriteFlusher.completeWrite(WriteFlusher.java:384)
        at org.eclipse.jetty.io.ChannelEndPoint$3.run(ChannelEndPoint.java:139)
        at org.eclipse.jetty.util.thread.Invocable.invokePreferred(Invocable.java:128)
        at org.eclipse.jetty.util.thread.Invocable$InvocableExecutor.invoke(Invocable.java:222)
        at org.eclipse.jetty.util.thread.strategy.EatWhatYouKill.doProduce(EatWhatYouKill.java:294)
        at org.eclipse.jetty.util.thread.strategy.EatWhatYouKill.run(EatWhatYouKill.java:199)
        at winstone.BoundedExecutorService$1.run(BoundedExecutorService.java:77)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
        at java.lang.Thread.run(Thread.java:748)
Caused by: java.io.IOException: Broken pipe
        at sun.nio.ch.FileDispatcherImpl.write0(Native Method)
        at sun.nio.ch.SocketDispatcher.write(SocketDispatcher.java:47)
        at sun.nio.ch.IOUtil.writeFromNativeBuffer(IOUtil.java:93)
        at sun.nio.ch.IOUtil.write(IOUtil.java:51)
        at sun.nio.ch.SocketChannelImpl.write(SocketChannelImpl.java:471)
        at org.eclipse.jetty.io.ChannelEndPoint.flush(ChannelEndPoint.java:270)
        ... 11 more
```
