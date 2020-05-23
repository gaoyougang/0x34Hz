---
title: docker学习笔记
date: 2020-05-16 22:59:04
author:
img:
top:
cover:
coverImg:
password:
toc:
mathjax:
summary:
categories: 技术
tags: docker
reprintPolicy:
---
# docker学习笔记

### 简介

**虚拟化**：是一种资源管理技术，是将计算机的各种实体资源，如服务器、网络、内存及存储等，予以抽象、转换后呈现出来，打破实体结构间的不可切割的障碍，使用户可以比原来的组态更好的方式来应用这些资源。

**docker**：实现轻量级的操作系统虚拟化解决方案，其基础是linux容器（LXC）等技术，用于搭建开发测试环境。

![docker](https://i1.wp.com/blog.docker.com/wp-content/uploads/illus5-colorful-square.jpg?fit=1000%2C1000&ssl=1)

### docker安装与启动（环境：centos 7）

1. 更新源：`sudo yum update`

2. 配置yum源为阿里云：`sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo`

3. 安装docker：`sudo yum install docker-ce`

4. 安装成功查看版本信息：`docker -v`

5. docker设置国内镜像，这里选择ustc：`vim /etc/docker/daemon.json`

   添加如下：

   ```
   {
   "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
   }
   ```

6. docker的启动与停止：

   ```
   #启动 systemctl start docker
   #停止 systemctl stop docker容器
   #重启 systemctl restart docker
   #状态 systemctl status docker
   #开机自动启动 systemctl enable docker
   ```

### 常用命令

#### 镜像相关命令

1. 查看本地镜像：`docker images`

2. 搜索网络镜像：`docker search 镜像名称`（如：nginx）

3. 拉取镜像(从网上拉到本地)：`docker pull 镜像名称`（如： docker pull centos:7）

4. ```
   #删除镜像：docker rmi 镜像ID
   #删除所有镜像： docker rmi `docker images -q` 
   注意：镜像创建容器后，无法删除镜像，需先删除容器
   ```

#### 容器相关命令

1. ```
   #查看运行中的容器	docker ps
   #查看所有容器		docker ps -a
   #查看最后运行的容器 docker ps -l
   #查看停止的容器	docker ps -f status=exited
   ```

2. 创建与启动容器：

   ```
   #交互方式创建容器	docker run -it --name=容器名 镜像名:标签 /bin/bash
   #退出当前容器 exit
   #守护式创建容器	docker run -di --name=容器名 镜像名称:标签
   #登录守护容器方式	docker exec -it 容器名称(或ID) /bin/bash
   (守护容器进入后退出，仍旧在后台运行)
   	参数说明：-i(表示运行容器);-t(容器启动后进入，分配伪终端);--name(容器名)
   	-v(目录映射关系：素主机目录 映射到素主机的目录);-d(创建一个守护容器后台运行)
   	-p(端口映射：前宿主机端口 映射到容器内的端口)
   #停止容器： docker stop 容器名称(或ID)
   #启动容器： docker start 容器名称(或ID)
   ```

3. 文件拷贝：

   ```
   #将文件拷贝到容器 			docker cp 需要拷贝的文件或目录 容器名称:容器目录
   #将文件从容器中拷贝出来	  docker cp 容器名称:容器文件或目录 需奥要拷贝的目录
   ```

4. 文件挂载：

   ```
   docker run -di -v 要挂载的目录：要挂载容器的目录 --name=容器名称 镜像:标签
   ```

5. 查看容器IP地址：

   ```
   docker inspect 容器名称(或ID) #能查看运行容器的各种数据
   docker inspect --format=’{{.NetworkSettings.IPAddress}}‘ 容器名称(或ID) #IP查询
   ```

6. 删除容器：`docker rm 容器名称(或ID)`(无法移除运行的容器)

#### 应用部署

##### MySQL部署

1. 拉取MySQL镜像：`docker pull centos/mysql-57-centos7`

2. 创建容器：(-p:表示端口映射)

   ```
   docker run -di --name=mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 centos/mysql-57-centos7
   ```

##### Tomcat部署

1. 拉取Tomcat镜像：`docker pull tomcat:7-jre7`

2. 创建容器：(-v 目录挂载，目录不存在会自动生成)

   ```
   docker run -di --name=tomcat -p 9000:8080 -v /usr/local/webapps:/usr/local/webapps tomcat:7-jre7
   ```

##### Nginx部署

1. 拉取Nginx镜像：`docker pull nginx`

2. 创建容器：

   ```
   docker run -di --name=nginx -p 80:80 nginx
   ```

##### Redis部署

1. 拉取Redis镜像：`docker pull redis`

2. 创建容器：

   ```
   docker run -id --name=redis -p 6379:6379 redis
   ```

#### 迁移与备份

1. 容器保存为镜像：`docker commit 容器名称 镜像名称 `
2. 镜像备份：`docker save -o 镜像备份(.tar) 镜像名称`
3. 镜像迁移导入：`docker load -i 镜像备份(.tar)`

#### Dockerfile

Dockerfile：一系列命令和参数构成的脚本，应用于基础镜像并最终创建一个新的镜像

##### 常用命令

| 命令                                  | 作用                               |
| ------------------------------------- | ---------------------------------- |
| FROM image_name:tag                   | 定义了使用哪个基础镜像启动构建流程 |
| MAINTAINER user_name                  | 声明镜像的创建者                   |
| ENV key value                         | 设置环境变量(可多写)               |
| RUN command                           | 是Dockerfile的核心部分(可多写)     |
| ADD source_dir/file   dest_dir/file   | 将宿主机的文件复制到容器内         |
| COPY  source_dir/file   dest_dir/file | 和ADDi相似，但不能解压压缩文件     |
| WORKDIR path_dir                      | 设置工作目录                       |

##### 使用脚本创建镜像

步骤：(举例：jdk)

1. 创建一个目录：`mkdir -p /usr/local/dockerjdk8`

2. 将jdk8的压缩文件导入创建的目录内：`mv jdk-8xx-linux-x64.tar.gz /usr/local/dockerjdk8`

3. 进入目标文件，创建Dockerfile(名字固定)：`cd /usr/local/dockerjdk8 && vim Dockerfile`

4. 添加内容如下：

   ```
   FROM centos:7 		
   MAINTAINER 0x34Hz	
   WORKDIR /usr
   RUN mkdir /usr/local/java
   ADD jdk-8xx-linux-x64.tar.gz /usr/local/java/
   
   ENV JAVA_HOME /usr/local/java/jdk1.8*
   ENV JRE_HOME $JAVA_HOME/jre
   ENV CLASSPATH $JAVA_HOME/bin/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH
   ENV PATH $JAVA_HOME/bin:$PATH
   ```

5. 构建jdk1.8镜像：`docker build -t='jdk1.8' .`
6. 构建成功查看：`docker images`

#### Docker私有仓库

##### 私有仓库搭建配置

1. 拉取私有仓库镜像：`docker pull registry`

2. 启动私有容器：`docker run -di --name=registry -p 5000:5000 registry`

3. 浏览器查看：

   ```
   http://ip:5000/v2/_catlog
   显示{"registry":[]}表示创建成功，内容为空
   ```

4. 修改daemon.json

   ```
   {"registry":["ip:5000"]}
   ```

5. 重启docker服务

##### 镜像上传至私有仓库

1. 标记镜像为私有仓库镜像：`docker tag jdk1.8 ip:5000/jdk1.8`
2. 上传标记镜像：`docker push ip:5000/jdk1.8`
