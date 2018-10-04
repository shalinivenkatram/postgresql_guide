## POSTGRES

### SELECT STATEMENT

Select statement is used to select the specific columns from the table

```
select * 
from actor;
```

### SELECT STATEMENT WITH DISTINCT CLAUSE
The DISTINCT clause is used in the SELECT statement to remove duplicate rows from a result set.
```
Select distinct country_id 
from city;
```
	
### WHERE CLAUSE
The WHERE clause is used in the SELECT statement to filter the rows returned from the SELECT statement
```       
select title,rental_rate
from film
where rental_rate=0.99;
```
### OR & AND OPERATOR
If you want to know who paid the rental with amount is either less than 1USD or greater than 8USD, you can use the following query with OR operator:  
```
SELECT customer_id,amount,payment_date 
FROM payment 
WHERE amount <= 1 OR amount >= 8;
```

AND operator is used to get the result that satisfy the both condition.If one condition false it does not returns.
```
select * 
from payment
where staff_id=2 AND amount=7.99;
```

Some of the operator as follows
```
      <> or  !=     not equal
      =            equal
      >           Greater than
      <           less than
      >=           Greater than and equal to 
      <=          less than and equal to 
```

### LIMIT
LIMIT  is used to select the limited values from the table
```
select *
from country
limit 5;
```
OFFSET clause is used to return before the limited values.
```
 select * from customer                                                                                                
 limit 12 offset 134;             (It shows 12 values but after 133 values)
 ```  
 ```
 select *
 from rental 
 where customer_id  in (select customer_id from rental)
 Limit 5 offset 272;
 ```
##  IN OPERATOR
IN operator in the WHERE clause to check if a value matches any value in a list of values. 
The syntax of the  IN operator is as follows: 
```
valueIN(value1,value2,...) 
```
The expression returns true if the value matches any value in the list i.e., value1, value2, etc. The list of values is not limited to a list of numbers or strings but also a result set of a SELECT  Statement as shown in the following query: 
```
valueIN(select value from  tbl_name);      //The statement inside the parentheses is called a subquery, which is a query nested inside another query.//

```
 
##  NOT IN OPERATOR
You can combine the IN operator with the NOT operator to select rows whose values do not match the values in the list. The following statement selects rentals of customers whose customer id is not 1 or 2. 
```
select customer_id,rental_id,return_date 
from rental
where customer_id  NOTIN(1,2); 
```
## BETWEEN OPERATOR
We use the BETWEEN operator to match a value against a range of values.If the value is greater than or equal to the low value and less than or equal to the high value, the expression returns true, or vice versa.
 ```
 select * from Payment
 where amount between 2.99 and 5.99
 order by customer_id ASC;
 ```
We often use the BETWEEN operator in the WHERE clause of a SELECT, INSERT, UPDATE or DELETE statement.
##  LIKE OPERATOR
LIKE operator is used to get exact result from the data which not sure about name.
```
select * from customer
where first_name like '%san'; //ilike used for insitive case//
```
## ORDER BY 
The ORDER BY clause allows to sort the rows returned from the SELECT statement in ascending or descending order.
 ```
 select * from customer
 order by store_id ASC, address_id DESC;
 ```
##  GROUP BY 
The GROUP BY clause divides the rows returned from the SELECT statement into groups. For each group, you can apply an aggregate function e.g., to calculate the sum of items or count the number of items in the groups. 
```
select customer_id,
COUNT (amount)
from payment
group by customer_id;
```
##  HAVING
The HAVING  clause sets the condition for group rows created by the G ROUP BY clause after the G ROUP BY clause applies while the WHERE clause sets the condition for individual rows beforeG ROUP BY clause applies. This is the main difference between the HAVING and where clause.
 ```
select customer_id, 
SUM (amount)
from payment
GROUP BY customer_id 
HAVING SUM (amount) > 200;
```
## AGGREATE FUNCTION
###### SUM
```
select SUM(amount)from payment;
```
###### AVG
```
Select round (avg(amount),2) from payment;
```
###### MIN & MAX
````
Select Min(amount)from payment;  
Select MAX(amount) from payment;
````
## AS STATEMENT
AS statement  is used the re_name the selected column.
```
Select Customer_id,sum(amount) AS Customer_spend
from customer
Group by customer_id;
```
## JOINS
The PostgreSQL Joins clause is used to combine records from two or more tables in a database. A JOIN is a means for combining fields from two tables by using values common to each.
There are four types of joins
###### Inner join
Inner join returns with common cloumns of both tables.
```
select customer.customer_id,first_name,last_name,email,payment_date,amount
from customer
inner join payment 
on customer.customer_id=payment.customer_id;
```
#####  full outer join
Full outer join produces the set of all records in Table A and Table B, with matching records from both sides where available. If there is no match, the missing side will contain null.
```
Select columns
From table_A
Full join table_B
On Table_A.name=Table_B.name;
```
###### full outer join with where clause
To produce the set of records unique to Table A and Table B, we perform the same full outer join, then exclude the records we don't want from both sides via a where clause.
```
Select columns
From table_A
Full join table_B
On Table_A.name=Table_B.name
Where Table_A.id  is null
OR Table_b.id is null;
```
###### Left outer join
Left outer join produces a complete set of records from Table A, with the matching records (where available) in Table B. If there is no match, the right side will contain null.
```
Select columns
From table_A
Left join table_B
On Table_A.name=Table_B.name;
```
###### left outer join with where clause
To produce the set of records only in Table A, but not in Table B, we perform the same left outer join, then exclude the records we don't want from the right side via a where clause. 
```
Select columns
From table_A
Left join table_B
On Table_A.name=Table_B.name
Where table_B.id(col name) is NULL;
```
###### UNION
UNION operator combine result set of  two or more select statement into a single set.
```
Select col_1,col_2
From table_1
UNION
Select col_1,col-2
From table_2;
```
UNION removes all the duplicates coilums unless use UNION ALL operator.
UNON operator combine the result set of first query before, after or between the result set of second query.So if you want to order use ORDER BY clause.
## SUB-QUERY
A subquery is a query nested inside another query such as SELECT, INSERT, DELETE and UPDATE. 
```
select film_id,title,rental_rate 
from film
where rental_rate>( select avg(rental_rate) from film);
```
The query inside the brackets is called a subquery or an inner query. The query that contains the subquery is known as an outer query.
## Self-join
We can use self-join when we want to combine rows with other rows in the same.we must use AS statement in self-join.
```
select A2.customer_id,A2.first_name,A2.last_name,A1.customer_id,A1.first_name,A1.last_name
from customer as A1,customer AS A2        Another method:  from customer AS A1 join Customer AS A2
where                                                      ON A1.first_name=A2.last_name
A1.first_name=A2.last_name
Order by A1.customer_id;
```
## CREATE TABLE(DDL)
To create a new table in PostgreSQL, you use the CREATE TABLE statement. The following illustrates the syntax of the C REATE TABLE statement: 
```
CREATE TABLE account(
user_id serial PRIMARY KEY,
username VARCHAR (50) UNIQUE NOT NULL,
password VARCHAR (50) NOT NULL,
email VARCHAR (355) UNIQUE NOT NULL,
created_on TIMESTAMP NOT NULL,
last_login TIMESTAMP
);
```
 If we want to create same schema of another table just copy the schema of that table we can use:
  ```
  create table new-table (like old_table name);
  create table link_copy( Like link);
  
  ```
An existing table from which the new table inherits. It means the new table contains all columns of the existing table and the columns defined in the CREATE TABLE statement. This is a PostgreSQL’s extension to SQL.
```
create table table_name(
column_name TYPE column_constraint
table_constraint     table_constraint 
INHERITS  existing_table_name; 
 ```
## INSERT
Create table and then need to  insert data into the table.
``` 
Insert into table-name(col1,col2)
Values( Val1,Val2);
 ```
 ```
Insert into link(id,name,url)
Values(1,'Shalini','www.gogole.com'),
      ( 2,'venkat','www.yahoo.com');
 ```
Like create table from existing table we can insert the values from the existing table as well.
```
 Insert into link-copy
 ```
If you insert the values keep it mind you can also insert the integer value as well.

##   UPDATE
Update statement use to update existing values of the table.

```
Update table name
Set col1=val1,
     col2=val2
Where condition;
```
```
update link
set description='love'
where name ilike 'shalini'
returning id,url,name,description;         //using  to see the update values directly//

```                                 

##   DELETE
Use to delete rows in the table.The delete statement must use the where condition because if you don’t use the where condition it delete all the rows from the table.so we have to specify the rows which we want to delete from the table.

```
Delete  from table
Where condition;
```
```
Delete from link
where id=1;
```
##   ALTER TABLE
To change existing table structure.we can ADD,REMOVE,MODIFY,RENAME THE column.
Some of the key words we can use 

```
ADD COLUMN
DROP COLUMN
RENAME COLUMN
ADD CONSTRIANT
RENAME TO
```
```
ALTER TABLE table_name action;
```
```
Alter table link ADD COLUMN Active boolean;
```
```
Alter table link DROP column active;
```
```
Alter table link rename column title to title_new;
```
```
Alter table link to url ;             //rename the entire table from old name to new name//


```                                 

##   DROP TABLE

We can drop the entire table

```
 drop table (if exists) table_name 
 restrict;                           // restrict  refuses to drop the table if there is any object  depend on it//

```

##  CHECK CONSTRAINT
A check constraint is a kind of constraint that enables to check a condition when you insert or update data. For example, the values in the price column of the product table must be positive values. 
 
 ```
create table users(
id serial primary key,
First_name varchar(200)not null,
birth_date date check(birth_date>'2001-01-01'),
join_date date check(join_date>birth_date),
salary int check(salary>0)
);

```
## NOT NULL CONSTRAINT
NULL in an unknown and missing information.Null value is different from empty an zero value.
For example if you don’t know about the one person email address you can use NULL.But if he does not have email address you must leave as empty.

```
create table learn_null(
first_name varchar(100),
sales_number int not null
);
```

```
insert into learn_null(first_name)
Values('joe');                      //shows error because sales_number in not null//

```

## UNIQUE CONSTRAINT
Unique  constraint use to set the data type as unique and not repeated again.

```
create table test(
id serial primary key,
first_name varchar(20),
email varchar(200)unique
);

```

```
insert into test(id,first_name,email)
Values(1,'joe','shalu@gmail.com')
Values(2,'john','shalu@gmail.com')        //shows error //

```
##  VIEWS
A view is a database object that is of a stored query.
It accessible to view as a virtual table in Postgres SQL

```
Create view customer_info AS.  //Using to view the virtual table of this query//

```

```
select first_name,address,email,activebool
from address
join customer ON address.address_id=customer.address_id
order by first_name; 

```

##   ALTER VIEW

```
alter view customer_info rename to customer_master_list;

```


##   DROP VIEW

```

drop view Customer_master _list;

```






                            








