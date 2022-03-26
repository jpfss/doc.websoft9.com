---
sidebar_position: 3
slug: /jenkins/admin
tags:
  - Jenkins
  - DevOps
---

# 维护指南

## 场景

### 备份与恢复

[Backup plugin](https://plugins.jenkins.io/backup/) 提供对 Jenkins 的备份和恢复功能。  

### 升级

Jenkins 内置升级功能，操作简单：

1. 登陆Jenkins后台，如果当前版本不是最新的稳定版本，会在右上角警告栏出现提示
   ![](https://libs.websoft9.com/Websoft9/DocsPicture/zh/jenkins/jenkins-warning-websoft9.png)

2. 点击警告，在弹出页面选择自动升级
   ![](https://libs.websoft9.com/Websoft9/DocsPicture/zh/jenkins/jenkins-selectauto-websoft9.png)

3. 在升级页面等待直到自动升级完成
   ![](https://libs.websoft9.com/Websoft9/DocsPicture/zh/jenkins/jenkins-autoupdate-websoft9.png)

4. 重启jenkins服务，Jenkins已经更新到最新稳定版  
   ![](https://libs.websoft9.com/Websoft9/DocsPicture/zh/jenkins/jenkins-updatecok-websoft9.png)


## 故障速查

除以下列出的 Jenkins 故障问题之外， [通用故障处理](../troubleshooting) 专题章节提供了更多的故障方案。 

###  出现 502 错误？

**现象描述**：通过公司网络（固定IP）突然（以前可以访问）不能访问，而通过自己的手机wifi可以访问。   

**原因分析**：种条件下（例如：公司大量并发访问   

**解决方案**：修改  



## 问题解答

#### Jenkins 是否支持中文？

支持，可以很方便的切换多语言（包括中文），Jenkins根据浏览器的语言显示文本，详情参照[Jenkins 语言设置](https://www.jenkins.io/doc/book/using/using-local-language/)。

#### 如何扩展 Jenkins 的功能？

[安装插件](../jenkins#installplugin)以扩展其功能。 