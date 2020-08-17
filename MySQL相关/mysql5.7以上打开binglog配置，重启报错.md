# mysql5.7以上打开binglog配置，重启报错

## **在MySQL中开启Binlog**

首先进入数据库控制台，运行指令：

```text
mysql> show variables like'log_bin%';
+---------------------------------+-------+
| Variable_name                   | Value |
+---------------------------------+-------+
| log_bin                         | OFF   |
| log_bin_basename                |       |
| log_bin_index                   |       |
| log_bin_trust_function_creators | OFF   |
| log_bin_use_v1_row_events       | OFF   |
+---------------------------------+-------+
5 rows in set (0.00 sec)
```

可以看到我们的binlog是关闭的，都是OFF。接下来我们需要修改Mysql配置文件，执行命令：

```text
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```

在文件末尾添加：

```text
log-bin=/var/lib/mysql/mysql-bin
```

保存文件，重启mysql服务：

```text
sudo service mysql restart
```

重启完成后，查看下mysql的状态：

```text
systemctl status mysql.service
```

这时，如果你的mysql版本在5.7或更高版本，就会报错：

```text
Jan 06 15:49:58 VM-0-11-ubuntu mysqld[5930]: 2020-01-06T07:49:58.190791Z 0 [Warning] Changed limits: max_open_files: 1024 (requested 5000)
Jan 06 15:49:58 VM-0-11-ubuntu mysqld[5930]: 2020-01-06T07:49:58.190839Z 0 [Warning] Changed limits: table_open_cache: 431 (requested 2000)
Jan 06 15:49:58 VM-0-11-ubuntu mysqld[5930]: 2020-01-06T07:49:58.359713Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (se
Jan 06 15:49:58 VM-0-11-ubuntu mysqld[5930]: 2020-01-06T07:49:58.361395Z 0 [Note] /usr/sbin/mysqld (mysqld 5.7.28-0ubuntu0.16.04.2-log) starting as process 5930 ...
Jan 06 15:49:58 VM-0-11-ubuntu mysqld[5930]: 2020-01-06T07:49:58.363017Z 0 [ERROR] You have enabled the binary log, but you haven't provided the mandatory server-id. Please refer to the proper server
Jan 06 15:49:58 VM-0-11-ubuntu mysqld[5930]: 2020-01-06T07:49:58.363747Z 0 [ERROR] Aborting
Jan 06 15:49:58 VM-0-11-ubuntu mysqld[5930]: 2020-01-06T07:49:58.363922Z 0 [Note] Binlog end
Jan 06 15:49:58 VM-0-11-ubuntu mysqld[5930]: 2020-01-06T07:49:58.364108Z 0 [Note] /usr/sbin/mysqld: Shutdown complete
Jan 06 15:49:58 VM-0-11-ubuntu systemd[1]: mysql.service: Main process exited, code=exited, status=1/FAILURE
```

**You have enabled the binary log, but you haven't provided the mandatory server-id. Please refer to the proper server**

之前我们的配置，对于5.7以下版本应该是可以的。但对于高版本，我们需要指定server-id。

如果你不是分布式的部署Mysql，这个server-id随机给个数字就可以。

```text
server-id=123454
```