### 安装软件

```sh
$ yum install -y nfs-utils rpcbind  # 安装软件（客户端和服务端都需要安装）
```

#### 服务端

##### 修改配置文件

```shell
# /etc/exports
/data/nfs *(rw,no_root_squash,sync)
```

- /data/nfs：共享目录
- *：不限制连接地址
  - ro：只读
  - rw：读写
  - sync：同步，数据同步写到内存与硬盘中
  - async：异步，数据先暂存内存
  - root_squash：将root用户映射为来宾账号
  - no_root_squash：有root权限，不建议使用
  - all_squash：全部映射为来宾账号
  - anonuid,anongid：指定映射的来宾账号的UID和GID

##### 启动服务

```shell
$ systemctl start nfs
$ systemctl enable nfs
```

- 使用端口：111/tcp，111/udp，2049/tcp，2049/udp

#### 客户端

```shell
# 创建挂载点
$ mkdir /mnt/nfs1

# 挂载
$ mount -t nfs 192.168.89.235:/data/nfs /mnt/nfs1
$ showmount -e 192.168.89.235
```





