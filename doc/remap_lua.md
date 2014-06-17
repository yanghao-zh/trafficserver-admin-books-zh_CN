Remap与Lua
==========
在新版本里，lua已可嵌入到remap.config里，支持更灵活的基于http header编程

ts-lua 接口文档https://github.com/portl4t/ts-lua

### 配置说明
Remap 可支持配置Lua Hook点的五个阶段

* do_remap 

        remap前阶段，该阶段可以修改存储key，拒绝服务等等操作

* send_request

        回源发送request 阶段，该阶段可以读取、修改回源request header

* read_response

        读取回源response 阶段，该阶段可以读取、修改回源接收的response header

* send_response

        发送给client response 阶段，该阶段可以读取、修改发送给用户response header

* cache_lookup_complete

        Cache 读取阶段，该阶段可以判断读出的Cache状态，读取Cache中的response header等
        

### 配置示例

> 注： 一个remap 里可以使用多个状态，多个状态配合成一个脚本使用

* do_remap

        http www.taobao.com {
            map / http://www.taobao.com.inner.taobao.com {
                script do_remap {
                    -- 判断Useragent 做跳转
                    ts.ctx['is_forbidden'] = 0
                    local uagent = ts.client_request.header['User-Agent']
                    if string.find(uagent, 'haoyu') then
                        ts.ctx['is_forbidden'] = 1
                        ts.http.set_resp(302, "302 Moved")
                        return TS_LUA_REMAP_DID_REMAP_STOP
                    end
                }
                
                script send_response {
                    if ts.ctx['is_forbidden'] == 1 then
                        ts.client_response.header['Location'] = 'http://err.haoyu.com/'
                        return 0
                    end
                }
            }
        }
        

* send_request
        
        http www.taobao.com {
            map / http://www.taobao.com.inner.taobao.com {
                script send_request {
                    -- 把回源request Host 改成www.tmall.com
                    ts.client_request.header['Host'] = 'www.tmall.com'
                }
            }
        }
        

* read_response
        
        http www.taobao.com {
            map / http://www.taobao.com.inner.taobao.com {
                script read_response {
                    -- 按照Accept-Test 做多副本缓存
                    ts.server_response.header['Vary'] = "Accept-Test"
                }
            }
        }
        
* send_response
        
        http www.taobao.com {
            map / http://www.taobao.com.inner.taobao.com {
                script send_response {
                    -- 添加Response Header
                    ts.client_response.header['Test'] = 'yes'
                }
            }
        }
        
* cache_lookup_complete

        http www.taobao.com {
            map / http://www.taobao.com.inner.taobao.com {
                script cache_lookup_complete {
                    -- 获得Cache 状态
                    local cache_status = ts.http.get_cache_lookup_status()
                    if cached_status == TS_LUA_CACHE_LOOKUP_HIT_FRESH then
                        ts.ctx['cstatus'] = 'hit'
                    else
                        ts.ctx['cstatus'] = 'miss'
                    end
                }
                
                script send_response {
                    -- 添加Cache状态头
                    ts.client_response.header['Cache-Status'] = ts.ctx['cstatus']
                }
            }
        }
        
