# Nginx

## Objective
Explain useful snipets for tunning.

https://github.com/reikubonaga/isucon7-qualifier/blob/master/nginx.conf


### Basic

syntax Check

```
sudo nginx -t
```

nginx restart

```
sudo service nginx restart
```

Log rotation manually

```
sudo mv "/var/log/nginx/access.log" "/var/log/nginx/`date +"%Y%m%d%H%M%S"`_access.log"
```


### Load Balancer
Split load to one for dedicated for serving static content, one for dedicated for application and database, the other for dedicated for app and cache server.

```
upstream web {
  server 127.0.0.1:5000 weight=2;
  server 192.168.101.2:5000 weight=4;
  server 192.168.101.3:5000 weight=4;
}
```

With this configuration every 10 new requests will be distributed across the application instances as the following: 2 requests will be directed to srv1, four request will go to srv2, and another four request — to srv3.


Specific path for specific upstream.

```
location /profile {
        proxy_set_header Host $http_host;
        proxy_pass http://web1;
}
```

### Cache

Static content should be cached

```
location /favicon.ico {
  expires 24h;
  add_header Cache-Control public;
}
location /fonts/ {
  expires 24h;
  add_header Cache-Control public;
}
location /js/ {
  expires 24h;
  add_header Cache-Control public;
}
location /css/ {
  expires 24h;
  add_header Cache-Control public;
}
location /icons/ {
  expires 24h;
  add_header Cache-Control public;
}
```


### Gzip

```
gzip on;
gzip_disable "msie6";
open_file_cache max=100000 inactive=60s;
```

```
gzip_min_length 10240;
gzip_proxied expired no-cache no-store private auth;
gzip_types text/plain text/css text/xml text/javascript application/x-javascript application/json application/xml;
```


### HTTP2

HTTP2 works only under HTTPS

```
listen 443 ssl http2;
```



### Events

```
# provides the configuration file context in which the directives that affect connection processing are specified.
events {
    # determines how much clients will be served per worker
    # max clients = worker_connections * worker_processes
    # max clients is also limited by the number of socket connections available on the system (~64k)
    worker_connections 4000;

    # optmized to serve many clients with each thread, essential for linux -- for testing environment
    use epoll;

    # accept as many connections as possible, may flood worker connections if set too low -- for testing environment
    multi_accept on;
}

```

https://github.com/denji/nginx-tuning


### Misc

```
sendfile on;
tcp_nopush on;
tcp_nodelay on;
types_hash_max_size 2048;
# server_tokens off;

# keepalive setting
keepalive_timeout 65;
keepalive_requests 500;

```
