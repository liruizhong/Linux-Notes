## nginx 启用目录索引, 显示文件列表

![nginx](https://user-gold-cdn.xitu.io/2020/4/29/171c4e967b9714ed?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

在nginx中，如果特定目录中没有index.html 文件，则默认会返回 404 Not Found 的错误。

但是，Nginx 自动索引模块提供了一种**自动生成列表**的方法，添加自动索引非常容易，

使用 `autoindex on` 即可。
```
server {
        listen   80;
        ... ...

        location /somedir {
           autoindex on;
        }
}
```

除了简单地使用自动索引打开或关闭之外，还可以对其做其他的配置，包括：
* autoindex_exact_size; 显示输出的确切文件大小，还是最接近的KB，MB或GB。有2个选项：on/off。
* autoindex_format;     该指令指定Nginx索引列表应以什么格式输出。该指令有4个选项：html/xml/json/jsonp。
* autoindex_localtime;  该指令指定目录列表的时间应该输出为本地时间还是UTC。该指令有2个选项：on/off。

使用这几个配置后配置内容类似于如下内容：
```
location /somedirectory/ {
    autoindex on;
    autoindex_exact_size off;
    autoindex_format     html;
    autoindex_localtime  on;
}
```

### 参考链接

* [Enabling the Nginx Directory Index Listing - KeyCDN](https://www.keycdn.com/support/nginx-directory-index)



