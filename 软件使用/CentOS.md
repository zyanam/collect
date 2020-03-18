### 命令

### mkdir 创建目录

- -p 级联创建目录

  - ```shell
    mkdir -p /mydir/mytest
    ```

### tcpdump 抓包命令

```shell
tcpdump tcp -i eth1 -t -s 0 -c 100 and dst port ! 22 and src net 192.168.1.0/24 -w ./target.cap
```

- tcp: ip icmp arp rarp 和 tcp、udp、icmp这些选项等都要放到第一个参数的位置，用来过滤数据报的类型
- -i eth1 : 只抓经过接口eth1的包
- -t : 不显示时间戳
- -s 0 : 抓取数据包时默认抓取长度为68字节。加上-S 0 后可以抓到完整的数据包
- -c 100 : 只抓取100个数据包
- dst port ! 22 : 不抓取目标端口是22的数据包
- src net 192.168.1.0/24 : 数据包的源网络地址为192.168.1.0/24
- -w ./target.cap : 保存成cap文件，方便用ethereal(即wireshark)分析
- -XX 显示数据内容

### lsof 查看端口号

```shell
lsof -i : 51026
```

### netstat 查看端口号

```shell
sudo apt install net-tools
netstat 
netstat -nlpt
```

### top 

```shell
top -Hp PID  #查看某一进程详情
```

### ls

```shell
ls -lrt
```

- -r 倒序
- -t  sort by modification time

### ssh

```shell
$ ssh root@60.10.139.120 -p 2206

$ ssh -p8224 -i ./id_rsa root@60.10.139.111

#如果提示 “ WARNING: UNPROTECTED PRIVATE KEY FILE! ”，是私钥读写权限问题，权限修改为600，如下：
$ chmod 600 ./id_rsa
```

- -p 指定端口号（小写）

### scp

```shell
$ scp -P 2206 *.jar root@60.10.139.120:~/

$ scp -P8222 -i ~/rsakyes/id_rsa_test ./codeserver/bd_809_gateway/target/bd_809_gateway-1.0.3.jar root@60.10.139.111:/usr/local/beidou-809-server

```

- -P 指定端口号(大写)

### ps

查看线程占用cpu时间

```shell
ps -mp 5550 -o THREAD,tid,time
```

### tee  向 standout输出的同时也将内容输出到文件

```shell
ping baidu.com | tee ping-baidu.log #输出到控制台的同时，将内容保存到ping-baidu.log文件中
```

```shell
:w !sudo tee %  #vi 编辑完文件，没有权限保存
```

### tar

```shell
-zvf		仅打包，不压缩
-czvf 		压缩，*.tar.gz
-cjvf		压缩，*.tar.bz2
-xzvf		解压，*.tar.gz
-xjvf		解压，*.tar.bz2
-c			建立压缩档案
-x			解开压缩档案
-t			查看压缩档案内容
-z			gzip压缩
-j			bzip2压缩
-v			显示执行过程
-f			使用档案名，注意-f之后不能有其他参数，直接接档案名
(e.g.)tar -zvxf <tarfile>
-C			指定解压目录
```

### 高并发优化

#### 最大文件描述符

```shell
ulimit -SHn 1024000 
echo "ulimit -SHn 1024000" >> /etc/rc.d/rc.local 
source /etc/rc.d/rc.local
```

####  内核参数优化/etc/sysctl.conf 

```shell
#关闭ipv6
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1

#决定检查过期多久邻居条目
net.ipv4.neigh.default.gc_stale_time=120

#使用arp_announce / arp_ignore解决ARP映射问题
net.ipv4.conf.default.arp_announce = 2
net.ipv4.conf.all.arp_announce=2
net.ipv4.conf.lo.arp_announce=2 # 避免放大攻击
net.ipv4.icmp_echo_ignore_broadcasts = 1 # 开启恶意icmp错误消息保护
net.ipv4.icmp_ignore_bogus_error_responses = 1

#处理无源路由的包
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0

#core文件名中添加pid作为扩展名
kernel.core_uses_pid = 1 # 开启SYN洪水攻击保护
net.ipv4.tcp_syncookies = 1

#修改消息队列长度
kernel.msgmnb = 65536
kernel.msgmax = 65536

#timewait的数量，默认180000
net.ipv4.tcp_max_tw_buckets = 6000
net.ipv4.tcp_sack = 1
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_rmem = 4096 87380 4194304
net.ipv4.tcp_wmem = 4096 16384 4194304
net.core.wmem_default = 8388608
net.core.rmem_default = 8388608
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216

#限制仅仅是为了防止简单的DoS 攻击
net.ipv4.tcp_max_orphans = 3276800

#未收到客户端确认信息的连接请求的最大值
net.ipv4.tcp_max_syn_backlog = 262144
net.ipv4.tcp_timestamps = 0

#内核放弃建立连接之前发送SYNACK 包的数量
net.ipv4.tcp_synack_retries = 1

#内核放弃建立连接之前发送SYN 包的数量
net.ipv4.tcp_syn_retries = 1

#启用timewait 快速回收
net.ipv4.tcp_tw_recycle = 1

#开启重用。允许将TIME-WAIT sockets 重新用于新的TCP 连接
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_mem = 94500000 915000000 927000000
net.ipv4.tcp_fin_timeout = 1
```

#### 重载配置

```shell
$ sysctl -p
```

### 修改时区

```shell
$ 
```







