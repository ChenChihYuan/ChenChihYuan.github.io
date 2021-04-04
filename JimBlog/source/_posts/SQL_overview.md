---
title: SQL
date: 2020-06-13 11:50:43
tags:
- note
- SQL
---


# SQL
Structured Query Langauge

> SQL is a programming language used to talk to the databases

## Databases that use SQL
1. SQL server
2. Oracle
3. MySQL
4. Postgre SQL
5. SQLite
6. DB2
7. BigData

## Tools to write SQL
1. SQL Server Management Studio (SSMS)
2. SQL Developer
3. SQL Workbench
4. TOAD
   
# Create Database



---
# What is a Database(DB)?
## any collection of related information
- Phone Book
- Shopping list
- Todo list
- Your 5 best friends
- Facebook's User Base

## Database can be stored in different ways
- On paper
- In your mind
- On a computer
- This powerpoint
- Comment Section

## Computers + Databases = <3
### Amazon.com
- Keeps track of Products, Reviews, Purchase Orders, Credit Cards...etc
- Information is extremely valuable and critical to Amazon.com's functioning
- Security is essential

> Computers are great at keeping track of large amounts of information

# Database Managemeny Systems(DBMS)
- A special software program that hekps users create and maintain a database.
   - Make it easy to manage large amounts of information
   - Handle Security
   - Backups
   - Importing/Exporting data
   - Concurrency
   - Interacts with software applications
      - Programming Languages

> Amazon.com will interact with the **DBMS** in order to create, read, update and delete information.

# C.R.U.D
- Create
- Read
- Update
- Delete

# Two Types of Databases
## Rational Databases(SQL)
- Organize data into one or more tables
   - Each table has columns and rows
   - A unique key identifies each row

## Non-Retional(noSQL/ not just SQL)
- Organize data is anything but a tranditional table
   - Key-value stores
   - Documents(JSON, XML, etc)
   - Graphs
   - Flexible Table

## Relational Databases(SQL)
### Student Table
| ID# | Name | Major |
|---|---|---|
| 1 | Jack  | Biology  |
| 2 | Kate | Sociology  |
| 3 | Claire  | English  |
| 4 | John  | Chemistry  |

### User Table
|UserName   	|Password   	|Email   	|
|---	|---	|---	|
|jsmith22   	|wordpass   	|   	|
|catlover45   	|apple223   	|   	|
|gamerkid   	|   	|   	|

- Retional Database Management Systems(RDBMS)
   - Help users create and maintain a relational database
      - mySQL, Oracle, postgreSQL, mariaDB, etc.

- Structed Query Language(SQL)
  - Standardized language for interacting with RDBMS
  - Used to perfrom `C.R.U.D` operations, as well as other administrative tasks(user management, security, backup, etc)
  - Used to define tables and structures
  - SQL code used on one RDBMS is not always portable to another without modification

- Non-Relational Databases
  - Non-Relational Database Management Systems(NRDBMS)
    - Help users create and maintain a non-relational database
      - mongoDB, dynamoDB, apache, cassandra, firebase,etc
- Implementation Specific
  - Any non-relational database falls under this category, so there's no set language standard
  - Most NRDBMS will implement their own language for performing `C.R.U.D` and administrative operations on the database

# Summary
## Warp up 
- Database is any collection of related information
- Computer are great for storing databases
- Database Management Systems(DBMS) make it easy to create, maintain and secure a database.
- DBMS allows you to perform the C.R.U.D operations and other administrative tasks
- two types of Database, `Relational` & `Non-Relational`
- Relational databases uses SQL and store data in tables with rows and columns
- Non-Relational data store data using other data structures
  
---
  
| student_id |      name     |  major|
|----------|:-------------:|------:|
|  1  | Kate    | Sociology |
|  2  | Jack    | Biology |
|  3  | Claire  | English |
|  4  | Jack  | Biology |
|  5  | Mike  | Comp. Sci |
    
> Though there are two Jacks and they both major in Biology, we can use `primary key`, namely the **student id**, to identify them.

# Structured Query Language(SQL)
- SQL is a language used for interacting with Relational Database Management Systems(RDBMS)
   - You can use `SQL` to get the RDBMS to do things for you. 
     - Create, retrieve, update & delete data
     - Create & manage databases
     - Design & create databases tables
     - Perform administration tasks(security, user management, import/export, etc) 
  - SQL implementations cary between systems
     - Not all RDBMS' follow the SQL standard to a `T`
     - The concepts are the same but the implementation may vary

## Structured Query Language(SQL) _ Hybrid language
SQL is actually a **hybrid language**, it's basically 4 types of languages in one
### Data Query Language (DQL)
- Used to query the database for information
- Get information that is already stored there

### Data Definition Language (DDL)
- Used for defining database schemas
  
### Data Control Language (DCL)
- Used for controlling access to the data in the database.
- User & permissions managemnet

### Data Manipulation Language (DML)
- Used for inserting, updating and deleting data from the databases

# Queries
A query is a set of instructions given to the RDBMS(written is SQL) that tell the RDBMS what information you want it to retrieve for you.
- Tons of data in a DB
- Often hidden in a complex schema
- Goal is only get the data you need
  
```sql
SELECT employee.name, employee.age
FROM employee
WHERE employee.salary > 30000;
```


