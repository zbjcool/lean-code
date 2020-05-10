代理：
A同学在大众创业、万众创新的大时代背景下开启他的创业之路，目前他遇到的最大的一个问题就是启动资金，于是他决定去找马云爸爸借钱，可想而知，最后碰一鼻子灰回来了，情急之下，他想到一个办法，找关系开后门，经过一番消息打探，原来A同学的大学老师王老师是马云的同学，于是A同学找到王老师，托王老师帮忙去马云那借500万过来，当然最后事成了。不过马云并不知道这钱是A同学借的，马云是借给王老师的，最后由王老师转交给A同学。这里的王老师在这个过程中扮演了一个非常关键的角色，就是代理，也可以说是正向代理，王老师代替A同学办这件事，这个过程中，真正借钱的人是谁，马云是不知道的，这点非常关键。 我们常说的代理也就是只正向代理，正向代理的过程，它隐藏了真实的请求客户端，服务端不知道真实的客户端是谁，客户端请求的服务都被代理服务器代替来请求，某些科学上网工具扮演的就是典型的正向代理角色。用浏览器访问 http://www.google.com 时，被残忍的block，于是你可以在国外搭建一台代理服务器，让代理帮我去请求google.com，代理把请求返回的相应结构再返回给我。
反向代理：
大家都有过这样的经历，拨打10086客服电话，可能一个地区的10086客服有几个或者几十个，你永远都不需要关心在电话那头的是哪一个，叫什么，男的，还是女的，漂亮的还是帅气的，你都不关心，你关心的是你的问题能不能得到专业的解答，你只需要拨通了10086的总机号码，电话那头总会有人会回答你，只是有时慢有时快而已。那么这里的10086总机号码就是我们说的反向代理。客户不知道真正提供服务人的是谁。


反向代理隐藏了真实的服务端，当我们请求 www.baidu.com 的时候，就像拨打10086一样，背后可能有成千上万台服务器为我们服务，但具体是哪一台，你不知道，也不需要知道，你只需要知道反向代理服务器是谁就好了，www.baidu.com 就是我们的反向代理服务器，反向代理服务器会帮我们把请求转发到真实的服务器那里去。Nginx就是性能非常好的反向代理服务器，用来做负载均衡。

两者的区别在于代理的对象不一样：正向代理代理的对象是客户端，反向代理代理的对象是服务端


### nginx 出现413 Request Entity Too Large问题的解决方法

上传weex文件出现 nginx: 413 Request Entity Too Large 错误。
原来nginx默认上传文件的大小是1M，可nginx的设置中修改。
解决方法如下：

1. 打开nginx配置文件 nginx.conf, 路径一般是：/usr/local/etc/nginx/nginx.conf。

2. 在http{}段中加入 client_max_body_size 20m; 20m为允许最大上传的大小。

3. 保存后重启nginx，问题解决。sudo nginx -s reload




nginx 安装
brew install nginx
或者用https://www.widlabs.com/article/mac-os-x-compile-install-nginx
nginx.config 在/usr/local/etc/nginx

停止nginx进程
ps -ef | grep nginx
第二列是pid
ps aux | grep nginx

sudo kill -9 pid

nginx 重启
sudo nginx -s reload

#停止
sudo sbin/nginx -s stop


#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    #虚拟主机的配置
    server {
        #监听端口
        listen       80;
        #域名可以有多个，用空格隔开
        server_name  localhost;

        #后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        #charset koi8-r;

        #access_log  logs/host.access.log  main;


        #对 "/" 启用反向代理
        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        location /w/ {
          proxy_set_header X-Real-Location /w/;
             proxy_pass    http://127.0.0.1:5380/w/;
             proxy_redirect default ;
        }
        location /fauna/{
             proxy_pass    http://127.0.0.1:8080/fauna/;
             proxy_redirect default ;
        }
        location /fauna/w/{
          proxy_set_header X-Real-Location /fauna/w/;
             proxy_pass    http://127.0.0.1:5380/w/;
             proxy_redirect default ;
        }

        location /ddadmin/ {
            proxy_pass  http://127.0.0.1:5480/ddadmin/;
            proxy_redirect default ;
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


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
    include servers/*;
}


nginx的配置放在nginx.conf文件中，一般我们可以使用以下命令查看服务器中存在的nginx.conf文件。

locate nginx.conf
/usr/local/etc/nginx/nginx.conf
/usr/local/etc/nginx/nginx.conf.default

如果服务器中存在多个nginx.conf文件，我们并不知道实际上调用的是哪个配置文件，因此我们必须找到实际调用的配置文件才能进行修改。 


查看nginx实际调用的配置文件
1.查看nginx路径
ps aux|grep nginx
root              352   0.0  0.0  2468624    924   ??  S    10:43上午   0:00.08 nginx: worker process  
root              232   0.0  0.0  2459408    532   ??  S    10:43上午   0:00.02 nginx: master process /usr/local/opt/nginx/bin/nginx -g daemon off;  
root             2345   0.0  0.0  2432772    640 s000  S+    1:01下午   0:00.00 grep nginx

nginx的路径为：/usr/local/opt/nginx/bin/nginx 


2.查看nginx配置文件路径
使用nginx的 -t 参数进行配置检查，即可知道实际调用的配置文件路径及是否调用有效。

/usr/local/opt/nginx/bin/nginx -t
nginx: the configuration file /usr/local/etc/nginx/nginx.conf syntax is ok
nginx: configuration file /usr/local/etc/nginx/nginx.conf test is successful


https://blog.csdn.net/fdipzone/article/details/77199042

