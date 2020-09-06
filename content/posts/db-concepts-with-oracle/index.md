---
title: Database Concepts and Oracle DB Terminologies
cover:
date: 2020-09-05
description: Everything you need to know to work on a Database
tags: ['post','database']
draft: false
---

#### What is Database Schema?

In its true sense, it defines any kind of structure that we are defining around the data. This could include tables, views, fields, relationships, packages, procedures, indexes, functions, types, sequences, materialized views, queues, triggers, synonyms, database links, directories, xml schemas and other elements. A database generally stores its schema in a data dictionary. 

This structure helps us in understanding the data, that would otherwise take up a lot of time and efforts in finding the head and tail of the data. This structure and its corresponding data relationship is called **Schema** of the database.

The application of this structure is something that is specific to Database Vendors. For example, there are unstructured data stores which apply schema only when the data is read. In other words, data lives in juggled state, and we apply the structure to thequery code, called as **Schema On Read**. On the otherhand there are Databases which force structure as a condition before data gets written, called as **Schema On Write**.

The method of how a schema gets designed can influence different behaviours in a database, for example if a database schema is designed as a series of tables connected by primary keys, then it is likely something designed for reading and writing singular records which is ideal for applications focused on transaction-oriented tasks also known as **Online Transactional Processing (OLTP)**.

However, if a schema has a central table connected to keys supplied by surrounding tables then it's likely to be something designed to make read output highly efficient. This kind of schema is efficient for high scale information delivery, called as **star schema**.

When we are interacting with data, the schema of the data becomes important topic. A good schema can mean the difference between a query lasting a few seconds to a query lasting many hours.

But Oracle uses the term schema slightly differently in its databases from what it generally means. As mentioned on [Oracle Docs](https://docs.oracle.com/database/121/CNCPT/intro.htm#CNCPT939), an oracle schema is always associated with a user and it is a named collection of database objects, including logical structures such as tables and indexes belonging to the user. It has the same name as the database user who owns it. Simply put, there is one schema per user.

#### What is Database User?

The account that is created and used to connect to a database is called User. The database contains all the users you've created, and their data (and a bunch of predefined system users, tables, views, etc. which make the whole thing work).

You create users with the create user statement. This also "creates" the schema (initially empty) - you cannot create a schema as such, it is tied to the user. Once the user is created, an administrator can grant privileges to the user, which will enable it to create tables, execute select queries, insert, and everything else.
You can create a database with the create database statement, once you've installed the Oracle software stack.

  - The `CREATE USER` command creates a user. It also automatically creates a schema for that user.

  - The `CREATE SCHEMA` command does not create a "schema" as it implies, it just allows you to create multiple tables and views and perform multiple grants in your own schema in a single transaction.

Furthermore, a user can access objects in schemas other than their own, if they have permission to do so.

>
Think of a user as you normally do (username/password with access to log in and access some objects in the system) and a schema as the database version of a user's home directory. User "foo" generally creates things under schema "foo" for example, if user "foo" creates or refers to table "bar" then Oracle will assume that the user means "foo.bar".


**The Hierarchy in Oracle vs MS SQL Server**

Oracle : Database > User > Schema > Table

MS SQL : Instance > Database > Schema > Table

An example that shows how a user can access objects in schemas other than their own:

```sql
SQL> conn b/b
Connected.
SQL> select * from my_tab
2 /
OBJECT_ID OBJECT_NAME
---------- ------------------------------
80116 BIG_PK
80114 BIG_TABLE
45709 BP
45710 BP_PK

SQL> grant select on my_tab to a
2 /

Grant succeeded.

SQL> conn a/a
Connected.
SQL> select * from my_tab
2 /
OBJECT_ID OBJECT_NAME
---------- ------------------------------
80404 ACTOR
80414 ACTOR_NT
45707 AP
45708 AP_PK
52765 ASSIGNMENT
49747 A_ID
52768 A_OBJTYP

7 rows selected.

SQL> alter session set current_schema=b
2 /

Session altered.

SQL> select * from my_tab
2 /
OBJECT_ID OBJECT_NAME
---------- ------------------------------
80116 BIG_PK
80114 BIG_TABLE
45709 BP
45710 BP_PK

SQL> select username, schemaname 
2 from v$session 
3 where sid in (select sid from v$mystat)
4 /
USERNAME SCHEMANAME
--------- -----------
A B

SQL> 
```

Another example:

```sql
SQL> conn u2/u2
Connected.
SQL> select table_name from user_tables
  2  /
TABLE_NAME
------------------------------
T1
T2
T3

SQL> select * from t1
  2  /
      COL1 COL2
---------- ----------
         1 BBBBBBBBBB

SQL> grant select on t1 to u1
  2  /

Grant succeeded.

SQL> conn u1/u1
Connected.
SQL> select table_name from user_tables
  2  /
TABLE_NAME
------------------------------
T1
T2

SQL> select * from t1
  2  /
      COL1 COL2
---------- ----------
         1 AAAAAAAAAA

SQL> alter session set current_schema=U2
  2  /

Session altered.

SQL> select * from t1
  2  /
      COL1 COL2
---------- ----------
         1 BBBBBBBBBB

SQL> select * from t2
  2  /
select * from t2
              *
ERROR at line 1:
ORA-00942: table or view does not exist


SQL> select * from u1.t2
  2  /

no rows selected

SQL> select table_name from user_tables
  2  /
TABLE_NAME
------------------------------
T1
T2

SQL> 
```

>
##### References: 
>
- [Orcale Documentations](https://docs.oracle.com/database/121/CNCPT/intro.htm#CNCPT939)
>
Few More Important Resources that can be checked out:
   - [Oracle Glossary](https://docs.oracle.com/database/121/CNCPT/glossary.htm#CNCPT89131)