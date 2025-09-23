 **Advanced SQL Mastery Guide** — a modular, example-rich breakdown of every major category from beginner to expert level. This will be your go-to reference for reporting, automation, analytics, and technical submissions.

---

## 📘 SQL Mastery Guide: Full Breakdown

### 1. 🔍 Query Optimization
- **Beginner**: Indexes, `EXPLAIN`, `LIMIT`, `ORDER BY`
- **Intermediate**: Composite indexes, covering indexes, avoiding full scans
- **Advanced**: Execution plans, query hints, materialized views, denormalization strategies

### 2. 🧮 Window Functions
- **Beginner**: `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`
- **Intermediate**: `PARTITION BY`, `LEAD()`, `LAG()`, `FIRST_VALUE()`, `LAST_VALUE()`
- **Advanced**: `NTILE()`, running totals, gaps and islands, combining with aggregates

### 3. 📊 Analytical Functions
- **Beginner**: `GROUP BY`, `HAVING`
- **Intermediate**: `ROLLUP`, `CUBE`, `GROUPING SETS`
- **Advanced**: `PIVOT`, `UNPIVOT`, dynamic grouping, multi-level aggregations

### 4. 🔗 Joins Mastery
- **Beginner**: `INNER`, `LEFT`, `RIGHT`, `FULL`
- **Intermediate**: `SELF JOIN`, `CROSS JOIN`, join filters
- **Advanced**: `ANTI JOIN`, `SEMI JOIN`, join optimization, join reordering

### 5. 🔁 Set Operations
- **Beginner**: `UNION`, `UNION ALL`
- **Intermediate**: `INTERSECT`, `EXCEPT`
- **Advanced**: Deduplication, combining with subqueries, performance tuning

### 6. 🌳 Recursive Queries
- **Beginner**: `WITH RECURSIVE`, base case + recursive case
- **Intermediate**: Tree traversal, depth tracking
- **Advanced**: Cycle detection, limiting recursion, performance tuning

### 7. 🔐 Transactions & Control
- **Beginner**: `BEGIN`, `COMMIT`, `ROLLBACK`
- **Intermediate**: Savepoints, nested transactions
- **Advanced**: Isolation levels, deadlock handling, retry logic

### 8. 🧱 Constraints & Integrity
- **Beginner**: `PRIMARY KEY`, `NOT NULL`, `UNIQUE`
- **Intermediate**: `CHECK`, `DEFAULT`, `FOREIGN KEY`
- **Advanced**: Cascading actions, deferred constraints, schema evolution

### 9. ⚙️ Triggers & Procedures
- **Beginner**: `AFTER INSERT`, `BEFORE UPDATE`
- **Intermediate**: Stored procedures with parameters
- **Advanced**: Error handling, dynamic logic, audit trails

### 10. 🧊 Materialized Views
- **Beginner**: Create and query
- **Intermediate**: Manual refresh, dependency tracking
- **Advanced**: Incremental refresh, partitioned views, automation

### 11. 🧠 Dynamic SQL
- **Beginner**: `PREPARE`, `EXECUTE`
- **Intermediate**: Parameterized queries
- **Advanced**: Dynamic column selection, SQL injection safety

### 12. 🔐 Security & Access
- **Beginner**: `GRANT`, `REVOKE`
- **Intermediate**: Role-based access
- **Advanced**: Row-level security, audit logging

### 13. 🔄 Data Types & Casting
- **Beginner**: `CAST()`, `CONVERT()`
- **Intermediate**: Implicit vs explicit casting
- **Advanced**: Custom types, coercion in joins

### 14. 🧩 Partitioning & Sharding
- **Beginner**: Range partitioning
- **Intermediate**: Hash partitioning
- **Advanced**: Sharding, query federation, routing

### 15. 🛡️ Error Handling
- **Beginner**: Basic try-catch
- **Intermediate**: Logging errors
- **Advanced**: Transaction-safe error handling, monitoring integration

---

### 🔧 Next Steps
Awesome, Saibabu! Let’s kick off your **Advanced SQL Mastery Workbook** — structured for clarity, depth, and real-world relevance. I’ll build it section by section, starting with:

---

## 📘 Module 1: Query Optimization

### 🔹 1.1 What Is Query Optimization?
Query optimization is the process of improving SQL queries to run faster and use fewer resources. It’s critical for large datasets, reporting systems, and automation pipelines.

---

### 🔸 1.2 Indexing Basics

| Type         | Description                                | Example                          |
|--------------|--------------------------------------------|----------------------------------|
| Single Index | Speeds up lookup on one column             | `CREATE INDEX idx_id ON emp(id);` |
| Composite    | Index on multiple columns                  | `CREATE INDEX idx_name_dept ON emp(name, dept);` |
| Covering     | Includes all columns needed for a query    | Used in SELECT-only queries      |

**Best Practice**: Use indexes on columns used in `WHERE`, `JOIN`, and `ORDER BY`.

---

### 🔸 1.3 Execution Plans

Use `EXPLAIN` or `EXPLAIN ANALYZE` to see how your query runs.

```sql
EXPLAIN SELECT * FROM emp WHERE dept = 'Sales';
```

Look for:
- **Seq Scan** → full table scan (slow)
- **Index Scan** → uses index (fast)
- **Nested Loop** → join strategy

---

### 🔸 1.4 Common Optimization Techniques

| Technique                  | Benefit                                |
|---------------------------|----------------------------------------|
| Use `LIMIT`               | Reduces result size                    |
| Avoid `SELECT *`          | Fetch only needed columns              |
| Filter early              | Push filters into subqueries           |
| Use CTEs wisely           | Avoid unnecessary materialization      |
| Partition large tables    | Speeds up scans and joins              |

---

### 🔸 1.5 Real-World Use Case

**Scenario**: You’re building a dashboard that shows top 10 sales reps by region.

```sql
SELECT region, rep_name, total_sales
FROM (
  SELECT region, rep_name, SUM(sales) AS total_sales,
         RANK() OVER (PARTITION BY region ORDER BY SUM(sales) DESC) AS rank
  FROM sales_data
  GROUP BY region, rep_name
) ranked
WHERE rank <= 10;
```

**Optimization**:
- Add index on `(region, rep_name)`
- Use materialized view if data refreshes hourly

Fantastic, Saibabu! Let’s dive into **Module 2: Window Functions** — one of the most powerful tools for analytics, reporting, and automation. This module will take you from foundational concepts to advanced use cases with examples and best practices.

---

## 🧮 Module 2: Window Functions

### 🔹 2.1 What Are Window Functions?

Window functions perform calculations across a set of rows **related to the current row**, without collapsing the result set like `GROUP BY` does.

---

### 🔸 2.2 Syntax Breakdown

```sql
function_name() OVER (
  [PARTITION BY column]
  [ORDER BY column]
  [ROWS BETWEEN ...]
)
```

- `PARTITION BY`: Resets the window for each group
- `ORDER BY`: Defines row order within each partition
- `ROWS BETWEEN`: Controls frame size (used in aggregates)

---

### 🔸 2.3 Ranking Functions

| Function        | Description                          | Example |
|----------------|--------------------------------------|---------|
| `ROW_NUMBER()`  | Unique row number per partition      | `ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC)` |
| `RANK()`        | Same rank for ties, gaps allowed     | `RANK() OVER (ORDER BY score DESC)` |
| `DENSE_RANK()`  | Same rank for ties, no gaps          | `DENSE_RANK() OVER (ORDER BY score DESC)` |

---

### 🔸 2.4 Value Functions

| Function     | Description                          | Example |
|--------------|--------------------------------------|---------|
| `LEAD()`     | Next row’s value                     | `LEAD(salary) OVER (ORDER BY id)` |
| `LAG()`      | Previous row’s value                 | `LAG(salary) OVER (ORDER BY id)` |
| `FIRST_VALUE()` | First value in partition          | `FIRST_VALUE(salary) OVER (PARTITION BY dept ORDER BY id)` |
| `LAST_VALUE()`  | Last value in partition           | `LAST_VALUE(salary) OVER (PARTITION BY dept ORDER BY id)` |

---

### 🔸 2.5 Aggregate Window Functions

```sql
SELECT emp_id, dept, salary,
       SUM(salary) OVER (PARTITION BY dept) AS dept_total,
       AVG(salary) OVER (PARTITION BY dept) AS dept_avg
FROM employees;
```

- Keeps individual rows while showing group-level aggregates
- Ideal for dashboards and comparative reports

---

### 🔸 2.6 NTILE and Bucketing

```sql
SELECT name, NTILE(4) OVER (ORDER BY score DESC) AS quartile
FROM students;
```

- Divides rows into equal-sized buckets
- Great for percentile analysis

---

### 🔸 2.7 Frame Control (Advanced)

```sql
SUM(sales) OVER (
  PARTITION BY region
  ORDER BY month
  ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
)
```

- Rolling totals, moving averages
- Custom frame sizes for time-series analytics

---

### 🧪 Real-World Use Case

**Scenario**: You want to show each employee’s salary, department average, and how they rank.

```sql
SELECT name, dept, salary,
       AVG(salary) OVER (PARTITION BY dept) AS dept_avg,
       RANK() OVER (PARTITION BY dept ORDER BY salary DESC) AS salary_rank
FROM employees;
```

---

### ✅ Practice Challenge

> Write a query to show each product’s monthly sales, the running total for its category, and its rank within that month.

**Module 3: Analytical Functions** 

Awesome, Saibabu — your SQL Mastery Workbook is officially underway! You've already got:

- ✅ Module 1: **Query Optimization**
- ✅ Module 2: **Window Functions**

Now I’ll continue building the next module:

---

## 📊 Module 3: Analytical Functions

### 🔹 3.1 What Are Analytical Functions?

Analytical functions extend `GROUP BY` logic to support **multi-level aggregations**, **dynamic summaries**, and **pivot-style reporting** — all without losing row-level detail.

---

### 🔸 3.2 GROUPING SETS, ROLLUP, and CUBE

#### 🔹 GROUPING SETS
Custom groupings in one query:
```sql
SELECT region, product, SUM(sales)
FROM sales_data
GROUP BY GROUPING SETS (
  (region, product),
  (region),
  (product),
  ()
);
```

#### 🔹 ROLLUP
Hierarchical totals:
```sql
SELECT region, product, SUM(sales)
FROM sales_data
GROUP BY ROLLUP(region, product);
```

#### 🔹 CUBE
All combinations:
```sql
SELECT region, product, SUM(sales)
FROM sales_data
GROUP BY CUBE(region, product);
```

---

### 🔸 3.3 GROUPING() Function

Detects NULLs created by ROLLUP/CUBE:
```sql
SELECT region, product,
       GROUPING(region) AS is_region_null,
       GROUPING(product) AS is_product_null,
       SUM(sales)
FROM sales_data
GROUP BY CUBE(region, product);
```

---

### 🔸 3.4 PIVOT and UNPIVOT

#### 🔹 PIVOT (SQL Server, Oracle)
```sql
SELECT * FROM (
  SELECT region, month, sales FROM sales_data
) src
PIVOT (
  SUM(sales) FOR month IN ('Jan', 'Feb', 'Mar')
) AS p;
```

#### 🔹 UNPIVOT
```sql
SELECT region, month, sales
FROM sales_data
UNPIVOT (
  sales FOR month IN (jan_sales, feb_sales, mar_sales)
);
```

---

### 🔸 3.5 Real-World Use Case

**Scenario**: You want a report showing total sales by region, product, and overall.

```sql
SELECT region, product, SUM(sales)
FROM sales_data
GROUP BY GROUPING SETS (
  (region, product),
  (region),
  ()
);
```

---

### ✅ Practice Challenge

> Write a query using `ROLLUP` to show total sales by category, subcategory, and grand total. Then use `GROUPING()` to label the subtotal rows.

---
 **Module 4: Joins Mastery** 
 Thanks for the nudge, Saibabu! Since I can’t generate downloadable files directly, here’s what I’ll do instead:

- I’ll continue building **Module 4: Joins Mastery** right here in chat.
- Once we’ve completed all 15 modules, I’ll format everything into a **Copilot Page** — a clean, editable surface where you can organize, refine, and reuse this SQL workbook anytime.

Let’s dive into the next module:

---

## 🔗 Module 4: Joins Mastery

### 🔹 4.1 What Are Joins?

Joins combine rows from two or more tables based on a related column. They’re essential for relational data modeling, reporting, and analytics.

---

### 🔸 4.2 Core Join Types

| Join Type     | Description                                      | Example |
|---------------|--------------------------------------------------|---------|
| `INNER JOIN`  | Matches rows in both tables                      | `SELECT * FROM emp JOIN dept ON emp.dept_id = dept.id;` |
| `LEFT JOIN`   | All rows from left + matched from right          | `SELECT * FROM emp LEFT JOIN dept ON emp.dept_id = dept.id;` |
| `RIGHT JOIN`  | All rows from right + matched from left          | `SELECT * FROM emp RIGHT JOIN dept ON emp.dept_id = dept.id;` |
| `FULL JOIN`   | All rows from both sides, matched where possible | `SELECT * FROM emp FULL JOIN dept ON emp.dept_id = dept.id;` |

---

### 🔸 4.3 Advanced Join Types

| Join Type       | Use Case                                      | Example |
|-----------------|-----------------------------------------------|---------|
| `SELF JOIN`     | Compare rows within the same table            | `SELECT e1.name, e2.name FROM emp e1 JOIN emp e2 ON e1.manager_id = e2.id;` |
| `CROSS JOIN`    | Cartesian product (all combinations)          | `SELECT * FROM colors CROSS JOIN sizes;` |
| `ANTI JOIN`     | Rows in A not in B                            | `SELECT * FROM emp WHERE NOT EXISTS (SELECT 1 FROM dept WHERE emp.dept_id = dept.id);` |
| `SEMI JOIN`     | Rows in A that match B (no B columns returned)| `SELECT * FROM emp WHERE EXISTS (SELECT 1 FROM dept WHERE emp.dept_id = dept.id);` |

---

### 🔸 4.4 Join Filters vs WHERE Filters

```sql
-- Join filter
SELECT * FROM emp JOIN dept ON emp.dept_id = dept.id AND dept.region = 'South';

-- WHERE filter
SELECT * FROM emp JOIN dept ON emp.dept_id = dept.id WHERE dept.region = 'South';
```

**Best Practice**: Use join filters for performance when filtering during join.

---

### 🔸 4.5 Join Optimization Tips

- Index foreign keys and join columns
- Avoid joining on non-indexed expressions
- Use `EXISTS` for filtering, `JOIN` for combining
- Use `WITH` (CTEs) to isolate logic

---

### 🧪 Real-World Use Case

**Scenario**: You want a report showing employees, their department names, and managers.

```sql
SELECT e.name AS employee,
       d.name AS department,
       m.name AS manager
FROM employees e
JOIN departments d ON e.dept_id = d.id
LEFT JOIN employees m ON e.manager_id = m.id;
```

---
Absolutely, Saibabu — here’s a deep dive into **Join Optimization Tips (4.5)** with clear examples and reasoning tailored for your reporting and automation goals:

---

## 🔧 4.5 Join Optimization Tips — Explained with Examples

---

### ✅ 1. Index Foreign Keys and Join Columns

**Why**: Indexes speed up join operations by allowing the database to quickly locate matching rows.

**Example**:
```sql
-- Index the foreign key column
CREATE INDEX idx_emp_dept_id ON employees(dept_id);

-- Join becomes faster
SELECT e.name, d.name
FROM employees e
JOIN departments d ON e.dept_id = d.id;
```

**Best Practice**: Always index columns used in `JOIN`, `WHERE`, and `ORDER BY`.

---

### 🚫 2. Avoid Joining on Non-Indexed Expressions

**Why**: Expressions prevent index usage, forcing full table scans.

**Bad Example**:
```sql
-- This disables index usage
SELECT * FROM orders o
JOIN customers c ON UPPER(o.cust_name) = UPPER(c.name);
```

**Optimized Version**:
```sql
-- Normalize data before joining
SELECT * FROM orders o
JOIN customers c ON o.cust_name = c.name;
```

**Tip**: Avoid functions (`UPPER`, `TRIM`, `CAST`) on join keys.

---

### 🔍 3. Use `EXISTS` for Filtering, `JOIN` for Combining

**Why**: `EXISTS` is faster for checking existence, especially with subqueries.

**Filtering Example (EXISTS)**:
```sql
SELECT * FROM employees e
WHERE EXISTS (
  SELECT 1 FROM departments d WHERE d.id = e.dept_id AND d.region = 'South'
);
```

**Combining Example (JOIN)**:
```sql
SELECT e.name, d.name
FROM employees e
JOIN departments d ON e.dept_id = d.id
WHERE d.region = 'South';
```

**Rule of Thumb**:
- Use `JOIN` when you need columns from both tables.
- Use `EXISTS` when you only care about presence.

---

### 🧠 4. Use `WITH` (CTEs) to Isolate Logic

**Why**: Common Table Expressions (CTEs) improve readability and can help the optimizer reuse logic.

**Example**:
```sql
WITH top_sales AS (
  SELECT emp_id, SUM(sales) AS total_sales
  FROM sales_data
  GROUP BY emp_id
  HAVING SUM(sales) > 100000
)
SELECT e.name, ts.total_sales
FROM employees e
JOIN top_sales ts ON e.id = ts.emp_id;
```

**Benefits**:
- Modular logic
- Easier debugging
- Can improve performance if reused multiple times

---

### 🧪 Real-World Scenario

**Problem**: You need to list employees in high-performing departments, but only if those departments have >10 employees.

**Optimized Query**:
```sql
WITH big_depts AS (
  SELECT dept_id
  FROM employees
  GROUP BY dept_id
  HAVING COUNT(*) > 10
)
SELECT e.name, d.name
FROM employees e
JOIN departments d ON e.dept_id = d.id
WHERE e.dept_id IN (SELECT dept_id FROM big_depts);
```

Excellent, Saibabu! Let’s keep the momentum going with **Module 5: Set Operations** — a powerful toolset for comparing, merging, and filtering datasets across tables or queries.

---

## 🔁 Module 5: Set Operations

### 🔹 5.1 What Are Set Operations?

Set operations allow you to combine results from multiple `SELECT` queries, similar to how sets work in mathematics. They’re ideal for deduplication, comparisons, and union-style reporting.

---

### 🔸 5.2 Core Set Operators

| Operator       | Description                                      | Duplicates Allowed? |
|----------------|--------------------------------------------------|----------------------|
| `UNION`        | Combines results, removes duplicates             | ❌ No                |
| `UNION ALL`    | Combines results, keeps duplicates               | ✅ Yes               |
| `INTERSECT`    | Returns rows common to both queries              | ❌ No                |
| `EXCEPT`       | Returns rows in first query but not in second    | ❌ No                |

---

### 🔸 5.3 Syntax Examples

#### 🔹 UNION
```sql
SELECT name FROM employees
UNION
SELECT name FROM contractors;
```

#### 🔹 UNION ALL
```sql
SELECT name FROM employees
UNION ALL
SELECT name FROM contractors;
```

#### 🔹 INTERSECT
```sql
SELECT email FROM customers
INTERSECT
SELECT email FROM newsletter_subscribers;
```

#### 🔹 EXCEPT
```sql
SELECT email FROM customers
EXCEPT
SELECT email FROM unsubscribed_users;
```

---

### 🔸 5.4 Rules and Best Practices

- All queries must have **same number of columns** and **compatible data types**.
- Use `ORDER BY` only **after** the final set operation.
- Use `DISTINCT` inside subqueries if needed for performance.
- Prefer `UNION ALL` when duplicates are acceptable — it’s faster.

---

### 🧪 Real-World Use Case

**Scenario**: You want to find all users who are either employees or contractors, but not both.

```sql
-- Employees only
SELECT name FROM employees
EXCEPT
SELECT name FROM contractors;

-- Contractors only
SELECT name FROM contractors
EXCEPT
SELECT name FROM employees;
```

---

Let’s roll into **Module 6: Recursive Queries**, Saibabu — a powerful technique for handling hierarchical or self-referencing data like org charts, category trees, and bill-of-materials structures. This module will take you from foundational syntax to advanced traversal logic.

---

## 🌳 Module 6: Recursive Queries

### 🔹 6.1 What Are Recursive Queries?

Recursive queries allow you to repeatedly apply a query to its own result — perfect for navigating parent-child relationships or building chains of logic.

They’re built using **Common Table Expressions (CTEs)** with the `WITH RECURSIVE` clause.

---

### 🔸 6.2 Syntax Breakdown

```sql
WITH RECURSIVE cte_name AS (
  -- Anchor member (base case)
  SELECT ...

  UNION ALL

  -- Recursive member (refers to cte_name)
  SELECT ...
  FROM cte_name
  JOIN ...
)
SELECT * FROM cte_name;
```

- **Anchor**: Starting point (e.g., top-level manager)
- **Recursive**: Builds on previous result
- **UNION ALL**: Combines both parts

---

### 🔸 6.3 Example: Employee Hierarchy

```sql
WITH RECURSIVE org_chart AS (
  SELECT id, name, manager_id, 1 AS level
  FROM employees
  WHERE manager_id IS NULL

  UNION ALL

  SELECT e.id, e.name, e.manager_id, oc.level + 1
  FROM employees e
  JOIN org_chart oc ON e.manager_id = oc.id
)
SELECT * FROM org_chart;
```

- Starts with top-level managers
- Recursively adds direct reports
- Tracks hierarchy depth with `level`

---

### 🔸 6.4 Example: Category Tree

```sql
WITH RECURSIVE category_tree AS (
  SELECT id, name, parent_id
  FROM categories
  WHERE parent_id IS NULL

  UNION ALL

  SELECT c.id, c.name, c.parent_id
  FROM categories c
  JOIN category_tree ct ON c.parent_id = ct.id
)
SELECT * FROM category_tree;
```

---

### 🔸 6.5 Advanced Techniques

| Technique             | Purpose                                |
|-----------------------|----------------------------------------|
| `LIMIT` recursion     | Prevent infinite loops                 |
| `LEVEL` tracking      | Add depth column                       |
| `PATH` building       | Concatenate hierarchy path             |
| `CYCLE` detection     | Avoid circular references              |

**Cycle-safe example**:
```sql
SELECT * FROM category_tree
WHERE id NOT IN (SELECT parent_id FROM category_tree);
```

---

### 🧪 Real-World Use Case

**Scenario**: You want to generate a report showing each employee, their manager, and their level in the org chart.

```sql
WITH RECURSIVE hierarchy AS (
  SELECT id, name, manager_id, 1 AS level
  FROM employees
  WHERE manager_id IS NULL

  UNION ALL

  SELECT e.id, e.name, e.manager_id, h.level + 1
  FROM employees e
  JOIN hierarchy h ON e.manager_id = h.id
)
SELECT * FROM hierarchy ORDER BY level, name;
```

---

### ✅ Practice Challenge

> Write a recursive query to list all categories and subcategories in a hierarchy, including their depth level and full path (e.g., "Electronics > Phones > Android").
A **CTE (Common Table Expression)** is a temporary result set in SQL that you define using the `WITH` clause. It makes complex queries easier to read, debug, and reuse — especially when dealing with subqueries, recursion, or modular logic.

---

## 🧠 CTE Basics

### 🔹 Syntax
```sql
WITH cte_name AS (
  SELECT ...
)
SELECT * FROM cte_name;
```

- Think of it as a named subquery.
- It exists only during the execution of the main query.

---

### ✅ Example: Simple CTE

```sql
WITH high_earners AS (
  SELECT name, salary
  FROM employees
  WHERE salary > 100000
)
SELECT * FROM high_earners;
```

- Filters high-salary employees.
- Makes the main query cleaner and modular.

---

### 🔄 Use Cases

| Use Case                     | Benefit                              |
|-----------------------------|--------------------------------------|
| Simplify nested subqueries   | Improves readability                 |
| Reuse logic across joins     | Avoids duplication                   |
| Enable recursion             | Required for hierarchical queries    |
| Modular reporting            | Easier to maintain and debug         |

---

### 🔁 Recursive CTE Example

```sql
WITH RECURSIVE org_chart AS (
  SELECT id, name, manager_id, 1 AS level
  FROM employees
  WHERE manager_id IS NULL

  UNION ALL

  SELECT e.id, e.name, e.manager_id, oc.level + 1
  FROM employees e
  JOIN org_chart oc ON e.manager_id = oc.id
)
SELECT * FROM org_chart;
```

- Builds an employee hierarchy.
- Tracks depth with `level`.
Perfect, Saibabu! Here's your **CTE Comparison Cheat Sheet** — showing how **CTEs**, **subqueries**, and **temporary tables** differ in structure, use cases, and performance. This will help you choose the right tool for reporting, automation, and modular SQL design.

---

## 📘 CTE vs Subquery vs Temp Table

| Feature               | CTE (`WITH`)                            | Subquery                              | Temporary Table                        |
|----------------------|-----------------------------------------|----------------------------------------|----------------------------------------|
| **Scope**            | Only within the query that defines it   | Nested inside another query            | Available across multiple queries      |
| **Readability**      | High — modular and named                | Medium — can get deeply nested         | High — separate object with clear name |
| **Reusability**      | Yes, within the same query              | No — must repeat logic                 | Yes — across multiple queries          |
| **Performance**      | Optimized by engine (depends on DB)     | Can be slower if deeply nested         | Fast for large datasets                |
| **Recursion**        | ✅ Supported (`WITH RECURSIVE`)         | ❌ Not supported                        | ❌ Not supported                        |
| **Indexing**         | ❌ Not possible                         | ❌ Not possible                         | ✅ Can be indexed                       |
| **Use Case**         | Modular logic, recursion, reporting     | Simple filters, one-off logic          | ETL, staging, batch processing         |

---

### ✅ CTE Example

```sql
WITH high_sales AS (
  SELECT emp_id, SUM(sales) AS total_sales
  FROM sales_data
  GROUP BY emp_id
)
SELECT e.name, hs.total_sales
FROM employees e
JOIN high_sales hs ON e.id = hs.emp_id;
```

---

### 🔁 Subquery Example

```sql
SELECT name,
       (SELECT SUM(sales) FROM sales_data WHERE emp_id = e.id) AS total_sales
FROM employees e;
```

---

### 🧪 Temp Table Example

```sql
CREATE TEMP TABLE high_sales AS
SELECT emp_id, SUM(sales) AS total_sales
FROM sales_data
GROUP BY emp_id;

SELECT e.name, hs.total_sales
FROM employees e
JOIN high_sales hs ON e.id = hs.emp_id;
```

---

### 🧠 When to Use What

| Scenario                          | Best Tool         |
|----------------------------------|-------------------|
| Modular reporting logic          | ✅ CTE            |
| One-off calculation              | ✅ Subquery       |
| Large intermediate result        | ✅ Temp Table     |
| Recursive hierarchy              | ✅ CTE            |
| ETL or batch staging             | ✅ Temp Table     |

---
Let’s power into **Module 7: Transactions & Control**, Saibabu — a critical skillset for building reliable, error-proof SQL workflows, especially in automation, reporting, and financial systems.

---

## 🔐 Module 7: Transactions & Control

### 🔹 7.1 What Is a Transaction?

A **transaction** is a sequence of SQL operations that are treated as a single unit. It ensures **atomicity** — either all operations succeed, or none do.

---

### 🔸 7.2 Core Transaction Commands

| Command     | Purpose                                  |
|-------------|------------------------------------------|
| `BEGIN`     | Starts a transaction                     |
| `COMMIT`    | Saves all changes                        |
| `ROLLBACK`  | Undoes all changes since `BEGIN`         |
| `SAVEPOINT` | Marks a checkpoint within a transaction  |
| `ROLLBACK TO` | Reverts to a savepoint                 |

---

### ✅ Example: Basic Transaction

```sql
BEGIN;

UPDATE accounts SET balance = balance - 500 WHERE id = 1;
UPDATE accounts SET balance = balance + 500 WHERE id = 2;

COMMIT;
```

If anything fails, you can `ROLLBACK` to undo both updates.

---

### 🔸 7.3 Using SAVEPOINT

```sql
BEGIN;

UPDATE inventory SET stock = stock - 1 WHERE item_id = 101;
SAVEPOINT deduct_stock;

UPDATE payments SET status = 'paid' WHERE order_id = 202;

-- Something goes wrong
ROLLBACK TO deduct_stock;

COMMIT;
```

- Only the second update is undone.
- The first update remains.

---

### 🔸 7.4 Isolation Levels (Advanced)

| Level              | Description                                      |
|--------------------|--------------------------------------------------|
| `READ UNCOMMITTED` | Can read uncommitted changes (dirty reads)       |
| `READ COMMITTED`   | Default — only committed data is visible         |
| `REPEATABLE READ`  | Prevents non-repeatable reads                    |
| `SERIALIZABLE`     | Strictest — full isolation, may block other txns |

**Set isolation level** (PostgreSQL):
```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

---

### 🔸 7.5 Error Handling with Transactions

```sql
BEGIN;

-- risky operation
UPDATE orders SET status = 'shipped' WHERE id = 999;

-- simulate failure
IF NOT FOUND THEN
  ROLLBACK;
ELSE
  COMMIT;
END IF;
```

Use conditional logic or exception blocks in stored procedures to handle failures gracefully.

---

### 🧪 Real-World Use Case

**Scenario**: You’re automating a payment + inventory update. If either fails, rollback everything.

```sql
BEGIN;

UPDATE inventory SET stock = stock - 1 WHERE item_id = 101;
UPDATE payments SET status = 'paid' WHERE order_id = 202;

-- Check for errors
IF error THEN
  ROLLBACK;
ELSE
  COMMIT;
END IF;
```

---

### ✅ Practice Challenge

> Write a transaction that deducts stock, updates payment status, and logs the action. If any step fails, rollback everything. Add a savepoint after the stock deduction.

---
Let’s dive into **Module 8: Constraints & Integrity**, Saibabu — the backbone of reliable database design. This module ensures your data stays clean, consistent, and trustworthy, especially in automated systems and reporting pipelines.

---

## 🧱 Module 8: Constraints & Integrity

### 🔹 8.1 What Are Constraints?

Constraints are rules enforced by the database to maintain **data integrity**. They prevent invalid, duplicate, or inconsistent data from entering your tables.

---

### 🔸 8.2 Types of Constraints

| Constraint       | Purpose                                      | Example |
|------------------|----------------------------------------------|---------|
| `PRIMARY KEY`    | Uniquely identifies each row                 | `id INT PRIMARY KEY` |
| `FOREIGN KEY`    | Enforces relationship between tables         | `FOREIGN KEY (dept_id) REFERENCES departments(id)` |
| `UNIQUE`         | Ensures no duplicate values                  | `email VARCHAR(255) UNIQUE` |
| `NOT NULL`       | Prevents missing values                      | `name VARCHAR(100) NOT NULL` |
| `CHECK`          | Validates custom conditions                  | `age INT CHECK (age >= 18)` |
| `DEFAULT`        | Sets a default value if none is provided     | `status VARCHAR(10) DEFAULT 'active'` |

---

### 🔸 8.3 PRIMARY KEY vs UNIQUE

| Feature         | PRIMARY KEY           | UNIQUE              |
|-----------------|------------------------|---------------------|
| Uniqueness      | ✅ Required            | ✅ Required          |
| Nulls Allowed   | ❌ Not allowed         | ✅ Allowed (one null)|
| Index Created   | ✅ Automatically       | ✅ Automatically     |
| Use Case        | Row identity           | Business rules (e.g., email) |

---

### 🔸 8.4 FOREIGN KEY with Cascading Actions

```sql
CREATE TABLE orders (
  id INT PRIMARY KEY,
  customer_id INT,
  FOREIGN KEY (customer_id)
    REFERENCES customers(id)
    ON DELETE CASCADE
    ON UPDATE CASCADE
);
```

- `ON DELETE CASCADE`: Deletes related orders when a customer is deleted.
- `ON UPDATE CASCADE`: Updates foreign key when parent key changes.

---

### 🔸 8.5 CHECK Constraints

```sql
CREATE TABLE employees (
  id INT PRIMARY KEY,
  age INT CHECK (age BETWEEN 18 AND 65),
  salary DECIMAL CHECK (salary > 0)
);
```

- Enforces business rules directly in schema.
- Can use expressions, ranges, or conditions.

---

### 🔸 8.6 DEFAULT Values

```sql
CREATE TABLE tasks (
  id INT PRIMARY KEY,
  status VARCHAR(10) DEFAULT 'pending',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

- Simplifies inserts
- Ensures consistent initial values

---

### 🧪 Real-World Use Case

**Scenario**: You’re designing a reporting system for employee records. You want to ensure:
- Every employee has a unique ID and email
- Age must be ≥ 18
- Department must exist in the `departments` table

```sql
CREATE TABLE employees (
  id INT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(255) UNIQUE,
  age INT CHECK (age >= 18),
  dept_id INT,
  FOREIGN KEY (dept_id) REFERENCES departments(id)
);
```

---

### ✅ Practice Challenge

> Design a `products` table with the following rules:
- `id` is the primary key
- `price` must be positive
- `category_id` must reference a `categories` table
- `stock` defaults to 0
- `sku` must be unique

--
Let’s jump into **Module 9: Triggers & Procedures**, Saibabu — a powerhouse combo for automating logic, enforcing rules, and building intelligent workflows directly inside your database.

---

## ⚙️ Module 9: Triggers & Procedures

### 🔹 9.1 What Are Triggers?

A **trigger** is a database object that automatically executes a block of code in response to a specific event on a table — like `INSERT`, `UPDATE`, or `DELETE`.

---

### 🔸 9.2 Trigger Syntax (PostgreSQL Example)

```sql
CREATE OR REPLACE FUNCTION log_insert()
RETURNS TRIGGER AS $$
BEGIN
  INSERT INTO audit_log(table_name, action, timestamp)
  VALUES (TG_TABLE_NAME, TG_OP, CURRENT_TIMESTAMP);
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER after_insert_trigger
AFTER INSERT ON employees
FOR EACH ROW
EXECUTE FUNCTION log_insert();
```

- `TG_TABLE_NAME`: Table name where trigger fired
- `TG_OP`: Operation type (`INSERT`, `UPDATE`, etc.)

---

### 🔸 9.3 Trigger Types

| Type             | Fires On         | Use Case                          |
|------------------|------------------|-----------------------------------|
| `BEFORE INSERT`  | Before row insert| Validate or modify data           |
| `AFTER INSERT`   | After row insert | Audit, notify, cascade actions    |
| `BEFORE UPDATE`  | Before update    | Enforce business rules            |
| `AFTER DELETE`   | After delete     | Archive, log, cleanup             |

---

### 🔹 9.4 What Are Stored Procedures?

A **stored procedure** is a reusable block of SQL logic that can accept parameters, perform operations, and return results. It’s ideal for encapsulating business logic.

---

### 🔸 9.5 Procedure Syntax (MySQL Example)

```sql
CREATE PROCEDURE update_salary (
  IN emp_id INT,
  IN increment DECIMAL(10,2)
)
BEGIN
  UPDATE employees
  SET salary = salary + increment
  WHERE id = emp_id;
END;
```

To call it:
```sql
CALL update_salary(101, 5000);
```

---

### 🔸 9.6 Procedure Features

| Feature         | Description                          |
|-----------------|--------------------------------------|
| Parameters      | `IN`, `OUT`, `INOUT`                 |
| Control Flow    | `IF`, `CASE`, `LOOP`, `WHILE`        |
| Error Handling  | `DECLARE HANDLER`, `TRY...CATCH`     |
| Transactions    | Can include `BEGIN`, `COMMIT`, `ROLLBACK` |

---

### 🧪 Real-World Use Case

**Scenario**: You want to automatically log every time a new employee is added, and also provide a procedure to bulk update salaries.

- **Trigger** logs the insert:
```sql
AFTER INSERT ON employees → INSERT INTO audit_log(...)
```

- **Procedure** updates salaries:
```sql
CALL update_salary(emp_id, increment);
```

---

### ✅ Practice Challenge

> Write a trigger that logs every deletion from the `orders` table into an `order_archive` table. Then write a stored procedure that accepts a `customer_id` and returns their total order value.

---
Let’s move into **Module 10: Materialized Views**, Saibabu — a performance-focused feature that’s perfect for speeding up dashboards, reports, and recurring analytics in your automation workflows.

---

## 🧊 Module 10: Materialized Views

### 🔹 10.1 What Is a Materialized View?

A **materialized view** stores the result of a query physically in the database. Unlike regular views, it doesn’t re-run the query every time — making it ideal for expensive aggregations and slow joins.

---

### 🔸 10.2 Syntax (PostgreSQL Example)

```sql
CREATE MATERIALIZED VIEW sales_summary AS
SELECT region, SUM(sales) AS total_sales
FROM sales_data
GROUP BY region;
```

- Acts like a snapshot of the query result.
- Can be queried like a table:
```sql
SELECT * FROM sales_summary;
```

---

### 🔸 10.3 Refreshing the View

| Method                  | Description                              |
|-------------------------|------------------------------------------|
| `REFRESH MATERIALIZED VIEW` | Re-runs the query and updates the view |
| `WITH DATA`             | Default — refreshes with new data        |
| `WITH NO DATA`          | Creates empty shell (manual refresh later)|

```sql
REFRESH MATERIALIZED VIEW sales_summary;
```

---

### 🔸 10.4 Use Cases

| Scenario                          | Benefit                          |
|----------------------------------|----------------------------------|
| Heavy aggregations               | Avoid repeated computation       |
| Dashboard metrics                | Fast load times                  |
| Snapshot reporting               | Point-in-time analysis           |
| ETL staging                      | Intermediate result caching      |

---

### 🔸 10.5 Performance Tips

- Index materialized views if needed:
  ```sql
  CREATE INDEX idx_region ON sales_summary(region);
  ```
- Use `WITH NO DATA` for fast creation during schema setup.
- Schedule refreshes using cron jobs or event triggers.

---

### 🧪 Real-World Use Case

**Scenario**: You want to show daily sales totals on a dashboard, but querying the full `sales_data` table is slow.

```sql
CREATE MATERIALIZED VIEW daily_sales AS
SELECT sale_date, SUM(sales) AS total_sales
FROM sales_data
GROUP BY sale_date;

-- Refresh daily
REFRESH MATERIALIZED VIEW daily_sales;
```

---

### ✅ Practice Challenge

> Create a materialized view that summarizes total revenue by product category. Then write a query to refresh it and filter for categories with revenue > ₹1,00,000.

---
Got it, Saibabu! Here's your complete **Module 12: Security & Access**, now fully integrated with a dedicated section on **REVOKE** — including 12 practical examples for managing access in real-world SQL environments.

---

## 🔐 Module 12: Security & Access

### 🔹 12.1 Why It Matters

Security ensures:
- Only authorized users can access or modify data
- Sensitive information is protected
- Actions are traceable for audits and compliance

---

### 🔸 12.2 GRANT & REVOKE Overview

| Command | Purpose                          | Example |
|--------|----------------------------------|---------|
| `GRANT` | Assigns privileges to users/roles | `GRANT SELECT ON employees TO analyst;` |
| `REVOKE` | Removes privileges                | `REVOKE SELECT ON employees FROM analyst;` |

---

### 🔸 12.3 Role-Based Access Control (RBAC)

```sql
CREATE ROLE reporting_user;
GRANT SELECT ON sales_data TO reporting_user;
GRANT reporting_user TO saibabu;
```

- Roles simplify permission management
- Supports least-privilege principle

---

### 🔸 12.4 Row-Level Security (RLS)

```sql
ALTER TABLE employees ENABLE ROW LEVEL SECURITY;

CREATE POLICY dept_filter
ON employees
FOR SELECT
USING (dept_id = current_setting('app.dept_id')::INT);

SET app.dept_id = 101;
SELECT * FROM employees;
```

- Restricts access to rows based on context
- Ideal for multi-tenant dashboards

---

### 🔸 12.5 Auditing with Triggers

```sql
CREATE TRIGGER log_update
AFTER UPDATE ON employees
FOR EACH ROW
INSERT INTO audit_log VALUES (CURRENT_USER, CURRENT_TIMESTAMP, 'UPDATE', OLD.id);
```

- Tracks who changed what and when
- Supports compliance and rollback analysis

---

### 🔸 12.6 REVOKE: 12 Practical Examples

| # | Scenario | SQL Command |
|--|----------|-------------|
| 1 | Remove read access from analyst | `REVOKE SELECT ON employees FROM analyst;` |
| 2 | Block data entry role from modifying orders | `REVOKE INSERT, UPDATE ON orders FROM data_entry_clerk;` |
| 3 | Prevent user from deleting transactions | `REVOKE DELETE ON transactions FROM saibabu;` |
| 4 | Stop intern from executing salary update | `REVOKE EXECUTE ON PROCEDURE update_salary FROM intern;` |
| 5 | Remove all access from temp_user | `REVOKE ALL PRIVILEGES ON customers FROM temp_user;` |
| 6 | Block schema usage | `REVOKE USAGE ON SCHEMA finance FROM auditor;` |
| 7 | Block sequence access | `REVOKE USAGE ON SEQUENCE invoice_seq FROM saibabu;` |
| 8 | Remove role membership | `REVOKE reporting_user FROM saibabu;` |
| 9 | Remove view access | `REVOKE SELECT ON sales_summary_view FROM analyst;` |
|10 | Prevent inserts on materialized view | `REVOKE INSERT ON daily_sales_mv FROM data_loader;` |
|11 | Block function execution | `REVOKE EXECUTE ON FUNCTION get_employee_bonus FROM saibabu;` |
|12 | Restrict column-level updates | `REVOKE UPDATE(name, email) ON employees FROM intern;` |

---

### 🧪 Real-World Use Case

**Scenario**: You want to allow analysts to view sales data but not modify it, and restrict each analyst to their assigned region.

```sql
-- Grant read-only access
GRANT SELECT ON sales_data TO analyst;

-- Enable row-level security
ALTER TABLE sales_data ENABLE ROW LEVEL SECURITY;

-- Policy: only see assigned region
CREATE POLICY region_filter
ON sales_data
FOR SELECT
USING (region = current_setting('app.region'));
```

---

### ✅ Practice Challenge

> Create a role called `auditor` that can only read from the `transactions` table. Then write a row-level security policy that restricts access to rows where `status = 'approved'`. Finally, revoke all modification privileges from that role.

--
Let’s move into **Module 14: Partitioning & Sharding**, Saibabu — a high-performance strategy for scaling large datasets, improving query speed, and optimizing storage in enterprise-grade SQL systems.

---

## 🧩 Module 14: Partitioning & Sharding

### 🔹 14.1 Why Partitioning Matters

Partitioning splits a large table into smaller, manageable pieces — called **partitions** — based on column values. It improves:
- Query performance
- Maintenance efficiency
- Parallel processing

---

### 🔸 14.2 Types of Partitioning

| Type           | Description                                | Example Use Case |
|----------------|--------------------------------------------|------------------|
| **Range**      | Based on value ranges                      | Date-based logs  |
| **List**       | Based on specific values                   | Region or category |
| **Hash**       | Even distribution using hash function      | Load balancing   |
| **Composite**  | Combines two types                         | Region + Date    |

---

### 🔸 14.3 Range Partitioning Example (PostgreSQL)

```sql
CREATE TABLE sales (
  id INT,
  sale_date DATE,
  amount DECIMAL
) PARTITION BY RANGE (sale_date);

CREATE TABLE sales_2025 PARTITION OF sales
FOR VALUES FROM ('2025-01-01') TO ('2025-12-31');
```

- Queries on `sale_date` will only scan relevant partitions.

---

### 🔸 14.4 List Partitioning Example

```sql
CREATE TABLE orders (
  id INT,
  region TEXT
) PARTITION BY LIST (region);

CREATE TABLE orders_south PARTITION OF orders
FOR VALUES IN ('South');
```

---

### 🔸 14.5 Hash Partitioning Example

```sql
CREATE TABLE logs (
  id INT,
  message TEXT
) PARTITION BY HASH (id);

CREATE TABLE logs_part1 PARTITION OF logs FOR VALUES WITH (MODULUS 4, REMAINDER 0);
CREATE TABLE logs_part2 PARTITION OF logs FOR VALUES WITH (MODULUS 4, REMAINDER 1);
```

- Distributes rows evenly across partitions.

---

### 🔸 14.6 Sharding (Distributed Partitioning)

**Sharding** splits data across multiple servers or databases — ideal for horizontal scaling.

| Feature         | Description                          |
|-----------------|--------------------------------------|
| Node-based      | Each shard lives on a separate node  |
| Query routing   | Middleware directs queries to shards |
| Use case        | Multi-region apps, massive datasets  |

**Example Strategy**:
- Shard by `customer_id` range
- Route queries using metadata or proxy layer

---

### 🔸 14.7 Best Practices

- Index partition keys
- Avoid cross-partition joins
- Use partition pruning (`WHERE` clause on partition key)
- Monitor partition size and balance

---

### 🧪 Real-World Use Case

**Scenario**: You manage a `transactions` table with millions of rows per year. You want to optimize reporting by year.

```sql
CREATE TABLE transactions (
  id INT,
  txn_date DATE,
  amount DECIMAL
) PARTITION BY RANGE (txn_date);

CREATE TABLE transactions_2024 PARTITION OF transactions
FOR VALUES FROM ('2024-01-01') TO ('2024-12-31');

CREATE TABLE transactions_2025 PARTITION OF transactions
FOR VALUES FROM ('2025-01-01') TO ('2025-12-31');
```

---

### ✅ Practice Challenge

> Create a `logs` table partitioned by month using range partitioning. Then write a query that retrieves logs from September 2025 using partition pruning.

---

Let’s wrap up your SQL Mastery Workbook with **Module 15: Error Handling**, Saibabu — a vital skill for building resilient, audit-proof, and automation-ready SQL systems that don’t break under pressure.

---

## 🛡️ Module 15: Error Handling

### 🔹 15.1 Why Error Handling Matters

Error handling ensures:
- Transactions don’t leave data in an inconsistent state
- Failures are logged and traceable
- Automation scripts can recover or retry safely

---

### 🔸 15.2 Basic Transactional Safety

```sql
BEGIN;

-- risky operation
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

COMMIT;
```

If anything fails, use:
```sql
ROLLBACK;
```

---

### 🔸 15.3 SAVEPOINT and Partial Rollback

```sql
BEGIN;

UPDATE inventory SET stock = stock - 1 WHERE item_id = 101;
SAVEPOINT after_stock;

UPDATE payments SET status = 'paid' WHERE order_id = 202;

-- simulate failure
ROLLBACK TO after_stock;

COMMIT;
```

- Only rolls back to the savepoint
- Keeps earlier successful steps

---

### 🔸 15.4 Error Handling in Stored Procedures (MySQL)

```sql
DECLARE CONTINUE HANDLER FOR SQLEXCEPTION
BEGIN
  INSERT INTO error_log VALUES ('update_salary failed', NOW());
END;
```

- Catches SQL errors
- Logs them without crashing the procedure

---

### 🔸 15.5 Error Handling in PL/pgSQL (PostgreSQL)

```sql
BEGIN
  -- risky logic
EXCEPTION
  WHEN OTHERS THEN
    INSERT INTO error_log VALUES (CURRENT_TIMESTAMP, 'Failure in procedure');
    RAISE NOTICE 'Error handled';
END;
```

- `WHEN OTHERS` catches all exceptions
- You can log, notify, or re-raise

---

### 🔸 15.6 Retry Logic (Advanced)

Use loops or external scripts to retry failed operations:
```sql
DO $$
DECLARE
  attempts INT := 0;
BEGIN
  LOOP
    BEGIN
      -- risky operation
      EXIT;
    EXCEPTION
      WHEN OTHERS THEN
        attempts := attempts + 1;
        IF attempts >= 3 THEN
          RAISE NOTICE 'Max retries reached';
          EXIT;
        END IF;
    END;
  END LOOP;
END $$;
```

---

### 🔸 15.7 Logging Errors

```sql
CREATE TABLE error_log (
  error_time TIMESTAMP,
  error_message TEXT,
  user_name TEXT
);

-- Insert on failure
INSERT INTO error_log VALUES (NOW(), 'Payment failed', CURRENT_USER);
```

- Supports audit trails
- Can be integrated with monitoring tools

---

### 🧪 Real-World Use Case

**Scenario**: You’re automating a payment + inventory update. If either fails, rollback and log the error.

```sql
BEGIN;

UPDATE inventory SET stock = stock - 1 WHERE item_id = 101;
SAVEPOINT after_stock;

UPDATE payments SET status = 'paid' WHERE order_id = 202;

-- simulate failure
ROLLBACK TO after_stock;
INSERT INTO error_log VALUES (NOW(), 'Payment update failed', CURRENT_USER);

COMMIT;
```

---

### ✅ Practice Challenge

> Write a stored procedure that deducts stock and updates payment status. If any step fails, rollback and log the error with a timestamp and user name.

---
Absolutely, Saibabu! Here's your complete **SQL Mastery Workbook (Modules 1–15)** in a clean, structured **tabular format** — perfect for quick reference, reporting, and automation prep. Each module includes key concepts, examples, and use cases.

---

## 📘 SQL Mastery Workbook Summary

| Module | Title                        | Key Concepts                                                                 | Example / Use Case                                                                 |
|--------|------------------------------|------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| 1      | Query Optimization           | Indexing, EXPLAIN, filter pushdown, query hints                             | `CREATE INDEX idx_id ON emp(id);`<br>`EXPLAIN SELECT * FROM emp WHERE id = 101;`  |
| 2      | Window Functions             | `RANK()`, `ROW_NUMBER()`, `LEAD()`, `LAG()`, `NTILE()`                      | `RANK() OVER (PARTITION BY dept ORDER BY salary DESC)`                            |
| 3      | Analytical Functions         | `ROLLUP`, `CUBE`, `GROUPING SETS`, `PIVOT`, `UNPIVOT`                       | `GROUP BY GROUPING SETS ((region), (product), ())`                                |
| 4      | Joins Mastery                | `INNER`, `LEFT`, `RIGHT`, `FULL`, `SELF`, `CROSS`, `ANTI`, `SEMI`          | `JOIN departments d ON e.dept_id = d.id`                                          |
| 5      | Set Operations               | `UNION`, `UNION ALL`, `INTERSECT`, `EXCEPT`                                 | `SELECT email FROM customers EXCEPT SELECT email FROM unsubscribed_users`         |
| 6      | Recursive Queries            | `WITH RECURSIVE`, anchor + recursive member, hierarchy traversal            | Org chart: `SELECT name, manager_id, level FROM org_chart`                        |
| 7      | Transactions & Control       | `BEGIN`, `COMMIT`, `ROLLBACK`, `SAVEPOINT`, isolation levels                | `ROLLBACK TO savepoint_name;`<br>`SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;` |
| 8      | Constraints & Integrity      | `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `CHECK`, `DEFAULT`                 | `CHECK (age >= 18)`<br>`FOREIGN KEY (dept_id) REFERENCES departments(id)`         |
| 9      | Triggers & Procedures        | `AFTER INSERT`, `BEFORE UPDATE`, stored procedures with parameters          | `CREATE TRIGGER log_insert AFTER INSERT ON employees`                             |
| 10     | Materialized Views           | `CREATE MATERIALIZED VIEW`, `REFRESH`, indexing, snapshot reporting         | `REFRESH MATERIALIZED VIEW sales_summary;`                                        |
| 11     | Dynamic SQL                  | `PREPARE`, `EXECUTE`, `format()`, safe parameterization                     | `EXECUTE format('SELECT * FROM %I WHERE id = %L', tbl, id);`                      |
| 12     | Security & Access            | `GRANT`, `REVOKE`, roles, RLS, auditing                                     | `REVOKE SELECT ON employees FROM analyst;`<br>`CREATE POLICY dept_filter ...`     |
| 13     | Data Types & Casting         | `CAST()`, `TO_DATE()`, implicit vs explicit, custom types                   | `CAST('2025-09-23' AS DATE)`<br>`CREATE TYPE status_enum AS ENUM (...)`           |
| 14     | Partitioning & Sharding      | `PARTITION BY RANGE`, `LIST`, `HASH`, sharding strategies                   | `CREATE TABLE sales PARTITION BY RANGE (sale_date)`                               |
| 15     | Error Handling               | `EXCEPTION`, `ROLLBACK`, retry logic, logging                               | `BEGIN ... EXCEPTION WHEN OTHERS THEN INSERT INTO error_log ...`                  |

---

Would you like me to organize this into a Copilot Page so you can edit, reuse, and expand it anytime? Or shall we start building exercises and cheat sheets for each module?
