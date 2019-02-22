**环境变量目录**

`maven/bin`



**配置**

```xml
<!-- maven/conf/setting.xml -->

<localRepository>./m2/repository</localRepository>

<mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```



**Commands**

```shel
mvn package -Dmaven.test.skip=true #打包跳过测试
mvn -v #查看版本
mvn clean #清除target
mvn package #打包
mvn clean package #清除并打包
```


**pom.xml 指定JDK版本**

```xml
<build> 
  <plugins> 
    <plugin> 
      <groupId>org.apache.maven.plugins</groupId> 
      <artifactId>maven-compiler-plugin</artifactId> 
      <version>2.0.2</version> 
      <configuration> 
        <source>1.8</source> 
        <target>1.8</target> 
      </configuration> 
    </plugin> 
  </plugins> 
</build>  
```


