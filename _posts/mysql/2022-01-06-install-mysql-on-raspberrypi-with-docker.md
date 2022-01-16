---
layout: post
title: 树莓派4B上使用Docker安装MySQL
date: 2022-01-06 22:43:59.000000000 +08:00
tags: 
  - 树莓派
  - MySQL
  - Docker
---

树莓派4B属于ARM架构，想要在上面安装MySQL，必须选择适配ARM平台的MySQL Docker镜像。MySQL官方出了一个可以在ARM平台上运行的镜像：<a href="https://registry.hub.docker.com/r/mysql/mysql-server" target="_blank">https://registry.hub.docker.com/r/mysql/mysql-server</a>

```bash
$ docker pull mysql/mysql-server:8.0.27-aarch64
```

这里使用Docker-compose运行，编辑`docker-compose.yaml`内容如下：

```yaml
version: "3"
services:
  mysql-server:
    image: mysql/mysql-server:8.0.27-aarch64
    container_name: mysql-server
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: root_pass
    volumes:
      - "/xxx/docker/volumes/mysql-server/data:/var/lib/mysql"
    restart: always
```

启动MySQL docker容器：

```bash
$ docker-compose up -d mysql-server
```

使用命令`docker logs -f mysql-server`查看MySQL服务器日志，看到`ready for connections`字样就说明成功运行起来了。MySQL默认仅支持本地`localhost`连接，需要设置MySQL开启支持远程连接，步骤如下。

执行`docker exec -ti mysql-server bash`命令进入MySQL容器中，然后使用`mysql`命令连接MySQL：`mysql -h localhost -P 3306 -u root -proot_pass`。成功连接上后，给root用户授权远程连接：

```bash
mysql> USE mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> SELECT user, host FROM user;
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| healthchecker    | localhost |
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
+------------------+-----------+
5 rows in set (0.00 sec)

mysql> UPDATE user SET host='%' WHERE user = 'root';
Query OK, 1 row affected (0.02 sec)

mysql> SELECT user, host FROM user;
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| root             | %         |
| healthchecker    | localhost |
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
+------------------+-----------+
5 rows in set (0.01 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.02 sec)
```

成功执行`UPDATE user SET host='%' WHERE user = 'root';`之后记得执行`FLUSH PRIVILEGES;`刷新权限！

MySQL8之后，用户管理和权限管理跟之前版本命令的使用方式有所不同，参考官方说明：<a href="https://dev.mysql.com/doc/refman/8.0/en/grant.html" target="_blank">https://dev.mysql.com/doc/refman/8.0/en/grant.html</a>

<hr />

参考：

- <a href="https://dev.mysql.com/doc/refman/8.0/en/grant.html" target="_blank">https://dev.mysql.com/doc/refman/8.0/en/grant.html</a>
- <a href="https://www.cnblogs.com/dongxt/p/14883465.html" target="_blank">MySQL8.0以上版本创建用户并授权远程连接</a>

