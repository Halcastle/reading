# – –按日

`SELECT COUNT(*),DATE(CreateTime) FROM t_voipchannelrecord WHERE YEAR(CreateTime)='2016' GROUP BY DAY(CreateTime);`

# – –按周

` SELECT COUNT(*),WEEK(CreateTime) FROM t_voipchannelrecord WHERE MONTH(CreateTime) = '8' GROUP BY WEEK(CreateTime) `

# –周一到周五每天的统计结果

 `SELECT COUNT(*),DAYNAME(CreateTime) FROM t_voipchannelrecord WHERE YEAR(CreateTime) = '2016' GROUP BY DAYNAME(CreateTime) `

# –统计本周数据

`SELECT COUNT(*) FROM t_voipchannelrecord WHERE MONTH(CreateTime) =
MONTH(CURDATE()) AND WEEK(CreateTime) = WEEK(CURDATE())`

# –按月统计

`SELECT COUNT(*),MONTH(CreateTime) FROM t_voipchannelrecord WHERE YEAR(CreateTime) = '2016' GROUP BY MONTH(CreateTime) `

# –统计本月数据

`SELECT COUNT(*) FROM t_voipchannelrecord WHERE MONTH(CreateTime) =
MONTH(CURDATE()) AND YEAR(CreateTime) = YEAR(CURDATE())`

# –按季统计

`SELECT COUNT(*),QUARTER(CreateTime) FROM t_voipchannelrecord WHERE YEAR(CreateTime) = '2016' GROUP BY QUARTER(CreateTime) `

# –按年统计

`SELECT COUNT(*),YEAR(CreateTime) FROM t_voipchannelrecord  GROUP BY YEAR(CreateTime) `

# –时间段(该段参考：出处)

> N天内记录
> WHERE TO_DAYS(NOW()) - TO_DAYS(时间字段) <= N
> 当天的记录
> where date(时间字段)=date(now())
>  或
> where to_days(时间字段) = to_days(now());

##  查询一周：

`select * from table   where DATE_SUB(CURDATE(), INTERVAL 7 DAY) <= date(column_time);`

##  查询一个月：

`select * from table where DATE_SUB(CURDATE(), INTERVAL INTERVAL 1 MONTH) <= date(column_time);`

## 查询'06-03'到'07-08'这个时间段内所有过生日的会员：

 `Select * From user Where
DATE_FORMAT(birthday,'%m-%d') >= '06-03' and DATE_FORMAT(birthday,'%m-%d')
<= '07-08';`

## 统计一季度数据，表时间字段为：savetime 

`group by concat(date_format(savetime, '%Y '),FLOOR((date_format(savetime, '%m ')+2)/3))`
 或
`select YEAR(savetime)*10+((MONTH(savetime)-1) DIV 3) +1,count(*) 
from yourTable
group by YEAR(savetime)*10+((MONTH(savetime)-1) DIV 3) +1;`