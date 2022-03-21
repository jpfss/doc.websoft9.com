---
sidebar_position: 2
slug: /mysql/admin
tags:
  - MySQL
  - Cloude Native Database
---

# 维护参考


## 系统参数

MySQL 预装包包含 MySQL 运行所需一序列支撑软件（简称为“组件”），下面列出主要组件名称、安装路径、配置文件地址、端口、版本等重要的信息。

### 路径
MySQL 安装到Linux还是Windows系统，对应的路径有很大的差异，请根据实际情况参考：

> 如果是mariadb镜像，以上路径mysql替换即可

#### Linux

##### MySQL

MySQL 安装目录: *usr/local/mysql*  
MySQL 配置文件: *etc/my.cnf*   
MySQL 数据目录：*/data/mysql*   
MySQL 日志文件: */var/log/mysql/mysqld.log*   
MySQL PIN: */run/mysqld/mysqld.pid*   
MySQL Socket: */var/lib/mysql/mysql.sock*

##### phpMyAdmin on Docker

除了LAMP或LNMP环境之外，phpMyAdmin 是采用 Docker 方式来安装的

##### phpMyAdmin on PHP

对于 PHP 环境预装包来说（例如：LAMP/LNMP等），phpMyAdmin 是作为一个 PHP 应用程序来安装的   

phpMyAdmin 安装路径: */data/apps/phpmyadmin*  
phpMyAdmin 配置文件: */data/apps/phpmyadmin/config.inc.php*   
phpMyAdmin 虚拟主机配置文件: */etc/httpd/conf.d/phpMyAdmin.conf*   

#### Windows

* 目录：C:/websoft9/mysql
* 配置文件：C:/websoft9/mysql/etc/my.ini
* 数据文件目录:：C:/websoft9/mysql/data

### 端口号

在云服务器中，通过 **[安全组设置](https://support.websoft9.com/docs/faq/zh/tech-instance.html)** 来控制（开启或关闭）端口是否可以被外部访问。 

本应用建议开启的端口如下：

| 名称 | 端口号 | 用途 |  必要性 |
| --- | --- | --- | --- |
| phpMyAdmin on Docker | 9090 | 通过 HTTP 访问 phpMyAdmin | 可选 |
| MySQL | 3306 | 远程连接MySQL | 可选 |
| MariaDB | 3306 | 远程连接MariaDB | 可选 |

### 版本号

组件版本号可以通过云市场商品页面查看。但部署到您的服务器之后，组件会自动进行更新导致版本号有一定的变化，故精准的版本号请通过在服务器上运行命令查看：

```shell
# Check all components version
sudo cat /data/logs/install_version.txt

# Linux Version
lsb_release -a

# MySQL version
mysql -V

# MySQL Version
docker -v
```

### 服务

使用由 Websoft9 提供的 MySQL 部署方案，可能需要用到的服务如下：

#### Linux系统

##### MySQL
```shell
#For CentOS&Redhat
sudo systemctl start mysqld
sudo systemctl restart mysqld
sudo systemctl stop mysqld
sudo systemctl status mysqld

#For Ubuntu&Debian
sudo systemctl start mysql
sudo systemctl restart mysql
sudo systemctl stop mysql
sudo systemctl status mysql
```

##### MariaDB
```shell
systemctl start mariadb
systemctl stop mariadb
systemctl restart mariadb
```

#### Docker

```shell
sudo systemctl start docker
sudo systemctl restart docker
sudo systemctl stop docker
sudo systemctl status docker
```

#### Windows 系统

Windows下的镜像采用操作系统的服务管理功能，来实现 MySQL 的启动、停止和重启操作
![](https://libs.websoft9.com/Websoft9/DocsPicture/zh/mysql/mysql-servicewin-websoft9.png)


## 备份

### MySQL 备份

MySQL 数据库备份主要通过**导出数据库**实现最小化的备份方案。

下面以列表的方式介绍这种备份：
```
- 备份范围: 数据库和应用程序
- 备份效果: 一般
- 备份频率: 一周最低1次，备份保留30天
- 恢复方式: 重新导入
- 技能要求：非常容易
- 自动化：无
```

通用的手动备份操作步骤如下：

1. 使用phpMyAdmin等可视化工具，导出数据库（建议SQL格式）
2. 或使用 **mysqldump** 工具导出（效率更高，通用性更强）
   ```
   mysqldump -uroot -p databasename>databasename.sql
   ```
2. 将备份文件下载到本地，备份工作完成

在 phpMyAdmin 中，【导出】相当于备份数据库，【导入】相当于恢复数据库。

#### 导出

1. 登录 phpMyAdmin，打开顶部的【导出】标签页
   ![](http://libs.websoft9.com/Websoft9/DocsPicture/zh/mysql/phpmyadmin-export-websoft9.png)

2. 选择合适的备份文件类型、备份存放方式，然后开始备份

3. 最好将备份文件下载到本地存放




## 恢复

### 导入恢复

1. 登录 phpMyAdmin，打开顶部的【导入】标签页，根据向导开始导入
   ![](https://libs.websoft9.com/Websoft9/DocsPicture/zh/mysql/phpmyadmin-import-websoft9.png)

2. 导入过程中可能会出现数据库字符集不兼容的情况，需要人工干预处理

## 升级

### 系统级更新

运行一条更新命令，即可完成系统级更新：

``` shell
#For Ubuntu&Debian
apt update && apt upgrade -y

#For Centos&Redhat
yum update -y
```
> 本部署包已预配置一个用于自动更新的计划任务。如果希望去掉自动更新，请删除对应的Cron


### MySQL 更新升级

#### On Linux

上面的系统升级命令，支持小版本升级。例如：5.6.x to 5.6.y 或 5.7.x to 5.7.y

数据库大版本之间的差异较大，无法提供稳妥的升级方案

#### On Windows

Windows 上的 MySQL 升级分为两部分：

1. 使用Windows Update升级Windows系统
2. 下载最新的MySQL，停止MySQL服务，替换MySQL的旧文件

### phpMyAdmin 更新升级

不同的 phpMyAdmin 安装方式，对应不同的升级方案：

#### phpMyAdmin on Docker

Docker 版 phpMyAdmin 只需到 phpMyAdmin 的目录下，运行 docker-compose 命令即可升级：

```
cd /data/apps/phpmyadmin
docker-compose pull
docker-compose up -d
```

#### phpMyAdmin 普通安装

若通过 *http://服务器公网IP/phpmyadmin* 访问，则表示是通过普通的 PHP 应用程序的方式。  

对应的升级方案：  

1. 检查服务器上的 PHP 版本是否满足最新的 [phpMyAdmin](https://www.phpmyadmin.net/) 之要求

2. 备份旧版本到 */data/apps/phpmyadminBK* 文件夹

3. 下载版本 [phpMyAdmin](https://www.phpmyadmin.net/) 并覆盖 phpmyadmin 安装目录（目录名称与旧版本一致 ）

4. 设置用户和组和访问权限
   ```
   #更改文件夹用户权限
   chmod  -R 750 phpmyadmin
   #更改用户和组
   chown  -R apache:apache phpmyadmin
   ```
5. 备份目录中的配置文件 config.inc.php 拷贝正式目录

6. 测试安装结果


### 常见升级问题

#### 大版本升级后，无法更改数据库密码？
```
mysql_upgrade -u root -p 13456
```


## 故障处理

此处收集使用 MySQL 过程中最常见的故障，供您参考

> 大部分故障与云平台密切相关，如果你可以确认故障的原因是云平台造成的，请参考[云平台文档](https://support.websoft9.com/docs/faq/zh/tech-instance.html)

#### 导入数据库报错？

查看脚本里面是否有创建数据库的脚本

#### 数据库服务无法启动

数据库服务无法启动最常见的问题包括：磁盘空间不足，内存不足，配置文件错误。  
建议先通过命令进行排查  

```shell
# 查看磁盘空间
df -lh

# 查看内存使用
free -lh
```

#### 数据库日志文件太大，导致磁盘空间不足？

默认安装，mysql会自动开启binlog，binlog是一个二进制格式的文件，用于记录用户对数据库**更新的****SQL语句****信息**，例如更改数据库表和更改内容的SQL语句都会记录到binlog里。

binlog主要用于出现没有备份的情况下，恢复数据库。但binlog会占用较大空间，长期不清理会让剩余磁盘空间为0，从而影响数据库或服务器无法启动

如果对自己的备份有信心，不需要binlog功能，参考如下步骤关闭之：

1. 编辑 [MySQL 配置文件](/zh/stack-components.md#mysql)，注释掉 binlog 日志
  ~~~
  #log-bin=mysql-bin  
  ~~~
2. 重启mysql
  ~~~
  systemctl restart mysqld
  ~~~

#### MySQL 容器无法远程访问？

导致这个问题的可能原因有三点：

1. 端口映射设置错误，导致容器没有网络
2. 容器没有开启远程访问权限
3. MySQL 8.0 特殊设置要求

## 常见问题

#### 单台服务器上是否可以安装多个 MySQL实例？

理论上可以，但实际上不建议

#### 什么是 MySQL 的 Client 和 Server？

MySQL Server 是指 MySQL 程序本体，而 MySQL Client 指采用TCP协议用于连接程序本地的客户端。它们是两个完全不同的程序，也就是说它们并需要同时安装到同一台服务上。


#### MySQL 中的 test 数据库是什么？

在MySQL5.7 版本之前，安装 MySQL 时会默认包含一个 test 数据库，该数据库仅仅用来测试使用，但是所有能连接到MySQL的用户，几乎都拥有test库的所有权限，因此存在一定的安全隐患。从信息安全角度考虑，如果您发现您使用的 MySQL 中有该 test 数据库，请**务必删除**。

#### 是否可以修改 MySQL 根目录？

可以，但不建议修改

#### 数据库 root 用户对应的密码是多少？

密码存放在服务器相关文件中：`/credentials/password.txt`

#### 是否有可视化的数据库管理工具？

有，内置phpMyAdmin

#### 如何禁止外界访问phpMyAdmin？

连接服务器，编辑 [phpMyAdmin 配置文件](/zh/stack-components.md#phpmyadmin)，将其中的 `Require all granted` 更改为 `Require ip 192.160.1.0`，然后重启 Apache 服务

#### 如何自定义 MySQL 错误日志文件路径？

配置文件中增加下面的参数即可
```
log-error=/data/mysql/log.err
```