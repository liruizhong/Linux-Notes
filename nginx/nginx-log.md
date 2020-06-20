## nginx 日志按天切割

### 方法1
在配置文件中获取时间然后赋值给变量，用变量拼接日志文件名，达到日期更改文件更改的效果
```
http {
      log_format default_format '$remote_addr - $remote_user [$time_iso8601] "$request" '
                                '$status $body_bytes_sent "$http_referer" '
                                '"$http_user_agent" "$http_x_forwarded_for" "$request_body"';

      server {
          listen 443 ssl;
          ... ...
          if ($time_iso8601 ~ '(\d{4}-\d{2}-\d{2})') {
            set $tttt $1;
          }

          access_log /log/blog_access_$tttt.log;
      }
}
```
