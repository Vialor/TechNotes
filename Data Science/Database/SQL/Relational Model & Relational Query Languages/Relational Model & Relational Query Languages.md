# Relational Model
**attribute**: columns in relational tables  
  
**tuple**: rows in relational tables
**key**: a set of attributes
**superkey**: a key that can identify a tuple
**candidate key**: _minimal_ superkey  
_minimal_: you cannot remove any attribute from it

**primary key**: _selected_ candidate key
**foreign key**: attributes used as a primary key in other tables  
**foreign key constraint**: value in one relation must appear in another
> Know-hows:  
> one(r) to many(s): s has a foreign key of r  
> many to many: intermediary t has a foreign key of r and a foreign key of s

[[Relational Database Design]]
# Relational Query Languages
## Declarative
[[Relational Calculus]]
## Procedural
> SQL is based on Relational Algebra  
> SQL and relational algebra are still considered declarative in the context of programing languages as we do not worry about how to go through the table and find the exact rows  

[[Relational Algebra]]
[[SQL Grammar]]
[[Advanced SQL]]