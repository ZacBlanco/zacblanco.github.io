---
layout: page
title: CSE 232 - Databases Systems Review
permalink: /education/grad/cse232/review/
keywords: CSE, 232, UCSD, UC, San Diego, software, operating, system, database, dbms, rocksdb linux, C, rust, yannis, review
description: Review of Graduate Database Systems for CSE 232 at UC San Diego
mathjax: true 
---


* TOC
{:toc}

## terminology

- **Tables** are _relations_
- **Attributes** are _columns_
- **Schema** is the name of the relation _and_ the set of attributes
    - Take the ordering of items defined in a schema to be the _standard order_.
      i.e. `Movies(title, year, length, genre)`
- **Tuple** is a _row_ of a relation
    - Ordering must be consistent with _schema_
- **Domain** is the data type of a _attribute_
    - Must be a an elementary type (string, integer, double, etc)
- **Instance of a relation** is the set of tuples within
- **Key** is a set of attributes in a relation where we do not allow two of
  those tuples in the instance of a relation to have the same values for _all
  attributes of the key_ 

**ACID**
- _Atomicity_
- _Consistency_
- _Isolation_
- _Durability_

- Structured (relational) vs semi-structured (XML)

Overview of DBMS Components

![](../2020-10-01-18-31-05.png)

## SQL Review

### Create a table

```sql
CREATE TABLE Movies (
        title      CHAR(1OO),
        year       INT,
        length     INT,
        genre      CHAR(10),
        studioName CHAR(30),
        producerC# INT
);
```

### Delete a table

```sql
DROP TABLE Movies;
```

### Add a column

```sql
ALTER TABLE Movies ADD awards INT;
```

#### Set a default value when adding a column

```sql
ALTER TABLE Movies ADD awards INT DEFAULT 0;
```

### Remove a column

```sql
ALTER TABLE Movies DROP awards;
```


### Defining unique tuple identifiers

- Use `PRIMARY KEY`  for 1 attribute or `UNIQUE` (multiple attributes)

```sql
CREATE TABLE MovieStar (
    name      CHAR(30) PRIMARY KEY,
    address   VARCHAR(255),
    gender    CHAR(l),
    birthdate DATE
);
-- OR
CREATE TABLE MovieStar (
    name      CHAR(30),
    address   VARCHAR(255),
    gender    CHAR(l),
    birthdate DATE,
    PRIMARY KEY (name)
);
-- OR
CREATE TABLE Movies (
        title      CHAR(1OO),
        year       INT,
        length     INT,
        genre      CHAR(10),
        studioName CHAR(30),
        producerC INT,
        UNIQUE (title, year)
);
```
Two tuples in a relation cannot agree on all of the attributes in a
set of the primary keys, unless one of them is `NULL`

- `PRIMARY KEY` disallows `NULL`
- `UNIQUE` allows `NULL`

### Joins (as a concept)

- _Natural Join_ where two relations with exist with the same attributes are
  matched with one another


| U |
| A | B | C |
|---|---|---|
| 1 | 2 | 3 |
| 4 | 5 | 6 |
| 7 | 8 | 9 |

| V |
| B | C | D |
|---|---|---|
| 2 | 3 | 10 |
| 2 | 3 | 11 |
| 6 | 7 | 12 |

The natural join of `U` and `V` is

| A | B | C | D |
|-|-|-|-|
| 1 | 2 | 3 | 10 |
| 1 | 2 | 3 | 11 |

The concept of an **Outer Join** is where the dangling (non matching) tuples in
a relation are included in the join with the values in the left/right attributes
which don't match filled in as `NULL`

A **left outer join** or a **right outer join** is where the result of the join
only includes the `NULL` for the tuples in the table whose type of join it is.

FOr example, a `LEFT OUTER JOIN` statement for U and V means the non matching
tuples for table U (the left argument) would have the attribute on the join (D)
left as `NULL`

| A | B | C | D |
|-|-|-|-|
| 1 | 2 | 3 | 10 |
| 1 | 2 | 3 | 11 |
| 4 | 5 | 6 | `NULL` |
| 7 | 8 | 9 | `NULL` |

Similarly, the `RIGHT OUTER JOIN` would not have the tuples `(4, 5, 6 NULL)` or
`7, 8, 9, NULL)`. It would include though `(NULL, 6, 7, 12)`

### Pattern Matching

Can use the `LIKE` directive in a where clause to match patterns of string
fields

> get the names of movies whose title is 9 characters long and begins
> with "Star ", followed by 4 of any character

```sql
SELECT title
FROM Movies
Where title LIKE 'Star ____';
```

In this query we look for

> Retrieve the title of Movies whose title ends with "ing" and is preceded by
> anything else

```sql
SELECT title
FROM Movies
Where title LIKE '%ing';
```

### Dates, Times, and Timestamps

We can use `DATE`, `TIME`, or `DATETIME` expressions as constants in queries

```sql
SELECT title
FROM Movies
Where release_date = DATE 'yyy-mm-dd'
OR release_time = TIME 'hh:mm:ss'
OR release_datetime = TIMESTAMP 'yyy-mm-dd hh:mm:ss';
```

### NULLs

A `NULL` can mean any number of things

- Value is unknown
- Value doesn't apply or make sense for the attribute
- Value is witheld

Rules for `NULL`

- Any arithmetic operation on `NULL` is `NULL`
- Logic operations on a `NULL` arithmetic value can result in `UNKNOWN`

#### Logical rules for `UNKNOWN`

| X | Y | X && Y | X \|\| Y | NOT X |
|-|-|-|-|-|-|
| T | T | T | T | F |
| T | UNKNOWN | UNKNOWN | T | F |
| T | F | F | T | F |
| UNKNOWN | T | UNKNOWN | T | UNKNOWN |
| UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN |
| UNKNOWN | F | F | UNKNOWN | UNKNOWN |
| F | T | F | T | T |
| F | UNKNOWN | F | UNKNOWN | T |
| F | F | F | F | T |

### Order By

Order a query result by attribute ascending

> Select all movies which were produced by disney sorted by the runtime of the
> movie from shortest to longest with ties broken by by the titles alphabetized

```sql
SELECT *
FROM Movies 
Where studioName = 'Disney'
ORDER BY length, title
```

#### Order by descending

> Select all movies which were produced by disney sorted by the runtime of the
> movie from longest to shortest with ties broken by by the titles alphabetized
> in opposite order.

```sql
SELECT *
FROM Movies 
Where studioName = 'Disney'
ORDER BY length, title DESC
```

### Joins (without `JOIN`) in SQL

- Can think of a query as a nested loop.

> Select all movie stars who live at the same address

```sql
SELECT Star1.name, Star2.name
FROM MovieStar Star1, MovieStar Star2
WHERE Star1.address = Star2.address
  AND Star1.name < Star2.name
```

### Union, Intersection, and Difference

#### Intersection

> From two tables, select the female movie stars who are also movie execs with
> net worth over $10M

```sql
(SELECT name, address
FROM MovieStar
WHere gender = 'F')
INTERSECT
(SELECT name, address
FROM MovieExec
WHERE netWorth > 10000000);
```

#### Difference (`EXCEPT`)

> Select all movie stars who aren't producers

```sql
(SELECT name, address FROM MovieStar)
EXCEPT
(SELECT name, address FROM MovieExec);
```


> Who were the male stars in Titanic
```sql
SELECT name
FROM StarsIn, MovieStar
WHERE gender='M'
  AND movieTitle = 'Titanic'
  AND name = starName
```

### Practice Section

> Which movies are longer than _Gone with the Wind_

SELECT *
FROM Movies as M1, Movies as M2
WHERE M1.length > M2.length
AND M2.title = 'Gone with the Wind'

>  Give the manufacturer and speed of laptops with a HDD of at least 30GiB


```sql
SELECT maker, speed
FROM Product, Laptop
WHERE hd >= 30
  AND Product.model = PC.model;
```

> Find the model number and price of all products of any type made by
> manufacturer _B_

```sql
(SELECT model, price
FROM Product, PC
WHERE Product.maker = PC.maker
AND Product.maker = 'B')
UNION
(SELECT model, price
FROM Product, Laptop
WHERE Product.maker = Laptop.maker
AND Product.maker = 'B')
UNION
(SELECT model, price
FROM Product, PC
WHERE Product.maker = Printer.maker
AND Product.maker = 'B')
```

> Find those mfgs that have two different computers with speeds of at least 3.0

1. get all PC models > speed 3.0 
2. get all Laptop Models speed > 3.0
3. get a relation with schema (count, maker)
4. join on 

```sql
SELECT maker
FROM Product,(SELECT count(*) as num, P2.maker FROM (
(SELECT model from PC where speed > 3.0)
UNION
(SELECT model from Laptop where speed > 3.0)),
Product as P2
WHERE P2.model = model
GROUP BY P2.maker
) as P3
WHERE P3.num >= 2 AND Product.maker = P3.maker
```


### SQL Join Expressions

> Get the cartesian product of two relations

```sql
Movies CROSS JOIN StarsIn
```

#### Theta-Join

More traditional, join on keys (theta-join)

```sql
Movies JOIN StarsIn ON title = movieTitle AND year = movieYear
-- columns are replicated
```

#### Natural Join

- attributes with same name are not replicated

```sql
MovieStar NATURAL JOIN MovieExec;
```

#### Full Outer Join

- Join on attributes of same name, keep nulls on both sides for non-matches

```sql
MovieStar NATURAL FULL OUTER JOIN MovieExec
```

#### Left Outer Join

- Join on attributes of same name, keep nulls on the left side argument for
  non-matches, dropping tuples from the right argument which don't join on a
  matching attribute

```sql
MovieStar NATURAL LEFT OUTER JOIN MovieExec
```

#### Right Outer Join

- Join on attributes of same name, keep nulls on the right side argument for
  non-matches, dropping tuples from the left argument which don't join on a
  matching attribute

```sql
MovieStar NATURAL RIGHT OUTER JOIN MovieExec
```

#### CROSS JOIN as WHERE

```sql
A CROSS JOIN B
-- equivalent to
Select *
FROM A, B
```

### The Six Clauses of a Query

- `SELECT`
- `FROM`
- `WHERE`
- `GROUP BY`
- `HAVING`
- `ORDER BY`


### The `HAVING` Clause

- Restrict result based on aggregate property of the group.
- Use `HAVING` after a `GROUP BY` to filter **groups** based on aggregate
  properties

> Get the total runtime of movies produced by movie producers where at least one
> movie was produced before 1930.

```sql
SELECT name, SUM(length)
FROM MovieExec, Movies
WHERE producer# = cert#
GROUP BY name
HAVING MIN(year) < 1930
```

#### Aggregation Examples

> Find the average speed of PCs

```sql
SELECT AVG(speed)
FROM PC;
```

> Find the average speed of laptops over $1000

```sql
SELECT AVG(price)
FROM Laptop
WHERE price > 1000;
```

> Find the average price of PCs made by manufacturer A

```sql
SELECT AVG(price)
FROM Product, Laptop
WHERE maker = 'A'
AND Product.model = Laptop.model;
```

> Find the average price of PCs and laptops made by manufacturer D

```sql
SELECT AVG(price)
FROM Product, Laptop, PC
WHERE Product.maker = 'D'
AND (Product.model = Laptop.model OR Product.model = PC.model);
```

## Database Modifications

### Insert

General structure

```sql
INSERT INTO R(a1, a2, a3, ...) values (v1,v2, v3, ...);
```

Extend to subqueries

```sql
INSERT INTO R(a1)
    SELECT DISTINCT a2 FROM R2
    WHERE a2 NOT IN (SELECT a1 FROM R1);
```

### Deletion

Delete one or more columns: (simple form)

```sql
DELETE FROM R WHERE <condition>;
```

Cannot specify a tuple for deletion - must be done through the `WHERE` clause.

### Updates

Basic form

```sql
UPDATE R SET attribute = <value> where <condition>;
```

Ex, add "Pres. " to all MovieExecs who own studios

```sql
UPDATE MovieExec
SET name = 'Pres. ' || name
WHERE cert# in (SELECT presC# FROM Studio);
```

