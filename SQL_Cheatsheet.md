> Tested on 'MySQL' and 'SQLite'

- [Table Structure](#table-structure)
  - [Create Table](#create-table)
  - [Create Table  with Constraints](#create-table-with-constraints)
  - [Add new Column to existing Table](#add-new-column-to-existing-table)
- [CRUD](#crud)
  - [Insert](#insert)
  - [Select](#select)
  - [Update](#update)
  - [Delete](#delete)
- [Links](#Links)
  - [SQLite](#sqLite)

# Table Structure

## Create Table

```sql
CREATE TABLE celebs (id INTEGER, name TEXT, age INTEGER);
```

## Create Table with Constraints

```sql
CREATE TABLE celebs (
    id INTEGER PRIMARY KEY,
    name TEXT UNIQUE,
    date_of_birth TEXT NOT NULL,
    date_of_death TEXT DEFAULT 'Not Applicable',
);
```

| Name              | Characteristics                  |
| :---------------- | :------------------------------- |
| PRIMARY KEY       | Only one per table, Unique       |
| UNIQUE            | Unique value                     |
| NOT NULL          | Must have a value                |
| DEFAULT           | Set if no Value given            |


## Add new Column to existing Table

```sql
ALTER TABLE celebs ADD COLUMN twitter_handle TEXT;
```

# CRUD

## Insert

```sql
INSERT INTO celebs (id, name, age) VALUES (1, 'Justin Bieber', 21);
INSERT INTO celebs (id, name, age) VALUES (2, 'Beyonce Knowles', 33);
INSERT INTO celebs (id, name, age) VALUES (3, 'Jeremy Lin', 26);
INSERT INTO celebs (id, name, age) VALUES (4, 'Taylor Swift', 26);
```

## Select

```sql
SELECT name FROM celebs;
```

## Update

```sql
UPDATE celebs
SET age = 22
WHERE id = 1;
```

## Delete

```sql
DELETE FROM celebs WHERE twitter_handle IS NULL;
```

# Links

## SQLite

http://tech.marksblogg.com/sqlite3-tutorial-and-guide.html
