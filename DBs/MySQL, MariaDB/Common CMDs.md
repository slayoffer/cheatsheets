### Add read only user

```bash
GRANT SELECT ON *.* TO 'readonlyuser'@'localhost' IDENTIFIED BY 'password'; # only SELECT permissions
FLUSH PRIVILEGES;
```

### Create DB

```bash
CREATE DATABASE mysampledb;

SHOW DATABASES;
```

### List users

```bash
SELECT HOST, USER, PASSWORD FROM mysql.user;
```

### Create user

```sql
CREATE USER 'john'@'localhost' IDENTIFIED BY 'MyNewPass4!'; # will work for connection only from localhost

# for all IPs access
CREATE USER 'john'@'%' IDENTIFIED BY 'MyNewPass4!';
```

### Give access to DB

```bash
# create user, password and grant permissions
GRANT SELECT ON mysampledb.* TO 'appuser'@'localhost' IDENTIFIED BY 'password';

# for ful access
GRANT ALL ON mysampledb.* TO 'appuser'@'localhost' IDENTIFIED BY 'password';
```

### Delete user

```bash
DROP USER 'myuser'@'localhost';
```

### Select DB

```bash
USE mysampledb;
```

### Create table

```sql
> mysql> CREATE TABLE persons
(
	Name varchar(255),
	Age int,
	Location varchar(255)
);

SHOW TABLES;

# or
CREATE TABLE Employess (Name char(15), Age int(3), Occupation char(15));
SHOW COLUMNS IN Employees;
```

### Insert into table

```sql
INSERT INTO persons values
( "John Doe", 45, "New York");

SELECT * FROM persons;
```

### Insert data

```bash
INSERT INTO Employees VALUES ('Joe Smith', 26, 'Ninja');
SELECT * FROM Employees;
```

### Delete data

```bash
DELETE FROM Employees WHERE Name = 'Joe Smith';
```

### Delete table or DB

```bash
DROP TABLE Employees;

DROP DATABASE mysampledb;
```

### Give rights to user

```sql
# check rights
SHOW GRANTS FOR 'john'@'localhost';

# for all rights access
GRANT ALL PRIVILEGES ON <DB.TABLE> TO 'john'@'%';

# for one right
GRANT SELECT ON school.users TO 'john'@'%';

# for several rights
GRANT SELECT, UPDATE ON school.users TO 'john'@'%';

# for all tables
GRANT SELECT, UPDATE ON school.* TO 'john'@'%';

# for godlike
GRANT ALL PRIVILEGES ON *.* TO 'john'@'%';

# DONT FORGET
FLUSH PRIVILEGES;
```
