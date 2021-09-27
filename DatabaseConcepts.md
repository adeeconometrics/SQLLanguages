# Database Concepts

This document contains concepts in Database Management Systems (DBMS). 

## Three-Schema Approach 
The Three-Schema Approach is a guide to building information systems and systems information management that 
originated in the '70s. it proposes three views in systems development:

- External schema for user views. It represents various structures seen by the user.
- Conceptual schema that integrates external schemata. It represents the basic underlying structure of data as 
viewed by the enterprise as a whole.
- Internal schema that defines physical storage structures. The representation of data structure is seen by the database.

<p align = "center">
<img src= "assets/img/ThreeSchemaModel_1975.png" class = "center">
</p>

---
# Database Design
Database design is the organization of data according to a database model. The designer determines what data 
must be stored and how the data elements interrelate. With this information, they can begin to fit the data into
the database model

Database design involves classifying data and identifying interrelationships. This theoretical representation 
of the data is called an ontology. The ontology is the theory behind the database's design.

---
### DBMS Design 

intention: independent and fault-tolerant data storage
requirements:
- random access
- concurrent access by parallel writing process
- fault-tolerance 
- high-performance
- scalable

Design Paradigm: ACID
- Atomic - all changes take place or none
- Consistent - changes transform the database from one valid state to another valid state
- Isolated - transactions of different users working at the same time will not affect each other
- Durable - retains committed changes even if the system crashes afterward

---
### DBMS Designs

Heirarchical DBMS
- impose a heirarchical structure on the entities 
- every child has exactly one parent (except the root)
- does not provide the modeling of n:m relations
- no possible way to navigate directly to data stored in lower levels of the tree
	
Network DBMS
- data structures are represented as a complex network with links to one or more entity (cycles are also possible)
- 

Relational DBMS
- data structures are represented as relations(certain kind of table) and the relationship between those relations
- relationships are based purely upon content
- relationships in this model do not have any direction

Object-Oriented DBMS
- data structures are represented as objects
- pure DBMS does not support the central concepts of OOP (Type system, Inheritance, Polymorphism, Encapsulation)
	
NoSQL DBMS - stands for emerging group of DBMS that differs from others in central concepts
- does not necessarily comply with ACID
- data must not necessarily be structured according to any schema
- goals:
	- support fault-tolerance
	- support distributed data
- implementations differ widely in storing techniques
	- graph-oriented
	- key-value stores
	- document-oriented
- does not offer SQL interface

NewSQL: This class of DBMS seeks to provide the same scalable performance as NoSQL systems while maintaining the ACID paradigm, the relational model, and the SQL interface.


---
### SQL/RDBMS Data model

- based on mathematical concepts: relational algebra, boolean algebra
- data structures are represented as relations (tables) and attributes (columns)
- the information about one entity in the real world is stored within one row of a table
	- an entity in the real world may be broken down into its subparts
	- modelling a real-world entity depends on the domain-specific concept and information requirements 
- data model should conform to normal form


### Determining data to be stored
- This involves [requirement analysis](https://en.wikipedia.org/wiki/Requirements_analysis) that requiress skill on the part of the 
database designer to elicit the needed information with the domain knowledge. Usually, the database designer is a person with expertise 
in the area of database design rather han domain-specific expertise. Consequently,, a database designer might consult a domain expert 
for laying down its design requirements. 

### Determining Relationships
- Once the database designer is aware of the pieces of data to be stored in the database, they must then determine the relationships, 
and dependency between the data. 
### Structuring Data
- Once the relationships and dependencies amongst the various pieces of information have been determined, it is possible to arrange the 
data into a logical structure which can then be mapped into the storage objects supported by the database management system. In the case 
of relational databases the storage objects are tables which store data in rows and columns. In an Object database the storage objects 
correspond directly to the objects used by the Object-oriented programming language used to write the applications that will manage and 
access the data

<br>

### Design Process (from Microsoft Acces)
1. Determine the purpose of the database.
2. Find and organize the information required.
3. Divide the information into tables. 
4. Turn Information items into columns. 
5. Specify primary keys. 
6. Set up the table relationships.
7. Refine the design. 
8. Apply normalization rules. 


## Entity-Relationship Diagram

<br>

## Normalization
In the field of relational database design, normalization is a systematic way of ensuring that a database structure is suitable for 
general-purpose querying and free of certain undesirable characteristics—insertion, update, and deletion anomalies that could lead to 
loss of data integrity.

see: https://en.wikipedia.org/wiki/Database_normalization

<br>

## Schema

<br>

## Cardinality

<br>


---
# Reference
- https://en.wikipedia.org/wiki/Three-schema_approach
- https://en.wikipedia.org/wiki/Requirements_analysis
- https://en.wikipedia.org/wiki/Database_design
- 