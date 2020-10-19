--- 
layout: article 
title: nginx+docker 夫妻双双把家还
tags: nginx docker
categories: other
keywords: docker, nginx
intro: 一个前端切图仔承受着他不该承受的痛苦...
---

# {{page.title}}

----
#### 引言 ####

对于那些 nginx 入门教程就来一堆 "反向代理"/"负载均衡"/"动静分离" 的, 作为一名前端切图仔, 恐怕还没入门就直接放弃了.....

----
#### nginx 知识 ####

首先要知道, 安装完 nginx 后, 它会生成 /usr/share/nginx/html/index.html, 而 index.html 里面的内容, 
就是我们启动 nginx 后看到的默认界面.

其次, 它还会生成 /etc/nginx/conf.d/default.conf, default.conf 是 nginx 的默认配置文件, 比如它就把  /usr/share/nginx/html 作为默认网站根目录.
所以默认的, 安装完 nginx 启动后, 就会把上面提到的 index.html 显示出来.

我们可以通过修改 default.conf 配置, 把 /www 作为我们的网站根目录.

<abc>/etc/nginx/conf.d/default.conf(默认)</abc>
```
server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

----

#### docker 知识 ####
docker pull nginx  拉取 nginx 镜像

docker run nginx  运行 nginx 镜像

<abc>bin/bash</abc>

```bash
mkdir -p ~/projects/learning/docker/www ~/projects/learning/docker/conf.d
docker pull nginx  
docker run -d -p 8080:80 -v ~/projects/learning/docker/www:/www -v ~/projects/learning/docker/conf.d:/etc/nginx/conf.d  nginx

```

执行完上述命令, 就可以通过修改本地文件实现修改 nginx 配置的目的. 比如, 在 ~/projects/learning/docker/conf.d 新建  default.conf, 在 ~/projects/learning/docker/www 新建 index.html

<abc>~/projects/learning/docker/conf.d/default.conf</abc>
```
server {
    listen       80;
    server_name  localhost;
    location / {
        root   /www;
        index  index.html index.htm;
    }
}

```
----
#### 结语 ####
又是快乐的一天.

---