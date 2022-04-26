## Goal of the section is to focus on SQL Syntax. Same can be applied to any mayor type of SQL DB.

Quick notes
- ; is not mandatory.
- Capitalizing keywords is better for standardization.

- `SELECT` Statement
	- SELECT * FROM table_name; (when wanting all columns)
	- SELECT column_name FROM table_name;
	- SELECT column1, column2 FROM table_name;
- `SELECT DISTINCT`
	- Example: Column contains duplicate values and we only want to list unique/distinct values
	- SELECT DISTINCT (column) FROM table; -- () are optionals at this point
- `SELECT COUNT`
	- Returns the number of input rows that match a specific condition of a query
	- SELECT COUNT(column) FROM table; ---- () mandatory
	- Example: SELECT COUNT(DISTINCT column) FROM table;
- `SELECT... WHERE`
	- SELECT column1,column2 FROM table WHERE conditions;
	- Used to filter rows
	- Compare a column to a value
	- Comparison Operators (=, >, <, >, >=, <= <> or !=); 
	- Logical Operators: AND, OR, NOT
- `SELECT... ORDER BY`
	- Sorting ASC or DESC
	- SELECT column1, column2 FROM table ORDER BY column1 ASC/DESC;
- `SELECT... LIMIT`
	- Limit the amount of rows in the output
	- SELECT column1, column 2 FROM table LIMIT ##;
- `SELECT... BETWEEN`
	- Used to math a value against a range of values. value BETWEEN/NOT BETWEEN low AND high
	- SELECT column1, column 2 FROM table WHERE conditions BETWEEN / NOT BETWEEN
	- Can be used with dates ISO 8601 format YYYY-MM-DD. Includes also timestamp information.
- `SELECT .... IN`
	- Creates condition that checks to see if a value is included in a list of multiple options.
	- SELECT color FROM table WHERE color IN / NOT IN ('red','blue','green');
- `LIKE and ILIKE`
	- Allows us to perform pattern matching against string data with the use of wildcard characters:
	- % - Matches any sequence of characters
	- _ - Matches any single character
	- WHERE name LIKE 'A%'
	- WHERE name LIKE '%a'
	- ILIKE is not case sensitive
	- WHERE title LIKE 'Mission Impossible -underscore-'
	- Example: WHERE value LIKE 'Version#-underscore--underscore-'
	- WHERE name LIKE '-underscore-her%'

## Code
```sql
/*Base and DISTINCT*/
SELECT * FROM film;
SELECT first_name, last_name, email FROM customer;
SELECT DISTINCT(rating) FROM film;

/*COUNT and DISTINCT*/
SELECT * FROM payment;
SELECT COUNT(amount) FROM payment;
SELECT DISTINCT amount FROM payment;
SELECT COUNT(DISTINCT amount) FROM payment;

/*WHERE and operators*/
SELECT * FROM customer;
SELECT first_name,last_name FROM customer WHERE first_name = 'Linda';
SELECT * FROM film WHERE rental_rate > 4 AND replacement_cost >= 19.99 AND rating = 'R';
SELECT * FROM film WHERE rating = 'R' OR rating = 'PG-13';

/* Challenges */
SELECT * FROM customer;
SELECT email FROM customer WHERE first_name = 'Nancy' AND last_name = 'Thomas';
SELECT * FROM film;
SELECT title, description FROM film WHERE title = 'Outlaw Hanky';
SELECT * FROM address;
SELECT phone FROM address WHERE address = '259 Ipoh Drive';

/*ORDER BY*/
SELECT * FROM customer ORDER BY first_name ASC;
SELECT store_id, first_name, last_name FROM customer ORDER BY store_id DESC, first_name ASC;

/*LIMIT*/
SELECT * FROM payment;
SELECT * FROM payment WHERE amount != 0.00 ORDER BY payment_date DESC LIMIT 5;

/* Challenges */
SELECT customer_id, amount, payment_date FROM payment ORDER BY payment_date ASC LIMIT 10;
SELECT title, description, length FROM film ORDER BY length ASC LIMIT 5;
SELECT COUNT(title) FROM film WHERE length <= 50;

/*BETWEEN*/
SELECT * FROM payment;
SELECT COUNT(*) FROM payment WHERE amount BETWEEN 8 AND 9;
SELECT * FROM payment WHERE payment_date BETWEEN '2007-02-01' AND '2007-02-15';

/*IN*/
SELECT * FROM payment;
SELECT * FROM payment WHERE amount IN (0.99,1.98,1.99);
SELECT COUNT(*) FROM payment WHERE amount NOT IN (0.99,1.98,1.99);
SELECT * FROM customer WHERE first_name IN ('John', 'Jake', 'Julie');

/*LIKE ILIKE*/
SELECT * FROM customer;
SELECT * FROM customer WHERE first_name LIKE 'A%' AND last_name ILIKE '%s';
SELECT * FROM customer WHERE first_name LIKE '%er%';
SELECT * FROM customer WHERE first_name LIKE '___er%' AND first_name NOT LIKE 'Rob%';

/* ------- GENERAL CHALLENGES ------- */

/* Count - Payment Transactions greater than $5.00 = 3,618*/
SELECT COUNT(*) FROM payment WHERE amount > 5;

/* Count - Actors have a first name that starts with the letter P = 5*/
SELECT COUNT(*) FROM actor WHERE first_name LIKE 'P%';

/* Count - Unique districts our customers are from = 378*/
SELECT COUNT(DISTINCT district) FROM address;

/* Retreive list of Unique districts our customers are from = 378*/
SELECT DISTINCT district FROM address ORDER BY district ASC;

/* COUNT - Films have a rating if R and a replacement cost between $5 and $15 = */
SELECT COUNT(*) FROM film WHERE rating = 'R' AND replacement_cost BETWEEN 5 AND 15;

/* COUNT - Films have the word Truman in the title = 5*/
SELECT COUNT(*) FROM film WHERE title LIKE '%Truman%';

```

