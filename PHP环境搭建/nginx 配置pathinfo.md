### nginx 配置pathinfo

* 这是一个配置的虚拟主机例子

```shell
server {
   listen 80;
   server_name www.hxjb.com;
   index index.html;
   root "D:/work/HXJB-WEB/public";
   
   location / {   
	   index  index.php index.html index.htm;  
	   
	   if (!-e $request_filename) {
			rewrite  ^/(.*)$  /index.php/$1  last;
			break;
		}
    }
   
   location ~ \.php {
		root           D:/work/HXJB-WEB/public;
		fastcgi_pass   127.0.0.1:9000;
		fastcgi_index  index.php;
		fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
		
		set $real_script_name $fastcgi_script_name;
		if ($fastcgi_script_name ~ "^(.+?\.php)(/.+)$") {
			set $real_script_name $1;
			set $path_info $2;
		}
		
		fastcgi_split_path_info ^(.+\.php)(.*)$; #增加这一句
		fastcgi_param PATH_INFO $fastcgi_path_info; #增加这一句
		include        ../conf/fastcgi_params;
    }
}
```

