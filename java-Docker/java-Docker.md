### 构建java项目运行环境

#### 基础镜像

从网易蜂巢拉取基础环境镜像
  * Tomcat 服务容器： [library / tomcat](https://c.163.com/hub#/m/repository/?repoId=3105)
  * Mysql 数据库容器：[library / mysql](https://c.163.com/hub#/m/repository/?repoId=2955)

```shell
docker pull hub.c.163.com/library/tomcat:latest

docker pull hub.c.163.com/library/mysql:latest
```

#### 基于基础镜像构建容器

构建<code>tm-mysql</code>数据库容器

```shell
  # mysql:username   root
  # mysql:password   hehang
  # -p 3307:3306 映射3306端口至本地3307
  docker run -ti -p 3307:3306  --name tm-mysql -e MYSQL_ROOT_PASSWORD=hehang -d 9e64176cd8a2
```

构建<code>tm-web</code>服务器容器

``` shell
# -p 8080:8080 映射8080端口至本地8080
# -v /local/TM/WebRoot/:/usr/local/tomcat/webapps/TM/ 目录映射
# --link tm-mysql:tm-mysql   用来将两个容器进行关联,使tm-web可以通过tm-db别名去访问tm-mysql容器

docker run -it -d -p 8080:8080 --name tm-web --link tm-mysql:tm-db -v /local/TM/WebRoot/:/usr/local/tomcat/webapps/TM/ 3695a0fe8320
```

最后在本地通过<cdoe>3307</code>端口链接至mysql容器，初始化tm库表数据。


运行完美。
