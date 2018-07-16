# Spring data elasticsearch

## 重要接口

- org.springframework.data.elasticsearch.repository.ElasticsearchRepository: 
    > 封装基本的 elasticsearch 操作.  
    > 接口在存储库不存在代理 Bean.  
    > 继承 `ElasticsearchCrudRepository` 接口以提供迭代和分页功能.
    > `AbstractElasticsearchRepository` 抽象类包含基本操作的实现.


