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

### 제약조건

```sql
CREATE TABLE table_name (
    column1 datatype 제약조건,
    column2 datatype 제약조건,
    column3 datatype 제약조건,
    ....
);
```

**자주 쓰이는 SQL 제약조건s**

* `NOT NULL` - 필드의 값은 NULL을 가질 수 없다
* `UNIQUE` - 필드 안의 모든 값은 서로 다른 값을 가져야 한다
* `PRIMARY KEY` - `NOT NULL` 과  `UNIQUE`의 컴비네이션, 테이블의 각 데이터를 고유하게 식별한다
* `FOREIGN KEY` - 다른 테이블의 데이터를 고유하게 식별한다
* `CHECK` - 필드의 모든 값이 특정 조건을 만족하는지 확인한다
* `DEFAULT` - 값이 입력되지 않을 때 기본값을 설정한다
* `INDEX` - 빠른 속도로 데이터를 생성하고 검색하는 데 사용된다

### NOT NULL 제약조건

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255) NOT NULL,
    Age int
);
```

### UNIQUE 제약조건


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

여러 필드 제약조건 지정

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    제약조건 UC_Person UNIQUE (ID,LastName)
);
```

#### on ALTER TABLE

```sql
ALTER TABLE Persons
ADD UNIQUE (ID);
```

```sql
ALTER TABLE Persons
ADD 제약조건 UC_Person UNIQUE (ID,LastName);
```

#### DROP UNIQUE 제약조건

MySQL:

```sql
ALTER TABLE Persons
DROP INDEX UC_Person;
```

SQL Server / Oracle / MS Access:

```sql
ALTER TABLE Persons
DROP 제약조건 UC_Person;
```

### PRIMARY KEY 제약조건

* `PRIMARY KEY` 제약조건는 데이터베이스 테이블의 각 레코드를 고유하게 식별한다


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

여러 필드 제약조건 지정

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    제약조건 PK_Person PRIMARY KEY (ID,LastName)
);
```

#### on ALTER TABLE

MySQL / SQL Server / Oracle / MS Access:

```sql
ALTER TABLE Persons
ADD PRIMARY KEY (ID);
```

여러 필드 제약조건 지정

```sql
ALTER TABLE Persons
ADD 제약조건 PK_Person PRIMARY KEY (ID,LastName);
```

> ALTER TABLE로 PRIMARY KEY를 추가할 때는 테이블이 처음 생성될 때 NULL 값이 추가되지 않도록 선언되어야 함

#### DROP PRIMARY KEY 제약조건

MySQL:

```sql
ALTER TABLE Persons
DROP PRIMARY KEY;
```

SQL Server / Oracle / MS Access:

```sql
ALTER TABLE Persons
DROP 제약조건 PK_Person;
```


### FOREIGN KEY 제약조건

* `FOREIGN KEY`는 두개의 테이블을 연결할 때 쓰이는 키이다
* `FOREIGN KEY`는 다른 테이블의 `PRIMARY KEY`를 참조한다
* `FOREIGN KEY`를 가지고 있는 테이블을 `Child Table`이라고 부르며,
* `Candidate Key`를 가지고 있는 테이블은 참조된 테이블 또는 `Parent Table`이라고 부른다

#### on CREATE TABLE

MySQL:

```sql
CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);
```

SQL Server / Oracle / MS Access:

```sql
CREATE TABLE Orders (
    OrderID int NOT NULL PRIMARY KEY,
    OrderNumber int NOT NULL,
    PersonID int FOREIGN KEY REFERENCES Persons(PersonID)
);
```

여러 필드 제약조건 지정

```sql
CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    제약조건 FK_PersonOrder FOREIGN KEY (PersonID)
    REFERENCES Persons(PersonID)
);
```

#### on ALTER TABLE

MySQL / SQL Server / Oracle / MS Access:

```sql
ALTER TABLE Orders
ADD FOREIGN KEY (PersonID) REFERENCES Persons(PersonID);
```

여러 필드 제약조건 지정

```sql
ALTER TABLE Orders
ADD 제약조건 FK_PersonOrder
FOREIGN KEY (PersonID) REFERENCES Persons(PersonID);
```

#### DROP a FOREIGN KEY 제약조건

MySQL:

```sql
ALTER TABLE Orders
DROP FOREIGN KEY FK_PersonOrder;
```

SQL Server / Oracle / MS Access:

```sql
ALTER TABLE Orders
DROP 제약조건 FK_PersonOrder;
```

### CHECK 제약조건

* `CHECK` 제약조건는 필드에 넣을 수 있는 값의 범위를 제한할 때 사용된다

#### on CREATE TABLE

MySQL:

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CHECK (Age>=18)
);
```

SQL Server / Oracle / MS Access:

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int CHECK (Age>=18)
);
```

여러 필드 제약조건 지정

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    City varchar(255),
    제약조건 CHK_Person CHECK (Age>=18 AND City='Sandnes')
);
```

#### on ALTER TABLE

```sql
ALTER TABLE Persons
ADD CHECK (Age>=18);
```

여러 필드 제약조건 지정

```sql
ALTER TABLE Persons
ADD 제약조건 CHK_PersonAge CHECK (Age>=18 AND City='Sandnes');
```

#### DROP CHECK 제약조건

SQL Server / Oracle / MS Access:

```sql
ALTER TABLE Persons
DROP 제약조건 CHK_PersonAge;
```

MySQL:

```sql
ALTER TABLE Persons
DROP CHECK CHK_PersonAge;
```

### DEFAULT 제약조건

* `DEFAULT` 제약조건은 필드의 기본 값을 제공할 때 사용된다

My SQL / SQL Server / Oracle / MS Access:

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    City varchar(255) DEFAULT 'Sandnes'
);
```

* `DEFAULT` 제약조건은 GETDATE() 같은 함수를 사용해 시스템 값을 삽입할 때도 사용된다

```sql
CREATE TABLE Orders (
    ID int NOT NULL,
    OrderNumber int NOT NULL,
    OrderDate date DEFAULT GETDATE()
);
```

#### on ALTER TABLE

MySQL:

```sql
ALTER TABLE Persons
ALTER City SET DEFAULT 'Sandnes';
```

SQL Server / MS Access:

```sql
ALTER TABLE Persons
ALTER COLUMN City SET DEFAULT 'Sandnes';
```

Oracle:

```sql
ALTER TABLE Persons
MODIFY City DEFAULT 'Sandnes';
```

#### DROP DEFAULT 제약조건

MySQL:

```sql
ALTER TABLE Persons
ALTER City DROP DEFAULT;
```

SQL Server / Oracle / MS Access:

```sql
ALTER TABLE Persons
ALTER COLUMN City DROP DEFAULT;
```

### CREATE INDEX 명령어

* `CREATE INDEX` 명령어는 테이블에 인덱스를 만드는 데 사용된다

```sql
CREATE INDEX idx_pname
ON Persons (LastName, FirstName);
```

#### CREATE UNIQUE INDEX

* `CREATE UNIQUE INDEX`에서는 중복값이 허용되지 않는다

```sql
CREATE UNIQUE INDEX index_name
ON table_name (column1, column2, ...);
``` 

#### DROP INDEX 명령어

MySQL:

```sql
ALTER TABLE table_name
DROP INDEX index_name;
```

SQL Server:

```sql
DROP INDEX table_name.index_name;
```

DB2/Oracle:

```sql
DROP INDEX table_name.index_name;
```

### AUTO INCREMENT 필드

* `AUTO_INCREMENT`는 새로운 데이터가 테이블에 삽입되었을 때 고유번호가 자동으로 생성될 수 있도록 한다

MySQL:

```sql
CREATE TABLE Persons (
    ID int NOT NULL AUTO_INCREMENT,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    PRIMARY KEY (ID)
);
```

> 기본적으로 `AUTO_INCREMENT`의 시작은 `1`이다
> 다른 숫자로 시작하려면 다음 SQL문을 사용한다

```sql
ALTER TABLE Persons AUTO_INCREMENT=100;
```

Oracle:

```sql
CREATE SEQUENCE seq_person
MINVALUE 1
START WITH 1
INCREMENT BY 1
CACHE 10;
```

> 오라클에서는 시퀀스 객체를 통해 AUTO_INCREMENT 필드를 생성해주어야 한다

### SQL 날짜

#### SQL 날짜 데이터 유형

MySQL:

```sql
DATE - format YYYY-MM-DD
DATETIME - format: YYYY-MM-DD HH:MI:SS
TIMESTAMP - format: YYYY-MM-DD HH:MI:SS
YEAR - format YYYY or YY
```

SQL Server:

```sql
DATE - format YYYY-MM-DD
DATETIME - format: YYYY-MM-DD HH:MI:SS
SMALLDATETIME - format: YYYY-MM-DD HH:MI:SS
TIMESTAMP - format: a unique number
```

날짜 SELECT

```sql
SELECT * FROM Orders WHERE OrderDate='2008-11-11'
```

> 날짜에 시간 요소가 들어가 있을 시에는 위 구문으로 결과를 얻을 수 없다 (e.g. 2008-11-11 13:23:44)
> 쿼리를 간단하고 쉽게 유지하기 위해서는 시간 요소를 허용하지 않는 것이 중요하다

### VIEW

* SQL에서 `VIEW`는 SQL 문의 결과 셋을 기반으로하는 가상 테이블이다

#### CREATE VIEW 명령어

```sql
CREATE VIEW [Current Product List] AS
SELECT ProductID, ProductName
FROM Products
WHERE Discontinued = No;
```

> 뷰를 만들어 준 이후 다음 구문으로 쿼리값을 받아올 수 있다

```sql
SELECT * FROM [Current Product List];
```

#### UPDATING a VIEW

```sql
CREATE OR REPLACE VIEW [Current Product List] AS
SELECT ProductID, ProductName, Category
FROM Products
WHERE Discontinued = No;
```

#### DROP a VIEW

```sql
DROP VIEW view_name;
```
