###PHP自动加载机制

* 需求由来：在一个PHP文件中想要去用另一个PHP文件的方法，如何处之乎？

* 原始解决：include，require

  缺点：include文件太多不易管理（未体会到）

* 现在的解决方法

  在调用一个不存在的类时会触发spl_autoload_register（）这个函数，并且将类名传入进去。那么，我们如何利用这个工具来搞事情呢？？

  * 1.使用命名空间

    有如下操作

    TestClass类：(文件位置：D:\demo\test)

    ```php
    <?php
    namespace demo\test;
    class TestClass
    {
    	public function __construct(){
    		echo 'this is autoload class';
    	}
    }
    ```

    加载类（文件位置：D:\demo\test）

    ```php
    <?php
    use demo\test\TestClass;
    spl_autoload_register(function($class){
    	echo $class;
    });
    new TestClass;
    ```

    输出：demo\test\TestClass

    报错：

    | Fatal error: Uncaught Error: Class 'demo\test\TestClass' not found in D:\demo\test\test.php on line *6* |
    | ------------------------------------------------------------ |
    | ( ! ) Error: Class 'demo\test\TestClass' not found in D:\demo\test\test.php on line *6* |

    从这个例子我们可以知道几点：

    1.new一个不存在类时，会将use的空间传进去

  * 2.遵循psr4规范

    为什么要遵循？

    当尊寻这个规范，我们就可以根据规范的命名空间规则进行地址转换，从而将类文件加载进来了！！！

  * 使用命名空间来加载

    TestClass类不变

    ```php
    <?php
    use demo\test\TestClass;
    spl_autoload_register(function($class){
    	 $class_name = explode('\\',$class)[2];
    	 $class_file = __DIR__ . '\\' . $class_name . '.php';
    	 if (file_exists($class_file)) {
    	 	require_once $class_file;
    	 }else{
    	 	echo "没找着！！";
    	 }
    },true,true);
    $obj = new TestClass();
    ```

    结果：this is autoload class

    如此这般，就没有报错，输出结果正确

* 注意

  在加载命名空间的文件（即有namespace的类文件）时如果没有使用use来加载空间的话，会报错！就如TestClass不变，加载文件去掉use一行。