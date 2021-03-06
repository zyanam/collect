### 概念

### 命令

```dockerfile
usage: docker [options] COMMAND [arg...]
```

#### docker version 

#### docker info

#### docker --help 

### 镜像命令

#### docker images 镜像列表

```dockerfile
docker images -q -a --digests --no-trunc
```

- -q 只显示ID
- -a 显示全部 
- --digests 显示详细信息
- --no-trunc 不要截取

#### docker search 从hub.docker上查找镜像

```dockerfile
docker search -s 30 centos --automated
```

- -s star大于多少个
- --automated 自动构建的

#### docker pull cents 拉取镜像

```dockerfile

```

#### docker logs

```shell
docker logs container id
```



#### docker rmi 删除镜像

```shell
docker rmi -f image id
```

- -f 强制删除

### 容器命令

#### docker run



###### 交互运行

```shell
docker exec -it ba194da55684 /bin/bash
```

###### 挂载容器卷

```shell
$ docker run -p 51025:8080 -v /root/container-volume/bdwebapi/webapps:/usr/local/tomcat/webapps --name bdwebapi -d tomcat:jdk8
```

###### 真正的root权限

```shell
# 如果要是用systemctl 管理服务就要加上参数 --privileged 来增加权，并且不能使用默认的bash，换成 init
$ docker run -it --name webapi --privileged -p 51025:8080 -d fb746854f8e3 /usr/sbin/init
--privileged
```

###### 设置时区

```shell
$ docker run -e "TZ=Asia/Shanghai"
```



#### docker rm 删除容器

- -f  强制删除
- docker rm -f $(docker ps -q)  全部删除

#### docker ps 正在运行的容器

#### docker build 生成镜像，命令最后加 .

1. -f 指定dockerfile
   1. docker build -f docker-file
   2. 如果 DockerFile在当前目录，可以不用指定 
2. -t 指定名称
   1. docker build -f docker-file -t mycentos:1.3 .
   2. . 代表当前路径

#### docker run 运行容器

1. -p 端口映射
  
   ```shell
   docker run -p 7777:8080 tomcat #前面是宿主机端口，后面是容器内端口
   
   docker run -p 51025:8080 --name bdwebapi -d tomcat:jdk8
   ```
   
2. -P 使用随机端口映射

3. 指定CMD

   1. docker run tomcat ls -s

4. -d 后台运行

5. -name 指定名字

6. -v 指定容器卷

7. ```shell
   -v /var/tmp:/usr/local	#前面是宿主机路径，后面是容器路径
   ```

8. 

#### docker info 查看容器信息

#### docker exec 在容器里执行shell

```shell
docker run 89092829323 ls -l
```

#### docker commit 将容器提交成镜像

```shell
docker commit -a 作者 -m "说明" container id name:tag
```



### DockerFile



#### FROM   指定当前镜像的源

```dockerfile
FROM scratch
```



#### MAINTAINER  镜像维护者的姓名和邮箱地址

```dockerfile
MAINTAINER yanam<98346@sina.com>
```



#### RUN 容器构建时需要运行的命令

```dockerfile
RUN yum -y install vim
```



#### EXPOSE  对外暴露的端口

```dockerfile
EXPOSE 80
```



#### WORKDIR  指定登陆后的默认路径，如果不指定，默认为根目录

```dockerfile
WORKDIR /tmp
WORKDIR $mypath
```



#### ENV  设置环境变量

```dockerfile
ENV mypath /tmp 
```



#### ADD  将宿主机的文件拷贝到镜像内，并且自动处理URL和解压tar包

```dockerfile
ADD jdk-8u171-linux-x64.tar.gz /usr/local/
```



#### COPY  将宿主机的文件拷贝到镜像内，不处理URL和tar包

```dockerfile
COPY c.txt /usr/local/cincontainer.txt
```



#### VOLUME  挂载容器数据卷，相对应的宿主机目录可以使用docker info 查看

#### CMD  指定容器启动时要运行的命令，最后一个覆盖前面的

```dockerfile
CMD echo "ok"
CMD /bin/bash
CMD ["curl","-s","http://ip.cn"]
CMD tail -f /dev/null #能让控制台不退出

```



#### ENTRYPOINT  同CMD,追加到前面

```dockerfile
ENTRYPOINT ["curl","-s","http://ip.cn"]
```



#### ONBUILD  当镜像被继承时触发

```dockerfile
ONBUILD echo "hello"
```

### 参考

1. [使用idea调试运行在docker容器的Java实例](https://www.jianshu.com/p/7d685dce1440)
2. [idea远程部署war到docker](https://blog.csdn.net/yu757371316/article/details/81133167)
3. [IDEA调试Java+Docker+Tomcat+Spring程序](https://www.awaimai.com/2608.html)

### 部署

#### MySQL

```shell
docker run 
-p 3306:3306 
--name mysql 
-v D:\DockerDesktop\mysql\conf:/etc/mysql/conf.d 
-v D:\DockerDesktop\mysql\logs:/logs 
-v D:\DockerDesktop\mysql\data:/var/lib/mysql 
-e MYSQL_ROOT_PASSWORD=123456 
-d mysql:5.6
```

#### Tomcat

```shell
docker run -p 8080:8080 --name tomcat8 -d tomcat:8.0
docker exec -it tomcat8 bash
exit
docker cp Demo.war tomcat8:/usr/local/tomcat/webapps
docker restart tomcat8
```

### 开启SSH

```shell
$ yum install -y openssh*

#修改root密码
$ passwd

#修改配置文件
$ vi /etc/ssh/sshd_config
PermitRootLogin yes  #允许root用户ssh登录
UsePAM no            ##禁用PAM

#重启ssh
systemctl restart sshd
```



### 其它

#### 修改运行的容器的端口号

```shell
/var/lib/docker/containers/[hash_of_the_container]/hostconfig.json
$ docker inspact containerID
```




