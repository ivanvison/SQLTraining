Goal of the section is to focus on SQL Syntax. Same can be applied to any mayor type of SQL DB.

## Quick notes
- CASE
	- General: Execute SQL when conditions are met.
	- SYNTACT: CASE... WHEN condition1 THEN result1... ELSE some_other_result END
	- Example: SELECT a, CASE WHEN a = 1 THEN 'one' WHEN a = 2 THEN '2' ELSE 'other' AS label END FROM test;
	- Case expresion first evaluates then compares the result with each value in the WHEN clauses sequentially.
	- CASE expression WHEN value1 THEN result1 ELSE some_other_result END.
- COALESCE
	- Basically determines if the value is "NULL" and then replaces that null with another parameter.
- CAST
	- Convert from one data type into another
	- SELECT CAST ('5' AS INTEGER)  ---- SQL
	- SELECT '5'::INTEGER ----- PostgreSQL
	- SELECT CAST(date AS TIMESTAMP) FROM table
- NULLIF
	- takes 2 inputs and return null if both are equal, if not returns the first argument passed
- VIEWS
	- Quickly see queries frecuent use.
	- DB object "virtual table"
- IMPORT/EXPORT
	- Documentation, look for SQL-COPY



## Code
```sql
/*CASE*/
SELECT * FROM customer;

SELECT customer_id,
CASE
	WHEN (customer_id <= 100) THEN 'Premium'
	WHEN (customer_id BETWEEN 100 and 200) THEN 'Plus'
	ELSE 'Normal'
END AS customer_class
FROM customer;

SELECT customer_id,
CASE customer_id
	WHEN 2 THEN 'First Place Winner'
	WHEN 5 THEN 'Second Place Winner'
	ELSE 'Normal'
END AS raffel_results
FROM customer;

SELECT * FROM film;

SELECT 
SUM(CASE rental_rate
	WHEN 0.99 THEN 1
	ELSE 0
END) AS barg,
SUM(CASE rental_rate
	WHEN 2.99 THEN 1
	ELSE 0
END) AS regular,
SUM(CASE rental_rate
	WHEN 4.99 THEN 1
	ELSE 0
END) AS premium
FROM film

/*CASE CHALLENGE */

SELECT 
SUM(CASE rating
	WHEN 'R' THEN 1 ELSE 0
END) AS r,
SUM(CASE rating
	WHEN 'PG' THEN 1 ELSE 0
END) AS pg,
SUM(CASE rating
	WHEN 'PG-13' THEN 1 ELSE 0
END) AS pg13
FROM film

/*CAST Operator and Function*/
SELECT CAST('5' AS INTEGER) AS new_int;
SELECT '5'::INTEGER; /*postgreSQL only*/

SELECT inventory_id, CHAR_LENGTH(CAST(inventory_id AS VARCHAR)) FROM rental;

/*NULLIF*/
SELECT (
	SUM(CASE WHEN deprtment = 'A' THEN 1 ELSE 0 END) / 
	NULLIF(SUM(CASE WHEN deprtment = 'B' THEN 1 ELSE 0 END),0)
) AS dept_ratio
FROM depts

/* VIEWS */
SELECT * FROM customer;
SELECT * FROM address;

CREATE VIEW customer_info AS
SELECT first_name, last_name, address FROM customer
INNER JOIN address
on customer.address_id = address.address_id;

SELECT * FROM customer_info;

CREATE OR REPLACE VIEW customer_info AS
SELECT first_name, last_name, address, district FROM customer
INNER JOIN address
on customer.address_id = address.address_id;

DROP VIEW IF EXISTS customer_info;

ALTER VIEW cudtomer_info RENAME TO c_info;
```

