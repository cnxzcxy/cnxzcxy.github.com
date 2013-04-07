---
layout: page
title: 在Ubuntu上安装monit系统监控程序
tagline: 
category : linux
tags : [linux, ubuntu, monit, daemon, installation]
---
{% include JB/setup %}

安装monit以及通过简单配置后实现对http服务、磁盘空间进行监控，当达到指定报警条件时，通过邮件进行提醒。

## 安装monit
    sudo apt-get install monit

## 基本配置monit
修改配置文件/etc/monit/monitrc
去掉注释，或者新建配置文件
内容：

    set daemon 120
    set logfile /var/log/monit.log


执行 sudo monit 启动

### http服务监控配置
安装apache2

    sudo apt-get install apache2

启动 apache2

    sudo /etc/init.d/apache2 start

增加配置文件httpd

    check process httpd with pidfile "/var/run/apache2.pid"
    start program = "/etc/init.d/apache2 start"
    stop program = "/etc/init.d/apache2 stop"
    if failed host 127.0.0.1 port 80 protocol http then restart
    if 3 restarts within 5 cycles then timeout #如果在5次检查中有3次不正常就报警
    group httpd

### 磁盘空间监控配置
修改配置文件/etc/monit/monitrc
去掉注释，或者新建配置文件
内容:

    check filesystem datafs with path /dev/sda1
    start program = “/bin/mount /data”
    stop program = “/bin/umount /data”
    if space usage > 80% for 5 times within 15cycles then alert
    if space usage > 80% then alert
    if inode usage > 30000 then alert
    if inode usage > 99% then alert
    group server

### Mail通知配置
修改配置文件/etc/monit/monitrc
内容:

    set mailserver smtp.gmail.com port 587 username “xxx@gmail.com” password “xxx” using tlsv1 with timeout 30 seconds

### http控制界面配置
修改配置文件/etc/monit/monitrc
去掉注释，或者修改配置文件
内容:

    set httpd port 2812 and
    allow 127.0.0.1
    allow 192.0.0.1/255.0.0.0  #允许任何机器访问
    allow unpn:unpn         #使用unpn:unpn登录进行管理
    allow user:user readonly   #使用user:user登录，只读

执行 sudo monit 启动
