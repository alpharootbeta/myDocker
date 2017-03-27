# myDocker
[第一天](#page1)
[第二天](#page2)
[第三天](#page3)
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
