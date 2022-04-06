1. docker基本概念

Docker是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中,然后发布到任何流行的Linux、Windows、Mac操作系统的机器上,也可以实现虚拟化,容器是完全使用沙箱机制,相互之间不会有任何接口。
一个完整的Docker有以下几个部分组成：
* DockerClient客户端
* Docker Daemon守护进程
* Docker Image镜像
* DockerContainer容器

2. docker与虚拟机的区别
* 虚拟机(VMware)在宿主机器、宿主机器操作系统的基础上创建虚拟层、虚拟化的操作系统、虚拟化的仓库，然后再安装应用
* Container(Docker容器)，在宿主机器、宿主机器操作系统上创建Docker引擎，在引擎的基础上再安装应用
* 虚拟机体积大，速度慢，docker体积小，速度快

3. docker安装
https://www.docker.com/get-started/

4. docker常用命令
```
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
```

![docker](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2F20191210150243559.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hlaWFuXzk5%2Csize_16%2Ccolor_FFFFFF%2Ct_70&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1651836245&t=42fcbed208a61b42f5e9509bdaecc764)


5. docker技术原理
* Linux NameSpace Linux Namespace是Linux提供的一种内核级别环境隔离的方法
* Linux CGroup Linux CGroup全称Linux Control Group， 是Linux内核的一个功能，用来限制，控制与分离一个进程组群的资源（如CPU、内存、磁盘输入输出等）
* rootfs 在镜像中构造了一整套的操作系统根目录下的内容,这些会被挂载在容器根目录上, 为容器进程提供隔离后执行环境的文件系统,这个被称为根文件系统rootfs


6. 常用docker镜像
```
docker run hello-world
```

```
docker run -d -p 9000:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --name portainer portainer/portainer
```

```
```

7. 服务编排
* Docker公司Compose + Swarm
* Google + RedHat的Kubernetes
* mesosphere的Mesos + Marathon


