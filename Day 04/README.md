# PostgreSQL Bootcamp – Day 04 ( SQL JOINS)

## Overview
This session focused on understanding and applying SQL JOIN operations in PostgreSQL using both sample tables and the dvdrental database.


## The Two Methods
![The Two Methods of SQL JOINS](https://substackcdn.com/image/fetch/$s_!ANhb!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F795441b2-462e-4356-85dd-78fc842d2cbc_3626x1832.png)
*Image source: Substack (original author credited via Substack-hosted media).*

Imagine you’re working with two tables, **Table A** and **Table B**, and you need to bring their data together. The key question to ask is: **are you trying to combine rows or combine columns?**

SQL provides two powerful approaches for this:

- **Set Operators** – used to stack rows vertically (e.g., UNION, INTERSECT, EXCEPT)

**Joins** – used to merge columns horizontally based on related or matching values

Understanding this distinction helps you choose the right tool and write clearer, more efficient SQL queries.


## What is SQL Join?
![What is SQL Join](https://substackcdn.com/image/fetch/$s_!A0vS!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe94cbb73-b293-4bf7-91c1-1ae2ab35282b_2260x1890.png)
*Image source: Substack (original author credited via Substack-hosted media).*

An SQL JOIN connects rows from two tables based on a common column.

If you want to query from two tables into one result, you first need to connect them. Tables store related data separately, but SQL lets you combine them using a JOIN.

PostgreSQL join is used to combine columns from one (self-join) or more tables based on the values of the common columns between related tables. The common columns are typically the primary key columns of the first table and the foreign key columns of the second table.

PostgreSQL supports `inner join`, `left join`, `right join`, `full outer join`, `cross join`, `natural join`, and a special kind of join called `self-join`.

## Why We Join Tables?
Joins aren’t just about merging tables—they serve different purposes based on what you need.

### 1. Recombine Data (Big Picture)
![](https://substackcdn.com/image/fetch/$s_!iq6H!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb53135c9-a342-402d-91fe-0e3749d92e4b_3143x1381.png)

Data is often stored across multiple tables to keep it organized. But when you need a **full view**, you have to bring them back together.

- Example: A company stores employee details in one table and salaries in another. To generate a full employee report, you need to connect them.

- Use **INNER**, **LEFT**, or **FULL JOIN** to merge related data.

### 2. Data Enrichment (Extra Info)
![](https://substackcdn.com/image/fetch/$s_!iPDm!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7113d770-658c-4c48-86d6-f61f0a3676e6_3389x1146.png) 

Sometimes, you already have your main dataset, but you need to **add extra** details from another table.

- Example: You have a list of customers and want to include their latest purchase details. Instead of duplicating data, you just link their purchase records when needed.

- A **LEFT JOIN** ensures you keep all records from the main table while pulling extra information.

### 3. Check Existence (Filtering)
![](https://substackcdn.com/image/fetch/$s_!ddND!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd6b454d5-9474-476c-a14b-ea9193af79bc_3622x1282.png)

You may need to check if a record exists in another table or filter based on missing data.

- Example: You have a list of registered users but want to find out who has never made a purchase. By checking their presence in the sales table, you can identify them.

- Use **INNER JOIN** to return only matches.

- Use **LEFT** or **FULL JOIN** with **WHERE** to filter records missing from another table.

## The Three Possibilities When Joining Tables
When joining tables, your result set depends on which data you want to keep. There are three main possibilities:

![](https://substackcdn.com/image/fetch/$s_!A-mj!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff8eb53d2-48a3-4272-9b03-86048429ffb2_3840x1029.png)

## JOIN Types 
![Join Types](https://substackcdn.com/image/fetch/$s_!AsEp!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5cadb3a0-fd07-42c3-92be-f2d91839781e_3840x2160.png)


### Basic Joins 
- **No Join** → Returns data from both Table A and Table B with combining them.

**SQL TASK**

- Retrieve all data from customers and rentals as seperate results.

```sql
SELECT
	*
FROM
	customer;

SELECT
	*
FROM
	rental;
```

- **Inner Join** → Returns only matching rows from both tables.

**SQL TASK**

- Get customers along with their payments, but only customers who have made a payment.

```sql
SELECT
	c.customer_id,
    c.first_name,
    p.payment_id,
    p.amount
FROM
	customer AS c
INNER JOIN
    payment AS p
ON 
    c.customer_id = p.customer_id;
```

- **Left Join** → Returns all rows from Table A and matching rows from B.

**SQL TASK**

- Get all customers along with their payments, including those without payment.

```sql
SELECT
	c.customer_id,
    c.first_name,
    p.payment_id,
    p.amount
FROM
	customer AS c
LEFT JOIN
    payment AS p
ON 
    c.customer_id = p.customer_id;
```

- **Right Join** → Returns all rows from Table B and matching rows from A.

**SQL TASK**

- Get all customers along with their payments, including payment without matching customers.

```sql
SELECT
	c.customer_id,
    c.first_name,
    p.payment_id,
    p.amount
FROM
	customer AS c
RIGHT JOIN
    payment AS p
ON 
    c.customer_id = p.customer_id;
```

- **Full / Full Outer Join** → Returns all rows from both tables.

**SQL TASK**

- Get all customers and all payments, even if there's no match.

```sql
SELECT
	c.customer_id,
    c.first_name,
    p.payment_id,
    p.amount
FROM
	customer AS c
FULL JOIN
    payment AS p
ON 
    c.customer_id = p.customer_id;
```

### Advanced Joins
- **Left Anti Join** → Returns row from left that has NO MATCH in the right table.

**SQL TASK**

- Get all customers who haven't place any order.

```sql
SELECT
	c.customer_id,
    c.first_name,
    p.payment_id,
    p.amount
FROM
	customer AS c
LEFT JOIN
    payment AS p
ON 
    c.customer_id = p.customer_id
WHERE p.customer_id IS NULL;
```

- **Right Anti Join** → Returns row from rigth that has NO MATCH in the left table.

**SQL TASK**

- Get all payments without matching customers.

```sql
SELECT
	c.customer_id,
    c.first_name,
    p.payment_id,
    p.amount
FROM
	customer AS c
RIGHT JOIN
    payment AS p
ON 
    c.customer_id = p.customer_id
WHERE c.customer_id IS NULL;
```

- **Full Anti Join** → Rows that don’t match in either table.

**SQL TASK**

- Get customers without payments and payments without customers.

```sql
SELECT
	c.customer_id,
    c.first_name,
    p.payment_id,
    p.amount
FROM
	customer AS c
FULL JOIN
    payment AS p
ON 
    c.customer_id = p.customer_id
WHERE c.customer_id IS NULL OR p.customer_id IS NULL;
```

- **Cross Join (Cartesian Join)** → Combines every row from A with every row from B.

**SQL TASK**

- Generate all possible combination of customers and payments.

```sql
SELECT
	c.customer_id,
    c.first_name,
    p.payment_id,
    p.amount
FROM
	customer AS c
CROSS JOIN
    payment;
```

- **Natural Join** → 

- **Self Join** → 

### Multi-Table Joins
- **Multiple Inner Joins** → Joining more than two tables with matches.

- **Multiple Left Joins** → Keeping all rows from one main table.

- **Inner + Left Joins** → Mixing join types for complex queries.

Each JOIN type serves a different purpose, helping you get the right data for your queries!

