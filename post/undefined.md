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



### Mysql

```shell
source sqlfile.sql
show variables like '%max_connections%'
show processlist
show full processlist
show engine innodb status

mysql -uroot -p123456 --default-character-set=utf8

SET character_set_client = utf8;

set global wait_timeout=604800; 

===change password===
set password for root@localhost = password('<new password>'); 

jdbc:mysql://192.168.145.130:3306/mydb?characterEncoding=utf8&useSSL=false

```



```sql
===setting password===
set password for root@localhost = password('123456');
===group by===
SELECT status FROM orders GROUP BY status;
SELECT YEAR(orderDate) AS YEAR, SUM( customerNumber) AS cus_total FROM orders GROUP BY YEAR(orderDate) HAVING YEAR > 2003
===distinct===
SELECT DISTINCT [column] FROM [table]
SELECT COUNT(DISTINCT [column2]) FROM [table] WHERE [column1] = 'xx'
===index===
SHOW INDEX FROM [table]
CREATE INDEX [index_name] ON [table] ([column])
DROP INDEX [index_name] ON [table]
===view===
CREATE VIEW [view_name] AS SELECT [column1], [column2] FROM [table]
SELECT * FROM [view_name]
UPDATE [view_name] SET [column2]='XX' WHERE [column1]='XX'
DROP VIEW [view_name]
===subquery===
SELECT * FROM [table] WHERE [column1] =(SELECT MAX(column2) FROM [table])
SELECT * FROM(SELECT * FROM [table]) AS t

===function===
SELECT CONCAT( '%', 'd', '%' )

SELECT group_concat('listen ', 'to ', 'new ', 'age')

SELECT COUNT(DISTINCT yyyymmdd) FROM table //获取group by 总行数
```



### Fit

胰岛素与减脂肪

热量平衡

合成代谢

分解代谢

胰岛素：合成代谢激素，降血糖，抑制分解代谢

胰岛素敏感性

胰岛素抵抗



建议

- 少吃碳水
- 运动
- 减肥
- 不要频繁吃东西
- 适当延长空腹时间



提高新陈代谢

- 提高组织器官代谢
- 饮食（蛋白、均衡饮食）
- 锻炼（高强度最佳）
- 补充剂（茶、咖啡、苹果醋、辣椒）



### Design Pattern

**abstract class** A class whose primary purpose is to define an interface. An abstract class defers some or all of its implementation to subclasses. An abstract class cannot be instantiated.

**abstract coupling** Given a class A that maintains a reference to an abstract class B, class A is said to be abstractly coupled to B. We call this abstract coupling because A refers to a type of object, not a concrete object.

**abstract operation** An operation that declares a signature but doesn't implement it. In C++, an abstract operation corresponds to a pure virtual member function.

**acquaintance relationship** A class that refers to another class has an acquaintance with that class.

**aggregate object** An object that's composed of subobjects. The subobjects are called the aggregate's parts, and the aggregate is responsible for them.

**aggregation relationship** The relationship of an aggregate object to its parts. A class defines this relationship for its instances (e.g., aggregate objects).

**black-box reuse** A style of reuse based on object composition. Composed objects reveal no internal details to each other and are thus analogous to "black boxes."

**class** A class defines an object's interface and implementation. It specifies the object's internal representation and defines the operations the object can perform.

**class diagram** A diagram that depicts classes, their internal structure and operations, and the static relationships between them.

**class operation** An operation targeted to a class and not to an individual object. In C++, class operations are are called static member functions.

**concrete class** A class having no abstract operations. It can be instantiated.

**constructor** In C++, an operation that is automatically invoked to initialize new instances.

**coupling** The degree to which software components depend on each other.

**delegation** An implementation mechanism in which an object forwards or delegates a request to another object. The delegate carries out the request on behalf of the original object.

**design pattern** A design pattern systematically names, motivates, and explains a general design that addresses a recurring design problem in object-oriented systems. It describes the problem, the solution, when to apply the solution, and its consequences. It also gives implementation hints and examples. The solution is a general arrangement of objects and classes that solve the problem. The solution is customized and implemented to solve the problem in a particular context.

**destructor** In C++, an operation that is automatically invoked to finalize an object that is about to be deleted.

**dynamic binding** The run-time association of a request to an object and one of its operations. In C++,only virtual functions are dynamically bound.

**encapsulation** The result of hiding a representation and implementation in an object. The representation is not visible and cannot be accessed directly from outside the object. Operations are the only way to access and modify an object's representation.

**framework** A set of cooperating classes that makes up a reusable design for a specific class of software.A framework provides architectural guidance by partitioning the design into abstract classes and defining their responsibilities and collaborations. A developer customizes the framework to a particular application by subclassing and composing instances of framework classes.

**friend class** In C++,a class that has the same access rights to the operations and data of a class as that class itself.

**inheritance** A relationship that defines one entity in terms of another. Class inheritance defines a new class in terms of one or more parent classes. The new class inherits its interface and implementation from its parents. The new class is called a subclass or (in C++) a derived class. Class inheritance combines interface inheritance and implementation inheritance. Interface inheritance defines a new interface in terms of one or more existing interfaces.Implementation inheritance defines a new implementation in terms of one or more existing implementations.

**instance variable** A piece of data that defines part of an object's representation. C++ uses the term data member.

**interaction diagram** A diagram that shows the flow of requests between objects.

**interface** The set of all signatures defined by an object's operations. The interface describes the set of requests to which an object can respond.

**metaclass** Classes are objects in Smalltalk. A metaclass is the class of a class object.

**mixin class** A class designed to be combined with other classes through inheritance. Mixin classes are usually abstract.

**object** A run-time entity that packages both data and the procedures that operate on that data.

**object composition** Assembling or composing objects to get more complex behavior.

**object diagram** A diagram that depicts a particular object structure at run-time.

**object reference** A value that identifies another object.

**operation** An object's data can be manipulated only by its operations. An object performs an operation when it receives a request. In C++, operations are called member functions. Smalltalk uses the term method.

**overriding** Redefining an operation (inherited from a parent class) in a subclass.

**parameterized type** A type that leaves some constituent types unspecified. The unspecified types are supplied as parameters at the point of use. In C++, parameterized types are called templates.

**parent class** The class from which another class inherits. Synonyms are superclass (Smalltalk), base class (C++), and ancestor class.

**polymorphism** The ability to substitute objects of matching interface for one another at run-time.

**private inheritance** In C++,a class inherited solely for its implementation.

**protocol** Extends the concept of an interface to include the allowable sequences of requests.

**receiver** The target object of a request.

**request** An object performs an operation when it receives a corresponding request from another object. A common synonym for request is message.

**signature** An operation's signature defines its name, parameters, and return value.

**subclass** A class that inherits from another class. In C++, a subclass is called a derived class.

**subsystem** An independent group of classes that collaborate to fulfill a set of responsibilities.

**subtype** A type is a subtype of another if its interface contains the interface of the other type.

**supertype** The parent type from which a type inherits.

**toolkit** A collection of classes that provides useful functionality but does not define the design of an application.

**type** The name of a particular interface.

**white-box reuse** A style of reuse based on class inheritance. A subclass reuses the interface and implementation of its parent class,but it may have access to otherwise private aspects of its parent.



### Rule

- SOLID
  - S - SRP - Single Responsibility Principle
  - O - OCP - Open-Closed Principle
  - L - LSP - Liskov Substitution Principle
  - I - ISP - Interface Segregation Principle
  - D - DIP - Dependency Inversion Principle
- KISS - Keep It Simple, Stupid
- YAGNI - You Aren’t Gonna Need It
- DRY - Don’t Repeat Yourself



windows

**Service start/stop/delete**

```
sc delete <service>
net stop <service>
net start <service>
```

**查看 port**

```shell
netstat -ano | findstr <port>
tasklist|findstr <PID>
```

**查看win10系统激活情况**

```
slmgr.vbs -xpr
```



### VSCode

**Settings**

```json
{
    "editor.fontSize": 12,
    "explorer.confirmDelete": false,
    "workbench.iconTheme": "vscode-icons",
    "explorer.confirmDragAndDrop": false,
    "editor.tabSize": 2,
    "files.associations": {
        "*.ejs": "html"
    },
    "open-in-browser.default": "chrome"
}
```



**快捷键**

- ctrl+k+0 //折叠代码
- ctrl+k+j //打开代码
- shift+alt+i //编辑多行行尾
- shift+alt+选中 //多行编辑



**plugin**

- Local History
- open in browser
- Prettier
- vscode-icons



### Typora

ctrl + 1 //标题1

ctrl + 2 //标题2

...

ctrl + 5 //标题5

ctrl + u //下划线

ctrl + b //加粗

ctrl + i //斜体

ctrl + k //超链接 [百度](http://www.baidu.com)

ctrl + t //表格

\> + 空格 //引用

ctrl + l //选中行

ctrl + d //选中词

alt + shift + 5 //删除线

ctrl + f //搜索

ctrl + h //搜索替换

ctrl + shift + i //插入图片

\- + 空格 //无序列表

1 + . //有序列表



### Eclipse

**快捷键**

- ctrl+alt+down //复制并粘贴一行代码
- ctrl+shift + /(数字小键盘) //折叠代码
- ctrl+shift + *(数字小键盘) //展开代码



**代码提示**

preferences -> java -> editor -> content assist -> auto activation triggers for java -> 输入abcdefghijklmnopqrstuvwxyz. 



### Hexo

```shell
npm install -g hexo-cli
hexo -v
hexo init
npm i
hexo server

hexo new a
hexo new draft b
hexo server --draft

hexo publish b

hexo new page c

default_layout

[tag1, tag2]

categories:
- [cat1, cat1.1]
- [cat2]
- [cat3]

ssh-keygen -t rsa -C "752492509@qq.com"
C:\Users\bxm09\.ssh\id_rsa.pub
https://github.com/settings/ssh => new SSH key
ssh -T git@github.com
git config --global user.name "jamie956"
git config --global user.email "752492509@qq.com"

npm install hexo-deployer-git --save

hexo clean
hexo g
hexo d

[Writing](https://hexo.io/docs/writing.html)
```



### Regular Express

| 字符   | 描述                                                         |
| :----- | ------------------------------------------------------------ |
| $      | 匹配输入字符串的结尾位置                                     |
| ()     | 标记一个子表达式的开始和结束位置                             |
| {}     | 标记限定符表达式的开始                                       |
| \w     | 匹配包括下划线的任何单词字符,等价于“[A-Za-z0-9_]”            |
| \d     | 匹配一个数字字符。等价于[0-9]                                |
| \s     | 匹配任何空白字符，包括空格、制表符、换页符等等。等价于[ \f\n\r\t\v] |
| \D     | 匹配一个非数字字符。等价于[^0-9]                             |
| \W     | 匹配任何非单词字符。等价于[A-Za-z0-9]                        |
| []     | 字符范围，匹配指定范围内的任意字符                           |
| [xyz]  | 字符集合。匹配所包含的任意一个字符                           |
| [^xyz] | 负值字符集合。匹配未包含的任意字符。                         |
| [a-z]  | 字符范围。匹配指定范围内的任意字符                           |
| .      | 匹配除“\n”之外的任何单个字符                                 |
| *      | 匹配前面的子表达式零次或多次                                 |
| +      | 匹配前面的子表达式一次或多次                                 |
| ?      | 贪婪模式则尽可能多的匹配所搜索的字符串                       |
| {n}    | n是一个非负整数。匹配确定的n次                               |
| {n,}   | 至少匹配n次                                                  |
| {n,m}  | m和n均为非负整数，其中n<=m。最少匹配n次且最多匹配m次         |

```js
let s1 = 'a1b23~!@'.match(/\d/);//1
let s2 = 'a1b23~!@'.match(/\d+/);//1
let s3 = 'a1b23~!@'.match(/\d+/g);//1,23
let s4 = 'sduabnjkbf'.match(/[ab]/g);//a,b,b
let s5 = 'a1b23~!@'.match(/[a-z]/g);//a,b
let s6 = 'a1b23~!@'.match(/[^a-z]/g);//1,2,3,~,!,@
let s7 = 'a1b23~!@'.match(/b/);//b
let s8 = 'a1b23~!@'.match(/[^a-z0-9]/g);//~,!,@
```