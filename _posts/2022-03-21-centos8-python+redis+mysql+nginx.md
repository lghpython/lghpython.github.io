---
title: "centos8 安装python+redis+mysql+nginx"
published: true
---

- [- 9. 更改加密方式](#--9-更改加密方式)
- [编译安装 python](#编译安装-python)
  - [1. 安装依赖包](#1-安装依赖包)
  - [2. 下载包(华为镜像)](#2-下载包华为镜像)
  - [3. 解压](#3-解压)
  - [3. 编译安装](#3-编译安装)
  - [4. 快捷方式](#4-快捷方式)
  - [5. 验证安装成功](#5-验证安装成功)
  - [6. yum 依赖的python2.7, 防止yum工作异常](#6-yum-依赖的python27-防止yum工作异常)
- [编译安装nginx](#编译安装nginx)
  - [1. 安装依赖](#1-安装依赖)
  - [2.下载包( 华为源)](#2下载包-华为源)
  - [3. 解压包](#3-解压包)
  - [4. 编译安装](#4-编译安装)
  - [5. 常用命令](#5-常用命令)
  - [6. 查询启动](#6-查询启动)
  - [7. 创建软连接](#7-创建软连接)
  - [8. nginx永久加入到系统环境变量](#8-nginx永久加入到系统环境变量)
- [编译安装redis](#编译安装redis)
  - [1. 安装依赖](#1-安装依赖-1)
  - [2. 下载包( 华为源)](#2-下载包-华为源)
  - [3. 解压包](#3-解压包-1)
  - [4. 编译安装](#4-编译安装-1)
  - [5. 测试安装结果](#5-测试安装结果)
  - [6. 配置文件说明(redis.conf)](#6-配置文件说明redisconf)
  - [7. 其他](#7-其他)
- [安装mysql](#安装mysql)
  - [1. 下载包(rpm)](#1-下载包rpm)
  - [2.检查数据源：](#2检查数据源)
  - [3. 安装 MYSQL 命令](#3-安装-mysql-命令)
  - [4. 启动服务](#4-启动服务)
  - [5. 显示MySQL临时密码](#5-显示mysql临时密码)
  - [6. 登录](#6-登录)
  - [7. 密码修改](#7-密码修改)
  - [8. 开放远程访问](#8-开放远程访问)
  - [9. 更改加密方式](#9-更改加密方式)
---
**安装新建coding目录， 编译安装软件**
```shell
mkdir /coding
cd coding
```

## 编译安装 python

  ### 1. 安装依赖包
```shell
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel libffi-devel gcc make
```
### 2. 下载包(华为镜像)
```shell
wget https://mirrors.huaweicloud.com/python/3.8.5/Python-3.8.5.tar.xz
```
### 3. 解压
```shell
tar -zxvf Python-3.8.5.tar.xz
# 移动到自定义安装的目录
mv Python-3.8.5 /coding/py38
cd /coding/py38
```
### 3. 编译安装
```shell
./configure --prefix=/coding/py38
make && make install
```
### 4. 快捷方式
```shell
ln -s /coding/py38/bin/python3.8 /usr/bin/python3.8
ln -s /coding/py38/bin/python3.8 /usr/bin/python
ln -s /coding/py38/bin/pip3 /usr/bin/pip3
ln -s /coding/py38/bin/pip3 /usr/bin/pip
```
### 5. 验证安装成功
```shell
python --version
pip --version
```
### 6. yum 依赖的python2.7, 防止yum工作异常
```shell
find /usr/bin/ -type f -exec sed -i ':a;N;$!ba;s/\/usr\/bin\/python\([ \n]\)/\/usr\/bin\/python2.7\1/' {} \;

find /usr/libexec/ -type f -exec sed -i ':a;N;$!ba;s/\/usr\/bin\/python\([ \n]\)/\/usr\/bin\/python2.7\1/' {} \;
```

## 编译安装nginx
### 1. 安装依赖
```shell
yum -y install gcc gcc-c++ autoconf automake make  pcre-devel openssl-devel
```
### 2.下载包( 华为源)
```shell
wget https://mirrors.huaweicloud.com/nginx/nginx-1.21.6.tar.gz
```
### 3. 解压包
```shell
tar -zxvf nginx-1.21.6.tar.gz
mv nginx-1.21.6 /coding/nginx
cd /coding/nginx
```
### 4. 编译安装
```shell
./configure --prefix=/coding/nginx
make && make install
```
### 5. 常用命令
```shell
./nginx			//启动
./nginx -s stop	//停止
./nginx -s reload	//重载配置
```
### 6. 查询启动
```shell
ps -ef | grep nginx
```
### 7. 创建软连接
```shell
ln -s /coding/nginx/sbin/nginx /usr/local/sbin
nginx			//启动
nginx -s stop	//停止
nginx -s reload
```
### 8. nginx永久加入到系统环境变量
```shell
echo 'export NGINX_HOME=/usr/local/nginx' >> /etc/profile
echo 'export PATH=$PATH:$NGINX_HOME/sbin' >> /etc/profile
source /etc/profile
```
## 编译安装redis
### 1. 安装依赖
```shell
yum -y install gcc gcc-c++ automake autoconf libtool make
```
### 2. 下载包( 华为源)
```shell
wget https://mirrors.huaweicloud.com/redis/redis-6.2.5.tar.gz
```
### 3. 解压包
```shell
tar -zxvf redis-6.2.5.tar.gz
mv redis-6.2.5 /coding/redis
cd /coding/redis
```
### 4. 编译安装
```shell
 make PREFIX=/coding/redis install
```
### 5. 测试安装结果
```shell
make test
```
### 6. 配置文件说明(redis.conf)
    - 过滤注释行和空行： `grep -v ^# redis.conf | grep -v ^$`
```conf
bind 127.0.0.1  # 绑定IP
protected-mode yes
port 6379  # 端口
tcp-backlog 511
timeout 0
tcp-keepalive 300
daemonize no # 守护进程
supervised no
pidfile /var/run/redis_6379.pid
loglevel notice
logfile ""
databases 16
always-show-logo yes
# rdb 数据持久化（ 保存操作到本地文件）
# 触发持久化， 每900s 超过一次更改，就保存，
save 900 1          # 经过900s， 超过1次，保存，以此类推
save 300 10         
save 60 10000    
stop-writes-on-bgsave-error yes
rdbcompression yes
rdbchecksum yes
dbfilename dump.rdb  # 保存rdb文件
rdb-del-sync-files no
dir ./                 # rdb 保存rdb目录
replica-serve-stale-data yes
replica-read-only yes
repl-diskless-sync no
repl-diskless-sync-delay 5
repl-diskless-load disabled
repl-disable-tcp-nodelay no
replica-priority 100
acllog-max-len 128
lazyfree-lazy-eviction no
lazyfree-lazy-expire no
lazyfree-lazy-server-del no
replica-lazy-flush no
lazyfree-lazy-user-del no
oom-score-adj no
oom-score-adj-values 0 200 800
# aof 数据持久化（ 保存操作到本地文件） 
appendonly no  # 默认关闭
appendfilename "appendonly.aof" #本地数据库文件名
appendfsync everysec  # 写入频率  可设置always 和 no
no-appendfsync-on-rewrite no
# aof 重写触发
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
# 
aof-load-truncated yes
aof-use-rdb-preamble yes

lua-time-limit 5000
slowlog-log-slower-than 10000
slowlog-max-len 128
latency-monitor-threshold 0
notify-keyspace-events ""
hash-max-ziplist-entries 512
hash-max-ziplist-value 64
list-max-ziplist-size -2
list-compress-depth 0
set-max-intset-entries 512
zset-max-ziplist-entries 128
zset-max-ziplist-value 64
hll-sparse-max-bytes 3000
stream-node-max-bytes 4096
stream-node-max-entries 100
activerehashing yes
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit replica 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60
hz 10
dynamic-hz yes
aof-rewrite-incremental-fsync yes
rdb-save-incremental-fsync yes
jemalloc-bg-thread yes
```
### 7. 其他
- 开启服务端: `./redis-server`
- 客户端访问: `./redis-cli`
- 修改自启文件权限: `chmod 775 /etc/init.d/redis`
- 设置自启: `chkconfig redis on`
## 安装mysql
**参考：[centos8安装mysql8.0.22教程]**(https://blog.csdn.net/qq_39150374/article/details/112471108)

### 1. 下载包(rpm)
```shell
wget https://dev.mysql.com/get/mysql80-community-release-el8-1.noarch.rpm

yum install mysql80-community-release-el8-1.noarch.rpm
```

### 2.检查数据源：

```shell
yum repolist enabled | grep "mysql.*-community.*"
```
> 错误： No match for argument: mysql-community-server
> 解决方法：yum module disable mysql

### 3. 安装 MYSQL 命令
```shell
yum install mysql-community-server
```
> 错误：Error:GPG check FAILED 
> 解决方法：yum 安装时添加 --nogpgcheck 参数
```shell
yum -y install xx --nogpgcheck
```
### 4. 启动服务
```shell
genie -s # 在wsl子系统中实现systemd
systemctl start mysqld
```
### 5. 显示MySQL临时密码
```shell
grep 'temporary password' /var/log/mysqld.log
```
> 输出： 2022-03-14T02:26:55.430178Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: 33HnSqi/jFK#
### 6. 登录
```shell
mysql -u root -p //输入上面生成的密码
```
### 7. 密码修改
```shell
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'Root_21root';
```
> 错误 ERROR 1819 (HY000): Your password does not satisfy the current policy requirements

> 解决办法:
一定要先修改默认密码为: Root_21root 满足它的策略,再进行下面的操作:
```shell
# 查看密码策略
mysql> SHOW VARIABLES LIKE 'validate_password%'; 

# 修改密码长度：
mysql> set global validate_password.length=1; 
# 修改密码等级：
mysql> set global validate_password.policy=0; 

# 设置成自己想要的密码:
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';  
```
### 8. 开放远程访问
```shell
# 1、先创建权限记录
mysql> create user 'root'@'%' identified by 'root123'; 
# 2、授权
mysql> grant all privileges on *.* to 'root'@'%' with grant option; 
```

### 9. 更改加密方式
```shell
mysql> use mysql;
mysql> select user,plugin from user where user='root';
# 将用户的加密方式改为mysql_native_password。
mysql> alter user 'root'@'%' identified with mysql_native_password by 'Admin@123';
# 使权限配置项立即生效。
mysql> flush privileges;
```
> 解决错误：2059 - Authentication plugin ‘caching_sha2_password’ cannot be loaded: