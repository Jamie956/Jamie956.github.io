### 解压文件
```
tar -xjvf yourdownload.tar.bz2
```

### Set path
```
//nodejs
sudo nano /etc/profile
export NODE_HOME=/home/jamie/node-v8.11.3-linux-x64
export PATH=$NODE_HOME/bin:$PATH
source /etc/profile
echo $PATH

//maven
sudo nano /etc/profile
export MAVEN_HOME=/home/jamie/apache-maven-3.5.3
export PATH=$MAVEN_HOME/bin:$PATH
source /etc/profile
echo $PATH

//jdk
sudo nano /etc/profile
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
source /etc/profile
echo $PATH
```

### 关闭占用端口的程序
```
netstat -an //查看全部端口
netstat -tunlp |grep 8082 //查找8082端口的进程ID
kill -9 3383 //终止进程ID3382
```

### 搜狗输入法选词框乱码
```
cd ~/.config
sudo rm -rf SogouPYsogou*
```
重启系统

### 执行shell文件
```
touch hello.sh //创建sh文件
chmod u+x hello.sh //设置可执行权限
./hello.sh //执行 .sh
sh hello.sh //excute sh
. hello.sh //execute sh, !important use for "export" command
```

### wget命令下载
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

### nano
```
ctrl + k //删除一行
Ctrl + a //光标到行首
Ctrl + e //光标到行尾
```

### Groups
```
groups //show all groups
groupadd –g 888 users //create a groups users, GID 888
gpasswd –a user1 users //add user1 to groups users
gpasswd –d user1 users //remove user1 from groups users
sudo groupdel users //delete groups users
```

#### Command
```
===path===
pwd //当前位置
ls //list
ls <dir>
ls -l
ls -lt
ls -a //list all
ls | sort //排序

cd //到路径/home/user
cd - //到上一个路径
cd <path> //到某个路径下
cd .. //上一级目录

===others===
clear //清空terminal
exit //退出terminal
history //查看执行过的命令
!<num> //执行制定历史命令

mynameis=tom //赋值
echo $mynameis //取值

sudo du //取得root权限
visudo //编辑权限文件

chmod -R 777 PWD //设置权限

===dir===
mkdir dir1 dir2 dir3 //创建文件夹
rm -rf <dir> (or rm -r <dir>) //强制删除文件夹
mv <dir> <dir2> //文件夹重命名
file [file/dir] //查看文件或文件夹属性
mv <dir> <dir2> //移动文件夹

===file===
touch <filename> //创建文件 
rm <file> ( or rm -f <file> ) //删除文件
nano <file> //编辑文件
cat <file> //查看文件
cp <filepath> . //复制文件到当前目录
mv <file> <path> //移动文件到制定路径下
mv <old> <new> //文件重命名
locate <filename> //搜索文件
cat <file_a> > <file_b> //复制文件file_a的内容到file_b
cat <file> >> <file2>
wc <file> //获取文件行数,字数,字节数,文件名称
head -n <x> <file> //获取文件前x行内容
tail -n <x> <file> //获取文件最后x行内容
grep <keyword> <file> //模糊查找制定文件
grep -c <keyword> <file> //计算指定文件的关键字个数
ls | grep <keyword> //当前目录模糊查找

===command===
type <command> //查看执行命令所在路径

===user===
sudo adduser <name>

```

#### Install
```
===fast===
cp /etc/apt/sources.list /etc/apt/sources.list.backup
echo ""> sources.list
apt-get update

===install===
apt-get update
apt install <app>

===app===
docker
docker-compose
nginx
curl
maven
openssh-server: connect
openjdk-8-jdk
make
gparted //磁盘管理工具
```

#### Error
```
Q: Could not get lock /var/lib/dpkg/lock - open (11: Resource temporarily unavailable)
A: sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock

Q: Error writing docker: Permission denied
A: sudo nano <file>

Failed to fetch cdrom://Ubuntu-Server 16.04.3 LTS _Xenial Xerus_ - Release amd64 (20170801)/dists/xenial/main/binary-amd64/Packages
nano /etc/apt/sources.list
comment or remove lines that include cdrom => deb cdrom:[Ubuntu-Server 16.04 LTS _Xenial Xerus_ - Release amd64 (20160420.3)]/ xenial main restricted
```

#### Makefile
```
touch hello
hello:
	say:
		echo 'Hello World.'
	say2:
		echo 'hello world2'
make -f hello say2
```

### Shortcut
```
ctrl+alt+t //打开terminal
ctrl+shift+t //新建一个terminal tab
```

### 
```
ssh -i ~/Documents/your.pem ubuntu@<ip> //ssh连接服务器
scp -i your.pem src ubuntu@<ip>:target //上传单个文件
df -h //查看磁盘

```

### win10 ubuntu 双系统
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
