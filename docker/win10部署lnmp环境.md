# win10部署lnmp环境

首先把docker换成国内的镜像吧，国外的会超时拉不下来

Windows的点击右下角的小鲸鱼>settings>Docker Engine，加入

```json
{
  "registry-mirrors": ["http://hub-mirror.c.163.com"]
}
```



链接：https://blog.csdn.net/wpeishi/article/details/96730188



简介：本次主要记录自己在win10 docker 的docker-compose搭建项目，涉及到docker的相关知识请自行 百度 goole.
为了更清楚的展示 docker-compose 我这边一一举例展示

用到的命令有`

docker-compose up -d
docker-compose down

    1
    2

1 通过docker-compose 安装Nginx
我们安装nginx官方镜像
新建一个空文件夹 docker-demo（在 D:\demos\docker-demo）
1 新建文件 docker-compose.yml，内容如下：

```shell
version: "3"
services:
  web:
    image: nginx
    ports:
      - "8080:80"
    volumes:
      - ./www:/var/www/html/project
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
      - ./nginx/log/nginx:/var/log/nginx
    networks:
      - webnet
networks:
  webnet:
```

新建文件 docker-demo/nginx/default.conf ，内容如下：

    server {
       	listen          80;
       	index           index.php index.html;
       	server_name     _;
       	root            /var/www/html/project;
       		    
       	access_log      /var/log/nginx/access.log;
       	error_log       /var/log/nginx/error.log;
    	
    }

在 www 下新建 index.html 测试nginx是否成功 就是个html页面

    <!DOCTYPE html> 
    <html>
    	<head>
        	<meta charset="utf-8">
        	<title></title> 
        </head> 
        <body>
        	<h1>docker-compose nginx 成功啦 </h1> 
        </body>
    </html>

在 docker-demo 文件夹下执行:（以管理员身份运行 cmd）
住：在 docker-compose.yml 同级目录执行下面命令

    docker-compose up -d

-d (detach)参数，可以让容器在后台运行
在这里插入图片描述
如果本地没有 nginx 镜像，docker 会自动先拉取镜像
docker 先用默认驱动新建了一个网络 docker-demo_webnet，因为我们在 docker-compose.ym 中有配置网络 webnet，其中 docker-demo 是文件夹名（这是默认行为，为了防止重名）。然后新建了一个 nginx 容器 docker-demo_web_1
此时 nginx 在运行状态，打开浏览器，访问 http://localhost:8080/ 或者对应虚拟机的 ip 即可，运行结果如下：
在这里插入图片描述
查看 docker 运行状态

    docker ps

移除通过 docker-compose 部署的环境

    docker-compose down

接下来 加上 PHP
在 docker-compose.yml 中添加 php 服务：

    version: "3"
    services:
      web:
        image: nginx
        ports:
          - "8080:80"
        volumes:
          - ./www:/var/www/html/project
          - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
          - ./nginx/log/nginx:/var/log/nginx
        networks:
          - webnet
      php:
        image: php:7.0-fpm
        volumes:
          - ./www:/var/www/html/project
        networks:
          - webnet
    networks:
      webnet:

修改 nginx/default.conf 添加 FastCGI 配置：
	

	server {
	   	listen          80;
	   	index           index.php index.html;
	   	server_name     _;
	   	root            /var/www/html/project;
	   		    
	   	access_log      /var/log/nginx/access.log;
	   	error_log       /var/log/nginx/error.log;
	    location ~ \.php$ {
	        try_files $uri =404;
	        fastcgi_split_path_info ^(.+\.php)(/.+)$;
	        fastcgi_pass php:9000;
	        fastcgi_index index.php;
	        include fastcgi_params;
	        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	        fastcgi_param PATH_INFO $fastcgi_path_info;
		}
	}
最后在项目目录 www 中添加 phpinfo.php 文件

    <?php 
    	phpinfo();
    ?>

运行

    docker-compose up -d

测试成功！(～￣▽￣)～

    http://localhost:8080/phpinfo.php

在这里插入图片描述
安装 PDO 扩展
官方的镜像都是“纯净版”，也就是没有安装任何扩展，要用 PHP 连接数据库还需要先安装一下扩展，这里我们安装 PDO

所以接下来我们要自己动手构建一个镜像了
新建一个空的文件夹 php，新建文件 Dockerfile：

    FROM php:7.1-fpm
    RUN apt-get update \ 
    && docker-php-ext-install pdo pdo_mysql

构建 进入到PHP文件夹 执行 Dockerfile

    docker build -t youname/php:7.1-fpm .
    
    这里最后面是有个 空格  ‘ .’ 的不然会报错

注：youname == 这里最好是你在 https://hub.docker.com/ 的登录名 后面要用到

上传到 dockerhub （具体步骤可以查看 官方文档 ）

    docker login // 登录，随后会提示输入密码
    docker push youname/php:7.1-fpm

这样就可以在 docker-compose.yml 中将官方 php 镜像替换成我们刚刚构建成的带 PDO 扩展的镜像

    version: "3"
    services:
      web:
        image: nginx
        ports:
          - "8080:80"
        volumes:
          - ./www:/var/www/html/project
          - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
          - ./nginx/log/nginx:/var/log/nginx
        networks:
          - webnet
      php:
        image: wpeishi/php:7.1-fpm
        volumes:
          - ./www:/var/www/html/project
        networks:
          - webnet
    networks:
      webnet:

重新加载一下 （之前是在PHP目录执行加载pdo 下面要退出来 到docker-demo）

    docker-compose up -d
    http://localhost:8080/phpinfo.php

MYSQL
修改 docker-compose.yml 如下：

    version: "3"
    services:
      web: # 添加 nginx 服务
        image: nginx
        ports:
          - "8080:80"
        volumes:
          - ./www:/var/www/html/project
          - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
          - ./nginx/log/nginx:/var/log/nginx
        networks:
          - webnet
      php: # 添加php-fpm 服务
        image: wpeishi/php:7.1-fpm
        volumes:
          - ./www:/var/www/html/project
        networks:
          - webnet
      mysql: # 添加 mysql 服务
        image: mysql:5.7
        environment:
          - TZ=Asia/Shanghai # 设置时区
          - MYSQL_ROOT_PASSWORD=lwx123 # 设置 root 用户密码
        command: --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci # 设置编码
        networks:
          - webnet
    networks:
      webnet:

加载mysql

    docker-compose up -d

进入容器：

    docker ps
    docker exec -it CONTAINER ID bash

在这里插入图片描述
在docker mysql 登录mysql

    mysql -u root -p
    show databases;
    create database auth;
    use auth;
    create table users (user_name char(16) default '123',user_password varchar(48) default '456');

在这里插入图片描述
在这里插入图片描述
在这里插入图片描述
在 www文件下 新建文件 connect_mysql.php：

    <?php
        try{
            $conn = new PDO("mysql:host=mysql:3306;dbname=auth", "root", "lwx123");
            echo "连接成功"."</br>";
            $stmt = $conn->prepare("SELECT * FROM users");
            $stmt->execute();
            $stmt->setFetchMode(PDO::FETCH_ASSOC);
            foreach ($stmt->fetchAll() as $key => $value) {
                echo $value['user_name'] .'----'. MD5($value['user_password'])."</br>";
            }
        }catch(PDOException $e){
            echo $e->getMessage();
        }
    ?>

在这里插入图片描述
为了数据持久化需求，最后还需要挂载一个数据卷（Volume）到本机上：
最终 修改 docker-compose.yml

    version: "3"
    services:
      web: # 添加 nginx 服务
        image: nginx
        ports:
          - "8080:80"
        volumes:
          - ./www:/var/www/html/project
          - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
          - ./nginx/log/nginx:/var/log/nginx
        networks:
          - webnet
      php: # 添加php-fpm 服务
        image: wpeishi/php:7.1-fpm
        volumes:
          - ./www:/var/www/html/project
        networks:
          - webnet
      mysql: # 添加 mysql 服务
        image: mysql:5.7
        ports:
          - "3306:3306"
        volumes:
          - ./mysql:/var/lib/mysql
        restart: always
        environment:
          - TZ=Asia/Shanghai # 设置时区
          - MYSQL_ROOT_PASSWORD=lwx123 # 设置 root 用户密码
        command: --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --innodb_use_native_aio=0 # 设置编码
        networks:
          - webnet
    networks:
      webnet:

好啦就这些了！
