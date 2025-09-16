# Postgres JOINS (Customer & Orders Example with PK/FK)

## Learning Objectives

Able to...

1. Understand INNER, LEFT, RIGHT, and FULL JOINS in PostgreSQL

---

## Model 1

**Customers**

| customer_id | customer_name |
| ----------- | ------------- |
| c1          | Alice         |
| c2          | Bob           |
| c3          | Carol         |
| c4          | David         |

**Orders**

| order_id | customer_id |
| -------- | ----------- |
| o1       | c1          |
| o2       | c2          |
| o5       | c5          |
| o6       | c6          |

Questions

1. Which customers have matching orders?

    c1 and c2

2. Which customers do not have any orders?

    c3 and c4

3. Which orders do not match any existing customers?

    c5 and c6

---

## Model 2

Query

```sql
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
INNER JOIN Orders o ON c.customer_id = o.customer_id;
```

Result
| customer_id | customer_name | order_id |
| ----------- | ------------- | -------- |
| c1	        | Alice	        | o1       |
| c2	        | Bob	          | o2       |

Questions

1. Which customers are not included in the result?

    c3 and c4

2. Why do you think they are not in the result?

    Because there is no Order id match the customer_id in Customer table and customer_id

3. Which orders are not included in the result?

    o5 and o6

4. Why do you think they are not in the result?

    Because c5 and c6 are not in customers table.

5. When is a row from Customers or Orders included?

    Its only happen when we used Joins.

6. What is the meaning of INNER JOIN?

    Inner Join only includes rows where the join condition is true.

---

## Model 3

Query

```sql
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
LEFT OUTER JOIN Orders o ON c.customer_id = o.customer_id;
```

Result
| customer_id | customer_name | order_id |
| ----------- | ------------- | -------- |
| c1          | Alice	        | o1       |
| c2	        | Bob	          | o2       |
| c3	        | Carol	        |          |
| c4	        | David	        |          |

Questions

1. Which customers are not included in the result?

    c5 and c6

2. Which orders are not included in the result?

    o5 and o6

3. When is a row included?

    When we do "INSERT INTO ___ VALUES____ "
    then its added a row in table.

4. What is the meaning of LEFT OUTER JOIN?

    It takes all data from left table and only required columns from the right table where condition matched.
---

## Model 4

Query

```sql
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
RIGHT OUTER JOIN Orders o ON c.customer_id = o.customer_id;
```

Result
| customer_id | customer_name | order_id |
| ----------- | ------------- | -------- |
| c1          | Alice	        | o1       |
| c2	        | Bob	          | o2       |
|   	        |      	        | o5       |
|   	        |     	        | o6       |


Questions

1. Which customers are not included in the result?

    c3 and c4

2. Which orders are not included in the result?

    o3 and o4

3. When is a row included?

    When the Join condition is satisfy.

4. What is the meaning of RIGHT OUTER JOIN?

    It takes all data from right table and only required columns from the left table where condition matched.
---

## Model 5

Query

```sql
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
FULL JOIN Orders o ON c.customer_id = o.customer_id;
```

Result
| customer_id | customer_name | order_id |
| ----------- | ------------- | -------- |
| c1          | Alice	        | o1       |
| c2	        | Bob	          | o2       |
| c3	        | Carol	        |          |
| c4	        | David	        |          |
|   	        |      	        | o5       |
|   	        |     	        | o6       |

Questions

1. Which customers are not included in the result?

    c5 and c6

2. Which orders are not included in the result?

    o3 and o4

3. When is a row included?

    When Join condition is satisfied.

4. What is the meaning of FULL JOIN?

    In Full join its include both tables data where Join condition is stisfied.



---

## Model 6

Confirm the above by creating the tables in Postgres and running the queries. Paste the SQL for creating and populating the tables below.

```sql
CREATE TABLE Customers (
    customer_id VARCHAR(10) PRIMARY KEY,
    customer_name VARCHAR(50)
);

CREATE TABLE Orders (
    order_id VARCHAR(10) PRIMARY KEY,
    customer_id VARCHAR(10),
    CONSTRAINT fk_customer FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);


INSERT INTO Customers (customer_id, customer_name) VALUES
('c1', 'Alice'),
('c2', 'Bob'),
('c3', 'Carol'),
('c4', 'David');

INSERT INTO Customers (customer_id, customer_name) VALUES
('c5','xyz'),
('c6','Abc');

INSERT INTO Orders (order_id, customer_id) VALUES
('o1', 'c1'),
('o2', 'c2'),
('o5', 'c5'),
('o6', 'c6');

SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
INNER JOIN Orders o ON c.customer_id = o.customer_id;

SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
LEFT OUTER JOIN Orders o ON c.customer_id = o.customer_id;

SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
RIGHT OUTER JOIN Orders o ON c.customer_id = o.customer_id;

SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
FULL JOIN Orders o ON c.customer_id = o.customer_id;
```
