Goal of the section is to focus on SQL Syntax. Same can be applied to any mayor type of SQL DB.

## Quick notes
- Documentation: https://www.postgresql.org/docs/current/datatype.html
- Data Types
	- Boolean
		- True or False
	- Character
		char, varchar, text
	- Numeric
		- Integer, float
	- Temporal
		date, time, timestamp, interval
	- UUID (Universally Unique Ientifiers)
	- Array
	- JSON
	- Hstore key-value pair
	- Special types like network addresses and geometric data
- Primary Keys and Foreign Keys
	- PK is a column or group of column used to identify a row uniquely in a table
	- FK field or group of fields in a table that uniquely identifies a row in another table.
	- The table that contains the FK is called referencing table or child table.
	- The table to which the FK references is called recerenced table or parent table.
	- A table can have multiple FK.
- Constraints
	- Rules enforced on data columns on table to prevent invalid data from being entered.
	- NOT NULL
	- UNIQUE
- CREATE Table
	- CREATE TABLE table_name ( column_name TYPE... table_constraint table_constraint );
	- SERIAL create a sequence object and set the next value genrated
- INSERT
	- Add data to a table
	- Can insert from another table
- UPDATE
- DELETE
- ALTER Table - changes table
- DROP Table - complete removal of a column
	- use CASCADE to remove columns with dependencies
- CHECK Constraint



## Code
```sql
/* CREATE TABLE */
CREATE TABLE account (
	user_id SERIAL PRIMARY KEY,
	username VARCHAR(50) UNIQUE NOT NULL,
	password VARCHAR(50) NOT NULL,
	email VARCHAR(250) UNIQUE NOT NULL,
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	created_on TIMESTAMP NOT NULL,
	last_login TIMESTAMP
);

CREATE TABLE job (
	job_id SERIAL PRIMARY KEY,
	job_name VARCHAR(200) UNIQUE NOT NULL
);

CREATE TABLE account_job (
	user_id INTEGER REFERENCES account(user_id),
	job_id INTEGER REFERENCES job(job_id),
	hire_date TIMESTAMP
);

/* INSERT */
SELECT * FROM account;

INSERT INTO account (username, password, email, first_name, last_name, created_on)
VALUES
('jose', 'password','jose@mail.com', 'Jose', 'Perez', CURRENT_TIMESTAMP);

SELECT * FROM job;
INSERT INTO job (job_name)
VALUES ('President');

INSERT INTO account_job(user_id, job_id, hire_date)
VALUES
(1,1,CURRENT_TIMESTAMP);

SELECT * FROM account_job;

/* UPDATE */
UPDATE account SET last_login = CURRENT_TIMESTAMP;
UPDATE account SET last_login = created_on;

UPDATE account_job
SET hire_date = account.created_on
FROM account
WHERE account_job.user_id = account.user_id;

SELECT * FROM account_job;

UPDATE account
SET last_login = CURRENT_TIMESTAMP
RETURNING *;

/* DELETE */
SELECT * FROM job;
INSERT INTO job (job_name)
VALUES
('Burudega');

DELETE FROM job
WHERE job_name = 'Burudega'
RETURNING job_id, job_name;

/*ALTER*/
CREATE TABLE information (
	info_id SERIAL PRIMARY KEY,
	title VARCHAR(500) NOT NULL,
	person VARCHAR(50) UNIQUE NOT NULL
);

ALTER TABLE information
RENAME TO new_info;

SELECT * FROM new_info;

ALTER TABLE new_info
RENAME COLUMN person TO people;

ALTER TABLE new_info
ALTER COLUMN people DROP NOT NULL;

INSERT INTO new_info (title)
VALUES
('Some new Title');

SELECT * FROM new_info;

/* DROP */
ALTER TABLE new_info
DROP COLUMN IF EXISTS people; /* CASCADE if there are dependencies*/

/*CHECK constraint*/
CREATE TABLE employees (
	emp_id SERIAL PRIMARY KEY,
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	birthdate DATE CHECK (birthdate > '1900-01-01'),
	hire_date DATE CHECK (hire_date > birthdate),
	salary INTEGER CHECK (salary > 0)
);

SELECT * FROM employees;

INSERT INTO employees (first_name, last_name, birthdate, hire_date, salary)
VALUES
('Sammy', 'Samm', '1990-11-03','2010-01-01',100);
```

