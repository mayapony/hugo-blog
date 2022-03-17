---
title: "Nginx的使用"
date: 2022-03-06T20:26:10+08:00
draft: false
tags: ["nginx"]
categories: ["学习记录"]
keywords:
- nginx
description: "nginx学习记录"
---

# 一、Nginx

> 占用内存小、并发能力强

## 功能

1. **反向代理**加速(无缓存)，简单的负载均衡和容错；
2. **负载均衡**
3. **处理静态文件**，索引文件以及自动索引；
4. FastCGI，简单的负载均衡和容错；
5. 模块化的结构。
6. SSL 和 TLS SNI 支持；
7. IMAP/POP3 代理服务功能

## 使用

### 1. 安装

`sudo pacman -S nginx`

### 2. 运行

`sudo nginx`

产生报错
```shell
2022/02/23 18:49:26 [warn] 8823#8823: could not build optimal types_hash, you should increase either types_hash_max_size: 1024 or types_hash_bucket_size: 64; ignoring types_hash_bucket_size
2022/02/23 18:49:26 [emerg] 8823#8823: bind() to 0.0.0.0:80 failed (98: Address already in use)
2022/02/23 18:49:26 [emerg] 8823#8823: bind() to 0.0.0.0:80 failed (98: Address already in use)
2022/02/23 18:49:26 [emerg] 8823#8823: bind() to 0.0.0.0:80 failed (98: Address already in use)
2022/02/23 18:49:26 [emerg] 8823#8823: bind() to 0.0.0.0:80 failed (98: Address already in use)
2022/02/23 18:49:26 [emerg] 8823#8823: bind() to 0.0.0.0:80 failed (98: Address already in use)
2022/02/23 18:49:26 [emerg] 8823#8823: still could not bind()
```
但其实已经可以正常访问 `localhost`。

### 3. 常用命令

```shell
nginx -s stop 停止
nginx -s quit 安全退出
nginx -s reload 重新加载配置文件
ps aux | grep ngix 查看nginx进程
```

### 4. 反向代理配置

```
# 负载均衡
# weigth 为权重
upstream domain {
	server 127.0.0.1:8080 weight=1
	server 127.0.0.2:8080 weight=1
}


server {
	listen: 80;
	server_name localhost

	location / {
		root html;
		index index.html; 
		proxy_pass http://domain; # 反向代理
	}
}
```

利用反向代理使用子域名
例如：

```
server {
	listen: 80;
	server_name xxx.mayapony.com

	location / {
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header Host $http_host;
		proxy_pass http://0.0.0.0:3000;
	}
}
```
