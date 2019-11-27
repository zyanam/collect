#### 修改端口使用限制

- HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\tcpip\Parameters\TcpTimedWaitDelay to 30
- HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\tcpip\Parameters\MaxUserPort to 65534

#### 统计端口连接数

```powershell
netstat -an|find "xxx.xxx.xxx.xxx:端口" | find  "ESTABLISHED" /c
```

#### 抓包命令

```powershell
netsh trace start capture=yes traceFile="c:\\snmp.etl" overwrite=yes correlation=no protocol=tcp ipv4.address=11.11.11.11

netsh trace stop

#参考：https://www.microsoft.com/en-us/download/confirmation.aspx?id=44226
```

```powershell
netsh trace start capture=YES report=YES persistent=YES
netsh trace stop
```





