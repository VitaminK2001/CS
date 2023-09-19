# where
where一定是紧跟在from之后的
where后面跟着的是
> 多表的连接条件
> 不包含聚合函数的过滤条件

# having
having后面可以跟
>  聚合函数的过滤条件
>  普通的过滤条件

```sql
having MAX(salary) > 10000 and department_id in(10, 20);
```

但是建议把普通过滤条件中声明在where中
```sql
where department_id in(10, 20)
group by department_id
having MAX(salary) > 10000;
```
> 因为在关联查询中，where的效率高于having，where是先筛选，后连接，而having是先连接后筛选
> 所以==将普通过滤过滤条件声明在where中==执行效率更高

# sql语句的执行过程
from ...,... -> on -> (left / right join) -> where -> group by -> having -> select 前面都是行上的过滤，唯独select是列上的过滤 -> distinct -> order by -> limit 

执行过程解释了两个问题
1. 为什么where后面不能跟聚合函数
	1. 因为group by语句执行的顺序在where之后
2. 为什么普通过滤放在where中执行效率高
	1. 因为where是在分组之前先将数据条数筛选变少
	2. 如果没有where 则直接分组，可能出现冗余计算，因为很多行的数据可能在having普通过滤中最终才被筛掉，前面聚合函数的条件限定白白计算了


