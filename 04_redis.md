# Redis

## Objective
Understand redis installation and usage.


### install

```
# Ubuntu / Debian:
sudo apt-get install build-essential tcl wget

# RHEL-ish systems:
sudo yum groupinstall 'Development Tools'
sudo yum install tcl wget
```

```
wget http://download.redis.io/releases/redis-5.0-rc4.tar.gz

tar -xvf redis-5.0-rc4.tar.gz

cd redis-5.0-rc4
make
sudo make install
cd utils/
./install_server.sh
```

start

```
sudo systemctl start redis_6379

```

Enable
```
sudo systemctl enable redis_6379
```


### Cli

```
redis-cli
> set foo bar
> get foo
```

### Golang client

https://github.com/go-redis/redis

Get/set

Get Range

HGet/HSet for structure

How to store/retrieve list
