# SQL Concepts


This document contains higher-level concepts introduced in SQL that are implemented in different SQL providers such as Oracle, MySQL, PostgreSQL, etc. 

**Note**: SQL implementations are incompatible between vendors and do not necessarily completely follow standards. In particular, date 
and time syntax, string concatenation, NULLs, and comparison case sensitivity vary from vendor to vendor


# Introduction
SQL is a domain-specific language used for programming and managing data held in a relational database management system (RDBMS). 
SQL offers two main advantages over using read-write APIs such as [ISAM](https://bityl.co/8rK8) or [VASAM](https://bityl.co/8rKB):
1. SQL introduced the concept of accessing many records with one single command. 
2. SQL eliminates the need to specify *how* to reach a record (with or without an index).

In addition, a database provides the following benefits over manually written data repositories:
1. Program-data independence
2. Planned data redundancy 
3. Improved data consistency
4. Improved data sharing
5. Increased application development productivity
6. Enforcement of standards
7. Improved data quality
8. Improved data accessibility and responsiveness
9. Reduced program maintenance 

<br>

# SQL Statements 
The scope of SQL includes data query, data manipulation (insert, update and delete), data definition (schema creation and modification), 
and data access control.

SQL has many types of statements makes it a collection of sublanguages which are commonly consists of:
- [Data Definition Language](https://en.wikipedia.org/wiki/Data_definition_language) (DDL) - the sublanguage used for defining data 
elements.

- [Data Manipulation Language](https://en.wikipedia.org/wiki/Data_manipulation_language) (DML) - the sublanguage used for manipulating 
data stored in the database such as inserting and deleting contents.

- [Data Control Language](https://en.wikipedia.org/wiki/Data_control_language) (DCL) - the sublanguage used for controling the access of 
data stored in the database.

- [Data Query Language](https://en.wikipedia.org/wiki/Data_query_language) (DQL) - the sublanguage used for querying data within schema 
objects. The purpose of DQL commands is to get the schema relation based on the query passed to it.

<br>

## Language Elements of SQL
- Clause - which are constituent components of statements and queries. (In some cases, these are optional.)
- Expressions - are statements that produce values either sclar or table with rows and columns. 
- Predicates - specifies conditions that evaluates to [ternary logic](https://en.wikipedia.org/wiki/Ternary_logic).
- Queries - retrieves data based on specific criteria. 
- Statements - which may have a persistent effect on schemata and data, or may control transactions, program flow, connections, 
sessions, or diagnostics. 


<br>

---
# Objects
1. Entity - a name of a concept represented as a collection of data attributes.
    - Strong Entity
    - Weak Entity
    - Associative Entity

2. Table - are objects that contains rows and columns. 
    - row - corresponds to an intance of an entity
    - column - corresponds to the attributes 

3. Relation - may specify if the relationship is mandatory or optional
    - many to one
    - one to many
    - many to many

4. View
5. Transaction
6. Transaction Log
7. Trigger
8. Index
9. Cursor
10. Partition

<br>


---
# Operators

### Binary Operators

| Operator | Description                                   |
| ---------| ----------------------------------------------|
| `=`      | Equal to                                      |
| `<>`     | Not equal to                                  |
| `>`      | Greater than                                  |
| `<`      | Less Than                                     |
| `>=`     | Greater Than or Equal to                      |
| `<=`     | Less than or Equal to                         |
| `as`     | Used to changes columns when viewing results. |

<!-- ### Unary Operator -->


---
# References

- https://en.wikipedia.org/wiki/SQL
- https://en.wikipedia.org/wiki/Data_definition_language
- https://en.wikipedia.org/wiki/Data_manipulation_language
- https://en.wikipedia.org/wiki/Data_control_language
- https://en.wikipedia.org/wiki/Data_query_language
- https://en.wikipedia.org/wiki/Ternary_logic