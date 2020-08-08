##系统基本信息查看

``` uname -a                查看内核/操作系统/CPU信息```
``` head -n 1 /etc/issue    查看操作系统版本```
``` cat /proc/cpuinfo       查看CPU信息```
``` hostname                查看计算机名```
``` lspci -tv               列出所有PCI设备```
``` lsusb -tv               列出所有USB设备```
``` lsmod                   列出加载的内核模块```
``` env                     查看环境变量```

##资源
``` free -m                查看内存使用量和交换区使用量```
``` df -h                  查看各分区使用情况```
``` du -sh <目录名>        查看指定目录的大小```
``` grep MemTotal /proc/meminfo   查看内存总量```
``` grep MemFree /proc/meminfo    查看空闲内存量```
``` uptime                 查看系统运行时间、用户数、负载```
``` cat /proc/loadavg      查看系统负载```

##磁盘和分区
``` mount | column -t      查看挂接的分区状态```
``` fdisk -l               查看所有分区```
``` swapon -s              查看所有交换分区```
``` hdparm -i /dev/hda     查看磁盘参数(仅适用于IDE设备)```
``` dmesg | grep IDE       查看启动时IDE设备检测状况```

##网络
``` ifconfig               查看所有网络接口的属性```
``` iptables -L            查看防火墙设置```
``` route -n               查看路由表```
``` netstat -lntp          查看所有监听端口```
``` netstat -antp          查看所有已经建立的连接```
``` netstat -s             查看网络统计信息```

##进程
``` ps -ef                 查看所有进程```
``` top                    实时显示进程状态```

##用户
``` w                        查看活动用户```
``` id <用户名>              查看指定用户信息```
``` last                    查看用户登录日志```
``` cut -d: -f1 /etc/passwd   查看系统所有用户```
``` cut -d: -f1 /etc/group    查看系统所有组```
``` crontab -l                查看当前用户的计划任务```

##服务
``` chkconfig --list       列出所有系统服务```
``` chkconfig --list | grep on    列出所有启动的系统服务```

##程序
``` rpm -qa                查看所有安装的软件包```

##查看CPU信息（型号）
```shell
cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c`
     8  INTEL(R)  E5-2690 v3 CPU          2.60GHZ
(看到有8个逻辑CPU, 也知道了CPU型号)
```

```shell
cat /proc/cpuinfo | grep physical | uniq -c
      4 physical id      : 0
      4 physical id      : 1
(说明实际上是两颗4核的CPU)
```

``` getconf LONG_BIT
   32
(说明当前CPU运行在32bit模式下, 但不代表CPU不支持64bit)

```shell
cat /proc/cpuinfo | grep flags | grep ' lm ' | wc -l
   8
(结果大于0, 说明支持64bit计算. lm指long mode, 支持lm则是64bit)
```

###cpu详细信息, 不过大部分我们都不关心而已.
```shell
dmidecode | grep 'Processor Information'
```
##查看内存信息
``` cat /proc/meminfo```

##查看操作内核版本
```
cat /proc/version
Linix version 2.6.18-400.1.1.e15 2014/12/14
```
```shell
uname -a
Linix version 2.6.18-400.1.1.e15 2014/12/14
```
##查看操作系统版本
```shell
lsb_release -a
Distributor ID: Red Hat Enterprise Linux Server
Description: Red Hat Enterprise Linux Server release 5.11(Tikanga)
Release: 5.11
适用于所有的Linux Release版本，包括Redhat、SuSE、Debian等。
```

```shell
cat /etc/issue | grep Linux
Red Hat Enterprise Linux AS release 4 (Nahant Update 5)
```

```shell
cat /etc/redhat-release
Red Hat Enterprise Linux AS release 4 (Nahant Update 8)
```


##查看机器型号
```dmidecode | grep "Product Name" ```

##查看网卡信息
``` dmesg | grep -i eth ```
