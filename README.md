# MySQL

## Relational Database Management System

Tables in relational database can be linked together
RDBMS is What we use to access and interact with the relational database

```SQL
SHOW DATABASES -- use to show all databases in the database
CREATE DATABASE test -- to create a new database with name 'test'
USE test -- chosse the database with name 'test' and query with it
SHOW TABLE -- show the tables in the database
DROP DATABASE test -- to drop the database with name 'test'
```

## Data Definition Language

### Data Types

NUMERIC DATA TYPES:

-   INT: Whole numbers
-   FLOAT(M,D): Decimal number (approximately)
-   DECIMAL(M,D): Decimal number (precise)

NON-NUMBER DATA TYPES:

-   CHAR(N): Fixed length character
-   VARCHAR(N): Varying length character
-   ENUM('M', 'F'): Value from a defined list
-   BOOLEAN: True of False values

DATE AND TIME TYPES:

-   DATE: Date (YYYY-MM-DD)
-   DATETIME: Date and time (YYYY-MM-DD HH-MM-SS)
-   TIME: Time (HHH-MM-SS)
-   YEAR: Year (YYYY)

### Primary and Foreign Keys

Primary key:

-   A primary key is a column, or set of columns, which uniquely identifies a record within a table.
-   A primary key must be unique.
-   A primary key can not be NULL.
-   A table can only have one primary key.
    Foreign key:
-   A foreign key is used to link two tables together.
-   A foreign key is a column whose values match the values of another table primary key column.
-   The table with the primary key is called the reference, or parent, table and the table with the foreign key is called the child table.
-   A table can have multiple foreign keys

### Creating the Coffee Store Database

```SQL
SHOW DATABASES;
CREATE DATABASE coffee_store;
USE coffee_store;
CREATE TABLE products(
	id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(30),
    PRICE DECIMAL(3,2)
);
CREATE TABLE customers(
	id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(30),
    last_name VARCHAR(30),
    gender ENUM("M", "F"),
    phone_number VARCHAR(11)
);
CREATE TABLE orders(
	id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT,
    customer_id INT,
    order_time DATETIME,
    FOREIGN KEY (product_id) REFERENCES products(id),
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
SHOW TABLES;
SELECT * FROM products
```

### Modifying Tables: Adding and Removing Columns

```SQL
SELECT * FROM products;
ALTER TABLE products ADD COLUMN coffee_origin VARCHAR(30);
ALTER TABLE products DROP COLUMN coffee_origin;
```

### Deleting Tables

```SQL
CREATE DATABASE example;
USE example;
CREATE TABLE test(
	id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(30),
    age INT
);
SELECT * FROM test;
SHOW TABLES;

DROP TABLE test;
DROP DATABASE example
```

### Truncating Tables

```SQL
CREATE DATABASE example;

USE example;

CREATE TABLE test(
	id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(30),
    age INT
);

SELECT * FROM test;

SHOW TABLES;

TRUNCATE TABLE test;
```

## More on alter table

### Creating Our Test Database

```SQL
CREATE DATABASE test;

USE test;

CREATE TABLE addresses(
    id INT,
    house_number INT,
    city VARCHAR(20),
    postcode VARCHAR(7)
);

CREATE TABLE people(
	id INT,
    first_name VARCHAR(20),
    last_name VARCHAR(20),
    address_id INT
);

CREATE TABLE pets(
	id INT,
    name VARCHAR(20),
    species VARCHAR(20),
    owner_id INT
);

SHOW TABLES;
```

### Add and Remove Primary Key

```SQL
USE test;

-- SQL TO ADD A PRIMARY KEY TO A TABLE

ALTER TABLE <tablename> ADD PRIMARY KEY (columnname);
-- SQL TO REMOVE A PRIMARY KEY FROM A TABLE

ALTER TABLE <tablename> DROP PRIMARY KEY;

DESCRIBE addresses;

ALTER TABLE addresses ADD PRIMARY KEY (id);

ALTER TABLE addresses DROP PRIMARY KEY;
```

### Add and Remove Foreign Key

```SQL
USE test;

-- SQL TO ADD A FOREIGN KEY TO A TABLE

-- ALTER TABLE <tablename> ADD CONSTRAINT <constraintname> FOREIGN KEY (columnname) REFERENCES <tablename>(columnname);
-- SQL TO REMOVE A FOREIGN KEY FROM A TABLE

-- ALTER TABLE <tablename> DROP FOREIGN KEY <constraintname>;

DESCRIBE addresses;
DESCRIBE people;

ALTER TABLE addresses ADD PRIMARY KEY (id);
ALTER TABLE people ADD PRIMARY KEY (id);
ALTER TABLE people ADD CONSTRAINT FK_PeopleAddress FOREIGN KEY (address_id) REFERENCES addresses(id);
ALTER TABLE people DROP FOREIGN KEY FK_PeopleAddress;
```

### Add Unique Constrain

```SQL
USE test;

-- HOW TO ADD A UNIQUE CONSTRAINT TO COLUMN
-- ALTER TABLE <tablemane> ADD CONSTRAINT <constraintname> UNIQUE (columnname);
-- HOW TO REMOVE A UNIQUE CONSTRAIN FROM A COLUMN
-- ALTER TABLE <tablename> DROP INDEX <constraintname>
DESCRIBE pets;
ALTER TABLE pets ADD CONSTRAINT u_species UNIQUE (species);
ALTER TABLE pets DROP INDEX u_species;
```

### Change Column Name

```SQL
USE test;
-- ALTER TABLE <tablename> CHANGE `old_column_name` `new_column_name` <data type>;
SELECT * FROM pets;
ALTER TABLE pets CHANGE `species` `animal_type` VARCHAR(20);
ALTER TABLE pets CHANGE `animal_type` `species` VARCHAR(20);
```

### Change Column Data Type

```SQL
USE test;
-- HOW TO CHANGE A COLUMNS DATA TYPE
-- ALTER TABLE <tablename> MODIFY <columnname> <data type>;
DESCRIBE addresses;
ALTER TABLE addresses MODIFY city VARCHAR(20);
```

### Exercise 1

-   Add a primary key to the id fields in the pets and people tables.
-   Add foreign key to the owner_id field in the pets table referencing id field in the people table.
-   Add a column name email to the people table.
-   Add a unique constraint to the email column in the people table.
-   Rename the name column in the pets table to "first_name".
-   Change the postcode data type to CHAR(7) in the addresses table.

```SQL
USE test;

-- 1.Add a primary key to the id fields in the pets and people tables.
DESCRIBE pets;
ALTER TABLE peole ADD PRIMARY KEY (id);
ALTER TABLE pets ADD PRIMARY KEY (id);
-- 2.Add foreign key to the owner_id field in the pets table referencing id field in the people table.
DESCRIBE pets;
ALTER TABLE pets ADD CONSTRAINT FK_owner FOREIGN KEY (owner_id) REFERENCES people(id);
-- 3.Add a column name email to the people table.
DESCRIBE people;
ALTER TABLE people ADD COLUMN email VARCHAR(30);
-- 4.Add a unique constraint to the email column in the people table.
DESCRIBE people;
ALTER TABLE people ADD CONSTRAINT u_email UNIQUE (email);
-- 5.Rename the name column in the pets table to "first_name".
DESCRIBE pets;
ALTER TABLE pets CHANGE `name` `first_name` VARCHAR(20);
-- 6.Change the postcode data type to CHAR(7) in the addresses table.
DESCRIBE addresses;
ALTER TABLE addresses MODIFY postcode CHAR(7);
```

## Data Manipulation Language

### Inserting Data Into Tables

```SQL
USE coffee_store;
SHOW TABLES;
SELECT * FROM products;
-- INSERT INTO <tablename> (<column1>, <column2>, <column3>) VALUES ("value1", "value2", "value3")
INSERT INTO products (name, price, coffee_origin) VALUES ('Espresso', 2.50, 'Brazil');

INSERT INTO products (name, price, coffee_origin) VALUES ('Macchiato', 3.00, 'Brazil'), ('Cappuccino',3.50,'Costa Rica');

INSERT INTO products (name, price, coffee_origin) VALUES ('Latte', 3.50, 'Indonesia'), ('Americano',3.00,'Brazil'), ('Flat White',3.50,'Indonesia'), ('Filter',3.00,'India');
```

### Updating Data in Tables

```SQL
USE coffee_store;
SELECT * FROM products;
-- UPDATE <tablename> SET <columnname> = 'value' WHERE <columnname> = 'value'

UPDATE products SET coffee_origin = 'Sri Lanka' WHERE id = 7;
SET SQL_SAFE_UPDATES = 0;

UPDATE products SET price = 3.25, coffee_origin = 'Ethiopia' WHERE name = 'Americano';

UPDATE products SET coffee_origin = 'Colombia' WHERE coffee_origin = 'Brazil'
```

### Deleting Data from Tables

```SQL
DELETE FROM <tablename> Where <columnname> = 'example'
```

### Completing the Coffee Store Database

```SQL
INSERT INTO orders (product_id,customer_id,order_time) VALUES (1,1,'2017-01-01 08:02:11'),(1,2,'2017-01-01 08:05:16'),(5,12,'2017-01-01 08:44:34'),(3,4,'2017-01-01 09:20:02'),(1,9,'2017-01-01 11:51:56'),(6,22,'2017-01-01 13:07:10'),(1,1,'2017-01-02 08:03:41'),(3,10,'2017-01-02 09:15:22'),(2,2,'2017-01-02 10:10:10'),(3,13,'2017-01-02 12:07:23'),(1,1,'2017-01-03 08:13:50'),(7,16,'2017-01-03 08:47:09'),(6,21,'2017-01-03 09:12:11'),(5,22,'2017-01-03 11:05:33'),(4,3,'2017-01-03 11:08:55'),(3,11,'2017-01-03 12:02:14'),(2,23,'2017-01-03 13:41:22'),(1,1,'2017-01-04 08:08:56'),(3,10,'2017-01-04 11:23:43'),(4,12,'2017-01-05 08:30:09'),(7,1,'2017-01-06 09:02:47'),(3,18,'2017-01-06 13:23:34'),(2,16,'2017-01-07 09:12:39'),(2,14,'2017-01-07 11:24:15'),(4,5,'2017-01-08 08:54:11'),(1,1,'2017-01-09 08:03:11'),(6,20,'2017-01-10 10:34:12'),(3,3,'2017-01-10 11:02:11'),(4,24,'2017-01-11 08:39:11'),(4,8,'2017-01-12 13:20:13'),(1,1,'2017-01-14 08:27:10'),(4,15,'2017-01-15 08:30:56'),(1,7,'2017-01-16 10:02:11'),(2,10,'2017-01-17 09:50:05'),(1,1,'2017-01-18 08:22:55'),(3,9,'2017-01-19 09:00:19'),(7,11,'2017-01-19 11:33:00'),(6,12,'2017-01-20 08:02:21'),(3,14,'2017-01-21 09:45:50'),(5,2,'2017-01-22 10:10:34'),(6,24,'2017-01-23 08:32:19'),(6,22,'2017-01-23 08:45:12'),(6,17,'2017-01-23 12:45:30'),(2,11,'2017-01-24 08:01:27'),(1,1,'2017-01-25 08:05:13'),(6,11,'2017-01-26 10:49:10'),(7,3,'2017-01-27 09:23:57'),(7,1,'2017-01-27 10:08:16'),(3,18,'2017-01-27 10:13:09'),(4,19,'2017-01-27 11:02:40'),(3,10,'2017-01-28 08:03:21'),(1,2,'2017-01-28 08:33:28'),(1,12,'2017-01-28 11:55:33'),(1,13,'2017-01-29 09:10:17'),(6,6,'2017-01-30 10:07:13'),(1,1,'2017-02-01 08:10:14'),(2,14,'2017-02-02 10:02:11'),(7,10,'2017-02-02 09:43:17'),(7,20,'2017-02-03 08:33:49'),(4,21,'2017-02-04 09:31:01'),(5,22,'2017-02-05 09:07:10'),(3,23,'2017-02-06 08:15:10'),(2,24,'2017-02-07 08:27:26'),(1,1,'2017-02-07 08:45:10'),(6,11,'2017-02-08 10:37:10'),(3,13,'2017-02-09 08:58:18'),(3,14,'2017-02-10 09:12:40'),(5,4,'2017-02-10 11:05:34'),(1,2,'2017-02-11 08:00:38'),(3,8,'2017-02-12 08:08:08'),(7,20,'2017-02-12 09:22:10'),(1,1,'2017-02-13 08:37:45'),(5,2,'2017-02-13 12:34:56'),(4,3,'2017-02-14 08:22:43'),(5,4,'2017-02-14 09:12:56'),(3,5,'2017-02-15 08:09:10'),(6,7,'2017-02-15 09:05:12'),(1,8,'2017-02-15 09:27:50'),(2,9,'2017-02-16 08:51:12'),(3,10,'2017-02-16 13:07:46'),(4,11,'2017-02-17 08:03:55'),(4,12,'2017-02-17 09:12:11'),(5,10,'2017-02-17 11:41:17'),(6,18,'2017-02-17 13:05:56'),(7,19,'2017-02-18 08:33:27'),(1,17,'2017-02-19 08:12:31'),(1,1,'2017-02-20 09:50:17'),(3,5,'2017-02-20 09:51:29'),(4,6,'2017-02-20 10:43:39'),(3,1,'2017-02-21 08:32:17'),(1,1,'2017-02-21 10:30:11'),(3,2,'2017-02-21 11:08:45'),(4,3,'2017-02-22 11:46:32'),(2,15,'2017-02-22 13:35:16'),(6,13,'2017-02-23 08:34:48'),(4,24,'2017-02-24 08:32:03'),(2,13,'2017-02-25 08:03:12'),(7,17,'2017-02-25 09:34:23'),(7,23,'2017-02-25 11:32:54'),(5,12,'2017-02-26 11:47:34'),(6,4,'2017-02-27 12:12:34'),(1,1,'2017-02-28 08:59:22');

INSERT INTO customers (first_name, last_name, gender, phone_number) VALUES ('Chris','Martin','M','01123147789'),('Emma','Law','F','01123439899'),('Mark','Watkins','M','01174592013'),('Daniel','Williams','M',NULL),('Sarah','Taylor','F','01176348290'),('Katie','Armstrong','F','01145787353'),('Michael','Bluth','M','01980289282'),('Kat','Nash','F','01176987789'),('Buster','Bluth','M','01173456782'),('Charlie',NULL,'F','01139287883'),('Lindsay','Bluth','F','01176923804'),('Harry','Johnson','M',NULL),('John','Smith','M','01174987221'),('John','Taylor','M',NULL),('Emma','Smith','F','01176984116'),('Gob','Bluth','M','01176985498'),('George','Bluth','M','01176984303'),('Lucille','Bluth','F','01198773214'),('George','Evans','M','01174502933'),('Emily','Simmonds','F','01899284352'),('John','Smith','M','01144473330'),('Jennifer',NULL,'F',NULL),('Toby','West','M','01176009822'),('Paul','Edmonds','M','01966947113');}
```

## Selecting from a Table

### Select Statement

```SQL
USE coffee_store;

SELECT * FROM customers;

SELECT last_name FROM customers;

SELECT last_name, phone_number FROM customers;
```

### Where Clause

```SQL
USE coffee_store;

SELECT * FROM products;

SELECT * FROM products WHERE coffee_origin = "Colombia";

SELECT * FROM products WHERE price = 3.00;

SELECT * FROM products WHERE price = 3.00 AND coffee_origin = "Colombia";

SELECT * FROM products WHERE price = 3.00 OR coffee_origin = "Colombia";
```

### Using Inequality Symbols

```SQL
USE coffee_store;

/*
>- greater than
>=- greater than or equal to
<- less than
<= less than or equalo
*/

SELECT * FROM products;

SELECT * FROM products WHERE price > 3.00;
SELECT * FROM products WHERE price >= 3.00;
SELECT * FROM products WHERE price < 3.00;
SELECT * FROM products WHERE price <= 3.00;
```

### Null Values

```SQL
USE coffee_store;

SELECT * FROM customers WHERE phone_number IS NULL;

SELECT * FROM customers WHERE phone_number IS NOT NULL;
```

### Exercise 1

1. From the customers table, select the first name and phone number of all the females who have a last name of Bluth.
2. From the products table, select the name for all products that have a price greater than 3.00 or coffee origin of Sri Lanka.
3. How many male customers don't have a phone number entered intothe customers table?

### Solution 1

```SQL
USE coffee_store;
-- 1. From the customers table, select the first name and phone number of all the females who have a last name of Bluth.
SELECT * FROM customers;
SELECT first_name, phone_number FROM customers WHERE gender = "F" AND first_name = "Bluth";
-- 2. From the products table, select the name for all products that have a price greater than 3.00 or coffee origin of Sri Lanka.
SELECT * FROM products;
SELECT name From products WHERE price > 3.00 OR coffee_origin = 'Sri Lanka';
-- 3. How many male customers don't have a phone number entered intothe customers table?
SELECT * FROM customers;
SELECT * FROM customers WHERE gender = 'M' AND phone_number IS NULL;
SELECT COUNT(*) FROM customers WHERE gender = 'M' AND phone_number IS NULL;
```

### In, Not In

```SQL
USE coffee_store;
SELECT * FROM customers;
SELECT * FROM customers WHERE last_name IN('Taylor', 'Bluth', 'Armstrong');
SELECT * FROM customers WHERE first_name NOT IN('Katie', 'John', 'George');
```

### Between

```SQL
USE coffee_store;
SELECT * FROM orders;
SELECT * FROM customers;

SELECT product_id, customer_id, order_time From orders WHERE order_time BETWEEN '2017-01-01' AND '2017-01-07';
SELECT product_id, customer_id, order_time From orders WHERE customer_id BETWEEN 5 AND 10;
SELECT * FROM customers WHERE last_name BETWEEN 'A' AND 'L';
```

### Like

```SQL
USE coffee_store;

/*
% - any number of characters
  - just one character
*/
SELECT * FROM customers;
SELECT * FROM customers WHERE last_name LIKE 'W%';
SELECT * FROM customers WHERE last_name LIKE '%o%';
SELECT * FROM customers WHERE first_name LIKE '_o_';
SELECT * FROM products;
SELECT * FROM products WHERE price LIKE '3%';
```

### Order By

```SQL
USE coffee_store;

SELECT * FROM products;
SELECT * FROM products ORDER BY price ASC;
SELECT * FROM products ORDER BY price DESC;

SELECT * FROM customers;
SELECT * FROM customers ORDER BY last_name ASC;
SELECT * FROM customers ORDER BY last_name DESC;

SELECT * FROM orders;
SELECT * FROM orders WHERE customer_id = 1 ORDER BY order_time ASC;
SELECT * FROM orders WHERE customer_id = 1 ORDER BY order_time DESC;
```

### Exercise 2

1. From the products table, select the name and price of all products with a coffee origin equal to Colombia or Indonesia. Ordered by name form A-Z.
2. From the orders table, select all the orders from February 2017 for customers with id's of 2, 4, 6 or 8.
3. From the customers table, select the first name and phone number of all customers who's last name contains the pattern 'ar'

### Solution 2

```SQL
USE coffee_store;
-- 1. From the products table, select the name and price of all products with a coffee origin equal to Colombia or Indonesia. Ordered by name form A-Z.
SELECT * FROM products;
SELECT name, price FROM products WHERE coffee_origin IN('Colombia','Indonesia') ORDER BY name ASC;
-- 2. From the orders table, select all the orders from February 2017 for customers with id's of 2, 4, 6 or 8.
SELECT * FROM orders;
SELECT * FROM orders WHERE customer_id IN (2, 4, 6, 8) AND order_time BETWEEN '2017-02-01' AND '2017-02-28';
-- 3. From the customers table, select the first name and phone number of all customers who's last name contains the pattern 'ar'
SELECT * FROM customers;
SELECT first_name, phone_number FROM customers WHERE last_name LIKE '%ar%'
```

### Distinct

```SQL
USE coffee_store;
SELECT coffee_origin FROM products;
SELECT DISTINCT coffee_origin FROM products;

SELECT DISTINCT customer_id FROM orders;
SELECT DISTINCT customer_id, product_id FROM orders WHERE order_time BETWEEN '2017-02-01' AND '2017-02-28';
```

### Limit

```SQL
USE coffee_store;

SELECT * FROM customers LIMIT 5;
SELECT * FROM customers LIMIT 5 OFFSET 5;
SELECT * FROM customers LIMIT 5 OFFSET 15;
SELECT * FROM customers LIMIT 10 OFFSET 5;
SELECT * FROM customers ORDER BY last_name LIMIT 10;
```

### Column Name Alias

```SQL
USE coffee_store;

SELECT name AS coffee, price, coffee_origin AS country FROM products;

SELECT * FROM products;
```

### Exercise 3

1. From the customers table, select distinct last name and order alphabetically from A-Z.
2. From the orders table, select the first 3 orders placed by customer with id 1, in February 2017.
3. From the products table, select the name, price and coffee origin but rename the price to retail_price in the results set.

### Solution 3

```SQL
USE coffee_store;

-- 1. From the customers table, select distinct last name and order alphabetically from A-Z.
SELECT * FROM customers;
SELECT DISTINCT last_name FROM customers ORDER BY last_name;
-- 2. From the orders table, select the first 3 orders placed by customer with id 1, in February 2017.
SELECT * FROM orders;
SELECT * FROM orders WHERE customer_id = 1 AND order_time BETWEEN '2017-02-01' AND '2017-02-28' ORDER BY order_time ASC LIMIT 3;
-- 3. From the products table, select the name, price and coffee origin but rename the price to retail_price in the results set.
SELECT * FROM products;
SELECT name, price AS retail_price, coffee_origin FROM products;
```
