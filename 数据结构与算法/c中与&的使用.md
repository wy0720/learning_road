### c中*与&的使用

* 参考：https://www.cnblogs.com/yep3575/p/3156677.html

* *定义一个指针，即开辟一个内存空间用来<u>存储地址</u>

  ```c
  int *a;
  ```

* &用来取变量的地址

* 示例一

  ```c
  viod find(int *a)
  {
      print('this is find method');
  }

  main()
  {
      int b = 1;
      find (&b);
  }
  ```

  ```c
  int b = 1;		find(&b)这样调用是进行了：int  *a = &b,创建了一个新的指向b的整型指针a作为find函数的局部变量
  ```

* 示例二

  ```c
  函数声明：int find(int &a);	函数调用：int b = 1;int n = find(b);

  调用时，实际上进行了int &a = b;其实是对b重命名为a,并没有创建新的变量
  ```

* 示例三

  ```c
  函数声明：int find(int a);  函数调用：int b = 1; int n = find(b);

  调用时，进行了int a = b; 创建了一个新的find函数的局部变量a,值与b相同
  ```

  ​

* PHP中&的用法

  ```php
  function demo(&$e) {
  	$e = 2333;
  	return 'success';
  }
  $a = 123;
  var_dump($a); //123
  var_dump(demo($a)); //success
  var_dump($a); //2333
  ```

  ​