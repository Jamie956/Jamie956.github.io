## Service start/stop/delete
```
sc delete <service>
net stop <service>
net start <service>
```

## 查看 port
```shell
netstat -ano | findstr <port>
tasklist|findstr <PID>
```

## 查看win10系统激活情况
```
slmgr.vbs -xpr //win10查看系统激活情况
```