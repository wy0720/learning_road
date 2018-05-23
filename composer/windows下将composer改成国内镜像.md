#windows下将composer改成国内镜像

## 分析

步骤

```
1.找到配置文件
2.修改配置文件
```

1.修改composer成国内镜像需要修改composer的配置文件，如何找到配置文件呢？

```
cmd下找到config.json
composer config -l -g
```

2.找到配置文件如何修改呢？

```json
{
    "config": {},
	"repositories": {
        "packagist": {
            "type": "composer",
            "url": "https://packagist.phpcomposer.com"
        }
    }
}
```



更改国内镜像的第二种方式，直接命令行修改

```shell
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```

