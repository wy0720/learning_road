# review

## elasticsearch

### 搜索快的原理

#### 正向索引

##### 数据结构

例如“文档1”经过分词，提取了20个关键词，每个关键词都会记录它在文档中的出现次数和出现位置。

得到**正向索引**的结构如下：

​       “文档1”的ID > 单词1：出现次数，出现位置列表；单词2：出现次数，出现位置列表；…………。

​       “文档2”的ID > 此文档出现的关键词列表。

##### 缺点

**当用户在主页上搜索关键词“华为手机”时，假设只存在正向索引（forward index），那么就需要扫描索引库中的所有文档，找出所有包含关键词“华为手机”的文档，再根据打分模型进行打分，排出名次后呈现给用户。**因为互联网上收录在搜索引擎中的文档的数目是个天文数字，**这样的索引结构根本无法满足实时返回排名结果的要求。**

#### 倒排索引

**将正向索引重新构建为倒排索引**，即把文件ID对应到关键词的映射转换为**关键词到文件ID的映射**，每个关键词都对应着一系列的文件，这些文件中都出现这个关键词。

##### 数据结构

得到**倒排索引**的结构如下：

​       “关键词1”：“文档1”的ID，“文档2”的ID，…………。

​       “关键词2”：带有此关键词的文档ID列表。

#### 简单例子

比如需要查一个字：

**正向索引**就是去翻整本书，一直到找到这个字。【这个字所在的页码就是文档id】

**倒排索引**就是先去查目录【这个就是索引】，找到这个字所在的位置，然后去找到文档



### 为什么es比mysql检索要快

#### mysql中的索引类型

##### 从数据结构角度划分

* B+树索引(O(log(n)))：btree索引

  * myisam的索引文件仅保存数据记录的<font color=red>地址</font>
  * inodb树的叶节点data域保存了完整的数据记录

* hash索引:只支持<font color='red'>Memory引擎</font>

* FULLTEXT索引（现在MyISAM和InnoDB引擎都支持了）

  * 主要用来查找文本中的关键字，而不是直接与索引中的值相比较。fulltext索引跟其它索引大不相同，它更像是一个搜索引擎，而不是简单的where语句的参数匹配。fulltext索引配合**match against**操作使用，而不是一般的where语句加like。

    ```
    SELECT * FROM article WHERE MATCH(title, content) AGAINST('查询字符串')
    ```

* R-Tree索引（空间索引）（用于对GIS数据类型创建SPATIAL索引），只有<font color="red">myisam支持</font>

##### 从物理角度划分

* 聚集索引（clustered index）

* 非聚集索引（non-clustered index）

##### 从逻辑角度

* 普通索引或者单列索引
* 唯一索引或者非唯一索引
  * 与前面的普通索引类似，不同的就是：索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。它有以下几种创建方式：
* 主键索引：主键索引是一种特殊的唯一索引，一个表只能有一个主键，不允许有空值，一般是在建表的时候同时创建主键索引：
* 多列索引（组合索引）：
  * 指多个字段上创建的索引，只有在查询条件中使用了创建索引时的第一个字段，索引才会被使用。使用组合索引时遵循<font color=red>最左前缀</font>集合

* 空间索引

<font color=red>注意：mysql中普通索引就是btree索引</font>

#### 简单理解

mysql的索引只是存储field的内容（如果过长，只是存前多少位的内容为索引）并没用分词
es存储的是分词以后的索引，每个词都在哪些文档中出现过。
如果是搜索 keyword这种基本没啥影响
但是如果是mysql的like "%word%" mysql全表查，es只需要查"word"这个词包含的文档id 速度明显不是一个级别。

## mysql

### 分库分表的分页问题

### binglog

- 默认开启
- 可以设置最大值，默认1g



### 主从复制

#### 原理

- 主服务器上任何数据的更改都会记录到binglog日志中
- 住服务器开启客户端进程链接到从服务器
- 从库读取读取到的二进制存到Realy log
- 从库开启SQL thread定时检查Realy log

#### 类型

基于语句的复制

> 在从库上再执行一次语句。那如果执行时间不同，数据是会产生偏差的。

基于行的复制

> 将主库变更的数据全部同步到从库，如果有大量更改会造成资源开销大

混合模式

> MySQL自动选择，默认优先语句复制，如果造成数据偏差就会使用行复制

主从复制架构中，写操作只能在主库执行。

#### 配置

```shell
配置Master的my.cnf文件(关键性的配置)/etc/my.cnf
log-bin=mysql-bin

server-id= 1

binlog-do-db=icinga

binlog-do-db=DB2     //如果备份多个数据库，重复设置这个选项即可

binlog-do-db=DB3   //需要同步的数据库，如果没有本行，即表示同步所有的数据库

binlog-ignore-db=mysql  //被忽略的数据库

配置Slave的my.cnf文件(关键性的配置)/etc/my.cnf

log-bin=mysql-bin

server-id=2

master-host=10.1.68.110

master-user=backup

master-password=1234qwer

master-port=3306

replicate-do-db=icinga

replicate-do-db=DB2

replicate-do-db=DB3   //需要同步的数据库，如果没有本行，即表示同步所有的数据库

replicate-ignore-db=mysql   //被忽略的数据库

```



### MySQL优化

- 开启query_cache
  - 这在MySQL5.8去掉了，因为这个会带来性能上的问题，而且高性能的MySQL服务器qc缓存优势不大。
- 适当建立索引
- 查询优化

## PHP

### 框架

#### laravel

##### 主要设计模式

###### ico容器

1、什么是依赖，举个例子：

```
//支付宝支付
class Alipay {
      public function __construct(){}
}
//微信支付
class Wechatpay {
      public function __construct(){}
}
//支付账单
class PayBill {

      private $payMethod；

      public function __construct( )
      {
          $this->payMethod= new Alipay ();
      }
}
```

在paybill的里面的构造函数new的类就是这个类的<font color=red>依赖</font>

2、依赖的注入，再举个栗子

```

//支付类接口
interface Pay
{
    public function pay();
}


//支付宝支付
class Alipay implements Pay {
      public function __construct(){}

      public function pay()
      {
          echo 'pay bill by alipay';
      }
}
//微信支付
class Wechatpay implements Pay  {
      public function __construct(){}

      public function pay()
      {
          echo 'pay bill by wechatpay';
      }
}

//付款
class PayBill {

      private $payMethod;

      public function __construct( Pay $payMethod)
      {
          $this->payMethod= $payMethod;
      }

      public function  payMyBill()
      {
           $this->payMethod->pay();
      }
}

//生成依赖
$payMethod =  new Alipay();
//注入依赖
$pb = new PayBill( $payMethod );
$pb->payMyBill();
```

外部调用payBill的时候传入的支付方式就是依赖的注入，不过这里的注入是<font color=red>手动注入</font>

3、IOC容器：简单理解就是创建一个容器将对象的实例化丢到容器中自己加工，想要用的时候取出来就行了，就不用new对象了：build创建实例、make获取实例

参考一下一篇好文【工厂+IOC】：<https://xueyuanjun.com/post/769>

从此我的工厂可以不switch case了

```
class Container//这个容器就是一个大工厂
{
    protected $binds;

    protected $instances;

    public function bind($abstract, $concrete)
    {
        if ($concrete instanceof Closure) {
            $this->binds[$abstract] = $concrete;
        } else {
            $this->instances[$abstract] = $concrete;
        }
    }

    public function make($abstract, $parameters = [])
    {
        if (isset($this->instances[$abstract])) {
            return $this->instances[$abstract];
        }

        array_unshift($parameters, $this);

        return call_user_func_array($this->binds[$abstract], $parameters);
    }
}



// 创建一个容器（后面称作超级工厂）
$container = new Container;

// 向该 超级工厂 添加 超人 的生产脚本
$container->bind('superman', function($container, $moduleName) {
    return new Superman($container->make($moduleName));
});

// 向该 超级工厂 添加 超能力模组 的生产脚本
$container->bind('xpower', function($container) {
    return new XPower;
});

// 同上
$container->bind('ultrabomb', function($container) {
    return new UltraBomb;
});

// ******************  华丽丽的分割线  **********************
// 开始启动生产
$superman_1 = $container->make('superman', 'xpower');
$superman_2 = $container->make('superman', 'ultrabomb');
```



###### facade

作用：提供访问容器中对象功能的类

举个例子：

```
文件系统中：
File::exist ($path)，判断文件是否存在。有没有感觉很方便，我们来看一下为什么在业务代码中直接这样调用就可以了

1、FilesystemServiceProvider服务被注册到启动的服务中了，也就是在启动时候会将这个服务注册到IOC容器中
2、看一下是怎么注入的
 /**
     * Register the native filesystem implementation.
     *
     * @return void
     */
    protected function registerNativeFilesystem()
    {
        $this->app->singleton('files', function () {
            return new Filesystem;
        });
    }
    
3、在调用时File的门面
class File extends Facade
{
    /**
     * Get the registered name of the component.
     *
     * @return string
     */
    protected static function getFacadeAccessor()
    {
        return 'files';
    }
}
```



#### yii

##### laravel和yii的区别

* 源码可读性yii要好一些，点一下就进去了，laravel的faced阅读有困难
* laravel的artisan和yii2的gii功能很像
* laravel的blade模板引擎很好用，而yii2耦合的比较严重
* yii2比laravel更轻
* yii2是oop框架

#### tp



### 常用函数和数组

```
__autoload:实例化一个未定义的类时，就会触发此函数
spl_autoload_register:这个函数需要手动调用

##内置类
可以利用Closure【闭包类】判断是否是匿名函数
ReflectionClass：反射类，获取类的相关东西，eg:$ref = new ReflectionClass(Student::class)
```

### 魔术方法

```php
__constract
__destract
__call
__callStatic
__get
__set
__isset
__unset
__sleep：serilaise
__wakeup
__toString
__invoke:用函数的方式调用一个对象
class Person
{
    public function __invoke() {
        echo '这可是一个对象哦';
    }

}
$person = new Person(); // 初始赋值
$person();

__set_state:使用var_export导出时触发

__clone
__autoload
__debugInfo:var_dump触发
```

### 魔术常量

```
__LINE__	返回当前行号
__FILE__	文件的完整路径和文件名。如果用在被包含文件中，则返回被包含的文件名。
__DIR__		文件所在的目录。如果用在被包括文件中，则返回被包括的文件所在的目录。
__FUNCTION__	函数名称常量返回该函数被定义时的名字（区分大小写）。
__CLASS__	类的名称常量返回该类被定义时的名字（区分大小写）。
__TRAIT__	Trait 的名字常量返回 trait 被定义时的名字（区分大小写）。
__METHOD__	类的方法名返回该方法被定义时的名字（区分大小写）。
__NAMESPACE__	当前命名空间的名称（区分大小写）。此常量是在编译时定义的（PHP 5.3.0 新增）。
```

### 超全局变量

```
服务器变量： $_SERVER
环境变量： $_ENV
HTTP GET变量： $_GET
HHTP POST变量： $_POST
request变量： $_REQUEST
HTTP文件上传变量： $_FILES
HTTP Cookies: $_COOKIE
Session变量： $_SESSION
Global变量： $_GLOBALS
```

### psr0-psr4规范

psr0：命名空间规范，psr4是对他的升级，psr4



## 缓存

### redis

##### 数据类型

* string

  * 操作：set、get、del、setex【过期时间】、psetex【毫秒为单位】、incr、decr

* hash

  | 插入操作           | hset、hmset、hsetnx                               |
  | ------------------ | ------------------------------------------------- |
  | 删除操作           | hdel                                              |
  | 查询操作           | hgetall、hget、hmget、hkeys、hvals、hexists、hlen |
  | 整数型、浮点型操作 | hincrby、hincrbyfloat                             |

* list

  操作类型	常用命令
  插入操作	lpush、rpush、linsert、lpushx、rpushx
  弹出、删除操作	lpop、rpop、lrem、blpop、brpop、brpoplpush、ltrim
  修改操作	lset
  查询操作	lrange、llen、lindex

* set

  插入操作	sadd
  弹出、删除操作	spop、srem
  集合操作	sdiff、sinter、sunion、sdiffstore、sinterstore、sunionstore、sismember、smove、srandmember
  查询操作	smembers【返回集合中所有的成员】、scard【返回集合中的数量】

* sort set：评论的点赞【】

  插入操作	zadd
  删除操作	zrem、zremrangebyscore、zremrangebylex、zremrangebyrank
  查询操作	zrangebyscore、zcard、zcount、zlexcount、zrangebylex、zrank、zrevrange、zrevrangebyscore、zrevrank、zscore
  集合操作	zinterstore、zunionstore、zincrby
  ————————————————
  原文链接：https://blog.csdn.net/HG_Harvey/article/details/78034600



## Linux

#### Nginx

##### nginx优化

1. gzip压缩和Expres设置

2. 反向代理（动静分离）

   proxy_pass

3. 负载均衡

   upstream

   反向代理会导致服务器只能获取到代理服务器的ip



## 微信开发

### 小程序

### 订阅号

## Vue



## 关于时间复杂度

## 项目

###  基于swoole和websocket实现的即时聊天系统

服务端

swoole中的fd是每个客户端链接的标志，不会重复。

一般情况下，会将前端传来的uid和后端生成的进程fd匹配，进行消息的push

```
$server    = new swoole_websocket_server("127.0.0.1", 9502);

$server->on('open', function($server, $req) {
    echo "新连接: #{$req->fd}\n";
});

//这里处理客户端过来的消息
$server->on('message', function($server, $frame) use ($FdMapping) {})

$server->on('close', function($server, $fd) use ($FdMapping) {
    echo "连接断开: #{$fd}\n";
    $FdMapping->delFd($fd);
});

$server->start();
```



客户端

```
var ws = new WebSocket("ws://127.0.0.1:9502");

ws.onopen = function (evt) {
    console.log("Connection open ...");
    ws.send('{"type":"sign", "uid":"1001"}');
};

ws.onmessage = function (evt) {
    console.log("Received Message: " + evt.data);
    // ws.close();
};

ws.onclose = function (evt) {
    console.log("Connection closed.");
};
```





## 一些高级知识

### 如何优化PHP网站

- nignx：做负载均衡

- cdn加速
- 代码层面：减少代码冗余，多使用内置方法

- 发挥opcache的作用

- 使用缓存：redis、memcache

- 数据库层面：优化查询

- 反向代理、负载均衡集群、分布式：将业务打散分布

- 负载均衡集群
  - 使用集群为大流量网站分压
  - 使用集群保证一台主机宕机后服务可用【及高可用集群】

### http协议，tcp长链接

### 一些概念

- 正向代理与反向代理

  > 举个例子：正向代理就是：A要找C借钱，但是A不认识C，A通过B找C借钱，换言之，就是A清楚的知道目标是谁，只是通过代理触达到了这个目标。而反向代理就是：A找B代理借钱，B找了其他某某某借钱然后再借给你。换言之，就是A不关心B找谁把资源给你，A只需要知道B能提供这个服务。

- 负载均衡

  产生原因：对于大流量网站，单个服务器的资源总是有限的，如果能够提供多个服务器资源那就可以分压了。

- 分布式

  将业务打散，分布到集群服务中

## 算法和数据结构

排序：<https://www.awaimai.com/1980.html>



## 解决问题的一些案例

### 评论审核时es同步不及时的处理方案

mysql和es的同步存在时间差，拉取数据走的是es，造成一种后果就是刚审核通过的数据会再次在待审核状态中出现。当时没有找到问题根源，卡了一整子。

解决方案：在审核操作时将审核过的评论的id缓存起来，在查询es的时候再筛掉这些数据。

总结：解决问题需要透过问题看本质

### 在处理大表时count的方案

问题：在大表进行查询的时候一般会count一次，然后select一次，扫描航就是双倍的量

解决方案：使用EXPLAIN进行查询

### 关注公众号发红包的问题

微信获取access_token，这个在easyWeChat中有缓存设置，原来是在文件系统中设置。业务模式是当用户关注我们的服务号时：通过openid和access_token换取unionid，然后给用户发红包。上线后出现一个问题：有的用户可以领到红包，有的用户不能领到红包！！

解决方案：在获取access的地方打了日志，把日志拉出来看，反反复复看，发现host的地址不一样！！恍然大悟，我们的服务做了负载！每次请求的都不一样的。原来的easywechat中设置的文件缓存只存在于改主机中。于是立马把access_token 的缓存迁出到了redis中。问题解决