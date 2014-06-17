Cache 配置
=========


#### Disk Cache
* 配置Disk Cache(```storage.config```)

    1、编辑```/etc/trafficserver/storage.config```
    <br> 目录配置方式

        # 目录  分配大小
        /var/cache/trafficserver 256M

    <br> 裸盘配置方式
    
        # 直接填写磁盘进去，一行一个
        /dev/sdb
        /dev/sdc
        /dev/sdd
        
      
    2、重启应用
    
        /etc/init.d/trafficserver restart

* 磁盘分区存储(```volume.config```、```hosting.config```)
    <br> volume.config
    
#### Ram Cache
* Ram 分区大小
  <br> 配置： ```records.config```
  <br> 单位： byte
     
         proxy.config.cache.ram_cache.size
  <br>  

* 最大Ram Cache 对象
  <br> 配置： ```records.config```
  <br> 单位： byte
        
        proxy.config.cache.ram_cache_cutoff
  <br>
  
* Ram 淘汰算法
  <br> 配置： ```records.config```
  <br> ### 0 - CLFUS
  <br> ### 1 - LRU
  
        proxy.config.cache.ram_cache.algorithm
  <br>
    