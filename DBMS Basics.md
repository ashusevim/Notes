# Basic DBMS

key - minimum set of attributes/columns to uniquely identify a row/tuple.

1. DDL - Data Definition Language
    - create, alter, drop, truncate
2. DQL - Data Query Language
    - select query
    - filtering with WHERE clause
    - sort with ORDER BY clause
3. DCL - Data control language
    - Grant, Revoke
4. DML - Data Manipulation Language
    - INSERT, UPDATE, DELETE
5. tcl - transaction control language
    - ROLLBACK
    - Commit
    - Save Point

---

- Column - Field - Attributes (same)
- Row - Tuples - Records (same)
- Simple key - key with only one attribute/column
- composite key - key with multiple attribute/column
- Candidata key - key with combination of all the unique keys
- primary key - a key that uniquely identifies each record in a table
- Alternate/secondary keys - candidate keys that are not primary keys
- **Entity Integrity** is a fundamental principle in relational database design that ensures the uniqueness and validity of each row in a table
- Relational Schema = table structure + Integrity contraints
- super key - candidate key ∪ (Union Symbol) attributes