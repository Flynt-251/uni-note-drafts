# 1 - Basic Concepts in SQL

In this section, the basic commands of SQL are listed, separated based on what describes it as a **Data Definition Language** (DDL), and a **Data Manipulation Language** (DML). If you're struggling with any of these concepts, I would suggest playing around with them using a DBMS package of your choice (e.g. Postgresql or MySQL).

## Data Definition Statements

### CREATE SCHEMA

```sql
CREATE SCHEMA GAMES AUTHORIZATION 'flynt';
```

Creates a new schema, or database. (Note that between DBMSs, there may also exist `CREATE DATABASE`, which functions slightly differently).

### CREATE TABLE

```sql
CREATE TABLE DISCS (
    serialNum   VARCHAR(64) NOT NULL,
    title       VARCHAR(64) NOT NULL,
    genre       VARCHAR(16),
    developer   VARCHAR(64),
    publisher   VARCHAR(64) NOT NULL,
    releaseYear INT         NOT NULL CHECK (releaseYear > 1980),
    platform    VARCHAR(32) NOT NULL,
    loc         VARCHAR(16) NOT NULL DEFAULT "On Shelf",
    region      CHAR(1),
    PRIMARY KEY (serialNum),
    UNIQUE (title),
    FOREIGN KEY (platform) REFERENCES CONSOLES(name)
);
```

Creates a new table with the given attributes (aka columns). Below are the different data types you can typically define, but this can vary by DBMS.

```
CHAR(n)            Fixed-length string
VARCHAR(n)         Variable length string
NUMERIC(p,s)       Decimal values with custom precision (p) and scale (s)
INT                Integer Values
FLOAT              For Storing Decimal Values
DOUBLE PRECISION   Similar to FLOAT, but allows for greater scale and precision
DATE               Date values
DATETIME           Combined date and time value (e.g. in UNIX time)
BOOLEAN            True or False
```

### DROP

*the funny one*

```sql
DROP TABLE DISCS;
```

```sql
DROP SCHEMA GAMES;
```

Removes the specified table or schema, respectively. Use with caution (unless you're specifically not doing so, because you're being a silly goose :3).

## Data Manipulation Statements

### SELECT

```sql
SELECT title, publisher
FROM DISCS
WHERE platform = "Wii U" AND genre = "platformer"
ORDER BY title;
```

Output a table, given a set of desired attributes, the source table(s) and constraints. Below is an example output for this query:

|title                  |publisher|
|-----------------------|---------|
|New Super Mario Bros. U|Nintendo |
|Super Mario 3D World   |Nintendo |
|Super Mario 3D World   |Nintendo |

Wait, why do we have two copies of Super Mario 3D World? There may be multiple entries because two discs have different region codes, for example. We can filter out duplicates using `DISTINCT`.

```sql
SELECT DISTINCT title, publisher FROM DISCS WHERE ... ;
```

Under the `WHERE` statement, comparisons can be made using =, <, >, <=, >= and <> (not equal) between attributes and constants (or other attributes). To detect a substring, you can use `LIKE(s)`. Using `SELECT *` will output all attributes.

### INSERT

```sql
INSERT INTO DISCS VALUES (
    'RVL-RMCP-EUR', 'Mario Kart Wii', 'racing',
    'Nintendo EAD', 'Nintendo', 2008,
    'Wii', default, 'E');
```

Inserts a record into the specified table, with the set parameters, which are verified by the DBMS against the already-defined constraints, e.g. unique primary key, existence of foreign key, etc.

*Partial Records* can be inserted by specifying which values are defined.

```sql
INSERT INTO DISCS (serialNum, title, publisher, releaseYear, platform, loc) ... ;
```

Any undefined values are either set to `NULL` or their specified default value. In the above example, we only define values that are `NOT NULL` and that do not have a default.

### DELETE

```sql
DELETE FROM DISCS WHERE platform = 'Xbox One' OR platform = 'Xbox 360';
```

Remove all records that satisfy the constraints given. Leaving out the `WHERE` clause empties out the entire table.

### UPDATE

```sql
UPDATE DISCS
SET platform = 'GameCube', region = 'A'
WHERE platform = 'Codename Dolphin';
```

Modify a set of records that satisfy the constraints, with the specified new data.