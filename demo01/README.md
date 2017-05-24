### 安装Docker

#### 使用脚本安装 Docker

* 使用 sudo 或 root 权限登录 Centos。
* 确保 yum 包更新到最新。

```shell
$ sudo yum update

```
* 执行 Docker 安装脚本。
```shell
# 执行这个脚本会添加 docker.repo 源并安装 Docker。
curl -fsSL https://get.docker.com/ | sh
```

* 启动 Docker 进程。
```shell
$ sudo service docker start
```

* 从Docker Hub 下载一个测试镜像。
```shell
$ docker pull hello-world
```

* 在容器中执行一个测试的镜像。
```shell
$ sudo docker run hello-world
```

各个参数解析：
 1. <code>docker</code>: Docker 的二进制执行文件。
 2. <code>run</code>:与前面的 docker 组合来运行一个容器。
 3. <code>hello-world</code>指定要运行的镜像，Docker首先从本地主机上查找镜像是否存在，如果不存在，Docker 就会从镜像仓库 Docker Hub 下载公共镜像。



* 得到类似以下的信息，运行成功。

``` shell
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://cloud.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/
```
