# Accessing SQL From a Programming Language
JDBC (Java Database Connectivity) works with Java
ODBC (Open Database Connectivity) works with C, C++, C# and Visual Basic 
Embedded SQL
Also, PHP, Python, many other ways to access SQL
# Procedures & Triggers
## Procedure
Since PostgreSQL 11:
no return value (but can “return” values through changing `in/out` argument), good for complex sql
```SQL
create or replace procedure changeRemainingTickets
	(in tid integer, in change integer)
language plpgsql
as $$
begin
	update ticketClasses
	set num_tickets = num_tickets + change into remain
	where ticket_id = tid;
nde;
$$;
call changeRemainingTickets(1, 5);
```
## Trigger
In the db client browser, triggers can be found under corresponding tables
```SQL
-- trigger function
CREATE OR REPLACE FUNCTION reduce_tickets() RETURNS TRIGGER
LANGUAGE plpgsql
AS $$
BEGIN
    IF (SELECT num_tickets FROM ticketClasses WHERE ticket_id = NEW.ticket_id) >= NEW.tickets_count THEN
        UPDATE ticketClasses
        SET num_tickets = num_tickets - NEW.tickets_count
        WHERE ticket_id = NEW.ticket_id;
        RETURN NEW;
    ELSE
        RAISE EXCEPTION 'Not enough tickets';
    END IF;
END;
$$;
-- trigger
CREATE OR REPLACE TRIGGER remaining_tickets_updater
BEFORE INSERT ON reservations
FOR EACH ROW
EXECUTE FUNCTION reduce_tickets();
```
# Window functions
## `over`
`avg(salary) over()`: avg of all rows
`avg(salary) over (partition by department)`: avg by department
Window functions need `over` to work, and `rank` is a often-used window function.
## `rank` + `order by`
```SQL
select ID, rank() over (order by GPA desc) as s_rank
from student_grades
order by s_rank
```
`dense_rank()` to avoid gaps
```SQL
select ID, dept_name, rank()
	over (partition by dept_name order by GPA desc) as dept_rank
from dept_grades
order by dept_name, dept_rank
```
# Metadata
Under a schema called `information_schema`
All tables in information_schema:
```SQL
SELECT table_name
FROM information_schema.tables
WHERE table_schema = 'information_schema'
```
Frequent used tables:
```sql
INFORMATION_SCHEMA.tables
INFORMATION_SCHEMA.columns

select kcu.column_name, kcu.table_name as referencing_table, ctu.table_name as referenced_table
from information_schema.KEY_COLUMN_USAGE kcu
join information_schema.constraint_table_usage ctu
on kcu.constraint_name = ctu.constraint_name
where cu.constraint_name in (select constraint_name from information_schema.REFERENTIAL_CONSTRAINTS rc)
```