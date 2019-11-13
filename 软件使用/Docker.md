### 概念

### 命令

#### docker images 镜像列表

1. -q 只显示ID
2. -a 显示全部 
3. --digests 显示详细信息

#### docker rm 删除容器

1. -f  强制删除
   1. docker rm -f $(docker ps -q)  全部删除

#### docker ps 正在运行的容器

#### docker build 生成镜像，命令最后加 .

1. -f 指定dockerfile
   1. docker build -f docker-file
2. -t 指定名称
   1. docker build -f docker-file -t mycentos:1.3 .

#### docker run

1. -p 端口映射
   1. docker run -p 7777:8080 tomcat 前面是宿主机端口，后面是容器内端口
2. -P 使用随机端口映射
3. 指定CMD
   1. docker run tomcat ls -s

#### docker info 查看容器信息



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

#### COPY  将宿主机的文件拷贝到镜像内，不处理URL和tar包

#### VOLUME  挂载容器数据券，相对应的宿主机目录可以使用docker info 查看

#### CMD  指定容器启动时要运行的命令，最后一个覆盖前面的

```dockerfile
CMD echo "ok"
CMD /bin/bash
CMD ["curl","-s","http://ip.cn"]
```



#### ENTRYPOINT  同CMD,追加到前面

```dockerfile
ENTRYPOINT ["curl","-s","http://ip.cn"]
```



#### ONBUILD  当镜像被继承时触发

  



