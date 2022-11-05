---
title: 搭建KMS服务器
tags:
  - linux
  - kms
categories:
  - 网站运维
abstract: <font color=red>ʅ（´◔౪◔）ʃ &nbsp&nbsp&nbsp  文章已加密，有缘自会相见 ！！！</font>
message: 执意要看，请输入暗号 ！
date: 2022-11-05 16:43:02
password:
---

由于习惯了使用 office 套件，迫于每次安装都需要为寻找激活方法而浪费时间，所以需要搭建一套KMS激活服务。



#### 准备工作

- 宿主机：MacBook Pro 14 （M1 pro)
- 虚拟机：Parallels Desktop 18.1.0
- 服务器：CentOS 9

<!-- more -->

#### 资源下载

CentOS 9 下载地址：http://mirror.stream.centos.org/9-stream/BaseOS/aarch64/iso/

选择 CentOS-Stream-9-20221101.0-aarch64-boot.iso 下载，通过 Parallels Desktop 选择最小安装完成即可。



#### 服务安装

一键安装，开源地址：https://github.com/dakkidaze/one-key-kms

~~~bash
# 安装 wget
yum install wget

# 下载并运行安装脚本，回复 y 确认安装，等待安装完毕
wget https://raw.githubusercontent.com/dakkidaze/one-key-kms/master/one-key-kms-centos.sh && chmod +x one-key-kms-centos.sh &&./one-key-kms-centos.sh

# 下载运行脚本
wget https://raw.githubusercontent.com/dakkidaze/one-key-kms/master/kms.sh && chmod +x kms.sh

# 操作 KMS
sh kms.sh start | stop | restart | status
~~~



#### 开放端口

~~~bash
# 开放1688端口
firewall-cmd --zone=public --add-port=1688/tcp --permanent
# 刷新防火墙
firewall-cmd --reload
~~~



#### 激活产品

~~~bash
# 在Windows中激活Office，以管理员身份运行CMD，执行下列脚本

if exist "%ProgramFiles%\Microsoft Office\Office16\ospp.vbs" cd /d "%ProgramFiles%\Microsoft Office\Office16"
if exist "%ProgramFiles(x86)%\Microsoft Office\Office16\ospp.vbs" cd /d "%ProgramFiles(x86)%\Microsoft Office\Office16"
cscript ospp.vbs /inpkey:NMMKJ-6RK4F-KMJVX-8D9MJ-6MWKP
cscript ospp.vbs /sethst:192.168.0.104		# 这里为自己服务器IP
cscript ospp.vbs /act
cscript ospp.vbs /dstatus
~~~



