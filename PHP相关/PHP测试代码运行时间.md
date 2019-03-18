### PHP测试代码运行时间

```php
$startTime = explode(' ', microtime());
代码块
$endTime = explode(' ', microtime());
echo $startTime[0] + $startTime[1] . '<br/>';
echo $endTime[0] + $endTime[1] . '<br/>';
echo '执行耗时：' . round($endTime[0] + $endTime[1] - ($startTime[0] + $startTime[1]), 4) . ' 秒。';
```

