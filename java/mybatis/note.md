1. 半自动化 orm 框架
2.




- errors

  1. org.apache.ibatis.binding.BindingException

  > 背景：Spring整合Mybatis
  > 报错：org.apache.ibatis.binding.BindingException: Invalid bound statement (not found)

  > 解释：就是说，你的Mapper接口，被Spring注入后，却无法正常的使用mapper.xml的sql；

  > 这里的Spring注入后的意思是，你的接口已经成功的被扫描到，但是当Spring尝试注入一个代理（MyBatista实现）的实现类后，却无法正常使用。这里的可能发生的情况有如下几种；

    1. 接口已经被扫描到，但是代理对象没有找到，即使尝试注入，也是注入一个错误的对象（可能就是null）
    2. 接口已经被扫描到，代理对象找到了，也注入到接口上了，但是调用某个具体方法时，却无法使用（可能别的方法是正常的）
    当然，我们不好说是那种情况，毕竟报错的结果是一样的，这里就提供几种排查方法：

    `mapper` 接口和 `mapper.xml` 是否在同一个包 (package) 下; 名字是否一样（仅后缀不同）。

    比如，接口名是 `NameMapper.java` ;对应的 `xml` 就应该是 `NameMapper.xml`
    `mapper.xml` 的命名空间 `namespace` 是否跟 `mapper` 接口的包名一致？

    比如，你接口的包名是 `com.abc.dao` ,接口名是 `NameMapper.java` , 那么你的 `mapper.xml` 的 `namespace` 应该是 `com.abc.dao.NameMapper` 接口的方法名，与xml中的一条sql标签的id一致

    比如，接口的方法List<User> findAll();那么，对应的xml里面一定有一条是<select id="findAll" resultMap="**">****</select>  
    如果接口中的返回值List集合（不知道其他集合也是），那么xml里面的配置，尽量用resultMap（保证resultMap配置正确）,不要用resultType
    最后，如果你的项目是maven项目，请你在编译后，到接口所在目录看一看，很有可能是没有生产对应的xml文件，因为maven默认是不编译的，因此，你需要在的pom.xml的<build></build>里面，加这么一段：

```xml
<resources>  
  <resource>  
    <directory>src/main/java</directory>  
    <includes>  
      <include>**/*.xml</include>  
    </includes>  
    <filtering>true</filtering>  
  </resource>  
</resources>  
```
