> Tested on 'MySQL' and 'SQLite'

- [Table Structure](#table-structure)
  - [Create new Table](#create-new-table)
- [CRUD](#crud)

# Table Structure

## Create new Table

```sql
CREATE TABLE celebs (id INTEGER, name TEXT, age INTEGER);
```

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
