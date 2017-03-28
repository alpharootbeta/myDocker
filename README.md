# myDocker
[第一天](#page1)
[第二天](#page2)
[第三天](#page3)
[第四天](#page4)
[第五天](#page5)
## <h2 id = "page1">第一天</h2>
### 简单命令的使用
```
docker info
docker version
docker search 搜索镜像 例 docker search java
docker pull
docker images 本地装载镜像
```
### 命令行交互
```
docker run -it（交互） java（镜像名） java -version（命令）
docker run -it java ps   ctrl+p ctrl+q 后台执行
docker attach 容器名 进入容器
docker run -it java uname
docker run java  ip addr（docker ip） 主机ping通
docker run java env(环境变量)
docker run -d（后台） image command   (docker exec进入容器 docker attach)
```
## <h2 id = "page2">第二天</h2>
### 创建 使用容器
```
docker create start stop kill pause unpause
docker create -it --name=myjava java java version
docker ps -a 终止状态的容器
docker create --name mysqlsrv1 -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 mysql
命令参数说明
--name 给容器起一个别名，可选，如果不指定，则Docker会自动生成不规则的字符串表示
-i 指定容器可以交互，有了此选项后，可以使用docker attach等与容器进行交互
-p 映射宿主机与容器中服务端口
-e 设置容器运行所需要的环境变量
通过docker ps –l可以查看容器是否正确启动 
```
docker 交互
```
docker create --name myjava1 -it java /bin/bash
docker start myjava1
docker ps 运行状态的容器
docker ps -a 所有的docker容器
docker exec -it f9249587a4c6 /bin/bash 在运行的容器 启动新进程
docker rm 容器名 删除停止的容器
docker rmi 镜像名：标签名 
docker rmi $(docker images -q)删除所有镜像
docker rmi $(docker images -q ubuntu)删除ubuntu下的所有镜像
docker logs -f -t -tail 
docker top 容器名
```
## <h2 id = "page3">第三天</h2>
```
mkdir yujie_docker_test
```
### 打成镜像
```
docker commit  f9249587a4c6 myjava1
docker run -it myjava1 ls
```
核对镜像信息
```
 docker inspect myjava1
```
build文件
```
mkdir yujie_docker
cd yujie_docker
vi dockerfile
```
```
#FROM ubuntu:16.04  
#MAINTAINER Srijan Kishore <s.kishore@ispconfig.org> 
#RUN echo 'hello'
#RUN echo 'build success'
```
build文件
```
docker build -t="myjava1/build" .
Status: Downloaded newer image for ubuntu:16.04
 ---> f49eec89601e
Step 2 : MAINTAINER Srijan Kishore <s.kishore@ispconfig.org>
---> Running in 873d964b16c7
---> 76b252e02ef0
Removing intermediate container 873d964b16c7
Step 3 : RUN echo 'hello'
---> Running in f302a3c1f085
hello
---> f25d9a55e829
Removing intermediate container f302a3c1f085
Step 4 : RUN echo 'build success'
 ---> Running in e9a4a31215d1
build success
---> ee9c111c473b
```
```
docker images myjava1/build
```
## <h2 id = "page4">第四天</h2>
### 在容器中部署静态网站-nginx部署流程
#### 创建映射80端口的交互式容器
```
docker pull ubuntu
docker run -it -p 80 --name=html ubuntu /bin/bash
docker start html
docker ps
docker exec -it xxxx  /bin/bash
```
#### 安装nginx
```
apt-get update 防止出现 E: Unable to locate package
apt-get install nginx
```
#### 安装vim
```
apt-get install vim
```
#### 创建静态页面
```
mkdir -p /var/html
cd /var/html
vim index.html
<html>
<head>my html</head>
<body>this is my first website</body>
</html>

```
#### 修改nginx配置文件
```
whereis nginx
cd /etc/nginx/sites-enabled
vi default
root /var/www/html/index.html

```
#### 运行nginx
```
nginx
ps -ef 查看nginx服务是否成功
```
#### 验证网站访问
```
docker port html
docker top html
curl http://127.0.0.1:端口号
或者
docker inspect 9ab9ab55e2b7
curl http://172.17.0.2   容器对应的ip地址

```
## <h2 id = "page5">第五天</h2>
### 数据卷
docker run -it -v 本机目录：docker容器目录  镜像名 /bin/bash
添加权限ro只读
```
docker run -v -it 本机目录：docker容器目录:ro --name 容器名 镜像名 /bin/bash
```
使用dockerfile
```
from ubuntu:1404
VOLUME[/data1,/data2]
docker bulit -t docker/data
docker run --name data_test -it docker/data
ls
```
### 数据卷容器
实现不同容器间的数据共享
```
docker run --volumes-from 容器名
docker run -it --name data_test ubuntu
docker run -it --name data_new --volumes-from data_test ubuntu /bin/bash
```
删除数据卷
```
docker rm -v 数据卷名
docker rm -v data_new
```
### 进行数据管理
数据备份
```
docker run --volumes-from 容器名 -v ${pwd}(本地地址):/backup（容器地址） ubuntu
tar cvf /backup/XXX.tar  容器文件目录
```
数据还原
```
docker run --volumes-from 容器名 -v ${pwd}(本地地址):/backup（容器地址） ubuntu
tar xvf /backup/XXX.tar  容器文件目录
```

