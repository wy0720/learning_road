# PHP相关

* 在看laravel依赖注入是发现一段代码

  ```php
  if ($concrete instanceof Closure) {
              $this->binds[$abstract] = $concrete;
          } else {
              $this->instances[$abstract] = $concrete;
    }
  ```

  思索找寻其含义。。。。。

* 1.instanceof用用法

  * 判断是否是某个类的实例

  ```php
  class MyClass1{}
  class MyClass2{}

  $a = new MyClass1();
  var_dump($a instaceof MyClass1);	//true
  ```

  * 判断是否继承于某个实例

  ```php
  class SonClass extends FatherClass{}
  $sonClass = new SonClass();
  var_dump($sonClass instanceof FatherClass);	//true
  ```

* Closure查手册有这样的说法

  ```
  用于代表 匿名函数 的类. 

  匿名函数（在 PHP 5.3 中被引入）会产生这个类型的对象。在过去，这个类被认为是一个实现细节，但现在可以依赖它做一些事情。自 PHP 5.4 起，这个类带有一些方法，允许在匿名函数创建后对其进行更多的控制。 

  除了此处列出的方法，还有一个 __invoke 方法。这是为了与其他实现了 __invoke()魔术方法 的对象保持一致性，但调用匿名函数的过程与它无关。 

  ```

  那么开头的问题就是判断这个参数是否是匿名（闭包）函数。



回调的好处，可以将函数以参数的形式传入，参数可变，于是执行代码时可以实现自动化

```php
class Container
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
		//在执行这个回调时候，只需要在外部调用本类的方法时将预先绑定的函数名$abstract传入即可实现自动执行
        return call_user_func_array($this->binds[$abstract], $parameters);
    }
}
```

## PHP7中关于isset

问题：我在controller当中使用isset时候，laravel5.6给我报错，报错信息为：

```php
Cannot use isset() on the result of an expression (you can use "null !== expression" instead)
```

但是同样的我在model当中使用isset却可以正常使用，没有报错信息

controller当中代码：

 ```php
if (request('project_name') || request('ticket_sn') || isset(request('uid')) || request('price')){
    .....
}
 ```

model当中代码：

```php
if (isset($search['uid'])){
    if ($search['uid']==0){
    $tb->where('uid','=',0);
  }else{
    $tb->where('uid','<>',0);
  }
 }
```

猜想：

1.laravel5.6中不能使用isset来判断request()???



