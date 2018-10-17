### IOC(控制反转)

* 参考：https://www.insp.top/learn-laravel-container


* 控制反转：所谓的反转的控制是什么？

  所谓的控制即类的依赖。

* 解决依赖的方式之一：工厂模式

  * 工厂模式（举例生产超人功能的类）

    工厂生产超人所需要的功能，设置switch 根据关键字返回功能实例。超人类调用工厂进行按需获取，在实例化时传入关键词触发工厂调用返回实例。

    ```php
    class Super
    {
        protected power;
        
        public function __construct($models)
        {
        	$factory = new Factory()
            foreach($models as $modelName => $modelOpetion){
                $this->power[] = $factory->makePower($modelName);
            }
        }
    }

    new Super([
        'Fight',
        'Shot'
    ]);
    ```

  * 工厂缺陷：超人从对超能力的依赖变成了对工厂的依赖，如果超人需要更更多的能力，那么工厂的switch case将非常的多，而且需要不断的去改工厂类，注意，划重点，改工厂~~

* 工厂的改进：依赖注入（DI）

  只要不是由内部生产（比如初始化、构造函数 `__construct` 中通过工厂方法、自行手动 new 的），而是由外部以参数或其他形式注入的，都属于 `依赖注入（DI）`

  ```php
  class Superman
  {
      protected $module;

      public function __construct(SuperModuleInterface $module)
      {
          $this->module = $module //注入的超能力实例必须是实现了接口的
      }
  }
  ```


  $superModule = new XPower;

  // 初始化一个超人，并注入一个超能力模组依赖
  $superMan = new Superman($superModule);
  ```php

  一个超人拥有的能力变少，但是创建超人变得容易（不是很理解这句话）

  * 缺陷：需要手动（自己new了传进去）注入超能力

* IOC容器来了

  原理：外部使用，先绑定后获取。

  ```php
  class Container
  {
      protected binds;
      protected instance;
      
      public function bind($abstract , $concrect)
      {
          if($concrect instanceof Clusure){
              $this->binds[$abstract] = $concrect;
          }else{
              $this->instance[$abstract] = $concrect;
          }
      }
      
      public function make($abstract , $params = [])
      {
      	if($this->instance[$abstract] ?? ''){
              return $this->instance[$abstract];
      	}
      	
      	return call_user_func_array($this->binds[$abstract] , $params);
      }
  }
  ```

  ```php
  $container = new Container;

  //这段代码是否有些似曾相识。。。
  //创造超人，需要注入超能力！！！！
  $container->bind('superman' , function($container , $moduleName){
      return new Superman($container->make(moduleName));
  });

//注册x射线超能力
 $container->bind('xpower', function($container) {
    return new XPower;
 });

 $container->bind('ultrabomb', function($container) {
    return new UltraBomb;
 });

  $superman_1 = $container->make('superman', ['xpower']);
  $superman_2 = $container->make('superman', ['ultrabomb']);
  ```

  IOC容器创建实例的动作是通过绑定注册到容器中的，达到解耦

  ​