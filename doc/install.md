### **一、安装软件包**
安装TS依赖如下软件包
- t-libevent
- t-openssl-libs
- t-spdylay
- tcl

从yum 安装

    yum install trafficserver -b current -y


### **二、常用文件说明**

##### 1.配置文件
TS 的配置文件全放在/etc/trafficserver/ 目录下，基本是分模块配置，一个配置文件代表一种功能
```
/etc/trafficserver/ #主配置目录
├── ae_ua.config
├── cache.config          #Cache规则配置
├── cluster.config        #cluster机器列表，自动配置
├── congestion.config     #限流配置
├── default_cluster.config
├── health_check.config   #七层健康监测配置
├── hosting.config        #磁盘分区分配配置
├── icp.config            #icp查询配置
├── ip_allow.config       #ip访问控制配置
├── log_hosts.config      #
├── logs_xml.config       #日志系统配置
├── mgr.cnf
├── parent.config         #parent回源配置
├── plugin.config         #插件配置
├── plugin.db             
├── proxy.pac
├── records.config        #TS控制参数配置
├── remap.config          #业务规则配置
├── snapshots
├── socks.config
├── splitdns.config       #splitdns配置
├── ssl_multicert.config  #SSL 配置
├── stats.config.xml      #统计信息配置
├── storage.config        #存储配置
├── update.config         #预热配置
├── vaddrs.config         #
├── volume.config         #磁盘分区配置
└── wpad.dat
```

##### 2.日志文件
运行日志，记录TS的运行状态和一些debug信息

    /var/log/trafficserver/traffic.out

Error log
访问出错的日志

    /var/log/trafficserver/error.log


##### 3.命令行工具
TS 提供命令行的操作工具，可以在命令行配置参数
<br>例：读取参数值

    traffic_line -r proxy.config.proxy_name

修改参数值

    traffic_line -s traffic_line -r proxy.config.proxy_name -v cn8


### **三、启动服务**

    /etc/init.d/trafficserver start


### **四、激活服务**

    /etc/init.d/trafficserver activate


