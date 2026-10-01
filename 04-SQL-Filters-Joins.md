# SQL Filters and Joins: Lab Notes and Quiz Answers

Notes from the Google Cybersecurity course (SQL filtering, joins, and aggregate functions).

## Table of Contents

- [Part 1: Filtering Quiz](#part-1-filtering-quiz)
- [Part 2: Which Join Type Is Appropriate?](#part-2-which-join-type-is-appropriate)
- [Part 3: Lab Walkthrough: SQL Joins](#part-3-lab-walkthrough-sql-joins)
- [Part 4: Joins Quiz](#part-4-joins-quiz)
- [Aggregate Functions](#aggregate-functions)

---

## Part 1: Filtering Quiz

### Question 1

**Answer:**

```sql
WHERE date BETWEEN '01-01-2015' AND '01-04-2015';
```

**Explanation:** The `BETWEEN` operator selects values within a specific range. The correct syntax requires the keyword `AND` to separate the start and end values.

### Question 2

**Answer:** `NOT`

**Explanation:** The `NOT` operator excludes specific records. To get everything *except* `'successful'` statuses, `WHERE NOT status = 'successful'` (or the `!=` / `<>` operators) is the most direct way to filter them out.

### Question 3

You are working with the **Chinook** database. Find the first and last names of customers whose `country` is either `'Brazil'` or `'Argentina'`.

**Answer:**

```sql
SELECT firstname, lastname, country
FROM customers
WHERE country = 'Brazil' OR country = 'Argentina';
```

This returns 6 customers.

### Question 4

Given this filter:

```sql
SELECT *
FROM customers
WHERE country = 'USA' AND state = 'NV';
```

**What will this query return?**

**Answer:** Information about customers who have a value of `'USA'` in the `country` column *and* a value of `'NV'` in the `state` column.

**Explanation:** The `AND` operator is inclusive, meaning **both** conditions must be true for a record to be returned. This query only shows customers located in Nevada, USA.

---

## Part 2: Which Join Type Is Appropriate?

The tables below can be joined on the `username` column. For each scenario, identify the join type that returns the needed data.

**Left table: `log_in_attempts`**

| Column |
| --- |
| event_number |
| username |
| date |
| time |
| ip_address |

**Right table: `employees_remote`**

| Column |
| --- |
| employee_id |
| username |
| department |
| location |

Start of the SQL statement:

```sql
SELECT *
FROM log_in_attempts
```

**Scenario:** You need to check employee engagement for remote workers. You want all records from the `employees_remote` table and only the matching records (on `username`) from `log_in_attempts`.

---

## Part 3: Lab Walkthrough: SQL Joins

### Activity overview

As a security analyst, you'll often need data from more than one table.

A **relational database** is a structured database containing tables that are related to each other. **SQL joins** let you combine tables that share a column, which is useful when information is spread across different tables.

This walkthrough covers the previous Qwiklab activity, including instructions and solutions. Use it if you couldn't complete the lab or want extra guidance, and as preparation for the graded quiz in this module.

> **Note:** The terms *row* and *record* are used interchangeably.

### Scenario

You're investigating a recent security incident that compromised some machines, and you need to get the required information from the database.

1. Use an **inner join** to identify which employees are using which machines.
2. Use **left and right joins** to find machines that don't belong to any user, and users who don't have a machine assigned.
3. Use an **inner join** to list all login attempts made by all employees.

> **Note:** You work with the `organization` database. The lab starts with it already open in the MariaDB shell. If you exit by accident, reconnect with:
>
> ```bash
> sudo mysql organization
> ```

### Task 1. Match employees to their machines

Identify which employees use which machines. The data is in the `machines` and `employees` tables, and both include the `device_id` column, which you'll use for an **inner join**.

**Step 1.** Retrieve all records from the `machines` table:

```sql
SELECT *
FROM machines;
```

This isn't enough to perform the join or get the information you need.

**Step 2.** Complete the query to perform an inner join between `machines` and `employees` on `device_id`:

```sql
SELECT *
FROM machines
INNER JOIN employees ON machines.device_id = employees.device_id;
```

> **Note:** Placing `employees` after `INNER JOIN` makes it the right table. To join tables you link them on a common column, here `device_id`.

**How many rows did the inner join return?**
**Answer:** 185 rows.

### Task 2. Return more data

Return all machines and the employees who have machines, then do the reverse: all employees and any machines assigned to them. Use a **left join** and a **right join** on `device_id`.

**Step 1: Left join**

```sql
SELECT *
FROM machines
LEFT JOIN employees ON machines.device_id = employees.device_id;
```

> **Note:** In a left join, all records from the table after `FROM` (before `LEFT JOIN`) are included. Here, all `machines` records are returned whether or not they're assigned to an employee.

**What is the value in the `username` column for the last record returned?**
**Answer:** `NULL`

**Step 2: Right join**

```sql
SELECT *
FROM machines
RIGHT JOIN employees ON machines.device_id = employees.device_id;
```

> **Note:** In a right join, all records from the table after `RIGHT JOIN` are included. Here, all `employees` records are returned whether or not they have a machine.

**What is the value in the `username` column for the last record returned?**
**Answer:** `areyes`

### Task 3. Retrieve login attempt data

Retrieve all employees who have made login attempts by performing an inner join on `employees` and `log_in_attempts`, linked on the common `username` column.

```sql
SELECT *
FROM employees
INNER JOIN log_in_attempts ON employees.username = log_in_attempts.username;
```

> **Note:** Specify the table name with the column name (`table.column`) when joining tables.

**How many records are returned by this inner join?**
**Answer:** 200 records.

---

## Part 4: Joins Quiz

### Question 1

**Which join types return all rows from only one of the tables being joined? Select all that apply.**

- [x] `LEFT JOIN` (returns all rows from the left table)
- [x] `RIGHT JOIN` (returns all rows from the right table)
- [ ] `INNER JOIN`
- [ ] `FULL OUTER JOIN`

**Note:** An `INNER JOIN` returns only matches from both sides, and a `FULL OUTER JOIN` returns all rows from *both* tables.

### Question 2

**Which of the following queries has the correct `INNER JOIN` syntax?**

```sql
SELECT *
FROM employees
INNER JOIN machines ON employees.employee_id = machines.employee_id;
```

It specifies the left table after `FROM`, the right table after `INNER JOIN`, and the correct syntax after `ON` for the column to join on.

### Question 3

**Which join returns all records from the `employees` table, but only the records that match on `employee_id` from the `machines` table?**

**Answer:** `LEFT JOIN`

**Why?** `employees` is listed first (the "left" table), so a `LEFT JOIN` includes every row from it, whether or not there's a matching machine.

### Question 4

**What is the value in the `trackid` column of the first row returned from this query?**

```sql
SELECT *
FROM invoices
INNER JOIN invoice_items ON invoices.invoiceid = invoice_items.invoiceid;
```

**Answer:** `2`

**Explanation:** In the standard **Chinook** sample database, the first invoice in `invoices` has an `InvoiceId` of 1. Joined to `invoice_items`, its first line item corresponds to `TrackId` 2.

---

## Aggregate Functions

**Aggregate functions** perform a calculation over multiple data points and return the result of that calculation. The underlying data is not returned.

| Function | What it returns |
| --- | --- |
| `COUNT` | A single number: the number of rows returned by your query |
| `AVG` | A single number: the average of the numerical data in a column |
| `SUM` | A single number: the sum of the numerical data in a column |
