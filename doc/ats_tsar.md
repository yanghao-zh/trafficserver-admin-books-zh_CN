ATS监控数据采集
============

### Tsar 介绍
Tsar是淘宝的一个用来收集服务器系统和应用信息的采集报告工具，如收集服务器的系统信息（cpu，mem等），以及应用数据，收集到的数据存储在服务器磁盘上，可以随时查询历史信息，也可以将数据发送到nagios报警。

Tsar能够比较方便的增加模块，只需要按照tsar的要求编写数据的采集函数和展现函数，就可以把自定义的模块加入到tsar中

##### Tsar 安装

开源版

```bash
#git clone git://github.com/kongjian/tsar.git
#cd tsar
#make
#make install
```

##### Tsar配置ATS模块

```vim /etc/tsar/tsar.conf```

```bash
#debug_level(INFO DEBUG WARN ERROR FATAL)
debug_level FATAL
#[module] on/off to enable mod
mod_swap on
mod_tcp on
mod_io on
mod_pcsw on
mod_partition on
mod_tcpx on
mod_load on
mod_percpu on
mod_ts_client on  #ts前端信息
mod_ts_os on      #ts回源信息
mod_ts_cache on   #ts Cache信息
mod_ts_err on     #ts err信息
mod_ts_storage on #ts 存储信息
mod_ts_conn on    #ts 连接信息
mod_ts_codes on   #ts http_code

#output type:file,nagios,db
output_interface file

#[output_file] original data to store
output_file_path /var/log/tsar.data

#[output_stdio] these mod will be show as using tsar
output_stdio_mod mod_swap,mod_partition,mod_cpu,mod_mem,mod_traffic,mod_load,mod_tcp,mod_udp,mod_tcpx,mod_pcsw,mod_io,mod_ts_client,mod_ts_os,mod_ts_cache,mod_ts_err,mod_ts_storage,mod_ts_conn,mod_ts_codes
```

##### TS 监控数据说明
```bash
#tsar --ts
Time           --------------------ts------------------ 
Time              qps    cons     Bps      rt     rpc   
28/01/14-16:10   3.4K    1.2K   87.8M   64.66    2.87   
28/01/14-16:15   3.4K    1.2K   89.8M   61.59    2.83   
28/01/14-16:20   3.5K    1.2K   87.3M   63.80    2.82 
```
- qps  处理请求数/s
- cons 新建连接/s
- Bps  流量/s
- rt   响应时间(ms)
- rpc  连接复用率(平均每个连接服务多少个请求数)

------------------------------------------
```bash
#tsar --ts_os
Time           --------------ts_os------------- 
Time              qps    cons    mbps     rpc   
28/01/14-16:15 198.75    6.54    5.5M   30.39   
28/01/14-16:20 199.32    6.73    5.1M   29.62   
28/01/14-16:25 198.24    6.74    5.1M   29.40
```
- qps  回源请求数/s
- cons 回源新建连接/s
- mbps 回源流量
- rpc  回源连接复用

------------------------------------------
```bash
#tsar --ts_cache
Time           -------------ts_cache----------- 
Time              hit  ramhit    band  ssdhit   
28/01/14-16:15  94.50   84.80   91.70    0.00   
28/01/14-16:20  94.70   84.70   94.70    0.00   
28/01/14-16:25  94.60   84.80   93.80    0.00
```
- hit     请求命中率
- ramhit  内存命中率
- band    字节命中率
- ssdhit  分级存储，ssd命中率

------------------------------------------
```bash
#tsar --ts_storage
Time           ------------ts_storage---------- 
Time              ram    disk    objs    size   
28/01/14-16:15  24.0G    4.3T  130.1M   34.1K   
28/01/14-16:20  24.0G    4.3T  130.1M   34.1K   
28/01/14-16:25  24.0G    4.3T  130.2M   34.1K 
```
- ram    内存使用大小
- disk   磁盘使用大小
- objs   object数量
- size   平均object大小

------------------------------------------

```bash
#tsar --ts_conn
Time           -------------------------ts_conn------------------------ 
Time           client  server   cache    open   c_act   t_cli   t_srv   
28/01/14-16:20  96.3K  167.00   47.00   96.5K  168.00  268.00   13.00   
28/01/14-16:25  94.8K  149.00   49.00   94.9K  166.00  273.00   16.00   
28/01/14-16:30  96.4K  184.00   35.00   96.6K  127.00  261.00   16.00 
```
- client   前端连接数,#对文件句柄要求较高
- server   回源连接数
- cache    读取Cache的连接数
- open     总计打开的连接
- c_act    活跃的client连接数
- t_cli    正在传输的client连接数，最占用内存的，一个连接占用16k
- t_srv    正在传输的回源连接数