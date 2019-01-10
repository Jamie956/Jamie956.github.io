**Commands**

```shell
#Restart OS
reboot
#Shut down
poweroff

########wget########
#Rename
wget -O wordpress.zip URL
#Limit rate
wget --limit-rate=300k URL
#Continue from the point at last
wget -c URL
#Running in background
wget -b URL
#Check wget process
tail -f wget-log
#Setting user agent, if it's refuse to download
wget --user-agent="Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.204 Safari/534.16" URL
#Testing whether work or not
wget --spider URL
#Just retry
wget --tries=40 URL
#Download Files by a list URL
wget -i filelist.txt

ps

top

#Port ID of Process
pidof chrome
#List sign
kill -l
#List all process and then find it!
ps -ef | grep vim

ifconfig

########cd########
#User dir
cd
cd ~
#Last
cd -
#Back
cd ..

########ls########
#List files
ls
#List files under the path
ls <path>
#List files detail
ls -l
#List files by order
ls -lt
#List files by order reverse
ls -ltr
#List all
ls -a
#List files by letter
ls | sort
#List files by size
ls -lS

########cat########
#Print File
cat <File>
#Rewrite File
cat <File1> > <File2>
#Copy to File2 from File1
cat <File1> >> <File2>

#Print head of lines
head -n <Num> <File>

#Print tail of lines
tail -n <Num> <File>
#Observe File and Print
tail -f <File>
#Print from line 20 to the end
tail +20 file

########tr########
#To low case
echo "HELLO WORLD" | tr 'A-Z' 'a-z'
#Delete
echo "hello 123 world 456" | tr -d '0-9'
#Squeeze Repeats
echo "thissss is      a text linnnnnnne." | tr -s ' sn'

########wc########
#Print File lines,words,bytes
wc <file>
#Print bytes or chars
wc -c <File>
#Print lines
wc -l <File>
#Print words
wc -w <File>

#File detail
stat <File>

#Differ File
diff <File1> <File2>

########cp########
cp <filepath> . //复制文件到当前目录
cp file1 file2 // file1 -> file2，file2存在就重写，否则就创建
cp file1 file2 dir1 //file1,file2 -> dir1
cp dir1/* dir2 //复制dir1下的文件
cp -r dir1 dir2 //递归复制

########others########
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
#Create Directory
mkdir <directory...>
#Create File
touch <File>
locate <filename> //搜索文件
file <filename> //查看文件描述
less <filename> //浏览文本，q退出，/查找
printenv //查看环境变量

type <command> //显示命令类型
which <command> //显示会执行哪个可执行程序
help <command>
<command> --help //
man <command> //显示命令手册页
alias foo='cd /usr; ls; cd -' //创建命令别名
unalias foo //移除别名

########mv########
mv <item1> <item2> //重命名 或移动目录
mv <item...> <directory>

########rm########
rm <item...> //删除文件或目录
rm -r <item> //递归删除
rm -f <item> //强制删除
rm -i file1 //显示确认信息

########tar########
#压　缩
tar -jcv -f filename.tar.bz2
#查　询
tar -jtv -f filename.tar.bz2
#解压缩
tar -jxv -f filename.tar.bz2 -C

########grep########
grep pattern [file...] //打印匹配行
grep -c pattern [file...] //lines

########find########
find /home -name "*.txt"
find . -regex ".*\(\.txt\|\.pdf\)$"
#搜索大于10KB的文件
find . -type f -size +10k
```



## Set Path

```shell
sudo nano /etc/profile
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
source /etc/profile
echo $PATH
```



## Shell

```shell
touch hello.sh //创建sh文件
chmod u+x hello.sh //设置可执行权限
./hello.sh //执行 .sh
sh hello.sh //excute sh
. hello.sh //execute sh
```



## Groups

```
groups //show all groups
groupadd –g 888 users //create a groups users, GID 888
gpasswd –a user1 users //add user1 to groups users
gpasswd –d user1 users //remove user1 from groups users
sudo groupdel users //delete groups users
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



## Apt

```shell
# Update source.list 
cp /etc/apt/sources.list /etc/apt/sources.list.bak
echo ""> sources.list
apt-get update

# install packages
apt-get update
apt install <package>

sudo apt-get remove package #删除包
sudo apt-get remove package --purge #删除包，包括删除配置文件等

apt list <package> # 是否安装某个安装包
```



## Makefile

```shell
make <key>
make -f <filename> <key>

# example
touch Makefile
# Makefile
print:
	echo 'hi'
make print

# error
Makefile:4: *** missing separator.  Stop.
space -> tab
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



## Ubuntu & win10

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

1. `touch hi.sh`

2. ```shell
   sudo apt-get update
   sudo apt install git -y
   sudo apt install make -y
   git clone https://github.com/Jamie956/awe-shell.git
   ```

3. `. hi.sh`

4. Install packages as you need.

