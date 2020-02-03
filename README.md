# W5D1 - SQL Intro

### To Do
- [ ] Introduction to RDBMS
- [ ] The Relational Data Model (Tables, Columns, Rows)
- [ ] SELECT Statements
- [ ] Filtering, ordering, limiting, etc.
- [ ] Joining tables
- [ ] Grouping records
- [ ] Aggregate functions

### RDBMS
- Relational DataBase Management System - software/server
- Postgres
- SQLite
- MySQL
- MariaDB


- psql - Postgres REPL

### Protocol
- postgres:// 5432
- Client <--- HTTP/TCP ---> Server <--- postgres/TCP ---> Postgres RDBMS



# 20% Chance that everything will crash ACROSS THE BOARD

### Clauses
- SELECT FROM

### Primary Keys
- Unique identifier for a table (unique within the table)
- People have the same names, attributes

- Foreign Key === primary key stored in another table


# It's declarative!
- imperative







### SELECT Challenges

For the first 5 queries, we'll be using the `users` table.

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
