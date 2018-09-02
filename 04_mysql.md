# Common

Login

```
mysql -u isucon -p
Enter password: isucon
```

Restart MySQL

```
sudo /etc/init.d/mysql restart
```

設定ファイル優先度確認

```
mysql --help | grep my.cnf
```

Backup and dump for MySQL

```
scp ubuntu@ec2-54-95-78-211.ap-northeast-1.compute.amazonaws.com:~/for-dump/mysqld.cnf ~
```

Status Confirmation

```
> show variables like 'version';
> show status;
> show databases;
> use isubata;
> show tables;

> SHOW variables LIKE "%max_connections%";
> SHOW variables LIKE "%open_files_limit%";
```

起動ログの確認

```
sudo journalctl -u mysql
```

# 以降 実際にやること

# ダンプ => ローカル転送(バックアップ) => (ローカルにDBリストア)

isubata <= DB名
isucon <= User名


ダンプする

```
$ mysqldump -u isucon isubata | gzip > isubata.dump.gz
```

ローカルPC上でローカルにダンプしたものを転送する

```
$ scp ubuntu@ec2-54-95-78-211.ap-northeast-1.compute.amazonaws.com:~/isubata.dump.gz ~
```

(ローカルにMySQLのDBを立てる)

```
$ mysql -uroot
mysql> create database isubata;
```

(ローカル上でリストアする)

```
gzcat isubata.dump.gz | mysql -u root isubata
```

# Max Connection

Too many connections error が出る時は、max connections を大きくする。 ただし、5_os にしたがって「OS 側での connection, open file limit の設定の更新」をする必要がある（OS側で制限されてると、mysql 側ではその制限の範囲内でしか設定を変えれない）

OS側の設定

```bash
# For max connection for MySQL
echo '*    soft nofile 65535' >> /etc/security/limits.conf
echo '*    hard nofile 65535' >> /etc/security/limits.conf
echo 'root soft nofile 65535' >> /etc/security/limits.conf
echo 'root hard nofile 65535' >> /etc/security/limits.conf
echo 'session    required     pam_limits.so' >> /etc/pam.d/common-session
echo 'session    required     pam_limits.so' >> /etc/pam.d/common-nonsession
```

MySQL側の設定

```
# MySQLへログイン
> set global max_connections=10000;
> show global variables like '%connection%';
```

# スローログ

設定の確認

```
> show variables like 'slow%';
```

設定する

```
> set global slow_query_log = 1;
> set global long_query_time = 0;
> set global slow_query_log_file = "/tmp/slow.log";
```
