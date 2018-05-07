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

