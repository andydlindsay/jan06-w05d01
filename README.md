# W5D1 - SQL Intro

### To Do
- [x] Introduction to RDBMS
- [x] The Relational Data Model (Tables, Columns, Rows)
- [x] SELECT Statements
- [x] Filtering, ordering, limiting, etc.
- [x] Joining tables
- [x] Grouping records
- [x] Aggregate functions

### Relational Database Management System (RDBMS)
- A program that serves, **and** controls interactions with, one or more _Relational Databases_
- Communicates using a custom protocol (eg. `postgres://` for postgres) that sits on top of TCP
- Front End <-----`http` + `tcp`-----> Back End <-----`postgres` + `tcp`-----> RDBMS

### Structured Data
- The **S** in **SQL** is for _structured_. This means that our data must conform to a _structure_ in order to store it in the database.
- The data itself is stored in **tables** which define things such as field names, data types, and other data constraints
- You are probably familiar with tables already if you've used programs like Excel or Calc
- Tables are made up of **columns** and **rows**
  - Columns are called `fields`
  - Rows are called `records`
- Each table describes an entity (eg. `users`, `products`, `shifts`, `tweets`)
  - The fields represent properties of the entity
  - Each record represents one unique entity

### Primary Keys
- In order to reference a particular record in a table, each one is given a unique identifier we call a **Primary Key**
- Other tables can then make reference to a particular record in another table by storing the Primary Keys value
- We call a Primary Key stored in another table a **Foreign Key**
- It is through this Primary Key/Foreign Key relationship that our tables are _related_ to one another

### SELECT
- The **SELECT** clause queries the database and returns records that match the query
- Always accompanied by the **FROM** keyword which indicates which table we'd like to query
- SELECT takes a list of field names as an argument
- Every SQL command ends in a semicolon (;), that's how we tell the application that we are finished entering our query

```sql
-- basic SELECT query
SELECT username, email FROM users;

-- the asterisk (*) can be used as a wildcard to return all fields in the table
SELECT * FROM users;

-- it is customary to put each SQL clause or keyword on a separate line for readability
SELECT username, email
FROM users;
```

### JOIN
- We connect tables together using **JOIN**s
- The tables are joined together using the primary key and foreign key
- There are various types of joins:
  - `INNER JOIN`: The default. Return only records that have matching records in the other table.
  - `LEFT JOIN`: Return all records from the "left" table and only those from the other table that match.
  - `RIGHT JOIN`: The same as a _LEFT JOIN_, but from the _RIGHT_ instead.
  - `LEFT OUTER JOIN`: Return only records from the first table that don't match anything in the second table.
  - `RIGHT OUTER JOIN`: The same as _LEFT OUTER JOIN_, but from the _RIGHT_ instead.
  - `FULL OUTER JOIN`: Return only records from both tables that have no matches in the other table.

```sql
-- basic INNER JOIN
SELECT *
FROM table_one
INNER JOIN table_two
ON table_one.id = table_two.table_one_id;

-- since it is the default, you don't have to specify "INNER"
SELECT *
FROM table_one
JOIN table_two
ON table_one.id = table_two.table_one_id;
```

### SELECT Challenges

For the first 5 queries, we'll be using the `users` table.

![users table](https://andydlindsay-portfolio.s3.amazonaws.com/lighthouse/w5d1-users.io.png)

1. List total number of users

```sql
SELECT COUNT(*)
FROM users;
```

2. List users over the age of 18

```sql
SELECT *
FROM users
WHERE age > 18;
```

3. List users who are over the age of 18 and have the last name Barrows

```sql
-- this query does whatever
SELECT *
FROM users
WHERE age > 18 AND last_name = 'Barrows';
```

4. List users over the age of 18 who live in Canada sorted by age from oldest to youngest and then last name alphabetically

```sql
SELECT *
FROM users
WHERE age > 18 AND country = 'Canada'
ORDER BY age DESC, last_name;
```

5. List users who live in Canada and whose accounts are overdue

```sql
SELECT *
FROM users
WHERE country = 'Canada' AND payment_due_date < NOW();
```

For the rest of the queries, we'll be using the `albums` and `songs` tables.

![albums and songs](https://andydlindsay-portfolio.s3.amazonaws.com/lighthouse/albums-and-songs.png)

6. List all albums along with their songs

```sql
SELECT *
FROM albums
JOIN songs
ON albums.id = songs.album_id;
```

7. List all albums along with how many songs each album has

```sql
SELECT album_name, COUNT(songs.id)
FROM albums
JOIN songs
ON albums.id = songs.album_id
GROUP BY album_name;
```

8. Enhance previous query to only include albums that have more than 10 songs

```sql
SELECT album_name, COUNT(songs.id) AS num_songs
FROM albums
JOIN songs
ON albums.id = songs.album_id
GROUP BY album_name 
HAVING COUNT(songs.id) > 10;
```

9. List _ALL_ albums in the database, along with their songs if any

```sql
SELECT *
FROM albums
LEFT JOIN songs
ON albums.id = songs.album_id;
```

10. List albums along with average song rating

```sql
SELECT album_name, AVG(songs.rating) AS average_rating
FROM albums
LEFT JOIN songs
ON albums.id = songs.album_id
GROUP BY album_name;
```

11. List albums and songs with rating higher than album average

```sql
SELECT album_name,
  songs.song_name,
  songs.rating,
  (SELECT AVG(songs.rating) FROM songs WHERE songs.album_id = albums.id) as avg_rating
FROM albums
LEFT JOIN songs
ON albums.id = songs.album_id
WHERE songs.rating > (SELECT AVG(songs.rating) FROM songs WHERE songs.album_id = albums.id);
```

### Useful Links
- [Top 10 Most Popular RDBMSs](https://www.c-sharpcorner.com/article/what-are-the-most-popular-relational-databases/)
- [Another Ranking of DBMSs](https://db-engines.com/en/ranking)
- [SELECT Queries Order of Execution](https://sqlbolt.com/lesson/select_queries_order_of_execution)
- [SQL Joins Visualizer](https://sql-joins.leopard.in.ua/)
