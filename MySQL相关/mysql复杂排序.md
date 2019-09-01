### mysql复杂排序

在大多数应用场景下，我们使用mysql进行查询时只会用到'=', '>' , '<' , in, like 等常用的方法，看起来，大多数情况下，已经足以应付我们的小型应用了。不过，在一些特殊场景，则需要特殊的查询方式了。

　　1. 根据状态来排序的查询

　　　　假设现在一个记录有四种状态，未处理(0)、正在处理(2)、处理成功(1)、处理失败(4)，之所以他们的值是这个样子，是因为我们一般情况下是不会用它去排序，所以自然的就想到这样的一些值赋予意义。但是，在排序的时候怎么处理呢？ 假如要求的先后顺序是这样的：未处理>处理失败>正在处理>处理成功， 我能想到的就是，假设它们这些状态就是按照要求的排序的值去依次增加的，那就只需要一个order by 该字段即可。具体实现如下：

```
select * from tab_task b order by (CASE b.deal_status WHEN 0 THEN 10 WHEN 2 THEN 30 WHEN 1 THEN 40 WHEN 4 THEN 20  ELSE 40 END) ASC,b.add_time desc     //按照所需排序值，依次赋值
```

如果是要根据某个计算的值来排序，如按平均成绩排序，则实现如下：

```
SELECT * FROM score b GROUP BY uid ORDER BY AVG(b.`score`) DESC;　　　　//直接计算得出排序值，使用RAND()可以得到随机的排序
```

　　2. 比较复杂的模糊查询

　　　　有时，我们需要从某字段中筛选符合条件的值，但是该值又不是一个独立的字段，这时就需要一些高级的模糊查询（正则）。

如，需要查询某个值大于0的记录，实现如下：

```
SELECT * FROM tab_day_data WHERE retention_days_full REGEXP '.*rd1=([0][1-9]+|[1-9][0-9]*).*';      //查询rd{x}大于0的记录，其中rd可能是许多个类似的不重要的值，被储存至一个字段，同理于其他
```

mysql正则语法，如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
// .  表示匹配任意一个字符
// | 作为OR操作符，表示匹配其中之一
// [ ] 匹配任何单一字符
// [:a;num:]    任意字母和数字（同 [a-zA-Z0-9]）
// [:alpha:]     任意字符（同 [a-zA-Z]）
// [:blank:]     空格和制表（同 [\\t]）
// [:cntrl:]        ASCII控制字符（ASCII 0到31和127）
// [:digit:]       任意数字（同[0-9]）
// [:graph:]    与["print:] 相同，但不包括空格
// [:lower:]      任意小写字线（同 [a-z]）
// [:print:]        任意可打印字符
// [:punct:]      既不在 [:alnum:] 又不在 [:cntrl:] 中的任意字符
// [space:]       包括空格在内的任意空白字符（同 [\\f\\n\\t\\r\\v]）
// [:upper:]     任意大小字母（同 [A-Z]）
// [:xdigit:]      任意十六进制数字（同 [a-fA-F0-9]）
// 元字符  .*?+{m}{m,}{m, n}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　语法与正宗的正则不太一致，但大体原理是一致的。

　　用mysql做复杂的匹配，虽然可以实现，但是相对来说效率低下，最好还是使用独立字段的形式进行查询，用空间换取时间！

 

\3. 检查校验和，验证表是否相等

数据在传输时，可能会发生变化，也有可能因为其它原因损坏，为了保证数据的一致，我们可以计算checksum（校验值）。

使用MyISAM引擎的表会把checksum存储起来，称为live checksum，当数据发生变化时，checksum会相应变化。

```
CHECKSUM TABLE tbl_name [, tbl_name] ... [QUICK | EXTENDED]
```

　　附：mysql进程查看，卡死现象的解决办法

```
SHOW FULL PROCESSLIST;       # 查看所有的进程情况，具体解决请百度

SHOW STATUS;              # 查看总体运行概况

KILL 213;          # 将运行慢或卡死的进程删除，注意数据的事务性，否则可能导致数据错误
```

 

　　

　　问？ 

　　mysql 分表之后（比如按年分表，多个相关联表同步拆分），查询应该怎样做才好？ 