Goal of the section is to focus on SQL Syntax. Same can be applied to any mayor type of SQL DB.

## Quick notes
- Timestamps
	- TIME
	- DATE
	- TIMESTAMP
	- TIMESTAMPZ -  Contains timezone
- Extract - Allows to obtain a sub-component of a date value
	- EXTRACT(YEAR FROM date_col)
- AGE() - Calulates and retuns the current age give a timestamp
	- AGE(date_col)
- TO_CHAR() - Convert data types to text, useful for timetamp formatting
	- TO_CHAR(date_col, 'mm-dd-yyyy')
- Documentation - https://www.postgresql.org/docs/12/functions-formatting.html
- Math Functions and Operators
- String Functions
- Sub-query
	- Allows us to construct complex queries, query on the results of another query.
	- Involves 2 SELECT statements
	- Example: SELECT stuent, grade FROM test_scores WHERE grade > (SELECT AVG(grade) FROM test_scores)
	- Can also use WHERE ... IN
	- WHERE EXISTS
- Self-Join
	- Query in which a table is joined to itself.
	- Useful for comparing values ina column of rows within the same rable.
	- Join of two copies of the same table.
	- Alias is needed
	- SYNTAX: SELECT tableA.col, tableB.col FROM table AS tableA JOIN table AS tableB ON TableA.some_col = tableB.other_col



## Code
```sql
SHOW TIMEZONE;
SELECT NOW()
SELECT TIMEOFDAY();
SELECT CURRENT_DATE;
SELECT CURRENT_TIME;

/*EXTRACT(), AGE(), TO_CHAR()*/
SELECT * FROM payment;
SELECT  EXTRACT(YEAR FROM payment_date) AS myyear FROM payment;
SELECT  EXTRACT(QUARTER FROM payment_date) AS myyear FROM payment;

SELECT AGE(payment_date) FROM payment;

SELECT TO_CHAR(payment_date, 'MM/dd/YYYY') FROM payment;

/* CHALLENGE */

/*1 - During which months did the payment occur? - Format month to get the full month name*/
SELECT DISTINCT(TO_CHAR(payment_date, 'Month')) as Month FROM payment;

/*2-How may payments ocurred on a Monday*/
SELECT COUNT(*) FROM payment WHERE TO_CHAR(payment_date, 'Day') = 'Monday' ; /*Fail*/
SELECT COUNT(*) FROM payment WHERE EXTRACT(dow FROM payment_date) = 1

/*Math Functions and Operators -  See documentation*/
SELECT * FROM film;
SELECT ROUND(rental_rate/replacement_cost,2)*100 AS percent_lost FROM film;

/* String Functions and Operators */
SELECT * FROM customer;
SELECT LENGTH(first_name) FROM customer;
SELECT first_name || ' ' || upper(last_name) AS Name FROM customer ;

SELECT LOWER(left(first_name,1)) || LOWER(last_name) || '@gmail.com'
FROM customer;

/*Sub Query*/
SELECT * FROM film;
SELECT title, rental_rate FROM film
WHERE rental_rate > (SELECT AVG(rental_rate) FROM film);

SELECT film_id, title FROM film
WHERE film_id IN (SELECT inventory.film_id FROM rental
INNER JOIN inventory ON inventory.inventory_id = rental.inventory_id
WHERE rental.return_date BETWEEN '2005-05-29' AND '2005-05-30') 
ORDER BY title ASC;

SELECT first_name, last_name
FROM customer AS c
WHERE EXISTS
(SELECT * FROM payment AS p
 WHERE p.customer_id = c.customer_id
 AND amount > 11);

/* Self-Join */
/* FINd all the pair of films with the same length*/
SELECT title, length FROM film
WHERE length = 117;

SELECT f1.title, f2.title, f1.length length FROM film AS f1
INNER JOIN film AS f2 ON
f1.film_id != f2.film_id
AND f1.length = f2.length
WHERE f1.length = 117;
```

