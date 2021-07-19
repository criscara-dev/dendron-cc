---
id: 0CjJLOqJFRLlz5Up3GBId
title: Databases
desc: ''
updated: 1626531330068
created: 1625429021643
---

# Databases

 DB: a system to store, use and organize data
 [DBs type](https://www.ibm.com/cloud/blog/brief-overview-database-landscape)
 DBMS: DataBase Management System ((ex. SQLite, PostGresSQL, Mongo, Redis, etc.))
 RDBMS: Relational DataBase Management System (ex. SQLite, PostGresSQL, etc.)
 S(english)QL: Structured  (Declarative) Query Language 
 Declarative(SQL, Python) vs Imperative (Java, Python)

## Models

- Hierarchical Model DB: One-to-Many (parent-to-child) relationship
- Network Model: Many-to-Many relationship
- Relational Model (13 Rules Codd)

SQL --- CRUD -> DBMS -> DB ( the software )
                  <-----|

DBs type in a Company (Amazon):
1. OLTP online transaction processing
2. OLAP online analytical processing

DCL Data Control Language
DDL Data Definition Language
DQL Data Query Data Language
DML Data Modification Language

SELECT column AS "<new_name>"
concatenate and rename the column, example:
`select concat(first_name, ' ' , last_name) as "Employee name" from employees`

2 type of functions:
1. aggregate: many records -> 1 value `AVG(), COUNT(), MIN(),MAX(),SUM()`
    ex. `select count( * ) from employees`
2. scalar: >=1 record -> multiple values (CONCAT)

exercise:
```sql
select avg(salary) from employees
select max(birth_date) from employees
select count(*) from "public"."towns"
select count("language") from "public"."countrylanguage" where isofficial = TRUE
select avg(lifeexpectancy) from "public"."country"
select avg(population) from "public"."city" where countrycode = 'NLD'

-- single line comment
/*
multi
line
comments
*/
```

ex.
```sql
select concat(first_name,' ',last_name) as "Name" from employees
-- and only the employee that has a specific name and last name to reduce search result
where first_name='Mayumi' AND last_name='Schueller'
```


Multiple selection:
`select first_name,last_name from employees`

```sql
" for table/column
' for text
```


## WHEN YOU USED WHERE, THERE IS AN ORDER OF OPERATIONS


### Logical operators precedence
```sql
FROM
WHERE
GROUP BY
HAVING  
SELECT
ORDER
```

###  operator precedence

[with equal precedence, from left to right or right to left](https://www.postgresql.org/docs/12/sql-syntax-lexical.html#SQL-PRECEDENCE)
```flowchart
()
*/
+-
Concatenation Operators
Comparison Conditions
IS NULL, LIKE, NOT IN, etc.
AND
OR
```

where SQL see an OR it start a new operation

```sql
WHERE (AND ... AND)
OR (... AND  ... AND)
```

ex. order execution:
```sql
select count( firstname ) from customers 
where (state='OR' OR state='NY') AND gender='F'
```

Select everything but ...
```sql
.... WHERE NOT ...
```

ex. with Queries orders
```sql
-- select count(firstname) from customers 
-- where (age<30 or age>=50) and income>50000 and (country='Japan' or country='Australia')

-- select sum(totalamount) from orders
-- where (orderdate>='2004-06-01' and orderdate<='2004-06-30')
-- and totalamount > 100
```

## NULL
Null will always be null

Filter out _NULL_ with the _IS_ operator (null, not null, True or False)

```sql
SELECT * FROM <table> WHERE <field> IS [NOT] NULL;
```

Clean up data

Coalesce = Replace _null_ with something else as a value
Coalesce set as well a default value if we are doing oprations (ex. sum() )
```sql
SELECT coalesce(<column1>,<column2>, 'Empty') AS column_alias FROM <table>
```

ex.
```sql
SELECT avg(coalesce(age, 15)) FROM "Student";
SELECT coalesce(name,'John'),coalesce(lastname, 'Doe') from "Student";
```

## Three value logic: True, Null, False

Is null checks for unkown?
```sql
SELECT <column>
FROM <table>
WHERE <column> IS NULL
```

## IN filtering

## partial lookup

```sql

LIKE '%xxxx_'

_ is a placeholder
% is a wildcard
```

to use LIKE you need use CAST on the column as TEXT

```sql
CAST(salary as text)
-- or
column::text
```

Case insensitive matches

```sql
-- ex.
name ILIKE 'CRIS%';
```

Time in SQL, use the standard UTC!

```sql
SET TIME ZONE 'UTC'

# set DB-user for PostGres to UTC forever ( not only the session ):
ALTER USER <name> SET timezone='UTC'
```

PostGres uses ISO-8601
YYY-MM-DDTHH:MM:SS
2021-0101T00:00:00+02:00 where +... is the Timezone

What DateTime is now?
```sql
select now()
```

Always consider with working with TimeStamp and TimeZone what you're storing
```sql
TIMESTAMP WITHOUT TIME ZONE
TIMESTAMP WITH TIME ZONE
``` 
Dates operators

```sql
SELECT now()::date
SELECT CURRENT_DATE

SELECT to_char(CURRENT_DATE, 'dd/mm/yy')
SELECT to_char(CURRENT_DATE, 'ddd')

-- d,m,y are FORMAT modifiers

-- dates calculations:
SELECT date '2000-01-01'
SELECT now() - '2000-01-01'

```


calculate age - age difference
```sql
-- pass age or column
SELECT AGE(date '1979/01/01')
SELECT AGE(<column>) ex. <column> = birthdate
-- difference
SELECT AGE(date '1979/01/01', '1980/12/31')
extract day, month, year
SELECT EXTRACT(DAY FROM date '1979/01/01') AS DAY
SELECT EXTRACT(MONTH FROM date '1979/01/01') AS MONTH
SELECT EXTRACT(YEAR FROM date '1979/01/01') AS YEAR


-- truncate: to check difference in same year, month, days
SELECT DATE_TRUNC('year', date '1979/01/01')
SELECT DATE_TRUNC('month', date '1979/01/01')

```

INTERVAL useful to subtract a defined value
```sql
now() - interval '1 year 2 months 1 day'
```

ex. 
```sql

-- return people ove ra certain age. calculated from birthdate
SELECT AGE(birth_date), * FROM employees WHERE ( EXTRACT (YEAR FROM AGE(birth_date)) ) > 60 ;
-- or
SELECT count(birth_date) FROM employees WHERE birth_date < now() - interval '61 years'

-- how many people hire in Feb?
SELECT count(emp_no) FROM employees where EXTRACT (MONTH FROM hire_date) = 2;

-- How many employees were born in november?
SELECT count(emp_no) FROM employees where EXTRACT (MONTH FROM birth_date) = 11;

-- Who is the oldest employee? (Use the analytical function MAX)
SELECT MAX(AGE(birth_date)) FROM employees;

-- How many orders were made in January 2004?
select count( orderid ) from orders where EXTRACT (MONTH FROM orderdate) = 1 and EXTRACT (year FROM orderdate) = 2004
SELECT COUNT(orderid) FROM orders WHERE DATE_TRUNC('month', orderdate) = date '2004-01-01';

```

DISTINT, to remove duplicates, pass column(s)

```sql
-- Question: What unique titles do we have?
SELECT DISTINCT title FROM titles;

-- Question: How many unique birth dates are there?
SELECT count(DISTINCT birth_date) FROM employees;

-- Question: Can I get a list of distinct life expectancy ages/ no nulls!
SELECT DISTINCT lifeexpectancy,NAME FROM country WHERE lifeexpectancy IS NOT NULL ORDER BY lifeexpectancy;

```


ORDER DATA: USE ORDER BY <COLUMN> [ASC,DESC]

```sql
select first_name, last_name from employees
ORDER BY first_name,last_name DESC
ORDER BY length(last_name) DESC

-- Sort employees by first name ascending and last name descending
elect first_name, last_name from employees ORDER BY first_name ASC,last_name DESC
--  Sort employees by age
select first_name, age(birth_date) from employees ORDER BY birth_date DESC
-- Sort employees who's name starts with a "k" by hire_date
select first_name from employees where first_name ilike 'k%' ORDER BY hire_date 
```

## MULTI TABLE SELECT
###  JOIN: aggregate data from 2 table thatc an map to the column of another table (link primary key to the foreign key)
```sql
SELECT a.emp_no, b.salary, b.from_date FROM employees AS a, salaries AS b 
WHERE emp_no = b.emp_no
ORDER BY a.emp_no

-- better way to write this with JOIN:
SELECT a.emp_no, CONCAT(first_name,last_name) AS "name", b.salary 
FROM employees AS a
INNER JOIN salaries AS b ON b.emp_no = a.emp_no
ORDER BY a.emp_no

-- self JOISELECT a.emp_no, CONCAT(first_name,last_name) AS "name", b.salary 
SELECT a.id, a.name AS "employee", b.name as "supervisor name" 
FROM employees AS a, employee as b
WHERE a.supervisodId = b.id
-- to INNER JOIN
SELECT a.id, a.name AS "employee", b.name as "supervisor name" 
FROM employees AS a
INNER JOIN employee as b
ON a.supervisodId = b.id
```
### LEFT JOIN /  RIGHT JOIN add the dayta that don't have a match from table B

```SQL
-- count everyone does not match the second column:
SELECT count(emp.emp_no) 
FROM employees AS emp
LEFT JOIN dept_manager as dep ON emp.emp_no = dep.emp_no
where dep.emp_no is null
```

```sql
SELECT a.emp_no ,
CONCAT(a.first_name, a.last_name) as "name" ,
b.salary,
coalesce( c.title, 'No title change' ) ,
COALESCE( c.from_date::text,'-') AS "title taken on"
FROM employees AS a
INNER JOIN salaries AS b ON a.emp_no = b.emp_no
LEFT JOIN titles AS c
ON c.emp_no = a.emp_no AND (
    c.from_date = (b. from_date + interval '2 days') OR
    c.from_date = b.from_date
)
ORDER BY a.emp_no
```

---

### CROSS JOIN
create a combination ( just right in the mathematical term ) of every possible row

### FULL JOIN
give back every row even if not 'match' where the not match empty is going to be a <null> field
ex.

id | id
--|--
1 | 1
2 | 2
3 | <null>
4 | <null>
<null> | 30
<null> | 40

ex. on JOIN:
```sql

--  Question: Get all orders from customers who live in Ohio (OH), New York (NY) or Oregon (OR) state
select cus.firstname, cus.lastname,o.orderid
from orders as o
inner join customers as cus
on o.customerid = cus.customerid
where cus.state in ('OH','NY', 'OR')
ORDER BY o.orderid

-- Question: Show me the inventory for each product
select i.quan_in_stock,p.prod_id
from products as p
inner join inventory as i
on p.prod_id = i.prod_id

-- Question: Show me for each employee which department they work in
select e.first_name, e.last_name, dp.dept_name
from employees as e
inner join dept_emp as de on de.emp_no = e.emp_no
inner join departments as dp on dp.dept_no = de.dept_no

```

and we can use `USING` to map to primary to foreign key, so the last above can be rewritten as:

```sql
select e.first_name, e.last_name, dp.dept_name
from employees as e
inner join dept_emp as de using(emp_no)
inner join departments as dp USING(dept_no)
```

## GROUP BY
We use almost exclusively 'GROUP BY' with aggregate functions

SPLIT-APPLY-COMBINE

```sql
select dept_no, count(emp_no) --every column not in the GROUP BY need an aggregate function
from dept_emp
group By dept_no
```

```sql
-- How many people were hired on any given hire date?
SELECT hire_date, COUNT(emp_no) as "amount"
FROM employees
GROUP BY hire_date
ORDER BY "amount" DESC;

-- Show me all the employees, hired after 1991 and count the amount of positions they've had
SELECT e.emp_no, count(t.title) as "amount of titles"
FROM employees as e
JOIN titles as t USING(emp_no)
WHERE EXTRACT (YEAR FROM e.hire_date) > 1991
GROUP BY e.emp_no
ORDER BY e.emp_no;

-- Show me all the employees that work in the department development and the from and to date.
SELECT e.emp_no, de.from_date, de.to_date
FROM employees as e
JOIN dept_emp as de USING(emp_no)
join departments as dep using(dept_no)
WHERE dept_name = 'Development'
GROUP BY e.emp_no, de.from_date, de.to_date
ORDER BY e.emp_no, de.to_date;
-- or
SELECT e.emp_no, de.from_date, de.to_date
FROM employees as e
JOIN dept_emp as de USING(emp_no)
WHERE dept_no = 'd005'
GROUP BY e.emp_no, de.from_date, de.to_date
```

WHERE VS HAVING

`where` apply a filter to every row, `having` to a group of row.

```sql
--  Show me all the employees, hired after 1991, that have had more than 2 titles
SELECT e.emp_no, count(t.title) as "# titles over 2"
FROM employees as e
JOIN titles as t USING(emp_no)
WHERE EXTRACT (YEAR FROM e.hire_date) > 1991
GROUP BY e.emp_no
having count(t.title) > 2
ORDER BY e.emp_no;

-- Show me all the employees that have had more than 15 salary changes that work in the department development

SELECT e.emp_no, count(s.from_date) as "amount of raises"
FROM employees as e
JOIN salaries as s USING(emp_no)
JOIN dept_emp AS de USING(emp_no)
WHERE de.dept_no = 'd005'
GROUP BY e.emp_no
HAVING count(s.from_date) > 15
ORDER BY e.emp_no;

-- Show me all the employees that have worked for multiple departments

SELECT e.emp_no, count(de.dept_no) as "worked for # departments"
FROM employees as e
JOIN dept_emp AS de USING(emp_no)
GROUP BY e.emp_no
HAVING count(de.dept_no) > 1
ORDER BY e.emp_no;
```


## UNION to SELECT from different tables but it does not remove duplicates

```sql
SELECT col1, SUM(col2)
FROM table
GROUP BY col1
UNION
SELECT SUM(col2)
FROM table

```

## GROUPING SETS
subclause of `GROUP BY` that allow you to define multiple grouping

Ex usage: if you want to find how much has been sold for day, month, year, easily done by avoiding to much `grouping`

```sql
select EXTRACT (year from orderdate) as "year",
       EXTRACT (MONTH from orderdate) as "month",
       EXTRACT (DAY from orderdate) as "year",
       sum(ol.quantity)
from orderlines as ol 
group by
    ROLLUP (
    EXTRACT (year from orderdate),
    EXTRACT (MONTH from orderdate),
    EXTRACT (DAY from orderdate)    
    )
order by 
EXTRACT (year from orderdate),
EXTRACT (MONTH from orderdate),
EXTRACT (DAY from orderdate)
```

## WINDOW FUNCTION
How do we apply functions against a set of rows related to the current row?

ex. add the AVG o every salary so we could visually see how much each employee is from the AVG.

WINDOW FUNCTIONS create new column based on functions performed on a subset or "window"of the data and to get data back faster I can use LIMIT ... BUT anyway, the OVER() go __over__ the full set of data provided, not the limited ;)

## PARTITION BY
to divide rows into groups to apply the function against (optional)
It expand the OVER clause
```sql
*, AVG(salary)
 over(Partition by d.dept_name)
from salaries
join dept_emp as de using (emp_no)
join departments as d using (dept_no)
```

