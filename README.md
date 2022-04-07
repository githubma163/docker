1. docker基本概念

Docker是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中,然后发布到任何流行的Linux、Windows、Mac操作系统的机器上,也可以实现虚拟化,容器是完全使用沙箱机制,相互之间不会有任何接口。

Docker是C/S架构设计

一个完整的Docker有以下几个部分组成：
* Docker Client客户端
* Docker Daemon守护进程
* Docker Image镜像
* Docker Container容器
* Repository 仓库

![docker](https://www.runoob.com/wp-content/uploads/2016/04/576507-docker1.png)

![docker](https://tse1-mm.cn.bing.net/th/id/OIP-C.oRJCjQgHIYEOHETjOXmD5gHaEp?pid=ImgDet&rs=1)


2. docker与虚拟机的区别
* 虚拟机(VMware)在宿主机器、宿主机器操作系统的基础上创建虚拟层、虚拟化的操作系统、虚拟化的仓库，然后再安装应用
* Container(Docker容器)，在宿主机器、宿主机器操作系统上创建Docker引擎，在引擎的基础上再安装应用
* 虚拟机体积大，速度慢，docker体积小，速度快

![docker](https://mc.qcloudimg.com/static/img/6afde94bc4570cba2c9e27f0bdc81350/image.png)

3. docker安装

* 图形化安装https://www.docker.com/get-started/
* linux命令行安装
```
yum install epel-release –y
yum clean all
yum list
yum install docker-io –y
安装指定版本
wget https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-17.03.2.ce-1.el7.centos.x86_64.rpm
rpm –ivh docker-ce-17.03.2.ce-1.el7.centos.x86_64.rpm
systemctl start docker
docker version
systemctl start docker
```
* 安装完成后需要配置阿里云镜像地址,mac上操作参考https://blog.csdn.net/weixin_42085428/article/details/107809804 ，linux上的操作如下
```
vi /etc/docker/daemon.json
{
  "registry-mirrors": ["https://****.mirror.aliyuncs.com"]
}

systemctl restart docker
docker info
```

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

docker run
docker exec -it d245 /bin/bash

docker container ls
docker container ls -a
docker container rm test
docker container rm -f test

docker login
docker logout
```

![docker](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2F20191210150243559.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hlaWFuXzk5%2Csize_16%2Ccolor_FFFFFF%2Ct_70&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1651836245&t=42fcbed208a61b42f5e9509bdaecc764)

5. Dockerfile

每一个指令都会在镜像上创建一个新的层，每一个指令的前缀都必须是大写的。

Dockerfile 是一个用来构建镜像的文本文件，文本内容包含了一条条构建镜像所需的指令和说明。
```
FROM    centos:6.7
MAINTAINER      Fisher "fisher@sudops.com"

RUN     /bin/echo 'root:123456' |chpasswd
RUN     useradd runoob
RUN     /bin/echo 'runoob:123456' |chpasswd
RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
EXPOSE  22
EXPOSE  80
CMD     /usr/sbin/sshd -D
ENTRYPOINT 
```

6. docker技术原理
* Linux NameSpace Linux Namespace是Linux提供的一种内核级别环境隔离的方法
* Linux CGroup Linux CGroup全称Linux Control Group， 是Linux内核的一个功能，用来限制，控制与分离一个进程组群的资源（如CPU、内存、磁盘输入输出等）
* rootfs 在镜像中构造了一整套的操作系统根目录下的内容,这些会被挂载在容器根目录上, 为容器进程提供隔离后执行环境的文件系统,这个被称为根文件系统rootfs

![docker](https://pic4.zhimg.com/v2-d7ff605838017f4f2d16f5aaa179b213_r.jpg)


7. 常用docker镜像
```
docker run hello-world
```

```
docker run -d -p 9000:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --name portainer portainer/portainer
```

```
docker run -d -p 80:80 --name nginx nginx
```

```
docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -d mysql
```

```
docker run -itd --name redis -p 6379:6379 redis
```

```
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:6.3.2
```

```
docker run -it -d -e ELASTICSEARCH_URL=http://10.41.132.126:9200 -p 5601:5601 --name kibana docker.elastic.co/kibana/kibana:6.3.2
```

```
docker run -d --name hbase -h hbase -p 2181:2181 -p 16000:16000 -p 16010:16010 -p 16020:16020 harisekhon/hbase
docker exec -it hbase /bin/bash
hbase shell
```

8. 服务编排
* Docker公司Compose + Swarm

Docker Compose可以轻松、高效的管理容器，它是一个用于定义和运行多容器Docker的应用程序工具。通过Docker Compose，您可以使用 YML 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YML 文件配置中创建并启动所有服务。 https://www.runoob.com/docker/docker-compose.html?ivk_sa=1024320u

```
Docker Compose 使用的三个步骤
（1）使用 Dockerfile 定义应用程序的环境。
（2）使用 docker-compose.yml 定义构成应用程序的服务，这样它们可以在隔离环境中一起运行。
（3）最后，执行 docker-compose up 命令来启动并运行整个应用程序
```

docker compose 安装
```
curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
docker-compose version
```

```
version: '3'

services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.3.2
    environment:
      - cluster.name=docker-cluster
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    ports:
      - "9200:9200"
      - "9300:9300"
  kibana:
    image: docker.elastic.co/kibana/kibana:6.3.2
    ports:
      - "5601:5601"
```

* Google的Kubernetes

Kubernetes(k8s)是一个开源的，用于管理云平台中多个主机上的容器化的应用，Kubernetes的目标是让部署容器化的应用简单并且高效（powerful）,Kubernetes提供了应用部署，规划，更新，维护的一种机制。https://www.kubernetes.org.cn/k8s

Kubernetes 组件
Kubernetes Master组件
```
1.Master组件 Master 组件可以在集群中的任何节点上运行
包含3个进程，kube-apiserver、kube-controller-manager 和 kube-scheduler。
2.API服务器 kube-apiserver对外暴露了Kubernetes API。它是的 Kubernetes 前端控制层
3.etcd etcd 用于 Kubernetes 的后端存储
4.kube-controller-manager kube-controller-manager运行控制器，它们是处理集群中常规任务的后台线程
（1）节点控制器
（2）副本控制器
（3）端点控制器
（4）服务账户和令牌服务器
5.云控制器管理器-(cloud-controller-manager) 云控制器管理器是用于与底层云提供商交互的控制器。云控制器管理器二进制是 Kubernetes v1.6 版本中引入的 Alpha 功能。
（1）节点控制器
（2）路由控制器
（3）服务控制器
（4）数据卷控制器
6.调度器 kube-scheduler
（1）kube-scheduler监视没有分配节点的新创建的 Pod，选择一个节点供他们运行
7.插件(addons)
插件是实现集群功能的 Pod 和 Service
8.DNS
但所有 Kubernetes 集群都应该具有Cluster DNS
9.Web UI (Dashboard)
10.容器资源监控
11.集群层面日志
```

Kubernetes Node组件
```
1.kubelet
kubelet是主要的节点代理,它监测已分配给其节点的 Pod(通过 apiserver 或通过本地配置文件)
2.kube-proxy
3.docker
4.rkt
5.supervisord
supervisord 是一个轻量级的进程监控系统，可以用来保证 kubelet 和 docker 运行
6.fluentd
fluentd 是一个守护进程，它有助于提供集群层面日志 集群层面的日志。
```

Kubernetes 对象
```
1.基本对象
(1)Pod
(2)Service
(3)Volume
(4)Namespace
2.高层对象
(1)ReplicaSet
(2)Deployment
(3)StatefulSet
(4)DaemonSet
(5)Job
```

Kubernetes 集群管理 https://docs.rancher.cn/

* mesosphere的Mesos + Marathon

9. docker应用
* 本地快速搭建软件或开发环境

nodejs环境，创建如下3个文件，分别是server.js,package.json,Dockerfile
```
'use strict';

const express = require('express');

// Constants
const PORT = 8080;
const HOST = '0.0.0.0';

// App
const app = express();
app.get('/', (req, res) => {
  res.send('Hello world\n');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);
```

```
{
  "name": "docker_web_app",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "First Last <first.last@example.com>",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.16.1"
  }
}

```

```
FROM node:10

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . .

EXPOSE 8080
CMD [ "node", "server.js" ]
```

```
执行如下命令
docker build -t ma/node-web-app .
docker run -p 8080:8080 -d ma/node-web-app
curl http://localhost:8080
```

* 应用自动化部署，持续集成（java web）
```
springboot docker部署
1.添加docker构建plugin
<build>
    <finalName>hello</finalName>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <version>2.0.5.RELEASE</version>
        </plugin>
        <plugin>
            <groupId>com.spotify</groupId>
            <artifactId>docker-maven-plugin</artifactId>
            <version>1.0.0</version>
            <configuration>
                <imageName>springboot/${project.artifactId}</imageName>
                <dockerDirectory>src/main/docker</dockerDirectory>
                <resources>
                    <resource>
                        <targetPath>/</targetPath>
                        <directory>${project.build.directory}</directory>
                        <include>${project.build.finalName}.jar</include>
                    </resource>
                </resources>
            </configuration>
        </plugin>
    </plugins>
</build>
2.创建src/main/docker目录，添加Dockerfile文件，文件内容如下
FROM java:8
ADD hello.jar .
RUN ls -a
EXPOSE 8080
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","./hello.jar"]
3.打包
mvn package docker:build
docker images
docker run -itd --name hello -p 8088:8080 springboot/hello
```

* AI开发（ai环境搭建复杂，依赖多，不同版本的差异大，如tensorflow，pytorch）
```
RUN pip install numpy==1.14.5
RUN pip install mxnet-cu90==1.4.1
RUN pip install tensorflow-gpu==1.12.0
RUN pip install easydict==1.7
RUN pip install opencv-python==3.4.0.12
RUN pip install -I Pillow==6.0.0
RUN pip install torchvision==0.2.1
RUN pip install -I torch==0.4.1

RUN pip install configparser
RUN pip install requests
RUN pip install Flask
RUN pip install flask-cors
```

