[TOC]

http://www.jianshu.com/p/3db8f93d8ce7

http://zachary-guo.iteye.com/blog/1358312

http://blog.csdn.net/u010648555/article/details/74360763

nginx启动、停止、与信号控制

- 启动
  - -c 配置文件
- 停止
  - 从容停止 kill -QUIT nginxpid  注[意停止master而非worker工作进程]
  - 快速停止 kill -TERM <pid> 或者 kill -INT <pid>
  - 强制停止 pkill -9 <pid>
- 重启 (平滑重启 -t验证配置文件语法是否正确)
  - nginx -s reload
  - kill -HUP <pid>

kill -l 查看所有signal



```shell
nginx -h
nginx -s signal

```

# 配置文件总览

设置用户

设置工作衍生进程数

设置错误文件存放路径

设计最大连接数

http

```nginx
# 设置用户
user nobody;
# 工作进程数，最佳数值与cpu有关
worker_processes 1;
error_log logs/error.log;
error_log logs/error.log notice;
pid logs/nginx.pid;

# 设置最大连接数
events {
    worker_connections 1024;
}

http {
    gzip on;
  	charset gb2312;
  	
}




```

# nginx虚拟主机配置

## ip配置

http > server配置

```shell
ifconfig # 查看现在的网卡ip信息
ifconfig eth0 192.168.1.9 netmask 255.255.255.0 # ip配置
ifconfig eth0:1 192.168.1.7 broadcast 192.168.1.255 netmask 255.255.255.0
ifconfig eth0:2 192.168.1.8 broadcast 192.168.1.255 netmask 255.255.255.0

# touch xnzj.conf
# netstat -tlnp
vim xnzj.conf


```

# 日志文件配置

- 格式配置
- 存放路径配置
- 日志文件切割
  - kill -USR1 <pid>
  - crontab 定时执行

```shell
# cutlog.sh
#!/bin/bash
D=$(date +%Y%m%d)
mv /usr/local/nginx/logs/access.log ${D}.log
kill -USR1 $(cat /usr/local/nginx/nginx.pid)
# cutlog.sh finish
# 定时
crontab
23 59 *** /bin/bash /usr/local/nginx/logs/cutlog.sh
```

# nginx 缓存配置

# nginx 压缩配置

gzip on;

gzip_min_length 1k; 达到阀值才压缩

gzip_buffers 4 16k;

gzip_http_version 1.1;

gzip_vary on; 验证客户端是否支持gzip

# nginx自动列目录

location > autoindex on;



# nginx下jsp开发环境概述







































