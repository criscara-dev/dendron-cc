---
id: 0CjJLOqJFRLlz5Up3GBId
title: Databases
desc: ''
updated: 1625928807937
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
SELECT
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