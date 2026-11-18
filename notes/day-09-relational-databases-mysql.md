# Day 9 - Relational databases and MySQL

## Session goal

Create two related tables in the Ubuntu lab, insert sample catalog data, query
with `WHERE` and `JOIN`, and use `EXPLAIN` to inspect a query plan. Learn why
indexes can speed up reads and add work to writes.

Suggested time: 90-120 minutes.

## 1. Fundamentals

A relational database stores data in tables with defined columns. A primary key
uniquely identifies a row. A foreign key relates a row to a row in another
table. SQL is used to define tables and read or modify data.

`SELECT` retrieves rows; `WHERE` filters them; `JOIN` combines related rows.
An index is an additional data structure that can help a database find rows
without scanning every row. Indexes consume storage and can slow inserts,
updates, and deletes because the index must also be maintained.

`EXPLAIN` shows how the optimizer plans to execute a query. Its estimates and
fields depend on the database version and schema; inspect the plan rather than
assuming every index should be used.

## 2. Practical lab

Install or use MySQL/MariaDB only in your personal Ubuntu VM or other disposable
lab. Package installation changes the system and may require administrator
access; do not use a work or production host. Follow the official installation
instructions for the Ubuntu release in your VM.

For the course's Ubuntu lab, install one server if it is not already present:

```bash
sudo apt update
sudo apt install mysql-server
```

If using MariaDB instead, install `mariadb-server`. Do not install both for
this exercise.

Connect to your local server (command may differ by installation):

```bash
sudo mysql
```

Create a database and two related tables:

```sql
CREATE DATABASE sre_catalog;
USE sre_catalog;

CREATE TABLE categories (
  category_id INT PRIMARY KEY,
  name VARCHAR(80) NOT NULL
);

CREATE TABLE catalog_items (
  item_id INT PRIMARY KEY,
  category_id INT NOT NULL,
  name VARCHAR(120) NOT NULL,
  price DECIMAL(10,2) NOT NULL,
  FOREIGN KEY (category_id) REFERENCES categories(category_id)
);

INSERT INTO categories VALUES (1, 'Network'), (2, 'Storage');
INSERT INTO catalog_items VALUES
  (101, 1, 'Router A', 125.00),
  (102, 1, 'Switch B', 80.00),
  (103, 1, 'Optic C', 14.50),
  (201, 2, 'Disk D', 90.00),
  (202, 2, 'Disk E', 110.00);
```

Filter rows:

```sql
SELECT item_id, name, price
FROM catalog_items
WHERE price >= 80.00;
```

Join items to their categories:

```sql
SELECT c.name AS category, i.name AS item, i.price
FROM catalog_items AS i
JOIN categories AS c ON c.category_id = i.category_id;
```

Inspect the plan for a filter:

```sql
EXPLAIN SELECT item_id, name
FROM catalog_items
WHERE category_id = 1;
```

Record the `type`, `possible_keys`, `key`, and estimated `rows` columns when
present. With only five rows, a full table scan can be reasonable. Optionally
add an index and compare:

```sql
CREATE INDEX idx_catalog_category ON catalog_items(category_id);
EXPLAIN SELECT item_id, name FROM catalog_items WHERE category_id = 1;
```

An optimizer may still prefer a scan for a tiny table. The exercise is to read
the plan, not to force a specific result.

## 3. Review and Day 9 notes

1. What does a primary key guarantee?
2. How does the foreign key connect the two tables?
3. What does the `JOIN` return?
4. What can an index improve, and what does it cost?
5. What did `EXPLAIN` show for your query?

- MySQL/MariaDB version:
- `WHERE` result:
- `JOIN` result:
- `EXPLAIN` notes before/after index:
- Three things learned:
  1.
  2.
  3.
- Confusing command / resolution:

## Done when

- [ ] Two related tables contain five catalog items.
- [ ] I ran a filter query and a join.
- [ ] I captured and can explain the main fields in one `EXPLAIN` plan.

## Further reading

- [MySQL Reference Manual: EXPLAIN](https://dev.mysql.com/doc/refman/8.4/en/explain.html)
- [How MySQL uses indexes](https://dev.mysql.com/doc/refman/8.4/en/mysql-indexes.html)

