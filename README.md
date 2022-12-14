
# Leetcode SQL l Study Plan Solutions solved using MySQL (https://leetcode.com/study-plan/sql)

## DAY 1

### 595 Big Countries
```MySQL
SELECT name, population, area
FROM World
WHERE area >= 3000000 OR population >= 25000000;
```
### 1757 Recyclable and Low Fat Products
```MySQL
SELECT product_id
FROM Products
WHERE low_fats = 'Y' AND recyclable = 'Y'
```
### 584 Find Customer Referee
```MySQL
SELECT name
FROM Customer
WHERE referee_id <> 2 OR referee_id IS NULL;
```
### 183 Customers Who Never Order
```MySQL
SELECT c.name AS Customers
FROM Customers AS c
LEFT JOIN Orders AS o
ON c.id = o.customerID
WHERE o.id IS NULL
ORDER BY c.name;
```

## DAY 2

### 1873 Calculate Special Bonus
```MySQL
SELECT employee_id, 
    (CASE 
    WHEN employee_id % 2 <> 0 AND name NOT LIKE 'M%'
    THEN salary
    ELSE 0 
    END) AS bonus
FROM Employees
ORDER BY employee_id;
```
### 627 Swap Salary
```MySQL
# Note that you must write a single update statement, do not write any select statement for this problem.
UPDATE Salary
SET sex =  
    CASE sex
    WHEN 'm' THEN 'f'
    ELSE 'm'
    END;
```
### 196 Delete Duplicate Emails
```MySQL
# Please write a DELETE statement and DO NOT write a SELECT statement.
DELETE p1
FROM Person AS p1, Person AS p2
WHERE p1.email = p2.email AND p1.id > p2.id;
```
## DAY 3

### 1667 Fix Names in a Table
```MySQL
SELECT user_id, CONCAT(UPPER(SUBSTR(name,1,1)), LOWER(SUBSTR(name,2))) AS name
FROM Users
ORDER BY user_id;

# CONCAT(A,B) where we concat two strings A+B

# UPPER(A) where A is string
# LOWER(A) where A is string
# SUBSTR(A,index,length) 
# where A is string index is starting index(indexing starts at 1) 
# and length (optional, if blank goes till end)

# To get first letter we can use SUBSTR(name,1,1)
# To get the remaining string we can use SUBSTR(name,2) - length is not required here
```
### 1484 Group Sold Products By The Date
```MySQL
SELECT sell_date, 
    COUNT(DISTINCT product) AS num_sold,  
    GROUP_CONCAT(DISTINCT product) AS products
FROM Activities
GROUP BY sell_date
ORDER BY sell_date;
```
### 1527 Patients With a Condition
```MySQL
SELECT patient_id, patient_name, conditions
FROM Patients
WHERE conditions LIKE '% DIAB1%' OR conditions LIKE 'DIAB1%';
```
## DAY 4

### 1965 Employees With Missing Information
```MySQL
SELECT temp.employee_id
FROM
    (SELECT * FROM Employees LEFT JOIN Salaries USING(employee_id)
    UNION
    SELECT * FROM Employees RIGHT JOIN Salaries USING(employee_id)
    ) AS temp
WHERE temp.name IS NULL OR temp.salary IS NULL
ORDER BY temp.employee_id;
```
### 1795 Rearrange Products Table
```MySQL
SELECT product_id, 'store1' AS store, store1 As price
FROM Products
WHERE store1 IS NOT NULL
UNION
SELECT product_id, 'store2' AS store, store2 As price
FROM Products
WHERE store2 IS NOT NULL
UNION
SELECT product_id, 'store3' AS store, store3 As price
FROM Products
WHERE store3 IS NOT NULL;
```
### 608 Tree Node
```MySQL
SELECT id, 
    CASE
        WHEN p_id IS NULL THEN "Root"
        WHEN id NOT IN (SELECT DISTINCT p_id FROM Tree WHERE p_id IS NOT NULL) THEN "Leaf"
        ELSE "Inner"
    END AS type
FROM Tree
ORDER BY id;
```
### 176 Second Highest Salary
```MySQL
SELECT IFNULL((SELECT DISTINCT salary 
FROM Employee
ORDER BY salary DESC LIMIT 1 OFFSET 1), Null) AS SecondHighestSalary;
```

## DAY 5

### 175 Combine Two Tables
```MySQL
SELECT p.firstName, p.lastName, a.city, a.state
FROM Person AS p
LEFT JOIN Address AS a
ON p.personID = a.personID;
```

### 1581 Customer Who Visited but Did Not Make Any Transactions
```MySQL
SELECT v.customer_id, COUNT(v.customer_id) AS count_no_trans
FROM Visits AS v 
LEFT JOIN Transactions AS t 
ON v.visit_id = t.visit_id
WHERE t.transaction_id IS NULL
GROUP BY v.customer_id;
```

### 1148 Article Views I
```MySQL
SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id
ORDER BY id;
```
## DAY 6

### 197 Rising Temperature
```MySQL
SELECT w1.id
FROM Weather AS w1, Weather AS w2 
WHERE w1.temperature > w2.temperature AND DATEDIFF(w1.recordDate, w2.recordDate) = 1;
```

### 607 Sales Person
```MySQL
SELECT s.name
FROM SalesPerson AS s
WHERE s.name NOT IN
    (SELECT s.name
    FROM SalesPerson AS s 
    LEFT JOIN Orders AS o 
    ON s.sales_id = o.sales_id
    LEFT JOIN Company AS c 
    ON c.com_id = o.com_id
    WHERE c.name = 'RED');
```
## DAY 7

### 1141 User Activity for the Past 30 Days I
```MySQL
SELECT activity_date AS day, COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE DATEDIFF('2019-07-27', activity_date) < 30 AND activity_date <= '2019-07-27'
GROUP BY activity_date;
```
### 1693 Daily Leads and Partners
```MySQL
SELECT date_id, 
    make_name, 
    COUNT(DISTINCT lead_id) AS unique_leads, 
    COUNT(DISTINCT partner_id) AS unique_partners
FROM DailySales
GROUP BY date_id, make_name;
```
### 1729 Find Followers Count
```MySQL
SELECT user_id, COUNT(DISTINCT follower_id) AS followers_count
FROM Followers
GROUP BY user_id;
```
## DAY 8

### 586 Customer Placing the Largest Number of Orders
```MySQL
SELECT customer_number
FROM Orders
GROUP BY customer_number
ORDER BY COUNT(customer_number) DESC LIMIT 1;
```

### 511 Game Play Analysis I
```MySQL
SELECT player_id, MIN(event_date) AS first_login
FROM Activity
GROUP BY player_id;
```

### 1890 The Latest Login in 2020
```MySQL
SELECT user_id, MAX(time_stamp) AS last_stamp 
FROM Logins 
WHERE YEAR(time_stamp) = 2020 
GROUP BY user_id;
```

### 1741 Find Total Time Spent by Each Employee
```MySQL
SELECT event_day AS day, emp_id, SUM(out_time-in_time) AS total_time
FROM Employees
GROUP BY day, emp_id;
```
## DAY 9
### 1393 Capital Gain/Loss
```MySQL
SELECT stock_name,  SUM(CASE
                    WHEN operation = 'Sell' THEN price
                    ELSE -price
                    END) AS capital_gain_loss
FROM Stocks
GROUP BY stock_name;
```

### 1407 Top Travellers
```MySQL
SELECT u.name,  IFNULL(SUM(r.distance), 0) AS travelled_distance
FROM Users AS u
LEFT JOIN Rides AS r
ON u.id = r.user_id
GROUP BY u.id
ORDER BY travelled_distance DESC, u.name; 
```

### 1158 Market Analysis I
```MySQL
SELECT u.user_id AS 'buyer_id', 
        u.join_date AS 'join_date', 
        COUNT(o.order_id) AS 'orders_in_2019'
FROM Users AS u 
LEFT JOIN 
    (SELECT * FROM Orders WHERE YEAR(order_date) = '2019') AS o
ON u.user_id = o.buyer_id
GROUP BY u.user_id;
```

## DAY 10

### 182 Duplicate Emails
```MySQL
SELECT DISTINCT p1.email
FROM Person AS p1, Person AS p2
WHERE p1.email = p2.email AND p1.id <> p2.id;
```

### 1050 Actors and Directors Who Cooperated At Least Three Times
```MySQL
SELECT actor_id, director_id
FROM ActorDirector 
GROUP BY actor_id, director_id
HAVING COUNT(actor_id) >= 3;
```
### 1587 Bank Account Summary II
```MySQL
SELECT u.name, SUM(t.amount) AS balance
FROM Users AS u
LEFT JOIN Transactions AS t
ON u.account = t.account
GROUP BY u.name
HAVING balance > 10000;
```
### 1084 Sales Analysis III
```MySQL
SELECT p.product_id, p.product_name
FROM Product AS p
LEFT JOIN Sales AS s
ON p.product_id = s.product_id
GROUP BY p.product_id
HAVING 
    MIN(s.sale_date) >= '2019-01-01' AND 
    MAX(s.sale_date) <= '2019-03-31';
```
