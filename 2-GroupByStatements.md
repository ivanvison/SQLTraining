Goal of the section is to focus on SQL Syntax. Same can be applied to any mayor type of SQL DB.

## Quick notes
- ; is not mandatory.
- Capitalizing keywords is better for standardization.
- GROUP BY Statement - Allow us to aggregate data and apply funtions to better understand how data is distributed per catergory.
	- SELECT category_col, AGG(data_col) FROM table GROUP BY category_col;
	- SELECT company, division, SUM(sales) FROM finance_table GROUP BY company, division;
	- SELECT company, SUM(sales) FROM finance_table GROUP BY company ORDER BY SUM(sales) LIMIT 5;
	- SELECT company, SUM(sales) FROM finance_table WHERE company != 'Google' GROUP BY company;
	- Should go Right after WHERE or FROM statement
- HAVING allows a filter after the aggregation
	- SELECT company, SUM(sales) FROM finance_table WHERE company != 'Google' GROUP BY company HAVING SUM(sales) > 1000
- Aggregate functions
	- Take multiple inputs and return single output
	- AVG() -- Returns a flotaing point value. ROUND() can be used to specify precision
	- COUNT() -- Returns number of rows
	- MAX()
	- MIN()
	- SUM()



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

