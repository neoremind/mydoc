# Docker commands cheatsheet

 [Reference](https://docs.docker.com/engine/reference/commandline/)

## 基本信息
- 显示版本
```
docker version
```

- 包括镜像和容器数
```
docker info
```

- stats
```
docker stats
```

- top
```
docker top mymysql
docker top mymysql ps -ef
```

## Image

- 从容器创建一个新的镜像
```
docker commit -a xu.zhang -m "this is message" {CONTAINER_ID} url:tagname

docker commit -a "runoob.com" -m "my apache" a404c6c174a2  mymysql:v1 

-a :提交的镜像作者；
-c :使用Dockerfile指令来创建镜像；
-m :提交时的说明文字；
-p :在commit时，将容器暂停。
```

- 列出本地镜像
```
docker images
```

- 删除本地一个或多少镜像
```
docker rmi runoob/ubuntu:v4
```
-f 强制删除

## Container生命周期

- start/stop/restart
```
docker stop|start {CONTAINER_ID}
```

- 列出容器
```
docker ps -a
```
-a :显示所有的容器，包括未运行的。


- run
```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

-d: 后台运行容器，并返回容器ID；
-i: 以交互模式运行容器，通常与 -t 同时使用；
-t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
--name="nginx-lb": 为容器指定一个名称；
-h "mars": 指定容器的hostname；
-e username="ritchie": 设置环境变量；
-m :设置容器使用内存最大值；
```

```
docker run -t -i --name my-batch-job {IMAGE_ID} /bin/bash  

docker run -it nginx:latest /bin/bash
```
交互式新建container
```
docker run --name my-batch-job -d 77d3df7f5b2f /bin/sh -c "while true; do ping 8.8.8.8; sleep 10; done" 
```
后台backgroud运行container

- 在运行的容器中执行命令
```
docker exec -it my-batch-job /bin/bash
```
进入刚刚后台运行的docker

```
docker exec -d my-batch-job su product -c 'export HADOOP_CLIENT_OPTS=-Xmx6000m && bash -x /home/product/xxx.sh' 
```
run a series of commands

```
docker exec -d my-batch-job touch 123
```
simple touch

```
sudo docker exec -it my-batch-job ps -ef
```
ps what's running inside the container

- attach 连接到正在运行中的容器

```
docker attach f654f7b297e7
docker attach --sig-proxy=false mynginx
```
要attach上去的容器必须正在运行，可以同时连接上同一个container来共享屏幕（与screen命令的attach类似）

官方文档中说attach后可以通过CTRL-C来detach，但实际上经过我的测试，如果container当前在运行bash，CTRL-C自然是当前行的输入，没有退出；如果container当前正在前台运行进程，如输出nginx的access.log日志，CTRL-C不仅会导致退出容器，而且还stop了。这不是我们想要的，detach的意思按理应该是脱离容器终端，但容器依然运行。好在attach是可以带上--sig-proxy=false来确保CTRL-D或CTRL-C不会关闭容器。


- 用于容器与主机之间的数据拷贝
```
docker cp XXX.txt {CONTAINER_ID}:/home/product/path  //copy files to docker container

将主机/www/runoob目录拷贝到容器96f7f14e99ab的/www目录下。
docker cp /www/runoob 96f7f14e99ab:/www/

将主机/www/runoob目录拷贝到容器96f7f14e99ab中，目录重命名为www。
docker cp /www/runoob 96f7f14e99ab:/www

将容器96f7f14e99ab的/www目录拷贝到主机的/tmp目录中。
docker cp  96f7f14e99ab:/www /tmp/
```

## Hub and registry
```
docker push url/batch-job:xuzhang

docker pull url/batch-job:xuzhang
```


