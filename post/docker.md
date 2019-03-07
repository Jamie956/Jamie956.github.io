```docker
docker -v //查看版本
docker info
```
### image

```docker
docker search <image>
docker pull <image:tag>
docker images
docker images -f dangling=true
docker images -a
docker rmi <image>
docker rmi $(docker images -q -f dangling=true)
docker rmi -f $(docker images -a -q)
docker tag <image> <new image>
docker run -it <image>
docker run -it -p <port>:<port> <image>
docker run -itd <image>
docker run -it --rm <image>
docker run -it --name <container_name> <image>
docker run -it -v </>:</> <image>
docker commit -a <username> <container> <username>/<repository>:<tag>
docker build -t <image> .
docker inspect <image>
docker commit -a jamie956 <container> <image>:<tag>
docker save -o <name>.tar <image>
docker load -i <name>.tar
```
Container

```docker
docker ps
docker ps -a
docker logs <container>
docker logs -f --tail  20  <container>
docker stop <container>
docker start <container>
docker start  $(docker ps -a)
docker restart <container>
docker rm <container>
docker rm -f <container>
docker rm $(docker ps -a -q)
docker exec -it <container> bash
docker cp <host> <container>:</>
```
### Docker hub
```docker
docker login
docker pull <username>/<repository>:<tag>
docker push <username>/<repository>:<tag>
```
### volume

```docker
docker volume create <name>
-v <volumename>:<path>
docker volume prune
docker volume rm <name>
```
### Restart

```docker
docker run --restart always <container>
docker run --restart on-failure:5 <container>
docker run --restart unless-stopped <container>
```



### Install Images

**Jenkins**

```docker
docker run \
--name myjenkins -u root --privileged=true \
-p 9090:8080 \
-v /var/run/docker.sock:/var/run/docker.sock \
-v $(which docker):/usr/bin/docker \
-v /var/run:/var/run:rw \
-v $PWD/data/jenkins:/var/jenkins_home \
-v /usr/lib/x86_64-linux-gnu/libltdl.so.7:/usr/lib/x86_64-linux-gnu/libltdl.so.7 \
-it <image>
```
**RabbitMQ**

```docker
docker run \
-d \
-p 15672:15672 \
-p  5672:5672 \
-e RABBITMQ_DEFAULT_USER=admin \
-e RABBITMQ_DEFAULT_PASS=admin \
--name myrabbitmq \
rabbitmq
```
**mongodb**

```docker
docker run \
--name mymongo \
-d \
-e MONGO_INITDB_ROOT_USERNAME=root \
-e MONGO_INITDB_ROOT_PASSWORD=123456 \
-e MONGO_INITDB_DATABASE=admin \
-p 27017:27017 \
-v ~/container/mongo:/data/db \
mongo \
--auth 

ls -alh|grep db

win10 => docker run -p 27017:27017 -d mongo
```
**mongo express**

```docker
docker run \
--name mymongo-express \
--link mymongo:mongo \
-p 8081:8081 \
-e ME_CONFIG_BASICAUTH_USERNAME="admin" \
-e ME_CONFIG_BASICAUTH_PASSWORD="admin" \
-e ME_CONFIG_MONGODB_ADMINUSERNAME="root" \
-e ME_CONFIG_MONGODB_ADMINPASSWORD="123456" \
--rm \
mongo-express
```
**mysql**

```docker
docker pull mysql:5.7
docker run -itd --name mysql -v /d/container/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 mysql:5.7
```
**adminer**

```docker
docker run -d --name adminer --link mysql-server:db -p 8085:8080 adminer
```
**phpmyadmin**

```docker
docker pull phpmyadmin/phpmyadmin
docker run -d --name phpmyadmin --link mysql:db -p 8084:80 phpmyadmin/phpmyadmin
```



### no sudo

```docker
sudo groupadd docker
sudo gpasswd -a jamie956 docker
newgrp docker
```





### Docker tomcat部署war包

#### 一，上传到tomcat容器

1. 启动tomcat容器

```shell
docker run -d --name tomcat -p 8081:8080 tomcat
```

1. 进入容器查看webapps的路径:/usr/local/tomcat/webapps

```shell
docker exec -it tomcat bash
```

1. 上传war到webapps路径下

```shell
docker cp demo.war tomcat:/usr/local/tomcat/webapps/demo.war
```

1. 到webapps路劲下检查是否上传成功
2. 重启tomcat

```shell
docker restart tomcat
```

#### 二，Dockerfile构建

1. 拉取镜像

```shell
docker pull tomcat
```

1. Dockerfile将war构建成一个新镜像

```dockerfile
FROM tomcat:9.0-slim
MAINTAINER "youremail <your@email.com>"
ADD demo.war /usr/local/tomcat/webapps/demo.war
CMD ["catalina.sh", "run"]
```

1. 构建

```shell
docker build -t demo .
```

1. 运行镜像

```shell
docker run -d --name demo -p 8081:8080 demo
```

**注意**

webapps里的war文件与访问路径要保持一致

**流程**

```makefile
pull-tomcat:
  docker pull tomcat:9.0-slim
clean-project:
  mvn clean
package-project:
  mvn package -Dmaven.test.skip=true
build-image:
  docker build -t demo .
run-container:
  docker run -d --name demo -p 8081:8080 demo
remove-container:
	docker stop demo
	docker rm -f demo
remove-image:
	docker rmi demo
clone-repo:
	git clone -b dev http://github/jamie/demo.git
pull-repo:
	git pull http://github/jamie/demo.git dev
```

