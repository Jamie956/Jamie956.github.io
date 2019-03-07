### CDN

CDN stands for “Content Delivery Network” and the technology provides a 
way of serving assets such as static HTML, CSS, Javascript, and images 
over the web much faster than serving them from a single origin server. 
It works by distributing the content across many “edge” servers around 
the world so that users end up downloading assets from the “edge” 
servers instead of the origin server. For instance in the image below, a
user in Spain requests a web page from a site with origin servers in 
NYC, but the static assets for the page are loaded from a CDN “edge” 
server in England, preventing many slow cross-Atlantic HTTP requests.



![CDN](..\img\CDN.png)



In general a web app should always use a CDN to serve CSS, Javascript, 
images, videos and any other assets. Some apps might also be able to 
leverage a CDN to serve static HTML pages.



### Feynman

- The Feynman Technique
  - Step 1: Narrow down on a topic which you find difficult to grasp. Learn about the topic.
  - Step 2: Explain the topic as though you are teaching it to someone in very simple terms
  - Step 3: Work through examples or demonstrate how it works
  - Step 4: Assess your knowledge about the topic, if still some concepts are unclear, learn more about them and repeat steps 2–4

- “Closed mindedness is terribly costly. It causes you to lose out on all kinds of wonderful possibilities and dangerous threats that other people might be showing you.” — Ray Dalio

- Today, so many people think that they have figured out the truth, see other people as opponents and their opinions as things to eliminate rather than to understand.

- This is dangerous, because if you’re too proud of what you know, you will learn less, make inferior decisions and fall short of your potential. You will fail to benefit from other people’s thinking. Therefore, it’s invaluable to know what you don’t know.

- Moreover, in cases of disagreement, life is so much more interesting when you attempt to understand rather than to persuade. You’ll have more fun and you’ll have better relationships.

- “With more knowledge comes a deeper, more wonderful mystery, luring one 
  on to penetrate deeper still. Never concerned that the answer may prove 
  disappointing, with pleasure and confidence we turn over each new stone 
  to find unimagined strangeness leading on to **more wonderful questions** and mysteries — certainly **a grand adventure**!”
- As a seeker, you’re no longer looking to protect your views and to convert others, but you’re willing to be changed.

- “Stay hungry. Stay foolish.” — Steve Jobs
  - Remaining foolish means not letting your need to right overshadow your need to find out what’s true.
  - Remaining hungry means choosing improvement over confirmation and 
    uncertainty over defensiveness — replacing attachment to always being 
    right with the joy of learning what’s true.
- Realize that the game of life is about *playing it—* not about winning it.



### Mongo

```shell
===basic===
mongod --dbpath "E:\MongoDB\data" --logpath "E:\MongoDB\logs\mongo.log" --install -serviceName "MongoDB"
net start MongoDB

===auth===
use admin
db.createUser({user:"admin",pwd:"admin",roles:[{"role":"userAdmin","db":"admin"},{"role":"root","db":"admin"},{"role":"userAdminAnyDatabase","db":"admin"}]})
db.auth("admin","admin")
sc delete MongoDB
mongod --dbpath "E:\MongoDB\data" --logpath "E:\MongoDB\logs\mongo.log" --auth --install -serviceName "MongoDB"
net start MongoDB

mongo -u admin -p 123456 localhost:27017/admin

mongodb://<user>:<pwd>@<host>:27017/<collection>

===create user===
use <db>
db.createUser({user:"jamie956", pwd:"123456", roles:["readWrite","dbAdmin"]})
db.auth("jamie956","123456")

===db===
show dbs
db
use <db>
db.dropDatabase()
	
===collection===
show collections
db.createCollection("<collection>")
db.createCollection("<collection>", { capped : true, autoIndexId : true, size : 6142800, max : 10000 } )
db.<collection>.drop()

===document===
db.<collection>.find()
db.<collection>.find().pretty()
db.<collection>.find({key1:"val1"}).pretty()
db.<collection>.find( { $or:[{key1:"val1"},{key2:"val2"}] } ).pretty()
db.<collection>.find().count()
db.<collection>.find( {num:{$lt:<num>}} ).pretty()
db.<collection>.find().sort({<key>:<1 or -1>}).pretty()
db.<collection>.find().limit(1)
db.<collection>.insert( { [<key>:"<val>", …] } )
db.<collection>.remove({key1:"val1"})

===field===
add => db.<collection>.update( {key1:"val1"}, {$set:{key3:"val3"}} )
update => db.<collection>.update( {key1:"val1"}, {$set:{key2:"update val2"}} )
db.<collection>.update( {key1:"val1"}, {$inc:{num:10}} )
db.<collection>.update( {key1:"val1"}, {$rename:{"key3":"rename key3"}} )
remove =>  db.<collection>.update( {key1:"val1"},{$unset:{key3:1}} )

```



### Redis

```
===basic===
start server => redis-server.exe redis.windows.conf
start client => redis-cli(or redis-cli.exe -h 127.0.0.1 -p 6379)

===command===
set <key> <val>
get <key>
keys *
flushall
incr <key> <default 1>
incrby <key> <num>
decr <key> <default 1>
decrby <key> <num>

===sort===
LPUSH <key> <num1 num2 … numn>
RPUSH <key> <num1 num2 … numn>
SORT <key>
SORT <key> DESC

LPUSH <key> <str>
SORT <key> ALPHA

SORT <key> LIMIT <offset> <count>

LPUSH uid <n>
SET <key1_n> <val1>
SET <key2_n> <val2>

SORT uid BY <key_*>
SORT uid GET <key_*>
SORT uid GET <key1_*> GET <key2_*>
SORT uid GET #

HMSET <hash_n> <key1> <val1> <key2> <val2> <key3> <val3> …
SORT uid BY <hash_*>-><key>
SORT uid BY <hash_*>-><key> GET <hash_*>-><key>

LRANGE <list> 0 -1
SORT <list> STORE <list2>

===hash===
HSET <hash> <key> <val>
HDEL <hash> <key>
HEXISTS <hash> <key>
HGET <hash> <key>
HGETALL <hash>
HINCRBY <hash> <key> <num>
HINCRBYFLOAT <hash> <key> <num>
HKEYS <hash>
HLEN <hash>
HMGET <hash> <key1> <key2>
HMSET <hash> [<key> <val> …]
HVALS <hash>
```



### Maven



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

