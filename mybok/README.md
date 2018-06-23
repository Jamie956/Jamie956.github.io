没有什么好介绍的

## Unzip file
* tar -xjvf yourdownload.tar.bz2

## linux shortcut
* ctrl+alt+t //call terminal
* ctrl+shift+t //new terminal tab

## Git ssh
* ssh-keygen -t rsa -C "your@eamil.com" //generate ssh key
* cat ~/.ssh/id_rsa.pub //show ssh key
* add the key to git setting
* ssh -T git@github.com //testing
* git config --global user.name "yournamee"
* git config --global user.email "your@eamil.com"

## Set node path
* sudo nano /etc/profile
* export NODE_HOME=/home/jamie/node-v8.11.3-linux-x64
* export PATH=$NODE_HOME/bin:$PATH
* source /etc/profile
* echo $PATH

## vscode 
* shift+alt+i //编辑多行行尾
* shift+alt+选中 //多行编辑
* format ejs 
    ```json
        "files.associations": {
            "*.ejs": "html"
        }
    ```

## set maven path
* sudo nano /etc/profile
* export MAVEN_HOME=/home/jamie/apache-maven-3.5.3
* export PATH=$MAVEN_HOME/bin:$PATH
* source /etc/profile
* echo $PATH

## Error
#### Q:Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=gasp
* nano ~/.profile
* unset _JAVA_OPTIONS
* source ~/.profile
or
sudo nano ~/.bashrc
unset _JAVA_OPTIONS

## apt install jdk
* sudo apt install openjdk-8-jdk -y
* sudo nano /etc/profile
* export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
* export PATH=$JAVA_HOME/bin:$PATH
* source /etc/profile
* echo $PATH

## port
* netstat -an //show all
* netstat -tunlp |grep 8082 //find port 8082
* kill -9 3383 //kill prosess 3383

## npm version
major: 主版本号
premajor: 预备主版本
minor: 次版本号
preminor: 预备次版本
patch: 修订号
prepatch: 预备修订版
prerelease: 预发布版本

version 0.1.0
* npm version preminor //v0.1.0-0
* npm version minor //v0.1.0
* npm version prepatch //v0.1.1-0
* npm version patch //v0.1.1
* npm version prerelease //v0.1.2-0
* npm version premajor //v1.0.0-0
* npm version major //v1.0.0

## ubuntu
### 搜狗输入法选词框乱码
* cd ~/.config
* sudo rm -rf SogouPY* sogou*

## .sh
* touch hello.sh //创建sh文件
* chmod u+x hello.sh //可执行权限
* ./hello.sh //excute sh
* sh hello.sh //excute sh
* . hello.sh //execute sh, !important use for "export" command

## download
* wget url
    * -b：后台下载，Wget默认的是把文件下载到当前目录
    * -O：将文件下载到指定的目录中
    * -P：保存文件之前先创建指定名称的目录
    * -t：尝试连接次数，当Wget无法与服务器建立连接时，尝试连接多少次
    * -c：断点续传，如果下载中断，那么连接恢复时会从上次断点开始下载
    * -r：使用递归下载
* wget -b -P <output path> <url>

## eclipse
* ctrl+alt+down //copy line

## need install //look .sh
* vscode
* git gitkraken
* sts
* sublime text 3
* firefox
* chrome //Oops
* sougou pinyin
* git
* nvm & nodejs
* java
* maven
* docker

## nano
* ctrl + k //delete line
 
## groups
* groups //show all groups
* groupadd –g 888 users //create a groups users, GID 888
* gpasswd –a user1 users //add user1 to groups users
* gpasswd –d user1 users //remove user1 from groups users
* sudo groupdel users //delete groups users