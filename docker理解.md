## docker

### docker是什么？

可以在服务器上用镜像划分多个容器，容器间跟宿主机间可以互相通信

### docer几个概念

1. 镜像
2. 容器

### docker命令

##### 一、操作镜像

1. 查看所有镜像

		docker images

2. 删除镜像

		docker rmi [镜像名称]

3. 获取一个新的镜像

		docker pull ubuntu:18.04

	**参数说明**

	- ubuntu:18.04 镜像名称:版本

4. 查找镜像

		docker search ubuntu

5. 创建镜像

	- 方法一：从已经创建的容器中更新镜像，并且提交这个镜像

		**步骤**
		1. 进入镜像后运行apt-get update进行更新
		2. 提交容器副本
		
				docker commit -m="update image" -a="yuanjiawei" e218edb10161 jiawei/image_name
			**参数说明**
		
			- -m 提交的描述信息
			- -a 指定的镜像作者
			- e218edb10161 容器ID
			- jiawei/image_name 要创建的镜像名称

	***
	- 方法二：使用 Dockerfile 指令来创建一个新的镜像

		我们使用命令 docker build ， 从零开始来创建一个新的镜像。为此，我们需要创建一个 Dockerfile 文件，其中包含一组指令来告诉 Docker 如何构建我们的镜像。

			ubuntu@7853f9cccfa7:~$ cat Dockerfile 
			FROM    ubuntu:18.04
			MAINTAINER      Fisher "fisher@sudops.com"
			
			RUN     /bin/echo 'root:123456' |chpasswd
			RUN     useradd jiawei
			RUN     /bin/echo 'jiawei:123456' |chpasswd
			RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
			EXPOSE  22
			EXPOSE  80
			CMD     /usr/sbin/sshd -D

		每一个指令都会在镜像上创建一个新的层，每一个指令的前缀都必须是大写的。

		第一条FROM，指定使用哪个镜像源
		
		RUN 指令告诉docker 在镜像内执行命令，安装了什么。。。
		
		然后，我们使用 Dockerfile 文件，通过 docker build 命令来构建一个镜像。
		
			ubuntu@7853f9cccfa7:~$ docker build -t ubuntu/ubuntu:18.04 ./Dockerfile
			Sending build context to Docker daemon 17.92 kB
			Step 1 : FROM centos:6.7
			 ---&gt; d95b5ca17cc3
			Step 2 : MAINTAINER Fisher "fisher@sudops.com"
			 ---&gt; Using cache
			 ---&gt; 0c92299c6f03
			Step 3 : RUN /bin/echo 'root:123456' |chpasswd
			 ---&gt; Using cache
			 ---&gt; 0397ce2fbd0a
			Step 4 : RUN useradd runoob
			......

		**参数说明**
		- -t 指定要创建的目标镜像名
		- ./Dockerfile 路径

		接下来我们就可以执行`docker iamges`看到刚才创建的镜像





##### 二、操作容器

1. 创建容器

	***
		# 创建通过镜像容器，并进入容器
		docker run -i -t ubuntu:18.04 /bin/bash
	
	**参数说明**

	 - run 创建容器，docker会根据镜像名称创建，如果本地找不到对应的镜像，会从网上下载
	 - -i 交互式操作
	 - -t 终端
	 - ubuntu:18.04 名为ubuntu的镜像冒号后面为版本
	 - /bin/bash 放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 /bin/bash
	 
	***

		# 后台创建容器
		docker run -d ubuntu:18.04 /bin/bash

	**参数说明**
	
	- -d 后台创建容器

2. 查看所有容器

		docker ps -a 

3. 启动容器

		docker start [容器id] 

4. 停止容器

		docker stop [容器id]

5. 进入容器 

		docker exec -i -t [容器id] /bin/bash

	**参数说明**

	- -i 交互式操作
	- -t 终端
	- /bin/bash 放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 /bin/bash

6. 退出容器执行`exit` 或 `Ctrl + D`（通过`docker exec`命令进入则在退出后不会停止容器）

7. 删除容器

		docker rm [容器id]