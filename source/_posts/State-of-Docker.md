---
title: State of Docker
date: 2023-04-06 13:53:51
tags: "docker"
category: "docker"
---
# State of Docker
## Find the current state of a docker container
在默认情况下，指令`docker ps` 展示了所有的docker容器的现有状态
```bash
$ docker ps -a
CONTAINER ID   IMAGE      COMMAND       CREATED              STATUS                          PORTS     NAMES
8f0b524f2d32   centos:7   "/bin/bash"   46 seconds ago       Created                                   strange_beaver
e6d798254d45   centos:7   "/bin/bash"   About a minute ago   Exited (0) About a minute ago             wizardly_cohen
```
以上的输出展示了当前机器中所有容器的Status以及其他的一些细节
我们也可以使用`docker inspect`指令来获取单独容器的Status信息
```bash
$ docker inspect -f '{{.State.Status}}' mycontainer
running
```
在这里，`mycontainer`是我们想要查询当前状态的容器的容器名，也可以用容器ID来代替
## Possible States of a Docker Container
在任何情况下，一个Docker container可以有六种可能的States
### Created
Docker将created state分配给那些自从创建后从未被启动的容器，因此，这个状态下的container没有CPU和内存分配

使用`docker create`指令创建的docker container的Status是created。
```bash
$ docker create --name mycontainer httpd
dd109e4be16219f1a6b9fc1cbfb050c1ae035d6a2c301ea0e93eb7d5252b8d2e
$ docker inspect -f '{{.State.Status}}' mycontainer
created
```
这里我们使用官方Docker镜像httpd创建了一个Docker container，mycontainer。
当我们想要进行一些大的项目时，这样的容器是非常有用的。
### Running
当我们使用`docker start`命令启动一个已经创建的容器，它进入了启动状态
```bash
$ docker create --name mycontainer httpd
8d60cb560afc1397d6732672b2b4af16a08bf6289a5a0b6b5125c5635e8ee749
$ docker inspect -f '{{.State.Status}}' mycontainer
created
$ docker start mycontainer
mycontainer
$ docker inspect -f '{{.State.Status}}' mycontainer
running
```
在上述例子中，我们首先使用官方的httpd Docker image创建一个Docker 容器，然后使用`docker start`指令来启动该容器。

使用`docker run`指令运行的容器也时相同的主嗯台
```bash
$ docker run -itd --name mycontainer httpd
685efd4c1c4a658fd8a0d6ca66ee3cf88ab75a127b9b439026e91211d09712c7
$ docker inspect -f '{{.State.Status}}' mycontainer
running
```
在这个状态下，容器消耗CPU和内存资源

### Restarting
简单来说，这个状态表明容器正在重启
Docker支持四种重启策略，分别是
- no
- on-failure
- always
- unless-stopped

重启策略决定了当容器exit时它的表现。默认情况下，重启策略是no，这意味着在容器exit之后它不会自动被启动
更新重启策略至`always`并且使用下面的例子修改容器状态
```bash
$ docker run -itd --restart=always --name mycontainer centos:7 sleep 5
f7d0e8becdac1ebf7aae25be2d02409f0f211fcc191aea000041d158f89be6f6
```
上述例子将会运行mycontainer并且执行`sleep 5`指令然后exit。但是因为我们已经改变了restart policy，在该容器exit之后它会自动重启。
五秒之后，这个容器的状态将是restarting
```bash
$ docker inspect -f '{{.State.Status}}' mycontainer
restarting
```

### Exited
当容器内的进程终止时，就会达到这种状态。 在此状态下，容器不会消耗任何 CPU 和内存。
容器可能因为以下几种原因exit
- 容器内的进程已经完成，因此容器exit
- 容器内的进程在运行的时候遇到了错误
- 使用`docker stop`命令有意停止容器
- 运行bash的容器没有设置交互式终端

```bash 
$ docker run -itd --name mycontainer centos:7 sleep 10
596a10ddb635b83ad6bb9daffb12c1e2f230280fe26be18559c53c1dca6c755f
```

无法使用 `docker exec` 命令访问处于退出状态的容器。 但是，我们可以使用 `docker start` 或 `docker restart` 启动容器，然后访问它。
```bash
$ docker start mycontainer
```

### Paused  
在此状态下，容器不占用CPU资源，但会消耗内存

```bash
$ docker pause mycontainer
mycontainer
$ docker inspect -f '{{.State.Status}}' mycontainer
paused
```
```bash
$ docker unpause mycontainer
mycontainer
```

### Dead
Docker 容器的死状态意味着该容器无法正常运行。 当我们尝试删除容器时会达到此状态，但无法删除，因为某些资源仍在被外部进程使用。 因此，容器被移动到死状态。
处于死状态的容器无法重新启动。 它们只能被移除。 由于处于死状态的容器被部分删除，因此它不会消耗任何内存或 CPU。