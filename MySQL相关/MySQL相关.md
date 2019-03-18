# MySQL相关

### 1.delete或者update本身查询出来的数据时会报错

错误信息为：

```mysql
You can't specify target table 'h_order_service' for update in FROM clause
```

解决方式：

* 将查询再嵌套一次查询，构建一个中间的表
* 注意：我在使用时候没有加distinct和as a（直接空格后加a）时候仍旧会报错执行不了

```mysql
DELETE
from h_order_service 
where id NOT IN (
select b.id
from (SELECT DISTINCT a.id FROM h_order_service as a WHERE  a.`status` = 0) b
)
```

### 2查询数据时sum和查询其他字段的问题

* 在一个查询语句当中有：

  ```mysql
  select sum（字段1，字段2）as s,字段3 from 表名.....
  ```

  此处将报错

* 正确的姿势

  ```mysql
  select sum (字段1，字段2) as s,字段3 from 表名...groupBy('字段3')；
  ```

  或者

  ```mysql
  select sum(字段1，字段2) as s,字段1，字段2 from 表名 ....
  ```

* 总结：在使用sum函数时想要查询其他字段时有两种情况：1.想要查询的字段在sum中；2.想要查询的字段不在sum中。

  对于1，不需要其他的姿势，直接查；

  对于2，需要将字段groupby一下，至于为何，还待研究

### 3.concat的使用

### 4.distinct

* 查询时会经常使用去重操作，例如

  ```sql
  select distinct username,mobile from table 
  ```

* 此时获得的数据是username，mobile两列数据，那么，想要获得去重后的所有数据信息如何操作？

  ```sql
  select * from table group by username,mobile
  ```

  这样就可以达到效果了

### 5.case when用来做字段状态判断替换，与ifelse同功效

### 6.IF语法

### 7.exist和in的区别使用

* exist返回的是布尔值，in返回结果集，exit走索引，关键exist可以将外表的参数传到子查询中当做条件来查询，比较灵活，同时扫描的表的行数也比in要少。

  https://www.cnblogs.com/beijingstruggle/p/5885137.html

### 8.在查询时查询其他表中符合条件的条数的方法

```sql
select * ,(select count(*) from b where a.b_id = b.id) as b_count from a
```

