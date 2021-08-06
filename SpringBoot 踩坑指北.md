## SpringBoot 踩坑指北

###  

### Spring Boot学习过程中各种不知道哪里来的错误的记录



##### Could not transfer artifact com.fiberhome.microserv:iotbusi:pom:0.2.0 from/to fh-njrd-local 日常拉不下来jar包的一些问题

###### 实测问题在于两个地方，其一：Proxifier 在Mac后台生成了用于数据过滤的虚拟网卡，桌面上关闭Proxifier时，虚拟网卡仍然在将流量发送忘错误的地方，本地Maven无法收到私服的回应。其二（关键）：私服证书已经过期，Maven对其不认可，需要在IDEA中进行配置，忽略证书问题。



##### 找不到加载的主类这种奇怪的问题（包括在configuration里面无法选中Spring Boot 的启动类)

###### 解决办法：重启大法好



##### java: 程序包org.springframework.boot不存在

###### 分析问题：按照常理，Maven应当会自动拉取依赖的包，猜测Maven的问题。尝试Maven拉取依赖，失败。检查settings.xml文件，是之前配置的内网私服配置为了全局。内网无相应的包。更改配置文件可解决（此处仅贴出mirrors相关的配置）：



``` settings.xml
  <mirrors>
    <!-- mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
     -->
  
     <mirror>
         <id>nexus-aliyun</id>
         <mirrorOf>nexus-aliyun</mirrorOf>
         <name>Nexus aliyun</name>
         <url>http://maven.aliyun.com/nexus/content/groups/public</url>
     </mirror>
     <mirror>
   <id>alispring</id>
   <name>alispring</name>
   <url>https://maven.aliyun.com/repository/spring</url>
   <mirrorOf>spring</mirrorOf>
  </mirror>
  <mirror>
  <id>alimaven</id>
  <name>aliyun maven</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
  <mirrorOf>alimaven_demo</mirrorOf>
  </mirror>
     <mirror>
         <id>yourself</id>
         <mirrorOf>fh-all</mirrorOf>
         <name>yourself</name>
         <url>yourself</url>
     </mirror>
     
 
  <mirror>
  <id>alijcenter</id>
  <name>aliyun jcenter</name>
  <url>https://maven.aliyun.com/repository/public/</url>
  <mirrorOf>alijcenter</mirrorOf>
 </mirror>
 
 <mirror>
  <id>alimaven</id>
  <mirrorOf>central</mirrorOf>
  <name>aliyun maven</name>
  <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
 </mirror>


 
 <!--
 <mirror>
  <id>ibiblio</id>
  <mirrorOf>central</mirrorOf>
  <name>Human Readable Name for this Mirror.</name>
  <url>http://mirrors.ibiblio.org/pub/mirrors/maven2/</url>
 </mirror>
 <mirror>
  <id>jboss-public-repository-group</id>
  <mirrorOf>central</mirrorOf>
  <name>JBoss Public Repository Group</name>
  <url>http://repository.jboss.org/nexus/content/groups/public</url>
 </mirror>
  
 <mirror>
  <id>central</id>
  <name>Maven Repository Switchboard</name>
  <url>http://repo1.maven.org/maven2/</url>
  <mirrorOf>central</mirrorOf>
 </mirror>
 <mirror>
  <id>repo2</id>
  <mirrorOf>central</mirrorOf>
  <name>Human Readable Name for this Mirror.</name>
  <url>http://repo2.maven.org/maven2/</url>
 </mirror>
 -->
  </mirrors>
```



##### 以上设置参数实现了内网私服与阿里Maven镜像的同步使用，供参考。



##### > tips: IDEA 在运行Spring Boot的时候最好将运行交给Maven来进行，自己容易再次出现包不存在的情况（如下）：

![截屏2021-08-05 下午5.09.13](https://github.com/jasondennis/SpringBoot-in-the-pit/blob/master/source/截屏2021-08-05%20下午5.09.13.png)



#### Mac 的Maven配置相关问题

##### Mac现在更新了终端，bash 不再成为一个选项。当前MacOS使用的是 zsh 终端，默认的配置文件已经不是`.bash_profile` 。因此，导致了一种问题：每次重启电脑，甚至仅仅重启终端后，在终端中调用Maven的mvn命令就会报错`Commond not found` 。

##### 解决方法：`vim ~/.zshrc`   此文件是zsh终端的默认配置文件。在`.zshrc` 结尾添加 `source ~/.bash_profile`  。 Problem solved.

#### Conclusion:当你遇到问题又不知道到底怎么办的时候，重启一下，总是当下最好的解决问题的办法。

