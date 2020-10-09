### docker 构建 php,nginx 运行环境

先构建php镜像
```
cd ~/docker-images/php-fpm7.1
docker build -t php-fpm71 .
```
构建nginx镜像
```
cd ~/docker-images/nginx1.19
docker build -t nginx .
```
创建容器
```
cd ~/docker-compose
# 记得去修改镜像名，以及挂载目录
docker-compose up -d
```
