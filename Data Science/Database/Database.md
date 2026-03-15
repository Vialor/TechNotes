**DBMS database management system**
**Database server**: the server that runs DBMS
- Layers of Abstraction of a DBMS:
    **View layer**: views can also hide information for security purposes, so that the same logical layer can have many different views for different applications
    **Logical layer**: describes data stored in database, and the relationships among the data
    **Physical layer**: the data structure actually used in the background
**schema**: logical structure of the db, including **logical schema** and **physical schema**
**instance**: the actual content of the db at a particular point in time
**physical data independence**: the ability to change logical schema without changing physical schema
**data model**: a collection of tools for describing data and data relationship  
entity-relationship/ER model (mainly for design), relational model, object-based data models, network model, hierarchical model, etc  

---

**DDL** data definition language: defines a set of tables stored in a **data dictionary**, which holds all these metadata

**DML** data manipulation language: procedural & declarative
> SQL statements include both DDL and DML

**SQL**(Structured Query Language) Database:
> Good for **Structured Data** (**data not predefined through data models**)
- **RDBMS** Relational database management system: MySQL, PostgreSQL (GUI Tools include pgAdmin, DBeaver)
    **Goals**: Data Integrity
	    Entity integrity: uniqueness of entities
	    Referential integrity: existence of foreign keys in the foreign tables
	    Domain integrity: attributes have correct format
    **Features**
	    Table Relationship: 1:1, M:1, M:N
	    Keys: attributes that make entity unique in a table
	    unique, never changing, never null
	    Foreign keys: keys from other tables
    
**NoSQL**(not only SQL) Database (It is an umbrella term)
> Good for **Unstructured Data** and have better scalability

---

**OLTP Online Transaction Processing**: focus on data consistency, data protection and throughput
	many small queries
**OLAP Online Analytical Processing**: focus on complex queries and their performance
	few large queries
**Data Mining**: focus on interesting patterns in the data
