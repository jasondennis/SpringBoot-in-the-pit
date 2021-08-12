### Spring Boot 学习指北



#### 如何启动一个Spring Boot项目

最常用的方法是在IDEA里使用Spring 启动器启动新建Spring Boot项目（本质上其实也是一个Maven Project）

#### parent 概念

Parent 本质上，是为整个Spring Boot工程提供一个相关的依据，例如确认Java 的具体编译版本，继承自Parent的相关依赖，在执行打包的时候需要什么样的配置，资源过滤等等。这些操作和定义都依赖于Parent，不论是继承自 `Spring-boot-starter-parent` 还是自定义的Parent。



#### banner 彩蛋： resources下新建 banner.txt 复制彩蛋banner进去就行了



####   Spring Boot微服务代码结构 

1. DAO : DAO 层实质上是一种数据存储的对象，例如，与数据库连接的代码就存储在这里（tips: SQL语句一般我们放在mapper文件里面）。DAO是对数据库的一种封装。
2. Service: Service层依据团队的开发规范，是业务逻辑主要实现的地方。是对DAO的搭建。
3. Controller: Controller依据开发规范，只是简单的对Service层的调用，负责控制业务逻辑 。
4. Mapper: 



