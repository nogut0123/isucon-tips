# Measurement


## OS

Debian
```
cat /etc/*_version
```

Red Hat
```
cat /etc/*-release
```

## middleware
MySQL
```
mysql -V
```

mysql latest is now 8.

Nginx

```
nginx -v
```
the lastest version is now 1.15


Redis

```
redis-server --version
```

apache
```
httpd -v
```


## language

`go version`

the latest 1.10


## Machine spec

`cat /proc/cpuinfo` - Displays CPU info

`cat /proc/meminfo` - Displays memory info

`cat /proc/swaps` - Displays swap info


https://www.tecmint.com/commands-to-monitor-swap-space-usage-in-linux/


## profiler

markelel ???


## network bandwidth

iperf
iperf works as Server and Client model. Both server and client have be installed.


```
# Debian
sudo apt-get install iperf3

# RedHat
sudo dnf install iperf3
sudo yum install iperf3
```

Start Server
```
iperf3 -s
```

Start Celient
```
iperf3 -c 192.168.149.69
```

```
iperf -c 192.168.149.69 -P 3
```

https://www.cyberciti.biz/faq/how-to-test-the-network-speedthroughput-between-two-linux-servers/


## OS resource usage

glances

```
curl -L https://bit.ly/glances | /bin/bash
```


htop
```
yum install htop
apt install htop
```


vmstat


https://orebibou.com/2015/07/vmstat%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%81%A7%E8%A6%9A%E3%81%88%E3%81%A6%E3%81%8A%E3%81%8D%E3%81%9F%E3%81%84%E4%BD%BF%E3%81%84%E6%96%B98%E5%80%8B/


## Nginx

### alb

install
download binary
https://github.com/tkuchiki/alp/releases

nginx log format

```
log_format ltsv "time:$time_local"
                "\thost:$remote_addr"
                "\tforwardedfor:$http_x_forwarded_for"
                "\treq:$request"
                "\tstatus:$status"
                "\tmethod:$request_method"
                "\turi:$request_uri"
                "\tsize:$body_bytes_sent"
                "\treferer:$http_referer"
                "\tua:$http_user_agent"
                "\treqtime:$request_time"
                "\tcache:$upstream_http_x_cache"
                "\truntime:$upstream_http_x_runtime"
                "\tapptime:$upstream_response_time"
                "\tvhost:$host";

```
execute

`./alp --load profile.dat`

#### TODO: Execute in real env


### kataribe
https://github.com/matsuu/kataribe


log_format
```
log_format with_time '$remote_addr - $remote_user [$time_local] '
                     '"$request" $status $body_bytes_sent '
                     '"$http_referer" "$http_user_agent" $request_time';
access_log /var/log/nginx/access.log with_time;
```

kataribe.toml


add bundle

```
[[bundle]]
# how to aggregate
regexp = '^(GET|HEAD) /history/.*'
# put name
name = "GET /history/:channel_id"

```

execute

`cat /var/log/nginx/access.log | ./kataribe [-f kataribe.toml]`


#### TODO: Execute in real env


ref:
https://github.com/reikubonaga/isucon7-qualifier/pull/9/files




## mysql

https://github.com/reikubonaga/isucon7-qualifier/issues/4

### check tables

`show tables;`
`desc table_name; desc table_name;`

volume

`SELECT table_name, engine, table_rows, avg_row_length, floor((data_length+index_length)/1024/1024) as allMB, floor((data_length)/1024/1024) as dMB, floor((index_length)/1024/1024) as iMB FROM information_schema.tables WHERE table_schema=database() ORDER BY (data_length+index_length) DESC;`

### check indexes

`show index from tablename; show index from tablename; `
