---
layout: post
title:  "SQL 데이터베이스"
categories: SQL
---

# SQL 데이터베이스

## SQL 데이터베이스 명령어

### CREATE DB 명령어

```sql
CREATE DATABASE testDB;
```

### DROP DB 명령어

```sql
DROP DATABASE testDB;
```

> 데이터베이스의 모든 정보를 날리기 때문에 DROP 할 시에는 주의가 필요함

### CREATE TABLE 명령어

```sql
CREATE TABLE Persons (
    PersonID int,
    LastName varchar(255),
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255) 
);
```

Persons의 항목들을 지정하고 테이블을 생성한다

```sql
CREATE TABLE new_table_name AS
    SELECT column1, column2,...
    FROM existing_table_name
    WHERE ....;
```

존재하는 다른 테이블을 사용해 새로운 테이블을 생성한다

### DROP TABLE 명령어

```sql
DROP TABLE tablename;
```

### TRUNCATE TABLE 	명령어

```sql
TRUNCATE TABLE tablename;
```

`TRUNCATE TABLE` 명령어는 테이블 자체는 지우지 않고 테이블 안에 있는 데이터만 지운다

### ATLER TABLE 명령어

```sql
ALTER TABLE tablename
ADD column_name datatype;
```

`ADD` 명령어는 필드를 추가할 때 사용된다

```sql
ALTER TABLE tablename
DROP COLUMN column_name;
```

`DROP COLUMN` 명령어는 필드를 삭제할 때 쓰인다

#### ALTER TABLE - ALTER / MODIFY COLUMN

`ALTER / MODIFY COLUMN` 명령어는 필드의 데이터 타입을 바꿀 때 사용된다

SQL Server / MS Access

```sql
ALTER TABLE tablename
ALTER COLUMN column_name datatype;
```

MySQL / Oracle (10G 이전 versions)

```sql
ALTER TABLE tablename
MODIFY COLUMN column_name;
```

Oracle 10G 이후 버전

```sql
ALTER TABLE tablename
MODIFY column_name;
```

### CONSTRAINTS

```sql
CREATE TABLE table_name (
    column1 datatype constraint,
    column2 datatype constraint,
    column3 datatype constraint,
    ....
);
```

**자주 쓰이는 SQL Constraints**

* `NOT NULL` - 필드의 값은 NULL을 가질 수 없다
* `UNIQUE` - 필드 안의 모든 값은 서로 다른 값을 가져야 한다
* `PRIMARY KEY` - `NOT NULL` 과  `UNIQUE`의 컴비네이션, 테이블의 각 데이터를 고유하게 식별한다
* `FOREIGN KEY` - 다른 테이블의 데이터를 고유하게 식별한다
* `CHECK` - 필드의 모든 값이 특정 조건을 만족하는지 확인한다
* `DEFAULT` - 값이 입력되지 않을 때 기본값을 설정한다
* `INDEX` - 빠른 속도로 데이터를 생성하고 검색하는 데 사용된다

### NOT NULL Constraint

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255) NOT NULL,
    Age int
);
```

### UNIQUE Constraint


#### on CREATE TABLE

SQL Server / Oracle / MS Access:

```sql
CREATE TABLE Persons (
    ID int NOT NULL UNIQUE,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int
);
```

MySQL:

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    UNIQUE (ID)
);
```

여러 필드 Constraint 지정

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CONSTRAINT UC_Person UNIQUE (ID,LastName)
);
```

#### on ALTER TABLE

```sql
ALTER TABLE Persons
ADD UNIQUE (ID);
```

```sql
ALTER TABLE Persons
ADD CONSTRAINT UC_Person UNIQUE (ID,LastName);
```

### DROP a UNIQUE Constraint

MySQL:

```sql
ALTER TABLE Persons
DROP INDEX UC_Person;
```

SQL Server / Oracle / MS Access:

```sql
ALTER TABLE Persons
DROP CONSTRAINT UC_Person;
```

### PRIMARY KEY Constraint

#### on CREATE TABLE

MySQL:

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    PRIMARY KEY (ID)
);
```

SQL Server / Oracle / MS Access:

```sql
CREATE TABLE Persons (
    ID int NOT NULL PRIMARY KEY,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int
);
```

여러 필드 Constraint 지정

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CONSTRAINT PK_Person PRIMARY KEY (ID,LastName)
);
```

#### on ALTER TABLE

MySQL / SQL Server / Oracle / MS Access:

```sql
ALTER TABLE Persons
ADD PRIMARY KEY (ID);
```

여러 필드 Constraint 지정

```sql
ALTER TABLE Persons
ADD CONSTRAINT PK_Person PRIMARY KEY (ID,LastName);
```

> ALTER TABLE로 PRIMARY KEY를 추가할 때는 테이블이 처음 생성될 때 NULL 값이 추가되지 않도록 선언되어야 함

### DROP a PRIMARY KEY Constraint

MySQL:

```sql
ALTER TABLE Persons
DROP PRIMARY KEY;
```

SQL Server / Oracle / MS Access:

```sql
ALTER TABLE Persons
DROP CONSTRAINT PK_Person;
```
