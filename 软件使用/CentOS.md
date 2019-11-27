### 命令

#### mkdir 创建目录

- -p 级联创建目录

  - ```shell
    mkdir -p /mydir/mytest
    ```

#### tcpdump 抓包命令

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

#### lsof 查看端口号

```shell
lsof -i : 51026
```

#### netstat 查看端口号

```shell
sudo apt install net-tools
netstat 
```

#### top 

```shell
top -Hp PID  #查看某一进程详情
```

#### ls

```shell
ls -lrt
```

- -r 倒序
- -t  sort by modification time

#### ssh

```shell
ssh root@60.10.139.120 -p 2206
```

- -p 指定端口号（小写）

#### scp

```shell
scp -P 2206 *.jar root@60.10.139.120:~/
```

- -P 指定端口号(大写)

#### ps

查看线程占用cpu时间

```shell
ps -mp 5550 -o THREAD,tid,time
```





