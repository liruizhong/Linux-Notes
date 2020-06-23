## nginx 安全优化之隐藏版本号
安全一直是互联网不可忽视的问题，今天我们就学习一下如何隐藏 nginx 的版本信息

别人不清楚你的版本信息，就无法使用针对 nginx 的漏洞进行攻击

```
# 查看 nginx 版本
[root@linuxidc ~]# curl -I 192.168.1.7|grep Server            
Server: nginx/1.8.1 #nginx的软件版本信息

# 51CTO的web容器版本
[root@linuxidc nginx-1.8.1]# curl -I www.51cto.com|grep Server
Server: Tengine   #隐藏了版本信息
隐藏了版本，让版本漏洞无法使用
```
### 修改 nginx 源代码来隐藏 nginx 版本信息
#### 1、文件： "nginx-1.8.1/src/core/nginx.h" 12行-23行
```
# 原配置文件
[root@linuxidc tools]# sed -n '12,23p' nginx-1.8.1/src/core/nginx.h  
#define nginx_version      1008001
#define NGINX_VERSION      "1.8.1"  #修改想要显示的版本如：2.2.23
#define NGINX_VER          "nginx/" NGINX_VERSION #将nginx修改成想要显示的软件名称
#ifdef NGX_BUILD
#define NGINX_VER_BUILD    NGINX_VER " (" NGX_BUILD ")"
#else
#define NGINX_VER_BUILD    NGINX_VER
#endif
#define NGINX_VAR          "NGINX"     #将nginx修改成想要显示的软件名称（Evan Web Server）
#define NGX_OLDPID_EXT     ".oldbin"

# 修改后
[root@linuxidc tools]# sed -n '12,23p' nginx-1.8.1/src/core/nginx.h
#define nginx_version      1008001
#define NGINX_VERSION      "2.2.23"             #版本修改为2.2.23
#define NGINX_VER          "EWS/" NGINX_VERSION #nginx修改为EWS
#ifdef NGX_BUILD
#define NGINX_VER_BUILD    NGINX_VER " (" NGX_BUILD ")"
#else
#define NGINX_VER_BUILD    NGINX_VER
#endif
#define NGINX_VAR          "EWS"                #nginx修改为EWS
#define NGX_OLDPID_EXT     ".oldbin" 
```
#### 2、文件： "nginx-1.8.1/src/http/ngx_http_header_filter_module.c" 49行
```
# 源文件
[root@linuxidc tools]# sed -n '49p' nginx-1.8.1/src/http/ngx_http_header_filter_module.c 
static char ngx_http_server_string[] = "Server: nginx" CRLF;  #将nginx修改为想要的版本

#修改后
[root@linuxidc tools]# sed -n '49p' nginx-1.8.1/src/http/ngx_http_header_filter_module.c 
static char ngx_http_server_string[] = "Server: EWS" CRLF;   #将nginx修改为了EWS
```
#### 3、文件： "nginx-1.8.1/src/http/ngx_http_special_response.c" 29行
```
# 源文件
[root@linuxidc tools]# sed -n '21,31p' nginx-1.8.1/src/http/ngx_http_special_response.c
"<hr><center>nginx</center>" CRLF  #将nginx修改为想要的版本信息

# 修改后
[root@linuxidc tools]# sed -n '29p' nginx-1.8.1/src/http/ngx_http_special_response.c    
"<hr><center>EWS</center>" CRLF
```
