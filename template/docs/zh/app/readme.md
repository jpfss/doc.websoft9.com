---
sidebar_position: 100
slug: /{{appname}}
tags:
  - Web 面板
  - 可视化
  - GUI
---

# 快速入门

{{trademark}} 介绍

部署 Websoft9 提供的 {{trademark}} 之后，请参考下面的步骤快速入门。

## 准备{% raw %}{#prepare}{% endraw %}

1. 在云控制台获取您的 **服务器公网IP地址** 
2. 在云控制台安全组中，确保 **Inbound（入）规则** 下的 **TCP:80** 端口已经开启
3. 在服务器中查看 {{trademark}} 的 **[默认账号和密码](./user/credentials)**  
4. 若想用域名访问 {{trademark}}，务必先完成 **[域名五步设置](./administrator/domain_step)** 过程

## {{trademark}} 初始化向导{% raw %}{#wizard}{% endraw %}

### 详细步骤

1. 使用本地电脑浏览器访问网址：*http://域名* 或 *http://服务器公网IP*, 进入初始化页面

2. 完成初始化工作

### 碰到问题？

若碰到问题，请第一时刻联系 **[技术支持](./helpdesk)**。也可以先参考下面列出的问题定位或  **[FAQ](./faq#setup)** 尝试快速解决问题。

**{{trademark}} 能打开，但总是出现 502 错误？**  

参阅：

## {{trademark}} 使用入门{% raw %}{#quickstart}{% endraw %}

下面以 **××××** 作为一个任务，帮助用户快速入门：

## {{trademark}} 常用操作{% raw %}{#guide}{% endraw %}

### 配置 SMTP{% raw %}{#smtp}{% endraw %}

1. 在邮箱管理控制台获取 [SMTP](./administrator/smtp) 相关参数

2. 填写 {{trademark}} 邮件相关配置

3. 测试邮件发送是否可用

### 安装插件{% raw %}{#plugin}{% endraw %}

### 重置管理员密码{% raw %}{#resetpw}{% endraw %}

忘记管理员密码时，请参考如下方案重置密码：  

### 域名额外配置（修改 URL）{% raw %}{#dns}{% endraw %}

**[域名五步设置](./administrator/domain_step)** 完成后，需设置 {{trademark}} 的 URL:  

1. 步骤1

2. 步骤2

### HTTPS 额外设置{% raw %}{#https}{% endraw %}

**[标准 HTTPS 配置](./administrator/domain_https)** 完成后，可能还需要如下步骤： 

1. 步骤1

2. 步骤2

## {{trademark}} 参数{% raw %}{#parameter}{% endraw %}

{{trademark}} 应用中包含 Docker, Portainer 等组件，可通过 **[通用参数表](./administrator/parameter)** 查看路径、服务、端口等参数。 

通过运行 `docker ps`，查看 {{trademark}} 运行时所有的服务组件：   

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                NAMES
```

### 路径{% raw %}{#path}{% endraw %}

{{trademark}} 配置文件： *path/config.php*    

### 端口{% raw %}{#port}{% endraw %}

除 80, 443 等常见端口需开启之外，以下端口可能会用到：  

| 端口号 | 用途                                          | 必要性 |
| ------ | --------------------------------------------- | ------ |
| 8080   | {{trademark}} 原始端口，已通过 Nginx 转发到 80 端口 | 可选   |

### 版本{% raw %}{#version}{% endraw %}

控制台查看

### 服务{% raw %}{#service}{% endraw %}

```shell
sudo docker start | stop | restart | stats {{appname}}
```

### 命令行{% raw %}{#cli}{% endraw %}

### API{% raw %}{#api}{% endraw %}
