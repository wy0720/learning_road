### docker安装

Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。通过 uname -r 命令查看你当前的内核版本

uname -r 
1
centos版本

3.10.0-514.el7.x86_64

1、初步安装和启动docker

```
yum update -y
yum -y install docker
systemctl start docker
```


2、设置镜像
vi /etc/docker/daemon.json

```
{
  "registry-mirrors": ["https://aj2rgad5.mirror.aliyuncs.com"]
}
```



4、重启docker
systemctl daemon-reload
systemctl restart docker.service

5、测试docker是否正常安装和运行
docker run hello-world

6、查看结果
Hello from Docker!
This message shows that your installation appears to be working correctly.