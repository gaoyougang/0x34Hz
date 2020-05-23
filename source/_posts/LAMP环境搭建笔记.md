---
title: LAMP环境搭建笔记
date: 2020-05-18 12:19:43
author:
img:
top:
cover:
coverImg:
password:
toc:
mathjax:
summary:
categories: 技术
tags:
- LAMP
- docker
- wordpress
reprintPolicy:
---
# LAMP环境搭建笔记

**使用docker容器创建LAMP环境(linux选择centos：7)**

### docker部署centos7环境

1. 拉取镜像：`docker pull centos:7`

2. 构建容器名为lamp：

   `docker run -di -p 80:80 -p 3306:3306 --privileged --name=lamp centos:7 /usr/sbin/init`

3. 进入容器：`docker exec -it lamp /bin/bash`

4. 更新系统：`yum update`

5. 下载辅助工具;`yum install vim wget tmux -y`

### 安装Apache/php/Mysql数据库的包

1. 安装：

   ```
   yum -y install httpd
   yum -y install php
   yum -y install php-fpm
   yum -y install mysql
   yum -y install php-mysql
   
   #从官网下载mysql-server
   wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
   rpm -ivh mysql-community-release-el7-5.noarch.rpm
   yum install mysql-community-server
   
   #安装Apache的扩展
   yum -y install httpd-manual mode_ssl mod_perl mod_auth_mysql
   
   #有防火墙，需要这个步骤：(没有省略)
   firewall-cmd --add-service=http --permanent
   systemctl restart firewall
   
   #安装php的扩展
   yum -y install php-gd php-xml php-mbstring php-ldap php-pear php-xmlrpc php-devel
   
   #安装mySQL的扩展
   yum -y install mysql-connector-odbc mysql-devel libdbi-dbd-mysql
   
   #下载wordpress
   wget https://cn.wordpress.org/wordpress-4.7.4-zh_CN.zip
   ```

### 配置

1. 设置开机启动apache：`systemctl enable httpd.service`

2. 设置开机启动mySQL：`systemctl enable mysqld`

3. 重启apache/php/mysql服务

   ```
   systemctl restart mysqld
   systemctl restart php-fpm
   systemctl restart httpd
   ```

4. 配置mySQL数据库，首次登录不需要密码：`mysql`

   ```
   show databases;			#查看数据库
   #修改数据库root密码
   set password for 'root'@'localhost' =password('123456'); 
   exit   #退出
   mysql -u root -p   #用root账户登录数据库，密码123456
   #创建数据库wordpress
   create database wordpress;
   ```

5. 环境检查：

   ```
   yum -y install net-tools
   netstat -tunlp
   
   #查看80端口和3306端口是否开启
   ```

6. 解压下好的wordpress文件：

   ```
   unzip *.zip
   #将其拷入/var/www/html文件内
   cp -r wordpress/* /var/www/html/
   #复制备份config文件
   cp wp-config-sample.php wp-config.php
   #修改配置
   vim wp-config.php
   
   #修改一下部分：
   /** WordPress数据库的名称 */
   define('DB_NAME', 'wordpress');
   
   /** MySQL数据库用户名 */
   define('DB_USER', 'root');
   
   /** MySQL数据库密码 */
   define('DB_PASSWORD', '123456');
   ```

   

7. 关闭SELINUX：`vim /etc/selinux/config`(没有不用配置)

   ```
   #注释掉：
   #SELINUX=enforcing
   #SELINUXTYPE=targeted
   #加入：
   SELINUX=disabled
   ```

8. 运行：`setenforce 0`，使配置生效。

9. 查看apache和php配置文件：

   ```
   #查找配置文件
   find / -name php.ini
   find / -name httpd.conf
   
   #修改对应文件路径下的文件
   vim /usr/lib/tmpfiles.d/httpd.conf
   #底部加入
   PHPIniDir /etc/php.ini
   
   #重启apache服务：
   systemctl restart httpd.service
   ```

   

### 测试

1. 测试apache服务：**ip**，这里装了wordpress：如图：

   ![wordpress](https://gitee.com/gaoyougang/myblog/raw/master/2020/05/18/LAMP%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA%E7%AC%94%E8%AE%B0/wordpress.png)
   
2. php环境测试：进入：/var/www/html中写一个简单的php页面：`vim test.php`

   ```php
   <?php
       echo "Test Page, hi!";
   	phpinfo();
   ?>
   ```

3. 浏览器查看：**IP/test.php**,效果如图

   ![phpinfo](https://gitee.com/gaoyougang/myblog/raw/master/2020/05/18/LAMP%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA%E7%AC%94%E8%AE%B0/php.png)