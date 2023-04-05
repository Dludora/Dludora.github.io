---
title: Vue+Django部署教程
date: 2023-03-30 12:41:31
tags: "vue"
---

# Vue-Django前后端分离项目部署教程

## 一 安装必要依赖

### 1 安装pip3

```bash
sudo apt-get update
sudo apt-get install pip3
```

### 2 安装Django

```python
// 注意大版本要相同
pip3 install django
```

### 3 安装nginx

```bash
sudo apt-get update
sudo apt-get install nginx
```

安装完成后。用你电脑的浏览器访问你的服务器的公网ip地址，看看安装成功没有

### 4 安装uwsgi

在你的本地电脑访问https://uwsgi-docs.readthedocs.io/en/latest/Download.html，下载Stable/LTS版本的源文件。

本地下解压这个源文件，然后用xftp把文件拖放到阿里云的Ubuntu的家目录(home)下，使用cd命令进入到该文件夹下，按顺序依次输入下面三条命令

```bash
sudo apt-get install python3-setuptools
sudo apt-get install python3-dev
sudo python3 setup.py install
```

### 5 安装mysql

若使用的是django自带数据库，则可以跳过

1. 输入下面安装命令：

   ```
   sudo apt-get install mysql-server mysql-client
   ```

   安装过程中会出现叫你输入密码，这个密码一定要记住！

2. 安装完成输入下面命令：

    ```
    mysql -u root -p
    ```
	然后输入你刚刚设置的密码，进去之后输入下面命令：

    ```
    create database myblog
    ```

    创建一个myblog数据库，这个数据库名字跟你将来要还原的数据库名字一样，用xftp把你在本地备份的sql文件拖到阿里云Ubuntu的家目录(home)下。

3. 还原数据库；进入家目录(home)，输入下面命令：

    ```
    sudo mysql -u root -p myblog<myblog.sql
    ```

4. 配置mysql文件：

   ```
   sudo vim/etc/mysql/mysql.conf.d/mysqld.cnf
   ```
	然后注释掉下面这行代码

## 二 配置项目

### 	1 配置后端

#### (1) 配置nginx

​	先用xftp把你的整个博客项目拖到家目录(home)那里，然后开始配置nginx文件：

```
cd /etc/nginx/conf.d
```

​	新建一个`xxx.conf`文件，粘贴如下代码

```
server {
  # listen 443 ssl;				
  # server_name smallpic.dludora.xyz;
  listen 80;					   # 在云服务器上配置安全组，开放80端口(监听http默认80端口)
  server_name xxx.xxx.xx;			# 云服务器的公网IP地址
	charset utf-8;
 	
 	# 后端项目媒体文件夹位置
	location /media {
		alias /home/backend/media;
	}
	
	# 后端项目静态文件夹位置
	location /static {
   		alias /home/backend/static;
	}
 
	location /api {							   # 与后端项目中url的前缀对应
		include /etc/nginx/uwsgi_params;
		uwsgi_pass 127.0.0.1:8080; 				# 记住这里的端口
	}
 	
 	# 以下是前端部署的部分，后面再说
 	location / {
		root /home/frontend;
    	index index.html index.htm;
    	try_files $uri $uri/ /index.html;
	}
 
  # 以下是配置域名的，暂时不考虑
  # ssl_certificate /etc/nginx/cert/9360524_smallpic.dludora.xyz.pem;
  # ssl_certificate_key /etc/nginx/cert/9360524_smallpic.dludora.xyz.key;
  # ssl_session_timeout 5m;
  # ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
  # ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
  # ssl_prefer_server_ciphers on;
}
```

​	**注意：location后面是有空格的，必须要有！alias后面也是有空格的；**

​	重启nginx服务

```
sudo service nginx restart
```

####  (2) 配置uwsgi

​	在后端项目的根目录下，也就是有manage.py文件的目录下，新建一个uwsgi.ini文件和一个run.log文件

​	然后我们使用vim编辑器编辑uwsgi.ini文件：

```ini
[uwsgi]
chdir = /home/backend						# 后端项目位置
module = backend:application 				 # :application前面要换成自己的文件夹名称
socket = 127.0.0.1:8080						# 这里与上面conf文件中的端口要一样！		
wsgi-file = /home/backend/backend/wsgi.py	  # 项目中的wsgi-file路径
master = true         
daemonize = /home/backend/run.log			 # 新建一个run.log文件记录信息
disable-logging = true
```

####  (3) 配置MySql（若用的是自带数据库，则可跳过）

修改setting.py 所在目录的那个 init.py文件使用vim编辑器打开init.py文件输入一下代码：

```python
import pymysql
pymysql.install_as_MySQLdb()
```

安装mysql驱动：

```python
pip3 install pymysql
```

#### (4) 修改settings.py文件

打开settings.py文件找到下面代码并修改：

```
DEBUG = False
ALLOWED_HOSTS = ['192.168.178.128']		// 改成['*']也行
```

注意其中的IP地址要替换成你自己阿里云公网的IP。

#### (5) 运行后端

在`uwsgi.ini`目录下运行

```bash
uwsgi --ini uwsgi.ini
```

如果出现

```
[uWsgi] getting INI configuration from uwsgi.ini
```

则说明运行成功，

可以输入指令`ps -aux | grep uwsgi`来查看运行的进程

### 2 配置前端

#### (1) 修改axios的baseurl

修改为：`http://xxx.xxx.xx/api` ，这里的`/api`要与后端接口中一致（因为这里前后端都部署在一个云服务器上，所以后端的所有接口都要有一个`/api`前缀进行区分）

#### (2) 打包前端

运行

```
npm run build
```

将打包后的`dist`文件夹放到`/home`目录下，在nginx的conf配置文件中修改，如下注释，root那里要与实际路径相同。

```
server {
  # listen 443 ssl;				
  # server_name smallpic.dludora.xyz;
  listen 80;					   # 在云服务器上配置安全组，开放80端口(监听http默认80端口)
  server_name xxx.xxx.xx;			# 云服务器的公网IP地址
	charset utf-8;
 	
 	# 后端项目媒体文件夹位置
	location /media {
		alias /home/backend/media;
	}
	
	# 后端项目静态文件夹位置
	location /static {
   		alias /home/backend/static;
	}
 
	location /api {							   # 与后端项目中url的前缀对应
		include /etc/nginx/uwsgi_params;
		uwsgi_pass 127.0.0.1:8080; 				# 记住这里的端口
	}
 	
 	# 以下是前端部署的部分，后面再说
 	location / {
		root /home/frontend;
    	index index.html index.htm;
    	try_files $uri $uri/ /index.html;
	}
 
  # 以下是配置域名的，暂时不考虑
  # ssl_certificate /etc/nginx/cert/9360524_smallpic.dludora.xyz.pem;
  # ssl_certificate_key /etc/nginx/cert/9360524_smallpic.dludora.xyz.key;
  # ssl_session_timeout 5m;
  # ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
  # ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
  # ssl_prefer_server_ciphers on;
}
```

