# 安装手册补充

1. 需要创建文件夹并进行授权

```
mkdir log
mkdir runtime
chmod -R 777 log
chmod -R 777 runtime
```

2. 修改 .env 会打开安装


3. 在默认的部署情况下，是不会触发安装的，这个就是为什么官方的文档里有一条是需要配合前台项目的原因

4. 需要授权的目录

```
runtime
    cache
    sess
    log

log

```

5. Nginx下使Thinkphp URL模式支持PATHINFO和REWRITE

https://msd.misuland.com/pd/3113337336633496000

如果是单独的 Nginx 需要增加配置

```
#pathinfo开始
fastcgi_split_path_info ^(.+?\.php)(/.*)$;
set $path_info $fastcgi_path_info;
fastcgi_param PATH_INFO       $path_info;
try_files $fastcgi_script_name =404;
#pathinfo结束
```

增加完之后变成

```
  location ~ [^/]\.php(/|$) {
    #fastcgi_pass remote_php_ip:9000;
    fastcgi_pass unix:/dev/shm/php-cgi.sock;
    fastcgi_index index.php;
    include fastcgi.conf;
    #pathinfo开始
    fastcgi_split_path_info ^(.+?\.php)(/.*)$;
    set $path_info $fastcgi_path_info;
    fastcgi_param PATH_INFO       $path_info;
    try_files $fastcgi_script_name =404;
    #pathinfo结束
  }

```
