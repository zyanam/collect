### 安装软件

```shell
$ yum -y install vsftpd
```

### 修改配置文件

```shell
$ vim /etc/vsftpd/vsftpd.conf
```

```properties
#是否允许匿名，默认no
anonymous_enable=NO

#这个设定值必须要为YES 时，在/etc/passwd内的账号才能以实体用户的方式登入我们的vsftpd主机
local_enable=YES

#具有写权限
write_enable=YES

#本地用户创建文件或目录的掩码
local_umask=022

#当dirmessage_enable=YES时，可以设定这个项目来让vsftpd寻找该档案来显示讯息！您也可以设定其它档名！
dirmessage_enable=YES

#当设定为YES时，使用者上传与下载日志都会被纪录起来。记录日志与下一个xferlog_file设定选项有关：
xferlog_enable=YES
xferlog_std_format=YES

#上传与下载日志存放路径
xferlog_file=/var/log/xferlog 

#开启20端口
connect_from_port_20=YES

#关于系统安全的设定值：
ascii_download_enable=YES(NO)
如果设定为YES ，那么 client 就可以使用 ASCII 格式下载档案。
一般来说，由于启动了这个设定项目可能会导致DoS 的攻击，因此预设是NO。
ascii_upload_enable=YES(NO)
与上一个设定类似的，只是这个设定针对上传而言！预设是NO。
ascii_upload_enable=NO
ascii_download_enable=NO

#通过搭配能实现以下几种效果： 
①当chroot_list_enable=YES，chroot_local_user=YES时，在/etc/vsftpd.chroot_list文件中列出的用户，可以切换到其他目录；未在文件中列出的用户，不能切换到其他目录。 
②当chroot_list_enable=YES，chroot_local_user=NO时， 
在/etc/vsftpd.chroot_list文件中列出的用户，不能切换到其他目录；未在文件中列出的用户，可以切换到其他目录。 
③当chroot_list_enable=NO， 
chroot_local_user=YES时，所有的用户均不能切换到其他目录。 
④当chroot_list_enable=NO， 
chroot_local_user=NO时，所有的用户均可以切换到其他目录。

chroot_local_user=YES
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd/chroot_list

#这个是pam模块的名称，我们放置在/etc/pam.d/vsftpd
pam_service_name=vsftpd

#当然我们都习惯支持TCP Wrappers的啦！
tcp_wrappers=YES

#不添加下面这个会报错：500 OOPS: vsftpd: refusing to run with writable root inside chroot()
allow_writeable_chroot=YES

#ftp的端口号
listen_port=60021

#启动被动式联机(passivemode)
pasv_enable=YES
#上面两个是与passive mode 使用的 port number 有关，如果您想要使用65400 到65410 这 11 个 port 来进行被动式资料的连接，可以这样设定
pasv_min_port=65400
pasv_max_port=65410

#FTP访问目录
local_root=/data/ftp/
```

### 创建用户

```shell
$ useradd -d /data/ftp/ -s /sbin/nologin ftpuser #以这种方式创建遇到权限问题，不知道为什么。
$ passwd ftpuser
```



### 日志

```shell
Thu Jan  2 22:15:57 2020 1 ::ffff:192.168.89.210 0 /mnt/nfs1/ftpservice/045645645611/20200103/1/1578021356858/I20200103-102449PAN1PC.avi b _ i r bd-ftp-user ftp 0 * i
```

| 记录                                                         | 含义                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Thu Jan  2 22:15:57 2020                                     | FTP传输时间                                                  |
| 1                                                            | 传输文件所用时间。单位/秒                                    |
| ::ffff:192.168.89.210                                        | ftp客户端名称/IP                                             |
| 0                                                            | 传输文件大小。单位/Byte                                      |
| /mnt/nfs1/ftpservice/045645645611/20200103/1/1578021356858/I20200103-102449PAN1PC.avi | 传输文件名，包含路径                                         |
| b                                                            | 传输方式： a以ASCII方式传输; b以二进制(binary)方式传输;      |
| _                                                            | 特殊处理标志位："_"不做任何处理；"C"文件是压缩格式；"U"文件非压缩格式；"T"文件是tar格式； |
| i                                                            | 传输方向："i"上传；"o"下载；                                 |
| r                                                            | 用户访问模式：“a”匿名用户；"g"访客模式；"r"系统中用户;       |
| bd-ftp-user                                                  | 登录用户名                                                   |
| ftp                                                          | 服务名称，一般都是ftp                                        |
| 0                                                            | 认证方式："0"无；"1"RFC931认证；                             |
| *                                                            | 认证用户id，"*"表示无法获取id                                |
| i                                                            | 完成状态："i"传输未完成；"c"传输已完成；                     |

