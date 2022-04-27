Goal of the section is to focus on SQL Syntax. Same can be applied to any mayor type of SQL DB.

## Quick notes
- JOINS will allow us to combine information from multiple tables.
- Resource: https://blog.codinghorror.com/a-visual-explanation-of-sql-joins/
- `AS` clause
	- Allows us to create an Alias for a column
	- Syntax: SELECT column AS new_name FROM table;
	- Example: SELECT SUM(amount) AS net_revenue FROM payment;
	- Gets executed prior to FROM
- Different kind of Joins (combinations)
	- `INNER JOINS`
		- Syntax: SELECT * FROM TableA INNER JOIN TableB ON TableA.col_math = TableB.col_math;
		- Example: SELECT * FROM Registrations INNER JOINS Logins ON Reginstrations.name = Logins.name;
		- Example: SELECT reg_id, Logins.name, log_id FROM Registrations INNER JOINS Logins ON Reginstrations.name = Logins.name;
	- `FULL OUTER JOINS`
		- Deal with values that are present in one of the tables being joined,
		- Syntax: SELECT * FROM TableA FULL OUTER JOIN TableB ON TableA.col_match = TableB.col_match;
		- Example: SELECT * FROM Registrations FULL OUTER JOIN Logins ON Registrations.col_match = Logins.col_match;
		- Syntax: SELECT * FROM TableA FULL OUTER JOIN TableB ON TableA.col_match = TableB.col_match WHERE TableA.id IS null OR TableB.id IS null; --- Unique, where null is located
	- `FULL JOINS`
		- `LEFT OUTER JOIN`
			- Result in the set of records that are in the left table, if there is no match with the right table, the results are null.
			- Syntax: SELECT * FROM TableA LEFT OUTER JOIN TableB ON TableA.col_match = TableB.col_match;
			- Syntax: SELECT * FROM TableA LEFT OUTER JOIN TableB ON TableA.col_match = TableB.col_match WHERE TableA.id IS null OR TableB.id IS null; --- Unique, where null is located
		- `RIGHT OUTER JOIN`
			- Result in the set of records that are in the right table, if there is no match with the left table, the results are null.
			- Syntax: SELECT * FROM TableA RIGHT OUTER JOIN TableB ON TableA.col_match = TableB.col_match;
			- Syntax: SELECT * FROM TableA RIGHT OUTER JOIN TableB ON TableA.col_match = TableB.col_match WHERE TableA.id IS null OR TableB.id IS null; --- Unique, where null is located
	- `UNIONS`
		- Used to combine the result-set of two or more SELECT Statements.
		- Concatenates two results together.
		- Syntax: SELECT column_name(s) FROM TABLE 1 UNION SELECT column_name(s) FROM TABLE 1;
		- Example: SELECT * FROM Sales2022_Q1 UNION SELECT * FROM Sales2022_Q2;



## Code
```sql
/*AS*/
SELECT COUNT(*) AS Transactions FROM payment;

SELECT customer_id, SUM(amount) AS total_spent FROM payment
GROUP BY customer_id HAVING SUM(amount) > 100

/* INNER JOINS */ 
SELECT * FROM payment;

SELECT * FROM payment
INNER JOIN customer
ON payment.customer_id = customer.customer_id;

SELECT payment_id, payment.customer_id, first_name FROM payment
INNER JOIN customer ON payment.customer_id = customer.customer_id;

/* FULL OUTER JOINS */
SELECT * FROM payment;

SELECT * FROM CUSTOMER
FULL OUTER JOIN payment
ON customer.customer_id = payment.customer_id
WHERE customer.customer_id IS null
OR payment.payment_id IS null;

/* LEFT OUTER JOINS */
SELECT * FROM film;
SELECT * FROM inventory;

SELECT film.film_id, title, inventory_id, store_id FROM film
LEFT JOIN inventory ON inventory.film_id = film.film_id
WHERE inventory.film_id IS null;

/* ------- GENERAL CHALLENGES ------- */

/*1. Alert customer who lives in California trought emails of a Sales tax law change */
SELECT * FROM customer;
SELECT * FROM address;

SELECT first_name, last_name, email, district FROM address
INNER JOIN customer
ON customer.address_id = address.address_id
WHERE district = 'California';

/*2. Customer is fan of Nick Wahlberg and want to know the movies he is in. Get a list. */
SELECT * FROM film; /*film_id, title, description*/
SELECT * FROM actor; /*actor_id, first_name, last_name*/
SELECT * FROM film_actor; /*actor_id, film_id*/
SELECT * FROM actor WHERE first_name = 'Nick' AND actor.last_name = 'Wahlberg'; /* ID #2*/
SELECT * FROM film_actor WHERE actor_id = 2; /* 25 rows*/

SELECT title FROM film
LEFT JOIN film_actor
ON film.film_id = film_actor.film_id

/*Solution to challenge #2 - MULTIPLE JOINS*/
SELECT title, description FROM film_actor
INNER JOIN actor
ON film_actor.actor_id = actor.actor_id
INNER JOIN film
ON film_actor.film_id = film.film_id
WHERE first_name = 'Nick' AND last_name = 'Wahlberg';
```

