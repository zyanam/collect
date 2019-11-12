### 概念

### 命令

1. docker images	
   1. -q 只显示ID
   2. -a 显示全部 
   3. --digests 显示详细信息

### DockerFile

1. FROM   指定当前镜像的源

   ```dockerfile
   FROM scratch
   ```

2. MAINTAINER  镜像维护者的姓名和邮箱地址

   ```dockerfile
   
   ```

3. RUN 容器构建时需要运行的命令

4. EXPOSE  对外暴露的端口

5. WORKDIR  指定登陆后的默认路径，如果不指定，默认为根目录

6. EVN  设置环境变量

7. ADD  将宿主机的文件拷贝到镜像内，并且自动处理URL和解压tar包

8. COPY  将宿主机的文件拷贝到镜像内，不处理URL和tar包

9. VOLUME  挂载容器数据券，相对应的宿主机目录可以使用docker info 查看

10. CMD  指定容器启动时要运行的命令，最后一个生效

11. ENTRYPOINT  同CMD,每一个都生效

12. ONBUILD  当镜像被继承时触发

    



