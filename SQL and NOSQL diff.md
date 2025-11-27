# Difference between NoSQL and SQL databases

The key difference between nosql and sql lies in how they store data, manage and structure data.

1. SQL database(Relational databases)
    - Data is stored in tables with rows and columns.
    - Each table represent an entity.
    - Example: MySQL, PostgreSQL, SQLite, Oracle.
    - Typically scale vertically, meaning you can add more power.
    - Any changes to the schema require altering the table, which can be time-consuming and risky.
    - SQL databases uses sql(structured query language) for defining and manipulating data. It also supports complex queries, joins, and aggregations.
    - It is best for structured data, complex queries, applications requiring transactional consistency.
2. NoSQL database(Non-relational databases)
    - Data can be stored in various formats, such as key-value pairs, documents, wide-column stores, or graphs.
    - They don't require a predefined schema, which allows for more flexibility in how data is stored.
    - Especially designed to scale horizontally, meaning adding more servers to handle the load.
    - NoSQL have Schema-less or flexible schema meaning you can store different types of data together, and you can easily change the structure as our data evolves.
    - Each NoSQL have its own query language or methods for accessing data.
    - Queries are less complex and don't support multi-table joins.
    - It is ideal for large-scale data storage, and application requiring high scalability and flexibility.