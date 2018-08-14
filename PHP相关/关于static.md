###关于static

* 特性:只在第一次调用时候进行初始化（怎么理解这句话??）

  举例以static关键字声明的变量

  ```php
  function test(){
      $a = 0;
      echo $a;
      $a++;
  }

  function StaticTest(){
      static $a = 0;
      echo $a;
      $a++;
  }


  test();//0
  test();//0
  StaticTest();//0
  StaticTest();//1
  ```

  每次调用test，输出都为0；而调用staticTest，只有第一次调用时为0

  ​