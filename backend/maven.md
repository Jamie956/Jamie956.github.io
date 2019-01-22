```
maven/bin //设置环境变量目录

//配置
nano maven/conf/setting.xml
//设置repo保存目录
<localRepository>./m2/repository</localRepository>
//设置镜像
<mirror>
  <id>alimaven</id>
  <name>aliyun maven</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
  <mirrorOf>central</mirrorOf>
</mirror>
```


## 命令

### 打包跳过测试
```
mvn package -Dmaven.test.skip=true
```
### 查看版本
```
mvn -v
```
### 清除target
```
mvn clean
```
### 打包
```
mvn package
```
### 清除并打包
```
mvn clean package
```


### Maven项目指定JDK版本(pom.xml配置)
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


