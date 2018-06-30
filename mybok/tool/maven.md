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

//命令
mvn -v //查看版本
mvn clean //清除target
mvn package //打包
mvn clean package // 清除并打包

```