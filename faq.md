# FAQ



## Kimpi0进程为何占用了大量CPU资源？

Linux系统kipmi0进程利用CPU空闲时间做接口调节操作，实际不会影响其他进程的性能。

若您希望限制该进程，可使用以下命令（暂时生效）：

``` 
/etc/modprobe.d/ipmi.conf  
```

或在/etc/modprobe.d/ipmi.conf添加以下内容（永久生效）：

    options ipmi_si kipmid_max_busy_us=100

## 物理机云主机支持虚拟化吗？

支持。

但当前物理云网关限制，不支持桥接的网络模式，仅支持通过NAT模式配置虚拟化网络。
