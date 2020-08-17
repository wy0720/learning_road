### new self()和new static()的区别

* new self()返回的都是同一个实例【根父类】
* new static返回的是当前对象实例，延迟绑定

```php
class Father
{

    public function useSelf()
    {
        return new self();
    }

    public function useStatic()
    {
        return new static();
    }

}

$f = new Father();

echo get_class($f->useSelf())."\n";//  输出  Father
echo get_class($f->useStatic())."\n";//  输出  Father
```



```php
class Sun1 extends Father
{

}

class Sun2 extends Father
{

}

$sun1 = new Sun1();
$sun2 = new Sun2();

echo get_class($sun1->useSelf())."\n";     //  输出  Father
echo get_class($sun1->useStatic())."\n";  //  输出  Sun1

echo get_class($sun2->useSelf())."\n";    //  输出  Father
echo get_class($sun2->useStatic())."\n"; //  输出  Sun2
```



在使用方面：可以静态方法创建实例，不去new，好爽，面向对象~~~

```php
+    /**
+     * 获取推送实例
+     * @param int $shop_id
+     * @param int $account_id
+     * @return static
+     */
+    public static function instance($shop_id, $account_id)
+    {
+        $instance = new static();
+
+        $instance->shop_id    = $shop_id;
+        $instance->account_id = $account_id;
+
+        $instance->initDeviceList();
+
+        return $instance;
+    }
```

