## Docker tomcat部署war包

### 方法一，上传到tomcat容器
1. 启动tomcat容器
```shell
docker run -d --name tomcat -p 8081:8080 tomcat
```

2. 进入容器查看webapps的路径:/usr/local/tomcat/webapps
```shell
docker exec -it tomcat bash
```

3. 上传war到webapps路径下
```shell
docker cp demo.war tomcat:/usr/local/tomcat/webapps/demo
```

4. 到webapps路劲下检查是否上传成功

5. 重启tomcat
```shell
docker restart tomcat
```

### 方法二，Dockerfile构建
1. 拉去镜像
```shell
docker pull tomcat
```

2. Dockerfile将war构建成一个新镜像
```dockerfile
FROM tomcat
MAINTAINER "youremail <your@email.com>"
ADD demo.war /usr/local/tomcat/webapps/
CMD ["catalina.sh", "run"]
```

3. 构建
```shell
docker build -t demo/tomcat:v1 .
```

4. 运行镜像
```shell
docker run -d --name demo -p 8081:8080  demo/tomcat:v1
```

