---
sidebar_position: 2
slug: /awx/admin
tags:
  - AWX
  - DevOps
---

# 维护参考


## 系统参数

AWX 预装包包含 AWX 运行所需一序列支撑软件（简称为“组件”），下面列出主要组件名称、安装路径、配置文件地址、端口、版本等重要的信息。

## 组件

#### PostgreSQL

AWX 预装包中内置 PostgreSQL 容器，需要登录容器后使用命令对 PostgreSQL 进行操作。

1. 使用 SSH 登录服务器后，运行`docker ps`命令获取 awx-postresql 容器ID
  ![](https://libs.websoft9.com/Websoft9/DocsPicture/en/awx/awx-getcontainerid-websoft9.png)

2. 进入 awx-postgresql 容器

   ```
   docker exec -it 2ca9ad211678 /bin/bash
   ```
4. 运行上面的命令后，就进入了容器命令操作界面

5. 接下来可以使用命令操作 PostgreSQL 

> 阅读Websoft9提供的 [《PostgreSQL教程》](https://support.websoft9.com/docs/postgresql/zh/) ，掌握更多的PostgreSQL实用技能：修改密码、导入/导出数据、创建用户、开启或关闭远程访问、日志配置等


### 路径

本项目基于Docker安装，下面主要列出Docker有关的参数：

#### Docker Container

通过运行`docker ps`，可以查看到AWX运行时所有的Container：

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                NAMES
e240ed8209cd        awx_task:1.0.0.8    "/tini -- /bin/sh ..."   2 minutes ago       Up About a minute   8052/tcp                             awx_task
1cfd02601690        awx_web:1.0.0.8     "/tini -- /bin/sh ..."   2 minutes ago       Up About a minute   0.0.0.0:443->8052/tcp                 awx_web
55a552142bcd        memcached:alpine    "docker-entrypoint..."   2 minutes ago       Up 2 minutes        11211/tcp                            memcached
84011c072aad        rabbitmq:3          "docker-entrypoint..."   2 minutes ago       Up 2 minutes        4369/tcp, 5671-5672/tcp, 25672/tcp   rabbitmq
97e196120ab3        postgres:9.6        "docker-entrypoint..."   2 minutes ago       Up 2 minutes        5432/tcp                             postgres
```

#### Docker Volume

使用 *sudo docker volume ls* 查询所有 volumes，其中：

awx_postgres 挂载的目录：*/var/lib/postgresql/data*  
awx_rabbitmq 挂载的目录：*/var/lib/rabbitmq*  
awx_web 挂载的目录：*/var/lib/nginx*   
awx_task 挂载的目录：*/var/lib/nginx* 	


#### Docker

Docker 根目录: */var/lib/docker*  
Docker 镜像目录: */var/lib/docker/image*   
Docker 存储卷：*/var/lib/docker/volumes*  
Docker daemon.json 文件：默认没有创建，请到 */etc/docker* 目录下根据需要自行创建

#### Docker Compose

本环境使用 Docker Compose 作为容器编排（调度）工具，用于管理多个容器的启动和服务连接。

Docker Compose 命令位置：*/usr/local/bin/docker-compose*  
Docker Compose 配置目录 */data/.awx*  

#### PostgreSQL

PostgreSQL 数据持久存储：*/data/pgdocker*


### 端口号

下面是您在使用本镜像过程中，需要用到的端口号，请通过 [云控制台安全组](https://support.websoft9.com/docs/faq/zh/tech-instance.html)进行设置

| 名称 | 端口号 | 用途 |  必要性 |
| --- | --- | --- | --- |
| PostgreSQL | 5432 | TCP访问PostgreSQL | Optional |
| PgAdmin | 9090 | 可视化访问PostgreSQL | Optional |
| HTTP | 80 | 通过http访问AWX | 必须 |
| HTTPS | 443 | 通过https访问AWX | 可选 |

### 版本号

组件版本号可以通过云市场商品页面查看。但部署到您的服务器之后，组件会自动进行更新导致版本号有一定的变化，故精准的版本号请通过在服务器上运行命令查看：

```shell
# Linux Version
lsb_release -a

# Python Version
python --version

# Docker Version
docker -v

# Docker image lists(includes version)
sudo docker images

# Docker Compose Version
docker-compose --version
```

### 服务

使用由Websoft9提供的AWX部署方案，可能需要用到的服务如下：

#### Docker Compose

```shell
#创建容器
sudo docker-compose up
#创建容器并重建有变化的容器
sudo docker-compose up -d
```

#### Docker

```shell
sudo systemctl start docker
sudo systemctl restart docker
sudo systemctl stop docker
sudo systemctl status docker
```

#### AWX 容器

> 终止命令 `stop` 会从进程中释放容器的资源，但不会删除容器

```shell
#AWX-主程序
sudo docker pause awx_task
sudo docker stop awx_task
sudo docker restart awx_task

#AWX-Web界面
sudo docker pause awx_web
sudo docker stop awx_web
sudo docker restart awx_web

#RabbitMQ
sudo docker pause awx_rabbitmq
sudo docker stop awx_rabbitmq
sudo docker restart awx_rabbitmq

#PostgreSQL
sudo docker pause awx_postgres
sudo docker stop awx_postgres
sudo docker restart awx_postgres

#PostgreSQL
sudo docker pause awx_Memcached
sudo docker stop awx_Memcached
sudo docker restart awx_Memcached
```

## 备份

## 恢复

## 升级

升级AWX通过重新安装来完成。

1. 使用SSH登录服务器
2. 进入到 */data/awx/* 目录，从 Github 更新AWX源码
   ```
   git pull
   ```
3. 进入到 */data/awx/installer* 目录
4. 增加一个 update-vars.yml 文件，其中的内容如下（其中的密码为真实值）：
   ```
   admin_password: 'adminpass'
   pg_password: 'pgpass'
   rabbitmq_password: 'rabbitpass'
   secret_key: 'mysupersecret'
   ```
5. 运行如下命令，开始升级
   ```
   ansible-playbook -i inventory install.yml -e @update-vars.yml
   ```


## 故障处理

此处收集使用 AWX 过程中最常见的故障，供您参考

> 大部分故障与云平台密切相关，如果你可以确认故障的原因是云平台造成的，请参考[云平台文档](https://support.websoft9.com/docs/faq/zh/tech-instance.html)

#### 已经通过AWX安装了环境的受控端主机，更换镜像后，再次连接会报错？

找到主机缓存文件：*/var/lib/awx/.ssh/known_hosts*，删除其中的历史记录即可

#### 数据库服务无法启动

数据库服务无法启动最常见的问题包括：磁盘空间不足，内存不足，配置文件错误。  
建议先通过命令进行排查  

```shell
# 查看磁盘空间
df -lh

# 查看内存使用
free -lh
```

#### 登录界面显示"is updating"？

等待更新完成后，重启服务器，再访问

#### 创建项目选择手动（SCM 类型）提示 "WARNING: There are no available playbook directories in /var/lib/awx/projects...."？

原因：AWX容器的项目路径没有挂在到宿主机上  
方案：将/var/lib/awx/projects 映射到宿主机目录  /data/wwwroot/awx/projects

#### awx_redis 容器无法启动？

原因：redis.sock 权限不足导致  
方案：给 /data/.awx/redis_socket 文件夹授权

1. 编辑 */data/.awx/redis.conf* 文件中增加一行权限配置 `unixsocketperm 750`
   ```
   unixsocket /var/run/redis/redis.sock
   unixsocketperm 660
   port 0
   bind 127.0.0.1
   unixsocketperm 750
   ```
2. Redis 通信目录授权 `chmod -R 777 /data/.awx/redis_socket`
3. 进入到 AWX 目录后，重新运行容器即可
   ```
   cd /data/.awx
   docker-compose down -v
   docker-compose up -d
   ```

#### 能够正常进入 AWX 控制台，但无法运行 Job？

很有可能是 awx_redis 容器没有正常运行导致，通过命令 `docker ps` 查看 awx_redis 运行状态


## 常见问题 

#### AWX支持多语言吗？

支持[多种语言](https://docs.ansible.com/ansible-tower/latest/html/release-notes/supported_locales.html)，包括中文。它不提供语言切换菜单，而是自动适用浏览器首选语言。

#### AWX是如何与PostgreSQL连接的？

容器内部连接，即容器编排

#### AWX 是否支持 Ansible Galaxy？

![](https://libs.websoft9.com/Websoft9/DocsPicture/zh/awx/awx-setgalax-websoft9.png)

支持，参考官方文档 [Ansible Galaxy Support](https://docs.ansible.com/ansible-tower/latest/html/userguide/projects.html#ug-galaxy)

#### AWX 是否支持交互式变量？

支持，参考[额外变量](/zh/solution-more.md#额外变量)章节

#### AWX API 地址是多少？

http://AWX Server Internet IP/api/

#### 如果没有域名是否可以部署 AWX？

可以，访问`http://服务器公网IP` 即可

#### 数据库 Postgres 用户对应的密码是多少？

密码存放在服务器相关文件中：`/credentials/password.txt`

#### 是否有可视化的数据库管理工具？

为了安全考虑，没有提供可视化的数据库管理工具

#### 是否可以修改AWX的源码路径？

采用 Docker 安装，不可以修改

#### 部署和安装有什么区别？

部署是将一序列软件按照不同顺序，先后安装并配置到服务器的过程，是一个复杂的系统工程。  
安装是将单一的软件拷贝到服务器之后，启动安装向导完成初始化配置的过程。  
安装相对于部署来说更简单一些。 

#### 云平台是什么意思？

云平台指提供云计算服务的平台厂家，例如：Azure,AWS,阿里云,华为云,腾讯云等

#### 实例，云服务器，虚拟机，ECS，EC2，CVM，VM有什么区别？

没有区别，只是不同厂家所采用的专业术语，实际上都是云服务器