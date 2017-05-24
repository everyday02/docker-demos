### Docker容器使用

#### 交互式容器

Docker 允许你在容器内运行应用程序， 使用 docker run 命令来在容器内运行一个应用程序

```shell
# 1. 查找本地是否有ubuntu:15.10镜像，若没有，从Docker Hub下载。
# 2. 运行该镜像，并在容器中执行命令 /bin/echo "Hello world"
$ docker run ubuntu:15.10 /bin/echo "Hello world"
```

通过docker的两个参数 <code>-i -t</code>，能够让docker运行容器后，我们仍然可以操作容器，执行更多的操作。

```shell
$ docker run -i -t ubuntu:15.10 /bin/bash
```

各个参数解析：
1. -t : 在新容器内指定一个伪终端或终端。
2. -i : 允许你对容器内的标准输入 (STDIN) 进行交互。


此时我们已进入一个 ubuntu15.10系统的容器中。

尝试在容器中运行命令 cat /proc/version和ls分别查看当前系统的版本信息和当前目录下的文件列表
``` shell
root@ec5e0f79f603:/# cat /proc/version
Linux version 3.10.0-514.6.2.el7.x86_64 (builder@kbuilder.dev.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-11) (GCC) ) #1 SMP Thu Feb 23 03:04:39 UTC 2017
root@ec5e0f79f603:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

可以通过运行<code>exit</code>命令或者使用CTRL+D来退出容器。


#### 创建容器（后台模式）

使用以下命令创建一个基于<code>ubuntu:15.10</code>镜像生成的容器
```shell
$ docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
# 2b1b7a428627c51ab8810d541d759f072b4fc75487eed05812646b8534a2fe63

```


在输出中，我们没有看到期望的"hello world"，而是一串长字符
2b1b7a428627c51ab8810d541d759f072b4fc75487eed05812646b8534a2fe63
这个长字符串叫做容器ID，对每个容器来说都是唯一的，我们可以通过容器ID来查看对应的容器发生了什么。
首先，我们需要确认容器有在运行，可以通过 docker ps 来查看

```shell
docker ps
# CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
# 30731c9ad038        ubuntu:15.10        "/bin/sh -c 'while..."   3 seconds ago       Up 2 seconds                            admiring_panini
```

 * CONTAINER ID:容器ID
 * NAMES:自动分配的容器名称
 * 在容器内使用docker logs命令，查看容器内的标准输出

```shell
# docker logs admiring_panini 容器名称/id 查看容器输出结果
docker logs 30731c9ad038
# hello world
# hello world
# hello world
# hello world
# hello world

```

#### 停止容器

我们使用 docker stop 命令来停止容器:
``` shell
# docker stop admiring_panini
docker stop 30731c9ad038
```

再次执行<code>docker ps </code>，可以看到，已经没有容器处于运行状态。

#### 启动容器

容器停止运行后，可以使用<code>docker ps -a</code>来查看所有容器（包括已停止的）

```shell
docker ps -a
#30731c9ad038        ubuntu:15.10        "/bin/sh -c 'while..."   38 minutes ago      Exited (137) 34 minutes ago                        admiring_panini
#4e21550567c3        ubuntu:15.10        "/bin/sh -c 'while..."   42 minutes ago      Exited (137) 41 minutes ago                        nostalgic_allen
#ec5e0f79f603        ubuntu:15.10        "/bin/bash"              About an hour ago   Exited (0) 44 minutes ago                          infallible_meitner
#107b5bd391ac        ubuntu:15.10        "/bin/echo 'Hello ..."   About an hour ago   Exited (0) About an hour ago                       wizardly_kepler
#3592074c8574        hello-world         "/hello"                 About an hour ago   Exited (0) About an hour ago                       blissful_noether
#92b96d0c59df        hello-world         "/hello"                 About an hour ago   Exited (0) About an hour ago                       vigorous_lovelace
#44f75c1e14f4        hello-world         "/hello"                 About an hour ago   Exited (0) About an hour ago                       zealous_davinci

```

可以执行<code>docker start</code>再次启动
```shell
# docker start admiring_panini
docker start 30731c9ad038
#  docker restart 30731c9ad038  重启正在运行的容器
```

#### 移除容器

可以使用 docker rm 命令来删除不需要的容器

删除容器时，容器必须是停止状态，否则会报如下错误

```shell
docker rm 30731c9ad038
Error response from daemon: You cannot remove a running container 30731c9ad0385b2b8cdf654b9e541501a29444d6615be534e9b13d4edeebdb0b. Stop the container before attempting removal or force remove
```
