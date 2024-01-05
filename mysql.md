## DDL (data definition language)



###  database

> query all databases

`show databases`



> query current database

`select database()`



> create

`create database [ if not exists ] <database name> [ default charset <charset> ]`



> delete

`drop database [ if exists ] <database name>`



> use 

`use <database name>`



###  table

> query tables

`show tables`



> query structure of table

`desc <table>`



> query statement of create table

`show create table <table>`



> create table

```text
create table <table> (
	...
)
```

[^note]: copy a table, `create table <table> select ...` 



> alter table fields

```text
alter table <table> add <field name> <type>

alter table <table> drop <field name>

# modify field type
alter table <table> modify <field name> <new type>

alter table <table> change <old field> <new field> <new type>
```



> delete

```text
drop table [if exists] <table>
```



### constraint

> type

```
NOT NULL
UNIQUE
PRIMARY KEY			primary key (A1, A2, A3, ...)
AUTO_INCREMENT
DEFAULT				default value
CHECK(...)			check field value in proper range
FOREIGN KEY			[constraint <name>] foreign key (...) references table(...)
```



> foreign keys constraint

```
# delete or update operation for the parent table is rejected if there is a       # related foreign key value in the referenced table 
NO ACTION, RESTRICT

# The delete or update operation for parent table is also applied to referenced   
# table
CASCADE

# set key value of referenced table to null if delete key in parent table
SET NULL

# set default value
SET DEFAULT
```



> add constraint to a table

```
alter table <table> add constraint <name> foreign key(...) references table(...)
```

[^note]: it's also ok to query and delete constraints 



### data type

```
char(n)						fixed length n
varchar(n)					character varying, max length n
int							integer
smallint
numeric(p,d)				store any value with p digits and d decimals
							#eg. DECIMAL(5,2) range from -999.99 to 999.99.
```



## DML (data manipulation language)



### insert

```
insert into <table>[(a1,a2,...)] values (v1,v2,...),(v1,v2,...),....
```

[^note]: insert into the result of select -> `insert into <table>[(a1,a2,...)] select ....`



### update

```
update <table> set field1=value1, field2=value2, ... [where <condition>]
```



### delete

```
delete from <table> [where <condition>]
```

​        

## DQL (data query language)



### sytax

```text
select
	fields
from 
	tables
where
	conditions
group by
	groups
having
	group conditions
order by
	sequence
limit
	pages
```



### basics

> common

`select <field> from <table>`



> alias

`select <fieild> [as <alias>] ...`



> remove duplicates

`select distinct <field> from <table>`



### where \<condition>

> logic

```
and or &&
or or ||
not or !
```

> compare

```
>, >=, <, <=
= 								equal
<> or != 						not equal
is null 						no value
between ... and ...				range [... , ... ]
in(v1,v2,...)					select one from list
like <placeholder>				`_` matches any one char, `%` matches any chars
operator any(..), some(...)		any one value 
operator all(...)				all values
```



### aggregate functions

```text
# syntax: select <function(<list>)> from <table>
count, max, min, avg, sum
```



### group

group by, having ....



### order

`order by <field> <order>`

ASC: ascend

DESC: descend



### limit

```
# start : start index of record, show page 1 when <start> = 0, page 2 when <start> = 0+<records>, ...
limit <start>, <records>

# may omit <start> if page = 1
limit <records>
```



### set operations

```
union
intersect
except
```

[^note]: append `all` if reserve repetitions



### join

> inner join

```
select <field list> from <table1> [inner] join <table2> on <condition>
```



> outer join

```
# include intersect and left or right
select <field list> from <table1> <left | right> [outer] join <table2> on <condition>
```



> full join

```
full [outer] join
#union of left join and right join
```

[^note]: mysql doesn't support full join, but I guess you've know how to do it.



### with

```
with <table>(<field list>) as
(select ...)
```



### subquery

> scalar

return a single value, operation `> >= < <= = ...`



> column

return one column with one or more rows

operation`in, any, some, all`



> row

return one row with one or more columns

operation `=, <>, in, not in`



> table

multiple rows and columns



### transaction

```
BEGIN or START TRANSACTION;
...
COMMIT;
ROLLBACK;
```



### view



> create or modify

```
create [or replace] view <view> as <query expression>
alter view ...
```



> check option

```
# that every row that is inserted or updated through a view must conform to the   # definition of the view.
with [cascaded | local] check option
```



> delete

```
drop view [if exists] <view>
```



## DCL (data control language)



### user management

> query user

```
use mysql
select * from user
```



> create user

`create user '<username>'@'<hostname>' identified by '<password>'`




> modify user password

`alter user '<username>'@'<hostname>' identified with mysql_native_password by '<new password>'`



> delete user

`drop user '<username>'@'<hostname>'`



### privilege

> common privileges

```
ALL,ALL PRIVILEGES
SELECT
INSERT
UPDATE
DELETE
ALTER
DROP
CREATE
...
```



> query privilege

`show grants for '<username>'@'<hostname>'`



> grant privilege

`grant <privilege list> on <database>.<table> to '<username>'@'<hostname>'`



> revoke privilege

`revoke <privilege list> on <database>.<table> to '<username>'@'<hostname>'`



[^note]:\<database>.\<table> may be \*.\* to represent any databases and tables

