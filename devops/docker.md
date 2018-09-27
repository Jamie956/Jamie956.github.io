# 命令
## Base
```docker
docker -v //查看版本
docker info
```
## image相关命令
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
## Container相关命令
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
## Docker hub 拉取和提交
```docker
docker login
docker pull <username>/<repository>:<tag>
docker push <username>/<repository>:<tag>
```
## volume相关命令
```docker
docker volume create <name>
-v <volumename>:<path>
docker volume prune
docker volume rm <name>
```
## 容器重启
```docker
docker run --restart always <container>
docker run --restart on-failure:5 <container>
docker run --restart unless-stopped <container>
```

# Install Images
## Jenkins
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
## RabbitMQ
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
## mongodb
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
## mongo express
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
## mysql
```docker
docker pull mysql:5.7
docker run -itd --name mysql -v /d/container/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 mysql:5.7
```
## adminer
```docker
docker run -d --name adminer --link mysql-server:db -p 8085:8080 adminer
```
## phpmyadmin
```docker
docker pull phpmyadmin/phpmyadmin
docker run -d --name phpmyadmin --link mysql:db -p 8084:80 phpmyadmin/phpmyadmin
```

# Othrt
## docker命令不需要再使用sudo啦
```docker
sudo groupadd docker
sudo gpasswd -a jamie956 docker
newgrp docker
```