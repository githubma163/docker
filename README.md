1. docker基本概念


2. docker安装
https://www.docker.com/get-started/

3. docker常用命令
docker version
docker pull
docker search dockerui

docker images
docker images -aq
删除全部镜像
docker rmi -f $(docker images -aq)

docker ps
docker ps -a
docker rm 5c91
docker rmi 5c91

docker exec -it d245 /bin/bash

docker container ls
docker container ls -a
docker container rm test
docker container rm -f test

docker login
docker logout



4. docker技术原理
Linux NameSpace Linux Namespace是Linux提供的一种内核级别环境隔离的方法
Linux CGroup Linux CGroup全称Linux Control Group， 是Linux内核的一个功能，用来限制，控制与分离一个进程组群的资源（如CPU、内存、磁盘输入输出等）
rootfs 在镜像中构造了一整套的操作系统根目录下的内容,这些会被挂载在容器根目录上, 为容器进程提供隔离后执行环境的文件系统,这个被称为根文件系统rootfs


6.常用docker镜像

6.服务编排


