Goal of the section is to focus on SQL Syntax. Same can be applied to any mayor type of SQL DB.

## Quick notes
- JOINS will allow us to combine information from multiple tables.
- Resource: https://blog.codinghorror.com/a-visual-explanation-of-sql-joins/
- `AS` clause
	- Allows us to create an Alias for a column
	- SELECT column AS new_name FROM table;
	- SELECT SUM(amount) AS net_revenue FROM payment;
	- Gets executed prior to FROM
- Different kind of Joins
	- `INNER JOINS`
	- `OUTER JOINS`
	- `FULL JOINS`
	- `UNIONS`



## Code
```sql
SELECT MIN(replacement_cost) FROM film;
SELECT MAX(replacement_cost) FROM film;
SELECT MAX(replacement_cost), MIN(replacement_cost) FROM film;
SELECT ROUND(AVG(replacement_cost),2) FROM film; 
SELECT SUM(replacement_cost) FROM film;

/* GROUP BY */
SELECT * FROM payment;

SELECT customer_id FROM payment
GROUP BY customer_id ORDER BY customer_id ASC; 

SELECT customer_id, SUM(amount) FROM payment
GROUP BY customer_id ORDER BY SUM(amount) DESC;

SELECT customer_id, COUNT(amount) FROM payment
GROUP BY customer_id ORDER BY COUNT(amount) DESC;

/* Multiple groups */
SELECT customer_id, staff_id, SUM(amount) FROM payment
GROUP BY staff_id, customer_id ORDER BY customer_id, staff_id;

SELECT DATE(payment_date), SUM(amount) FROM payment
GROUP BY DATE(payment_date) ORDER BY SUM(amount) DESC;

/* ------- START OF CHALLENGES ------- */
/* 2 Staff members, staff ID's 1 and 2. COUNT who had more sales. */
SELECT * FROM payment;

SELECT staff_id, COUNT(amount) FROM payment
GROUP BY staff_id ORDER BY COUNT(amount) DESC;

/* Relationship between replacement cost and a move rating - Average replacement cost per MPAA rating */
SELECT * FROM film;

SELECT rating, ROUND(AVG(replacement_cost),2) FROM film
GROUP BY rating ORDER BY rating ASC;

/* Top 5 customers */
SELECT * FROM customer;

SELECT customer_id, SUM(amount) FROM payment
GROUP BY customer_id ORDER BY SUM(amount) DESC LIMIT 5;

/* ------- END OF CHALLENGES ------- */

/* HAVING */
SELECT customer_id, SUM(amount) FROM payment
GROUP BY customer_id HAVING SUM(amount) > 100;

SELECT store_id, COUNT(*) FROM customer
GROUP BY store_id HAVING COUNT(*) > 300;

/* Challenge */
SELECT customer_id, COUNT(amount) FROM payment
GROUP BY customer_id HAVING COUNT(amount) >= 40
ORDER BY COUNT(amount) DESC;

SELECT staff_id, customer_id, SUM(amount) FROM payment
WHERE staff_id = 2 GROUP BY staff_id, customer_id
HAVING SUM(amount) > 100 ORDER BY SUM(amount) DESC;
```

