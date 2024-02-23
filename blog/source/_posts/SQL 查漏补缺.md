---
title: SQL查漏补缺
date: 2023-10-15 18:44:08
---

# SQL查漏补缺

> 记录一下SQL刷题时候遇到的知识点漏洞

## 1. SQL != 和 == 的适用场景

> -- SQL 中，!= 和 == 只能用于数字判断，对于空值， 需要用 is， is not 来判断

* [584. Find Customer Referee](https://leetcode.com/problems/find-customer-referee/)



## 2. 计算日期差值

```datediff(date1, date2)``` 

结果为date1 - date2

* [197. Rising Temperature](https://leetcode.com/problems/rising-temperature/)



## 3. left/right join必须要on条件，而inner join不需要



## 4. where/having/join on区别

先看一下SQL的执行流程：

> 第一步：from 选择表
>
> 第二步：where 筛选条件，筛选对象--行
>
> 第三步：group by 将筛选出来的数据进行分组
>
> 第四步：having 筛选条件，筛选对象--组
>
> 第五步：select 选取最后的结果
>
> 第六步：order by 将结果按照特定顺序排列



> “where” 是一个约束声明，使用Where来约束来之数据库的数据，Where是在**结果返回之前**起作用的，且Where中**不能使用**聚合函数。
>
> “Having”是一个过滤声明，是在查询返回**结果集**以后对查询结果进行的**过滤操作**，在Having中**可以使用**聚合函数。

* Having里面**不能用列名**，只能用数值和聚合函数的对比。

  > 在 SQL 查询中，当你使用 `GROUP BY` 语句对结果进行分组时，除了被聚合的列（使用聚合函数如 `MIN`、`MAX`、`AVG`、`SUM` 等）之外，其他列的值将不明确。这些未被聚合的列通常需要通过聚合函数或者放入 `GROUP BY` 子句中，以明确它们在每个分组内的值。
  >
  > 所以，当你在 `HAVING` 子句中使用 `order_date` 而不是聚合函数，就是指 `order_date` 是未被聚合的列。`HAVING` 子句用于过滤基于聚合的结果，而未被聚合的列在这个语境下不是直接可用的。

  举例来说：

  ```sql
  -- 有效的查询，因为 COUNT 是一个聚合函数
  SELECT customer_id
  FROM Delivery
  GROUP BY customer_id
  HAVING COUNT(*) > 1;
  ```

  ```sql
  -- 无效的查询，因为 order_date 是未被聚合的列
  SELECT customer_id, order_date
  FROM Delivery
  GROUP BY customer_id
  HAVING order_date = MIN(order_date);
  ```

  在第二个查询中，`order_date` 是未被聚合的列，而 `HAVING` 子句中的条件要求使用 `MIN(order_date)`，这就是为什么会报错的原因。解决方法通常是通过子查询或者连接操作来获取你需要的结果。



### Mysql报错：group function is not allowed here

在where和on中，我们**不能使用聚合函数**。因为on和where都是针对**行**的筛选。而聚合函数，很明显是对于**整张表上的数据的统计**。

所以，当你将聚合函数的判定放在where和on中，会报错。因为你没办法对一个行进行统计。

聚合函数的判定，放having里。因为having是用来对**结果集**进行筛选的

* [570. Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/)





## 5. Mysql中if的用法

> 常用sum内嵌套if
>
> sum(if (aaa = 'xxx', 1, 0))
>
> 



### 算平均值时，除数是0，结果为null。但是需要把null改成0。怎么改？

> 使用nullif

nullif(1/0,0)

* [1934. Confirmation Rate](https://leetcode.com/problems/confirmation-rate/)



## 6. 统计字符串长度

> ```char_length```或者```len```

* [1683. Invalid Tweets](https://leetcode.com/problems/invalid-tweets/)



## 7. SQL判断字段奇偶

> mod(字段, 2) = 1  或=0   

* [620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies/)



## 8. SQL 判断日期是否在区间内

>  两种办法
>
> 1. 用BETWEEN AND
>
> ```sql
> SELECT *
> FROM table_name
> WHERE date_column_name BETWEEN start_date AND end_date;
> ```
>
> 2. 用大于小于
>
> ```sql
> SELECT *
> FROM table_name
> WHERE date_column_name >= start_date AND date_column_name <= end_date;
> ```

* [1251. Average Selling Price](https://leetcode.com/problems/average-selling-price/)



## 9. Mysql中的sum

> 通常配合groupby使用，在sum中，只要是对单行操作，都可以。比如可以是sum(同一行的价格 * 数量) , 就能求总价



## 10. Mysql日期的截断

> 正常datetime都是yy-mm-dd 这种格式构成的，如果我们只要yy-mm这一部分，怎么做呢。

使用 DATE_FORMAT(get_date, '%Y-%m-%d') 函数截取。

其中：get_date 是需要截取的字段名；'%Y-%m-%d' 是截取后的日期格式。

select date_format('1997-10-04 22:23:00','%y %M %b %D %W %a %Y-%m-%d %H:%i:%s %r %T');

结果：97 October Oct 4th Saturday Sat 1997-10-04 22:23:00 10:23:00 PM 22:23:00

```sql
-- get_date = "2006-12-07"
SELECT count(*) FROM t_get_video_temp Where DATE_FORMAT(get_date, '%Y-%m-%d')='2006-12-07'; 
```

```sql
SELECT count(*) FROM t_get_video_temp Where get_date like '2006%-07%';
```

* [1193. Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/)



## 11. Mysql中日期大小

可以用min()取到最小日期



## 12. where (字段) in (查询)

[1174. Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii/)

对一张表的**重复操作**有奇效。先筛一次数据，然后对筛选结果进行汇总。



### 13. 日期要用引号包裹。



### 14. Round函数的使用

round(值, 0) => 整数

round(值, 1) => 一位小数

round(值, 2) => 两位小数



### 15. Union的使用

Union主要用于合并两个结果集. 简而言之就是在原先行的情况下往下追加行