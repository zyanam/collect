### redis.conf

```properties
bind 0.0.0.0

#从节点设置
slaveof  192.168.89.235  6379

port 6379
daemonize yes
pidfile /var/run/redis_6380.pid
#指定log文件
logfile "/opt/redis/logs/2.log"	
dir /opt/data/redis1
```



### sentinel.conf

```properties
# port <sentinel-port>
# The port that this sentinel instance will run on
port 26379

# By default Redis Sentinel does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis-sentinel.pid when
# daemonized.
daemonize yes

# When running daemonized, Redis Sentinel writes a pid file in
# /var/run/redis-sentinel.pid by default. You can specify a custom pid file
# location here.
pidfile /var/run/redis-sentinel1.pid

# Specify the log file name. Also the empty string can be used to force
# Sentinel to log on the standard output. Note that if you use standard
# output for logging but daemonize, logs will be sent to /dev/null
logfile "/opt/logs/sentinel1.log"

# dir <working-directory>
# Every long running process should have a well-defined working directory.
# For Redis Sentinel to chdir to /tmp at startup is the simplest thing
# for the process to don't interfere with administrative tasks such as
# unmounting filesystems.
dir /opt/logs/sentinel-data/1



```

