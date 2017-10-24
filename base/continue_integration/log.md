```
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

```
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

```
# sonarqube's new token for jenkins
8fce20e507448c653d87174102db4b25e17b28db
```


```
mvn package sonar:sonar   -Dsonar.host.url=http://192.168.1.88:9000   -Dsonar.login=5f6f68885d1cf72e86bf223411adb9bb5fb50b3e -Dsonar.sources=src/main/java/ -Dsonar.libraries=./src/main/webapp/WEB-INF/lib/*.jar
```
