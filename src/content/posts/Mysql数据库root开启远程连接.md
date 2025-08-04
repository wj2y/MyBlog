---
title: Mysql数据库root开启远程连接
published: 2025-08-04
description: 'Mysql数据库root开启远程连接'
image: ''
tags: [MySQL]
category: 'MySQL'
draft: false 
lang: ''
---
# 1. Mysql服务启动

## 1.1 输入命令回车输入密码可以正常连接

```
mysql -u root -p
```

![](https://img.quenjoy.com/blog/1754272654698-2f763837-43fd-4116-a2eb-61e1cb27a440.png)

## 1.2 查询所有数据库

```
show databases;
```

## 1.3 切换到mysql数据库

```
use mysql;
```

![](https://img.quenjoy.com/blog/1754272787752-db58c645-2c42-4333-86dc-8b89371fb577.png)

## 1.4 查询host

```
SELECT host, user FROM user WHERE user = 'root';
```

![](https://img.quenjoy.com/blog/1754272837876-084421e0-050c-486a-9544-d1fef1ff2f36.png)

## 1.5 更新任意ip都可以连接

```
update user set host = '%' where user = 'root' and host = 'localhost';
```

## 1.6 刷新权限使配置生效

```
flush privileges;
```