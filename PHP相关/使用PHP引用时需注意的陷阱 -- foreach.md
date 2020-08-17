**使用PHP引用时需注意的陷阱 -- foreach**

[](http://www.cleey.com/blog/single/id/778.html)

我们从一个面试题开始，如下所示：

```
$arr = array('1''2''3');
foreach($arr as &$v){}
foreach($arr as $v){}
var_dump($arr);
```

结果我直接贴出来：

```
array
  0 => string '1' (length=1)
  1 => string '2' (length=1)
  2 => &string '2' (length=1)
```

> 很多同学看到了吧，为什么最后输出的是1 2 2 不是 1 2 3呢；

这就是要注意的坑了 ，**地址引用**

我们知道对数组执行foreach循环时，是通过移动数组内部指针来实现的。对于上面的例子：当foreach循环结束的时候，由于$v为引用变量，所以$v 与 $arr[ 2 ] 指向了同一个地址空间（共享变量值），之后对$v的任何修改都会直接反映到数组$a中。

要不我们在程序中加入调试代码，看看运行过程的情况吧。

```
$arr = array('1''2''3');

foreach($arr as &$v){
}

foreach($arr as $v){
	var_dump($arr);  
    echo "<br/>"; 
}
```

程序运行的结果是：

```
array
  0 => string '1' (length=1)
  1 => string '2' (length=1)
  2 => &string '1' (length=1)

array
  0 => string '1' (length=1)
  1 => string '2' (length=1)
  2 => &string '2' (length=1)

array
  0 => string '1' (length=1)
  1 => string '2' (length=1)
  2 => &string '2' (length=1)
```

简单解释一下：

1. 在第一次 foreach 循环里面，$v 是个引用。在循环结束之后，$v 与 $arr[2] = 3，指向了同一个内存块；
2. 紧接着第二次循环，$v 引用并没有改变，还是$arr[2]的引用，这时 $v 值是 $arr[0] 的值，所以导致 $arr[2] = 1；
3. 同上面，$v 已被 $arr[1] 影响，其值为 2，导致 $arr[2] = 2；
4. 所以最终结果是 1 2 2.

#### 关于引用，你需要了解的

\1. 引用类似于指针，但是不同于指针。
下面的操作让 $a 和 $b 指向了同一个内存块，值为 “str”

```
$a = "str";
$b = &$a;

echo $a;
echo '<br />';
echo $b;
```

试下更改$a和$b中任何一个元素的值。另外一个值都为随之改变：

```
$a = "str";
$b = &$a;

$b = "www.cleey.com";

echo $a;
echo '<br />';
echo $b;
```

程序运行结果为：

```
www.cleey.com
www.cleey.com
```

\2. 引用作为函数参数传递时，是可以被函数内部更改的：

```
$a = "str";
$b = &$a;

function change(&$a){  
	$a = "www.cleey.com";
}  

change($a);

echo $a;
echo '<br />';
echo $b;
```

程序运行结果为：

```
www.cleey.com
www.cleey.com
```

\3. unset只会删除变量。并不会清空变量值对应的内存空间：

```
$a = "str";
$b = &$a;

unset($b); 

echo $a;
echo '<br />';
echo $b;
```

程序运行结果为：

```
str
Notice: Undefined variable: b
```

懂了吧最后总结一下：

> PHP的引用有上面的特点，在编码的过程中，要小心使用引用。防止陷入莫名其妙的尴尬。PHP采用的复制机制是“引用计数，写时复制”，也就是说，即便在PHP里复制一个变量，最初的形式从根本上说其实仍然是引用的形式，只有当变量的内容发生变化时，才会出现真正的复制。所以才会出现上面的问题。之所以这么做是出于节省内存消耗得目的，同时也提升了复制的效率。