# CI

- gitlab(version control)
- sonar(code quality control)
- jenkins(continue integration)

## sonar

[sonar](https://www.sonarqube.org/)
[Sonar GitLab Plugin](https://github.com/gabrie-allaigre/sonar-gitlab-plugin)
[Sonar Auth GitLab Plugin](https://github.com/gabrie-allaigre/sonar-auth-gitlab-plugin)

sonar 和 gitlab 审查代码相关插件, 需要自行编译, 编译指令:
1. version set: `mvn versions:set -DnewVersion=<new_version>`
2. package: `mvn package`
将编译好的 jar 文件放在 <sonarqube_path>/extensions/plugins/ 中。
版本对应关系参考 project／README.md

配置参考:
[sonar-gitlab-plugin 配置](http://blog.csdn.net/aixiaoyang168/article/details/78115646)

- 数据库

使用外部数据库, 生产环境中不要使用嵌入式数据库。 mysql 要求5.6及以上版本。

## gitlab CI

[gitlab](https://about.gitlab.com/)
[gitlab-runner](https://docs.gitlab.com/runner/install/)

gitlab CI 需要 gitlab-runner, 同时需要编写 `.gitlab-ci.yml` 和 `sonar_preview.sh`。

- `.gitlab-ci.yml` 设置代码审查规则。
- `sonar_preview.sh` commit 后自动执行。
