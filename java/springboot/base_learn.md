## spring boot


### spring boot annotation

- @SpringBootApplication

```java
@SpringBootApplication
    @SpringBootConfiguration
    @ComponentScan
    @EnableAutoConfiguration(exclude, excludeName)
        @AutoConfigurationPackage
        @Import
```

```java
@AutoConfiguration
    @Conditional
    @ConditionalOnClass
```

```java
// 启动自动加载配置
// @AliasFor(annotation = Component.class)
@Configuration
    @Component
        @Indexed
```
