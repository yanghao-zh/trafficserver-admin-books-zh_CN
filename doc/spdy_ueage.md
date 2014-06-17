SPDY 使用说明
=========

spdy说明：http://www.chromium.org/spdy

#### 一、安装插件

> 依赖包
<br>t-ts-fetcher
<br>t-spdylay

    yum insall t-ts-spdy -b test -y
    
#### 二、配置插件

    vim /etc/trafficserver/plugin.config
    /usr/lib64/trafficserver/plugins/libtsspdy.so
    
#### 三、重启ATS

    /etc/init.d/trafficserver restart
    
#### 四、验证

    /home/a/bin/spdycat -v -n http://127.0.0.1/ --no-tls -3 -H "Host:www.taobao.com"
    
![ATS SPDY USAGE](https://github.com/yanghao-zh/trafficserver-admin-books-zh_CN/raw/master/img/spdy_usage.jpg)