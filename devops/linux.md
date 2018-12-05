## Set ENV

```shell
sudo nano /etc/profile
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
source /etc/profile
echo $PATH
```



## Shell脚本

```shell
touch hello.sh //创建sh文件
chmod u+x hello.sh //设置可执行权限
./hello.sh //执行 .sh
sh hello.sh //excute sh
. hello.sh //execute sh, !important use for "export" command
```



## wget

```
wget url
    -b：后台下载，Wget默认的是把文件下载到当前目录
    -O：将文件下载到指定的目录中
    -P：保存文件之前先创建指定名称的目录
    -t：尝试连接次数，当Wget无法与服务器建立连接时，尝试连接多少次
    -c：断点续传，如果下载中断，那么连接恢复时会从上次断点开始下载
    -r：使用递归下载
wget -b -P <output path> <url>
```

## nano

```
nano <file> //编辑文件
ctrl + k //删除一行
Ctrl + a //光标到行首
Ctrl + e //光标到行尾
```

## Groups

```
groups //show all groups
groupadd –g 888 users //create a groups users, GID 888
gpasswd –a user1 users //add user1 to groups users
gpasswd –d user1 users //remove user1 from groups users
sudo groupdel users //delete groups users
```

## ls

```shell
ls //list 列出目录内容
ls <path> //指定目录
ls -l //详情
ls -lt //时间排序
ls -ltr //倒序
ls -a //全部，包括隐藏文件
ls | sort //字母排序
ls -lS //大小排序

```

## cd

```shell
cd //到home目录
cd - //到先前目录
cd <path> //到指定目录
cd .. //上一级目录
```

## cp

```shell
cp <filepath> . //复制文件到当前目录
cp file1 file2 // file1 -> file2，file2存在就重写，否则就创建
cp file1 file2 dir1 //file1,file2 -> dir1
cp dir1/* dir2 //复制dir1下的文件
cp -r dir1 dir2 //递归复制
```

## mv

```shell
mv <item1> <item2> //重命名 或移动目录
mv <item...> <directory>
```

## rm

```shell
rm <item...> //删除文件或目录
rm -r <item> //递归删除
rm -f <item> //强制删除
rm -i file1 //显示确认信息
```

## others

```
clear //清空终端
exit //退出终端
history //查看执行过的命令
!<num> //执行指定历史命令
NAME=tom //赋值
echo $NAME //取值
ssh -i ~/Documents/your.pem ubuntu@<ip> //ssh连接服务器
scp -i your.pem src ubuntu@<ip>:target //上传单个文件
tar -xjvf yourdownload.tar.bz2 //解压文件
date //日期时间
cal //日历
df //磁盘剩余容量
df -h //人看的
free //内存
pwd //当前目录
mkdir <directory...> //创建文件夹
touch <filename> //创建文件 
locate <filename> //搜索文件
file <filename> //查看文件描述
less <filename> //浏览文本，q退出，/查找
printenv //查看环境变量
```

## command

```shell
type <command> //显示命令类型
which <command> //显示会执行哪个可执行程序
help <command> //
<command> --help //
man <command> //显示命令手册页
alias foo='cd /usr; ls; cd -' //创建命令别名
unalias foo //移除别名
```

## redirection

```shell
ls -l /usr/bin > ls-output.txt //运行结果输送到文件 ls-output.txt
> ls-output.txt //清空文件内容
ls -l /usr/bin >> ls-output.txt //追加内容
ls -l /bin/usr &> ls-output.txt //重定向标准输出和错误到文件 ls-output.txt
```

## cat

```shell
cat <file> //显示文件内容
cat <file1> > <file2> //复制文件file1的内容到file2
cat <file> >> <file2>
```

## pipelines

```shell
command1 | command2 //一个命令的标准输出可以通过管道送至另一个命令的标准输入
ls -l /usr/bin | less
ls /bin /usr/bin | sort | less
```

## filter

```shell
sort //排序
uniq //省略重复行
uniq -d //显示重复行
wc <file> //打印文件中换行符，字，和字节个数
wc -l //输出行数
grep pattern [file...] //打印匹配行
grep -c pattern [file...] //详情看help
head -n <x> <file> //输出指定文件前x行内容
tail -n <x> <file> //输出指定文件后x行内容
ls /usr/bin | tail -n 5
tail -f /var/log/messages //监测并输出文件内容
ls /usr/bin | tee ls.txt | grep zip //从标准输入读取数据，同时写到标准输出和文件
```

## 权限

```shell
chmod -R 777 PWD //设置权限
sudo adduser <name>
sudo du //取得root权限
visudo //编辑权限文件
r //可读
w //可写
x //可执行
```

## 进程

```shell
ps x //查看全部进程（快照）
ps aux

top //显示进程（动态）

xlogo & // &后台运行进程

jobs – 列出活跃的任务
bg – 把一个任务放到后台执行
fg – 把一个任务放到前台执行

kill 28401 //杀死指定程序

killall – 杀死指定名字的进程
shutdown – 关机或重启系统


# 关闭占用端口的进程
netstat -an //查看全部端口
netstat -tunlp | grep 8082 //查找8082端口的进程ID
kill -9 3383 //终止进程ID3382

```

当系统启动的时候，内核先把一些它自己的活动初始化为进程，然后运行一个叫做 init 的程序。init，
依次地，再运行一系列的称为 init 脚本的 shell 脚本（位于/etc），它们可以启动所有的系统服务。
其中许多系统服务以守护（daemon）程序的形式实现，守护程序仅在后台运行，没有任何用户接口(User Interface)。
这样，即使我们没有登录系统，至少系统也在忙于执行一些例行事务。





## Apt Install

### 加速

```shell
cp /etc/apt/sources.list /etc/apt/sources.list.bak	# 备份sources.list
echo ""> sources.list # 写入源
apt-get update
```



### 安装步骤

```
apt-get update
apt install <app>
```



### 你可以装

docker
nginx
curl
maven
openssh-server: connect
openjdk-8-jdk
make
gparted //磁盘管理工具



## Makefile

```
touch hello
hello:
	say:
		echo 'Hello World.'
	say2:
		echo 'hello world2'
make -f hello say2

===error===
q:Makefile:4: *** missing separator.  Stop.
a:space -> tab
```



## Ubuntu

- shortcut

```
ctrl+alt+t //打开terminal
ctrl+shift+t //新建一个terminal tab
ctrl+h //显示隐藏文件
```



- 搜狗乱码

```shell
cd ~/.config
sudo rm -rf SogouPYsogou*
# 重启系统
```



- system program problem detected

```
sudo rm -rf /var/crash/*
```



- /boot 空间不足

```shell
df -h /boot // 查看 /boot
dpkg --get-selections |grep linux-image // 查看系统安装的内核镜像
uname -r //查看本机系统的内核版本
sudo apt-get remove [image] [image] ... //卸载
sudo dpkg -P [image] [image] ... //删除相应的配置信息
```



- Could not get lock /var/lib/dpkg/lock - open (11: Resource temporarily unavailable)

```sudo rm /var/cache/apt/archives/lock```
```sudo rm /var/lib/dpkg/lock```



## 双系统

```
win +x => 磁盘管理 => 压缩卷 => 51200M  =>未分配
禁用快速启动 //"选择电源按钮的功能" ->"更改当前不可用的设置" ->取消选择"启用快速启动"
UltraISO写入iso到U盘
备份win10
重启F12或del进入BIOS
如果Boot Mode是UEFI，把Secure Boot 设置Disable
如果Boot Mode是Legacy 就跳过
如果不能将Secure Boot 设置成Disable，去Security设置一下 Supervisor password
设置USB HDD启动为首选启动项
插入U盘安装Ubuntu
设置挂载点
	如果UEFI启动要设置EFI引导：500M 逻辑分区 空间起始位置 EFI系统分区
	如果Legacy启动要设置boot引导: /boot 200M 主/逻辑分区 空间起始位置 Ext4日志文件系统
	/swap 8192M 主分区 空间起始位置 交换空间
	/ 10000M 逻辑分区 空间起始位置 Ext4日志文件系统
	/home 20000M 逻辑分区 空间起始位置 Ext4日志文件系统
	/usr 10000M/剩余 逻辑分区 空间起始位置 Ext4日志文件系统
安装启动引导器的设备 选择/boot或者efi对应的sda号
如果是UEFI，BIOS => Security => Select an UEFI file as trusted for executing => EFI => Ubuntu => boot => grub
EasyBCD引导Ubuntu：添加新条目 => Linux/BSD操作系统 => 驱动器 => ~200M的Linux分区
```



## 系统目录

| 目录           | 描述                                        |
| -------------- | ------------------------------------------- |
| /              | 根目录                                      |
| /bin           | 系统启动和运行的二进制程序                  |
| /boot          | Linux内核， 初始 RAM 磁盘映像，启动加载程序 |
| /dev           | 包含设备结点的特殊目录                      |
| /etc           | 包含所有系统层面的配置文件                  |
| /home          | 给每个用户分配的一个目录                    |
| /lib           | 核心系统程序所使用的共享库文件              |
| /lost+found    | 每个使用 Linux 文件系统的格式化分区或设备   |
| /media         | 包含可移动介质的挂载点                      |
| /mnt           | 包含可移动介质的挂载点                      |
| /opt           | 安装“可选的”软件                            |
| /proc          | 由 Linux 内核维护的虚拟文件系统             |
| /root          | root 帐户的home                             |
| /sbin          | 完成重大系统任务的程序                      |
| /tmp           | 储存临时文件                                |
| /usr           | 包含普通用户所需要的所有程序和文件          |
| /usr/bin       | 系统安装的可执行程序                        |
| /usr/lib       | 由/usr/bin 目录中的程序所用的共享库         |
| /usr/local     |                                             |
| /usr/sbin      | 系统管理程序                                |
| /usr/share     | 由/usr/bin 目录中的程序使用的共享数据       |
| /usr/share/doc |                                             |
| /var           | 存放动态文件                                |
| /var/log       |                                             |





## 匹配

| 通配符        | 意义                               |
| ------------- | ---------------------------------- |
| *             | 匹配任意多个字符（包括零个或一个） |
| ?             | 匹配任意一个字符（不包括零个）     |
| [characters]  | 匹配任意一个属于字符集中的字符     |
| [!characters] | 匹配任意一个不是字符集中的字符     |
| [[:class:]]   | 匹配任意一个属于指定字符类中的字符 |



| 字符类    | 意义                   |
| --------- | ---------------------- |
| [:alnum:] | 匹配任意一个字母或数字 |
| [:alpha:] | 匹配任意一个字母       |
| [:digit:] | 匹配任意一个数字       |
| [:lower:] | 匹配任意一个小写字母   |
| [:upper:] | 匹配任意一个大写字母   |



| 模式                         | 匹配对象                                                  |
| ---------------------------- | --------------------------------------------------------- |
| *                            | 所有文件                                                  |
| g*                           | 文件名以“g”开头的文件                                     |
| b*.txt                       | 以"b"开头，中间有零个或任意多个字符，并以".txt"结尾的文件 |
| Data???                      | 以“Data”开头，其后紧接着3个字符的文件                     |
| [abc]*                       | 文件名以"a","b",或"c"开头的文件                           |
| BACKUP.\[0-9\]\[0-9\]\[0-9\] | 以"BACKUP."开头，并紧接着3个数字的文件                    |
| [[:upper:]]*                 | 以大写字母开头的文件                                      |
| [![:digit:]]*                | 不以数字开头的文件                                        |
| *[[:lower:]123]              | 文件名以小写字母结尾，或以 “1”，“2”，或 “3” 结尾的文件    |



## Ubuntu ENV

1. `sudo apt install git -y`
2. `git clone https://github.com/Jamie956/awe-shell.git `
3. `sudo apt-get update`
4. `sudo apt install make -y` 

























