热点扩散模块
=========

### 使用场景

在一致性Hash 的场景里，大量的大文件下载，如软件包、升级包下载，相同的URI全部Hash到一台单机上，这时会造成单机网卡流量被打满，IO被打满，而其他机器负载却相对较低，针对这一场景需要用到热点扩散模块，将热点文件扩散到其他较闲的机器上。

### 配置参数

* 热点URL最大条目
   <br> 配置： ```records.config```
   <br> 单位： 个数
   <br> 说明： 最大不能超过100，0表示不启用热点发现功能。建议不要超过10条，比如可以设置为5
   
        proxy.config.http.hoturls.max_count
   <br>
   
* 检测时间间隔
   <br> 配置： ```records.config```
   <br> 单位： 秒
   <br> 说明： 检测的时间间隔，至少为1秒，最多不能超过300秒，建议设置为10秒
   
        proxy.config.http.hoturls.detect_interval_secs
   <br>
   
* 检测流量阈值
   <br> 配置： ```records.config```
   <br> 单位： bps
   <br> 说明： 对外输出流量超过多大后才启用热点检测，与网卡带宽及集群机器数有关
   
        proxy.config.http.hoturls.detect_on_bps
   <br>
   
* 检测输出、输入流量比阈值
   <br> 配置： ```records.config```
   <br> 单位： 百分比，类型：浮点数
   <br> 说明： cluster输出流量和对外输出流量出现不均衡的情况下，才启用热点检测。比如设置为0.50，表示二者流量相差一倍的情况下启用。设置为0.25表示二者流量相差3倍的情况下才启用检测功能
   
        proxy.config.http.hoturls.detect_on_bps_ratio
   <br>
   
* 检测单个URL流量占比阈值
   <br> 配置： ```records.config```
   <br> 单位： 百分比，类型：浮点数
   <br> 说明： 单个URL流量占比超过阈值，则入选为热点URL。比如可以设置为0.10，表示单个URL的流量占比超过10%则入选
   
        proxy.config.http.hoturls.single_url_bps_ratio
   <br>
   
* 检测多个URL流量占比阈值
   <br> 配置： ```records.config```
   <br> 单位： 百分比，类型：浮点数
   <br> 说明： 多条URL流量占比超过阈值，则这几条URL均入选为热点URL。设置为0.00表示不按多条URL累计筛选
   
        proxy.config.http.hoturls.multi_url_bps_ratio
   <br>
   
* 热点url保存时间
   <br> 配置： ```records.config```
   <br> 单位： 天数，类型：整形
   <br> 说明： 热点URL保存天数。这个是配合内容管理PURGE的，目的是确保PURGE干净。最小值为1天，最大值为31天。通常设置为1天即可，数据文件在/var/run/trafficserver/hoturl.binlog
   
        proxy.config.http.hoturls.keep_days
   <br>