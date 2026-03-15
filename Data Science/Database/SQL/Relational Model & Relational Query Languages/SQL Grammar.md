# SQL DML: Query
## Skeleton
```SQL
WITH TempAttributes1 as (Subquery1),
TempAttributes2 as (Subquery2)
SELECT Attributes, ColumnsToGroup, aggregate_fun(ColumnsToGroup)
FROM Table1
JOIN Table2 ON JoinDataCondition
JOIN Table3 ON JoinDataCondition
WHERE FilterDataCondition
GROUP BY ColumnsToGroup
HAVING ConditionOnAggregateFunction
ORDER BY Attributes OrderStandard
LIMIT SampleSize
OFFSET SampleOffset
```
`TempAttributes` can be referenced in the rest of the query body
**Column aliases**: Can use `Attributes/Table/Query as NewName` to rename them, `as` is optional  
  
`SELECT *` to select all attributes
A typical evaluation order:  
FROM JOIN ⇒ WHERE ⇒ GROUP BY ⇒ HAVING ⇒ SELECT ⇒ ORDER BY, only later clauses can recognize the alias from prev clauses  
Unlike `FilterDataCondition` , `JoinDataCondition` has to respect the the Join methods. It cannot filter out the entries from the left table in LEFT JOIN, but `FilterDataCondition` can.
`SELECT DISTINCT` to force the elimination of duplicates, `SELECT ALL` to specify that duplicates not be removed
`FilterDataCondition`: LIKE, =, >,<, <>, IS NULL, IN, BETWEEN … AND … (both sides inclusive)
`OrderStandard`: ASC/DESC
### JOIN…ON…
NATURAL JOIN  
JOIN/OUTER JOIN
	JOIN is by default INNER JOIN
LEFT JOIN/LEFT OUTER JOIN: empty entries of the right would be NULL  
RIGHT JOIN/RIGHT OUTER JOIN: empty entries of the left would be NULL  
FULL JOIN/FULL OUTER JOIN  
  
`FROM A, B`: Cartesian Product $A\times B$﻿

> should use `IS NULL` and `IS NOT NULL` to check NULL not `=` because **any comparison with NULL returns unknown**
## Aggregate Functions
  
COUNT, COUNT(DISTINCT column), SUM, AVG, MIN, MAX

> Except for `COUNT(*)` , aggregate functions ignore null values. `COUNT(*)` count the number of rows
  
Skeleton of aggregate functions:
```SQL
SELECT GroupColumnsSubset, count(GroupColumns...), sum/avg/min/max(GroupedColumns)
FROM table
GROUP BY GroupColumns
HAVING ConditionOnAggregateFunction;
```
Notes:
You can only reference the result of aggregate function in HAVING, and you can only reference regular columns in WHERE.
Remember to group by every column you aren’t aggregating.
## Column Functions
```SQL
-- Conditions
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    WHEN conditionN THEN resultN
    ELSE result
END AS r;
-- Transform
CAST(column AS dtype)
UPPER() LOWER()
COALESCE(NULL, var) # returns the first non-null value in a lis
-- Text Matching
-- large piece of text
WHERE CONTAINS (P.description, 'brooklyn', 1) > 0 -- word-oriented
-- short strings
WHERE P.name LIKE '_string%\%' ESCAPE '\' -- % matches any substring, _ matches any character
-- ILIKE: LIKE ignoring case
~ '^[0-9]' -- regex
-- ~*: regex ignoring case
```
## Set Operations
```SQL
UNION, INTERSECT, EXCEPT
# keep the duplicates:
UNION ALL, INTERSECT ALL, EXCEPT ALL
```
  
```SQL
SOME(subquery)
ALL(subquery)
EXISTS(subquery) # check if empty
UNIQUE(subquery) # check duplicity
```
# SQL DML: Modification
```SQL
DELETE FROM <table>
WHERE ...;
# This is not atomic, SQL deletes rows one by one meaning the nature of WHERE condition may change
 
INSERT INTO table (Attributes)
	VALUES(Attribute_values);
UPDATE <table>
	SET Attribute=Value
	WHERE ...;
```
# SQL DDL
```SQL
DROP TABLE IF EXISTS instructor
CREATE TABLE instructor (
	ID char(5),
	name varchar(20) not null,
	dept_name varchar(20),
	salary numeric(8,2),
	primary key (ID),
	foreign key (dept_name) references department);
```
### Integrity Constraints
```SQL
not null
unique(Attributes)
check(Predicate)
```
```SQL
foreign key (Attributes) references department
	on delete cascade # remove rows when the referenced record is gone
	on update cascade # change when the referenced record is change
```
```SQL
ALTER TABLE r ADD A D;
// A: the name of the attribute to be added to relation r and D is the domain of A.
// All tuples in the relation are assigned null as the value for the new attribute.
ALTER TABLE r DROP A;
// A: the name of an attribute of relation r
// Dropping of attributes not supported by many databases.
```
### View
Update the underlying DB will update the view. However, for materialized view, whose content is stored physically, this update may not be immediate.
View Expansion: translating queries involves views into the equivalent queries that operates directly on the underlying db tables
```SQL
CREATE VIEW view_name as <expression>
INSERT INTO view_name as <expression>
DROP VIEW view_name
```
# Others
### Authorization Specification
```SQL
grant <privilege list> on <rekation name or view name> to <user list>
```
user list: user id, public or a role