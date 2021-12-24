# al 服务器折腾记录

[toc]

## V2ray

- http 地址：`127.0.0.1:10800`
- socks5 地址：`127.0.0.1:10801`
- 配置文件：`/usr/local/etc/v2ray/config.json`
- 作为客户端，连接 SkyServer-bw。

## LNMP

 MariaDB 目录: /usr/local/mariadb/
 MariaDB 数据库所在目录: /usr/local/mariadb/var/

**LNMP相关软件安装目录**
 PHP目录: /usr/local/php/
 默认网站目录: /home/wwwroot/default/
 Nginx 日志目录：/home/wwwlogs/
 /root/vhost.sh添加的虚拟主机配置文件所在目录：/usr/local/nginx/conf/vhost/
 Proftpd 目录: /usr/local/proftpd/
 Redis 目录: /usr/local/redis/

**LNMP相关配置文件位置**
 Nginx 主配置文件: /usr/local/nginx/conf/nginx.conf
 虚拟主机配置文件: /usr/local/nginx/conf/vhost/domain.conf
 MySQL 配置文件: /etc/my.cnf
 PHP 配置文件: /usr/local/php/etc/php.ini
 php-fpm 配置文件: /usr/local/php/etc/php-fpm.conf
 Proftpd 配置文件: /usr/local/proftpd/etc/proftpd.conf

Proftpd 用户配置文件: /usr/local/proftpd/etc/vhost/Username.conf
 Redis 配置文件: /usr/local/redis/etc/redis.conf

| **参数名称**          | **参数介绍**                  | **例子**                                                     |
| --------------------- | ----------------------------- | ------------------------------------------------------------ |
| Download_Mirror       | 下载镜像                      | 一般默认，如异常可[修改下载镜像](https://lnmp.org/faq/download-url.html) |
| Nginx_Modules_Options | 添加Nginx模块或其他编译参数   | —add-module=/第三方模块源码目录                              |
| PHP_Modules_Options   | 添加PHP模块或编译参数         | —enable-exif 有些模块需提前安装好依赖包                      |
| MySQL_Data_Dir        | MySQL数据库目录设置           | 默认/usr/local/mysql/var                                     |
| MariaDB_Data_Dir      | MariaDB数据库目录设置         | 默认/usr/local/mariadb/var                                   |
| Default_Website_Dir   | 默认虚拟主机网站目录位置      | 默认/home/wwwroot/default                                    |
| Enable_Nginx_Openssl  | Nginx是否使用新版openssl      | 默认 y，建议不修改，y是启用并开启到http2                     |
| Enable_PHP_Fileinfo   | 是否安装开启php的fileinfo模块 | 默认n，根据自己情况而定，安装启用的话改成 y                  |
| Enable_Nginx_Lua      | 是否为Nginx安装lua支持        | 默认n，安装lua可以使用一些基于lua的waf网站防火墙             |

## Netdata

- Headless

## Frp

- 面板：`0.0.0.0:7500`

## Plausible

- `0.0.0.0:8000`

## Portainer

- Portainer Server：8124(8000) 9443 9124(9000)(main)
- Portainer Agent：9125(9001)

## Nextcloud

