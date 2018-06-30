# Mysql
### command

```
source sqlfile.sql
show variables like '%max_connections%'
show processlist
show full processlist
show engine innodb status

mysql -uroot -p123456 --default-character-set=utf8

SET character_set_client = utf8;

set global wait_timeout=604800; 

===change password===
set password for root@localhost = password('<new password>'); 

jdbc:mysql://192.168.145.130:3306/mydb?characterEncoding=utf8&useSSL=false

```

### sql
```sql
===setting password===
set password for root@localhost = password('123456');
===group by===
SELECT status FROM orders GROUP BY status;
SELECT YEAR(orderDate) AS YEAR, SUM( customerNumber) AS cus_total FROM orders GROUP BY YEAR(orderDate) HAVING YEAR > 2003
===distinct===
SELECT DISTINCT [column] FROM [table]
SELECT COUNT(DISTINCT [column2]) FROM [table] WHERE [column1] = 'xx'
===index===
SHOW INDEX FROM [table]
CREATE INDEX [index_name] ON [table] ([column])
DROP INDEX [index_name] ON [table]
===view===
CREATE VIEW [view_name] AS SELECT [column1], [column2] FROM [table]
SELECT * FROM [view_name]
UPDATE [view_name] SET [column2]='XX' WHERE [column1]='XX'
DROP VIEW [view_name]
===subquery===
SELECT * FROM [table] WHERE [column1] =(SELECT MAX(column2) FROM [table])
SELECT * FROM(SELECT * FROM [table]) AS t

===function===
SELECT CONCAT( '%', 'd', '%' )

SELECT group_concat('listen ', 'to ', 'new ', 'age')


===date===
-- today
select * from orders where to_days(orderDate) = to_days(now());
-- yearterday
SELECT * FROM orders where DATE_SUB(CURDATE(), INTERVAL 7 DAY) <= date(orderDate)
-- this week
SELECT * FROM orders WHERE YEARWEEK(date_format(orderDate,'%Y-%m-%d')) = YEARWEEK(now());
-- last week
SELECT * FROM orders WHERE YEARWEEK(date_format(orderDate,'%Y-%m-%d')) = YEARWEEK(now())-1;
-- within 7 day
SELECT * FROM orders where DATE_SUB(CURDATE(), INTERVAL 7 DAY) <= date(orderDate)
-- this month
SELECT * FROM orders WHERE DATE_FORMAT( orderDate, '%Y%m' ) = DATE_FORMAT( CURDATE( ) , '%Y%m' )
-- this month
select * from orders where date_format(orderDate,'%Y-%m')=date_format(now(),'%Y-%m')
-- last month
SELECT * FROM orders WHERE PERIOD_DIFF( date_format( now( ) , '%Y%m' ) , date_format( orderDate, '%Y%m' ) ) =1
-- within 6 month
select * from orders where orderDate between date_sub(now(),interval 6 month) and now();
-- this season
select * from orders where QUARTER(orderDate)=QUARTER(now());
-- last season
select * from orders where QUARTER(orderDate)=QUARTER(DATE_SUB(now(),interval 1 QUARTER));
-- this year
SELECT * FROM orders WHERE DATE_FORMAT( orderDate, '%Y' ) = DATE_FORMAT( CURDATE( ) , '%Y' )
-- this year
select * from orders where YEAR(orderDate)=YEAR(NOW());
-- last year
select * from orders where year(orderDate)=year(date_sub(now(),interval 1 year));

```

### error
```
===sql_mode=only_full_group_by===
error msg:

Expression #4 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'pos2_dev.t2.currency' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by

solve:t2.currency => any_value(t2.currency)

```