---
layout: page
title: SQLite and SQL Basics
description: >
    Notes I keep for myself while learning the basics of SQL and SQLite.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## What I Know About SQL and SQLite So Far

**SQL (Structured Query Language)** is a language I use to communicate with relational databases. I can use it to create tables, insert data, retrieve information, update existing records, and delete data.

**SQLite** is a lightweight relational database engine. Unlike database systems such as PostgreSQL or MySQL, SQLite does not require a separate database server. It stores an entire database in a single file.

For example, I could have a database file called `library.db` containing all the information about books in my library application.

Some things I should remember:

- SQL is the language I use to work with the database.
- SQLite is a database engine that understands SQL.
- A database contains tables.
- Tables contain rows and columns.
- Each row represents a record.
- Each column represents a particular piece of information.

## Creating a Database

I can create or open an SQLite database using the SQLite command-line tool:

```bash
sqlite3 library.db
```

If the file does not exist, SQLite creates it. Otherwise, it opens the existing database.

I can exit the SQLite command-line interface with:

```sql
.quit
```

Some useful commands available in the SQLite command-line interface:

```sql
.tables
.schema
.databases
.headers on
.mode column
```

These are SQLite shell commands, not SQL statements.

- `.tables` lists the tables.
- `.schema` displays the database schema.
- `.databases` lists attached databases.
- `.headers on` displays column names in query results.
- `.mode column` formats results into columns.

## Tables and Data Types

A table is where I store records. When creating a table, I define its columns and their data types.

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT NOT NULL,
    age INTEGER,
    email TEXT
);
```

Here, I created a table called `users` with four columns.

SQLite supports five storage classes:

| Storage Class | Purpose |
|---|---|
| `NULL` | No value |
| `INTEGER` | Whole numbers |
| `REAL` | Floating-point numbers |
| `TEXT` | Text strings |
| `BLOB` | Binary data |

SQLite uses dynamic typing, meaning values have storage classes independently of the columns in which they are stored. Column declarations still provide type affinity, which influences how SQLite handles values.

Some common type names I will encounter:

- `INTEGER`
- `REAL`
- `TEXT`
- `BLOB`
- `NUMERIC`

I should remember that SQLite does not enforce rigid data types in the same way as some other database systems. If I want stricter type checking, I can use a `STRICT` table:

```sql
CREATE TABLE products (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    price REAL NOT NULL
) STRICT;
```

## Primary Keys

A **primary key** uniquely identifies each record in a table.

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT NOT NULL
);
```

I can use the `id` column to identify a particular user.

In SQLite, `INTEGER PRIMARY KEY` is an alias for the rowid, except in `WITHOUT ROWID` tables.

I can also use `AUTOINCREMENT`:

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT NOT NULL
);
```

However, I should know that `AUTOINCREMENT` is usually unnecessary. Without it, SQLite already generates an integer key automatically. `AUTOINCREMENT` mainly prevents SQLite from reusing previously deleted rowids.

## Inserting Data

I use `INSERT INTO` to add records to a table.

```sql
INSERT INTO users (username, age, email)
VALUES ('Alice', 25, 'alice@example.com');
```

I can insert multiple records:

```sql
INSERT INTO users (username, age, email)
VALUES
    ('Bob', 30, 'bob@example.com'),
    ('Charlie', 22, 'charlie@example.com'),
    ('Diana', 28, 'diana@example.com');
```

I should specify the column names rather than relying on the order of columns in the table.

## Retrieving Data with SELECT

I use `SELECT` to retrieve information from a table.

```sql
SELECT * FROM users;
```

The `*` means I want all columns.

I can retrieve only the columns I need:

```sql
SELECT username, age FROM users;
```

I can use `WHERE` to filter records:

```sql
SELECT * FROM users
WHERE age > 25;
```

Some comparison operators I can use:

| Operator | Meaning |
|---|---|
| `=` | Equal |
| `!=` or `<>` | Not equal |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal |
| `<=` | Less than or equal |

I can combine conditions:

```sql
SELECT * FROM users
WHERE age >= 18 AND age <= 30;
```

I can also use `OR` and `NOT`.

### LIKE

I use `LIKE` when I want to search for a text pattern.

```sql
SELECT * FROM users
WHERE username LIKE 'A%';
```

The `%` represents zero or more characters.

Some examples:

- `'A%'` matches text beginning with A.
- `'%a'` matches text ending with a.
- `'%li%'` matches text containing li.

The `_` wildcard represents exactly one character.

### ORDER BY

I use `ORDER BY` to sort results.

```sql
