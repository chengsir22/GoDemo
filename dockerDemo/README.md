# 制作自己的镜像
拉取基础镜像 alpine或ubuntu ` docker pull alpine:3.16` or `ubuntu:20.04`

> 注意，本机为mac，编译程序需要设置平台 GOOS=linux GOARCH=amd64 go build -o client ./client

```dockerfile
# https://hub.docker.com/_/alpine
# 最新的linux镜像，只有2.68M，bash 都需要单独安装
FROM alpine:3.16
RUN apk --update add bash
RUN mkdir -p /data/code
COPY ./greeter_* /data/code/
WORKDIR /data/code/
EXPOSE 50051

CMD cd /data/code/ && ./greeter_server
```

docker build . --file=./Dockerfile --network=host --platform=linux/amd64 -t ccr.ccs.tencentyun.com/lucheng/hellogrpc:v1.0

## 启动容器
docker run --name mytest --network=host -it ccr.ccs.tencentyun.com/lucheng/hellogrpc:v1.0 /bin/bash

## 推送镜像到镜像仓库
docker login ccr.ccs.tencentyun.com --username=100027097973
docker push ccr.ccs.tencentyun.com/lucheng/hellogrpc:v1.0 

# 基操
查看镜像 docker images
删除镜像 docker rmi -f gin-blog-docker
创建网络 docker network create mynet