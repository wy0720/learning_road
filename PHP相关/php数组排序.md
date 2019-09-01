### php数组排序



#  			[PHP：根据二维数组中的某个字段进行排序](https://www.cnblogs.com/wenzheshen/p/9455554.html) 		



首先了解下以下两个函数：

**1.array_column() 返回输入数组中某个单一列的值。**

![img](https://images2018.cnblogs.com/blog/949827/201808/949827-20180810154609306-1048628309.png)

 

**2.array_multisort() 函数返回排序数组。您可以输入一个或多个数组。函数先对第一个数组进行排序，接着是其他数组，如果两个或多**

**个值相同，它将对下一个数组进行排序。**

![img](https://images2018.cnblogs.com/blog/949827/201808/949827-20180810154530132-1954983260.png)

具体实现代码实例：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<?php
$data = array(
  array(
    'id' => 5698,
    'first_name' => 'Bill',
    'last_name' => 'Gates',
  ),
  array(
    'id' => 4767,
    'first_name' => 'Steve',
    'last_name' => 'Aobs',
  ),
  array(
    'id' => 3809,
    'first_name' => 'Mark',
    'last_name' => 'Zuckerberg',
  )
);

//根据字段last_name对数组$data进行降序排列
$last_names = array_column($data,'last_name');
array_multisort($last_names,SORT_DESC,$data);

var_dump($data);
?>
```