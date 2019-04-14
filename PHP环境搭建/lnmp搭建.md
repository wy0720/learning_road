### lnmp搭建

* 先把包下载下来

  ```
  wget http://nginx.org/download/nginx-1.14.0.tar.gz
  wget http://cn.php.net/distributions/php-7.2.17.tar.gz
  wget http://cdn.mysql.com/Downloads/MySQL-5.6/mysql-5.6.43.tar.gz
  ```


* 参考文章

  https://blog.csdn.net/nimo97/article/details/80536034

  https://blog.csdn.net/qq_40326199/article/details/80791266

* 1.安装nginx

  ```
  1.解压，再装nginx的一些依赖，用yum装了
  tar -xf nginx-1.14.0.tar.gz（解压完成后进入解压文件中）

  yum -y install gcc
  yum install -y pcre-devel
  yum install -y openssl-devel

  3.要设置nginx用户，不然启动时会报错：nginx: [emerg] getpwnam("nginx") failed
  useradd -s /sbin/nologin -M nginx
  id nginx

  2.设置config
  ./configure --prefix=/usr/local/nginx --user=nginx --group=nginx --with-http_ssl_module --with-http_flv_module --with-http_stub_status_module --with-http_gzip_static_module --http-client-body-temp-path=/var/tmp/nginx/client --http-proxy-temp-path=/var/tmp/nginx/proxy --http-fastcgi-temp-path=/var/tmp/nginx/fastcgi --http-uwsgi-temp-path=/var/tmp/nginx/uwsgi --http-scgi-temp-path=/var/tmp/nginx/scgi --with-pcre --with-file-aio --with-http_secure_link_module --with-threads

  3.make && make install

  到这里就装好了，检查有没有错误输入echo $?      0表述没有错误

  4.启动nginx
  /usr/local/nginx/sbin/nginx

  5.设置开机自启（这部参考了教程，有待验证）
  tail -1 /etc/rc.d/rc.local

  浏览器打开ip地址访问即可

  ```

  * nginx 配置文件中的PHP配置

    ```
    location ~ \.php$ {
                root           html;
                fastcgi_pass   127.0.0.1:9000;
                fastcgi_index  index.php;
                fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
            #    include        fastcgi_params;
                include        fastcgi.conf;#改成这句话才能访问
            }
    ```

    ​

* 2.PHP安装

  ```
  1.安装依赖
  yum -y install libxml2-devel bzip2-devel libmcrypt libmcrypt-devel mcrypt mhash mhash-devel curl-devel libjpeg-devel libpng libpng-devel freetype freetype-devel libxslt libxslt-devel

  2.解压软件包
  tar -xf php-7.2.17.tar.gz
  cd  php-7.2.17.tar.gz

  3.检测编译项目
  ./configure --prefix=/usr/local/php-7.2.17 --with-curl --with-freetype-dir --with-gd --with-gettext --with-iconv-dir  --with-libxml-dir --with-mysqli --with-openssl --with-pcre-regex --with-pdo-mysql --with-pdo-sqlite --with-pear --with-png-dir --with-jpeg-dir --with-xmlrpc --with-xsl --with-zlib --with-bz2 --with-mhash --enable-fpm --enable-bcmath --enable-libxml --enable-inline-optimization --enable-gd-native-ttf --enable-mbregex --enable-mbstring --enable-opcache --enable-pcntl --enable-shmop --enable-soap --enable-sockets --enable-sysvsem --enable-sysvshm --enable-xml --enable-zip

  这里出现了configure: WARNING: unrecognized options: --enable-gd-native-ttf这样一个问题，查阅资料说PHP7.2不支持编译这个选项，在编译时去掉上面的那个选项

  4.make && make install

  5.cd /usr/local/php/etc

  6.mv php-fpm.conf.default  php-fpm.conf这是进程的配置文件，需要修改一下。

  7.将解压的文件里的php.ini拷贝到安装的文件下
  cd 解压文件夹
  cp php.ini-development /usr/local/php-7.2.17/etc/php.ini

  8.执行命令
  cp /usr/local/php-7.2.17/etc/php-fpm.d/www.conf.detault /usr/local/php-7.2.17/etc/php-fpm.d/www/conf

  和第六步操作类似

  8.开启php-fpm
  /usr/local/php-7.2.17/sbin/php-pfm

  ```

  ​

* php编译常用项

  ```shell
  ./configure --prefix=/usr/local/php-7.2.7 --with-curl --with-freetype-dir --with-gd --with-gettext --with-iconv-dir  --with-libxml-dir --with-mysqli --with-openssl --with-pcre-regex --with-pdo-mysql --with-pdo-sqlite --with-pear --with-png-dir --with-jpeg-dir --with-xmlrpc --with-xsl --with-zlib --with-bz2 --with-mhash --enable-fpm --enable-bcmath --enable-libxml --enable-inline-optimization --enable-gd-native-ttf --enable-mbregex --enable-mbstring --enable-opcache --enable-pcntl --enable-shmop --enable-soap --enable-sockets --enable-sysvsem --enable-sysvshm --enable-xml --enable-zip
  ```

  ​

* 3.安装mysql

  参考：https://www.cnblogs.com/piaoyang2087/articles/7908795.html

  ```shell
  1.安装依赖
  yum -y install cmake bison git ncurses-devel gcc gcc-c++

  2.创建一个用户名为mysql的用户并加入mysql用户组
  groupadd mysql
  useradd -g mysql mysql

  2.解压文件
  tar -xf mysql-5.6.43.tar.gz
  mkdir /usr/local/mysql-5.6.43
  mkdir /usr/local/mysql-5.6.43/data
  cd mysql-5.6.43/

  3.执行编译检测
  cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql-5.6.43 -DMYSQL_UNIX_ADDR=/usr/local/mysql-5.6.43/mysql.sock -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DMYSQL_DATADIR=/usr/local/mysql-5.6.43/data -DMYSQL_TCP_PORT=3306 -DMYSQL_USER=mysql -DENABLE_DOWNLOADS=1

  4.make && make install（比较长时间）

  5.修改目录属主权限
  chown -R mysql:mysql /usr/local/mysql/data/
  chown -R mysql:mysql /usr/local/mysql/

  6.创建mysql server系统表
  yum install perl
  yum install perl-Module-Install.noarch

  scripts/mysql_install_db --user=mysql --datadir=/usr/local/mysql-5.6.43/data

  7.把初始化生成的 /usr/local/mysql/my.cnf 配置文件的属主数组更改为mysql
  chown -R mysql:mysql /usr/local/mysql-5.6.43/（执行这条命令前刚生成的conf文件时root组）

  8.配置启动脚本
  cp /usr/local/mysql-5.6.43/support-files/mysql.server /etc/init.d/mysql
  /etc/init.d/mysql status
  /etc/init.d/mysql start

  9.配置环境变量
   vi /etc/profile
   	```
   	最后一行加上
   	PATH=$PATH:$HOME/bin:/usr/local/mysql-5.6.43/bin:/usr/local/mysql-5.6.43/lib
  	export PATH
   	```
   变量生效
   source /etc/profile
   
   此时mysql已经可以正常访问了
   10.根据需要设置开机自动启动服务
   chkconfig mysql on
   
  ```

  ​