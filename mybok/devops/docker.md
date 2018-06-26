# Docker

### 容器重启机制
```docker
docker run --restart always <container>
docker run --restart on-failure:5 <container>
docker run --restart unless-stopped <container>
```