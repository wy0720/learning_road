### yii2

* 查询
  - [all()](https://www.yiiframework.com/doc/api/2.0/yii-db-query#all()-detail)：将返回一个由行组成的数组，每一行是一个由名称和值构成的关联数组（译者注：省略键的数组称为索引数组）。
  - [one()](https://www.yiiframework.com/doc/api/2.0/yii-db-query#one()-detail)：返回结果集的第一行。
  - [column()](https://www.yiiframework.com/doc/api/2.0/yii-db-query#column()-detail)：返回结果集的第一列。
  - [scalar()](https://www.yiiframework.com/doc/api/2.0/yii-db-query#scalar()-detail)：返回结果集的第一行第一列的标量值。
  - [exists()](https://www.yiiframework.com/doc/api/2.0/yii-db-query#exists()-detail)：返回一个表示该查询是否包结果集的值。
  - [count()](https://www.yiiframework.com/doc/api/2.0/yii-db-query#count()-detail)：返回 `COUNT` 查询的结果。
  - 其它集合查询方法：包括 [sum($q)](https://www.yiiframework.com/doc/api/2.0/yii-db-query#sum()-detail), [average($q)](https://www.yiiframework.com/doc/api/2.0/yii-db-query#average()-detail), [max($q)](https://www.yiiframework.com/doc/api/2.0/yii-db-query#max()-detail), [min($q)](https://www.yiiframework.com/doc/api/2.0/yii-db-query#min()-detail) 等。`$q` 是一个必选参数， 既可以是一个字段名称，又可以是一个 DB 表达式。