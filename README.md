
Here are some **important database interview questions** for mid-level and senior developers. I've grouped them by topic for clarity:

---

### **1. General Database Concepts**
- What are the differences between **SQL** and **NoSQL** databases? When would you choose one over the other?
- Explain database normalization and its importance. What are the **normal forms**?
- What is **denormalization**? When would you use it?
- What is an **index**, and how does it improve performance? What are the downsides of over-indexing?
- Explain the differences between a **primary key**, **unique key**, and **foreign key**.
- What is the **ACID** property in databases, and why is it important?
- Explain the difference between **OLTP** and **OLAP** systems.

---

### **2. Query Optimization**
- How would you optimize a slow SQL query?
- Explain the use of **EXPLAIN** or **QUERY PLAN** tools in analyzing query performance.
- What is a **query execution plan**, and how can you use it to improve performance?
- How would you handle **n+1 query problems** in relational databases?
- What are **partitioning** and **sharding**? How do they impact performance?

---

### **3. Transactions and Concurrency**
- Explain the difference between **pessimistic locking** and **optimistic locking**.
- What are database isolation levels? Can you explain the differences between **READ UNCOMMITTED**, **READ COMMITTED**, **REPEATABLE READ**, and **SERIALIZABLE**?
- How would you handle **deadlocks** in a database?
- What is a **two-phase commit**? When would you use it?

---

### **4. NoSQL Databases**
- What are the main types of NoSQL databases (e.g., **document stores**, **key-value stores**, **graph databases**, **column-family stores**)? Provide examples.
- Explain the **CAP theorem** and how it applies to distributed systems.
- How would you design a schema in a document database like MongoDB?
- What is the difference between a **replica set** and **sharded cluster** in MongoDB?

---

### **5. Database Design**
- How would you design a schema for a social media application (e.g., users, posts, likes, and comments)?
- What is an **ER diagram**, and how do you use it for database design?
- How do you manage relationships in a database (one-to-one, one-to-many, many-to-many)?
- How would you design a database to scale horizontally?

---

### **6. Performance and Scalability**
- How do you handle large datasets in a database?
- What are the differences between **vertical scaling** and **horizontal scaling** in databases?
- What is **read/write splitting**, and how can it improve performance?
- Explain **caching strategies** for databases (e.g., Redis, Memcached).
- How would you design a database to handle millions of concurrent transactions?

---

### **7. Backup, Security, and Maintenance**
- How do you ensure database backups are consistent and reliable?
- What are **database migrations**, and how do you handle them in a live environment?
- How do you implement **role-based access control** (RBAC) in a database?
- What are some strategies to secure sensitive data in a database (e.g., encryption, masking)?
- How do you handle schema changes in a production database?

---

### **8. Tools and Frameworks**
- Have you used any ORMs (e.g., TypeORM, Sequelize, Hibernate)? What are the pros and cons of using an ORM?
- What tools have you used for database monitoring and performance analysis?
- How would you use a tool like **pgAdmin**, **Mongo Compass**, or **MySQL Workbench** for database management?

---


# MYSQL-Notes

## What are the differences between **SQL** and **NoSQL** databases? When would you choose one over the other?
  
---

### **1. SQL Databases**

#### **Key Features**
- **Structured Data**: Data is stored in predefined rows and columns.
- **Relationships**: SQL databases enforce relationships through foreign keys.
- **Schema-Driven**: You must define the schema upfront, meaning you know the structure of the data ahead of time.
- **Strong Consistency (ACID)**: Transactions in SQL databases guarantee **Atomicity**, **Consistency**, **Isolation**, and **Durability**.

#### **Common Examples of SQL Databases**
- **MySQL**: Open-source, widely used for web applications.
- **PostgreSQL**: Supports advanced features like JSON, full-text search, and custom extensions.
- **Oracle Database**: Enterprise-grade SQL database with high scalability and support.
- **Microsoft SQL Server**: Microsoft’s robust relational database with integration into the .NET ecosystem.

#### **Example Scenario: Banking System**
A banking application requires:
1. Strict data integrity.
2. Complex relationships like customers, accounts, and transactions.
3. ACID-compliant transactions to ensure financial accuracy.

**Schema:**
```sql
CREATE TABLE customers (
  id INT PRIMARY KEY,
  name VARCHAR(255),
  email VARCHAR(255)
);

CREATE TABLE accounts (
  id INT PRIMARY KEY,
  customer_id INT,
  balance DECIMAL(10, 2),
  FOREIGN KEY (customer_id) REFERENCES customers(id)
);

CREATE TABLE transactions (
  id INT PRIMARY KEY,
  account_id INT,
  amount DECIMAL(10, 2),
  type ENUM('debit', 'credit'),
  transaction_date DATETIME,
  FOREIGN KEY (account_id) REFERENCES accounts(id)
);
```

**SQL Query Example:**
```sql
SELECT c.name, a.balance, t.type, t.amount
FROM customers c
JOIN accounts a ON c.id = a.customer_id
JOIN transactions t ON a.id = t.account_id
WHERE c.id = 1;
```

---

### **2. NoSQL Databases**

#### **Key Features**
- **Flexible Schema**: Data can be added without strictly defined schemas, allowing for unstructured or semi-structured data.
- **Horizontal Scalability**: Data is distributed across multiple servers, making it ideal for handling massive workloads.
- **High Availability**: NoSQL databases prioritize availability (eventual consistency) in distributed systems.
- **Optimized for Specific Use Cases**:
  - Document Stores (e.g., MongoDB): JSON-like documents.
  - Key-Value Stores (e.g., Redis): Simple key-value pairs.
  - Column Stores (e.g., Cassandra): Data stored in columns for analytics.
  - Graph Databases (e.g., Neo4j): Nodes and edges for relationship-heavy use cases.

#### **Common Examples of NoSQL Databases**
- **MongoDB**: Flexible document-based database, great for dynamic schemas.
- **Cassandra**: Distributed database for high availability and scalability.
- **Redis**: In-memory key-value store for caching and real-time use cases.
- **Neo4j**: Optimized for graph-based data and relationship queries.

#### **Example Scenario: E-commerce Product Catalog**
An e-commerce platform needs:
1. Flexible product schemas (e.g., some products have different attributes like color, size, or specs).
2. High write-read throughput for handling customer searches and updates.

**MongoDB Document Example:**
```json
{
  "_id": "p123",
  "name": "Laptop",
  "brand": "TechBrand",
  "price": 1200,
  "specifications": {
    "CPU": "Intel i7",
    "RAM": "16GB",
    "Storage": "512GB SSD"
  },
  "categories": ["electronics", "laptops"],
  "ratings": {
    "average": 4.5,
    "reviews": 125
  }
}
```

**MongoDB Query Example:**
```javascript
// Find laptops by brand and filter by price
db.products.find({
  brand: "TechBrand",
  price: { $lt: 1500 }
});
```

---

### **3. ACID vs BASE**

| **Feature**               | **SQL (ACID)**                                                                                          | **NoSQL (BASE)**                                                                                         |
|---------------------------|--------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| **Atomicity**             | Ensures all steps in a transaction are completed; if not, none are executed.                           | Transactions may be incomplete; eventual consistency is prioritized.                                   |
| **Consistency**           | Ensures the database remains in a valid state after a transaction.                                     | Data may temporarily violate consistency but becomes consistent over time.                             |
| **Isolation**             | Transactions do not interfere with one another.                                                       | High availability is prioritized over isolation.                                                       |
| **Durability**            | Changes persist, even in case of failure.                                                             | Durability may depend on replication and backups rather than immediate writes.                         |
| **Performance**           | More predictable but can be slower due to transaction overhead.                                       | Optimized for performance, especially in distributed environments.                                     |

---

### **4. Use Case Comparisons**

| **Use Case**                          | **SQL Example**                                                                                       | **NoSQL Example**                                                                                     |
|---------------------------------------|-------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| **Financial Applications**            | Banking, payroll (e.g., MySQL, PostgreSQL)                                                            | Rarely used due to lack of strong ACID compliance.                                                    |
| **Content Management Systems**        | Media libraries with structured metadata (e.g., PostgreSQL)                                           | Flexible content storage, e.g., MongoDB for storing blog posts with dynamic metadata.                 |
| **Real-Time Applications**            | Stock trading platforms (e.g., SQL Server)                                                            | IoT applications or real-time messaging, e.g., Redis for low-latency key-value lookups.               |
| **Big Data Analytics**                | Data warehouses like Amazon Redshift (SQL-based).                                                     | Column stores like Cassandra or HBase for distributed, large-scale analytics.                         |
| **Social Networks**                   | Managing structured profiles with relationships in SQL.                                               | Storing graph-like relationships (e.g., Neo4j for "friends-of-friends" queries).                      |
| **Online Marketplaces**               | Order and transaction data with strict consistency (e.g., MySQL).                                     | Product catalogs and reviews with dynamic fields, e.g., MongoDB or Elasticsearch for search.          |

---

### **Differences Between SQL and NoSQL Databases**

| **Aspect**              | **SQL Databases**                                                                                     | **NoSQL Databases**                                                                                  |
|--------------------------|------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Structure**           | Relational (Tables with rows and columns)                                                            | Non-relational (Documents, key-value pairs, wide-column stores, or graphs)                         |
| **Schema**              | Predefined, rigid schema (must define tables, columns, and data types before inserting data)         | Flexible schema (can store unstructured or semi-structured data)                                   |
| **Scalability**         | Vertically scalable (increase hardware resources like CPU/RAM of a single server)                    | Horizontally scalable (add more servers to the cluster for scaling out)                            |
| **Query Language**      | Uses SQL (Structured Query Language) for defining and manipulating data                              | Varies based on database type (e.g., MongoDB uses JSON-like queries, Cassandra uses CQL)           |
| **Transactions**        | Strong support for ACID transactions                                                                | Often uses BASE (Basically Available, Soft state, Eventual consistency); limited ACID support      |
| **Performance**         | Performs well for complex queries and joins                                                          | Optimized for high read/write operations and large datasets with simple queries                    |
| **Use Cases**           | Suitable for applications with structured data and relationships                                     | Suitable for unstructured or semi-structured data, large-scale and distributed systems             |

---


### **5. Scalability & Performance**

| **Feature**              | **SQL**                                                                                               | **NoSQL**                                                                                              |
|--------------------------|------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| **Scaling**              | Vertical scaling (upgrading server resources like RAM/CPU).                                          | Horizontal scaling (adding nodes to distribute load).                                                 |
| **Write Performance**    | Slower for large-scale writes due to transaction overhead.                                            | Optimized for high write workloads (e.g., time-series data in Cassandra).                             |
| **Read Performance**     | Excellent for structured data with indexes and joins.                                                | Excellent for distributed queries or key-based lookups.                                               |

---

### **6. Example Real-World Applications**
#### **SQL Example: Uber Rides**
Uber uses PostgreSQL for relational data like:
- User profiles.
- Payment details.
- Trip records with precise consistency needs.

#### **NoSQL Example: Netflix Streaming**
Netflix uses Cassandra for:
- Tracking real-time data (e.g., user views, recommendations).
- Storing billions of events per day, where eventual consistency is acceptable.

---

### **Conclusion**
- Choose **SQL** when your application needs structured data, strong consistency, and complex queries.
- Choose **NoSQL** when you need flexibility, high scalability, and high availability, particularly for unstructured or distributed systems.





## Transaction States in DBMS

A transaction is a series of operations or tasks that work together to complete a specific process, which may involve changing data in a database. To handle issues like system failures, transactions go through different states.

A transaction state shows the current stage or condition of a transaction as it runs in the database. It tracks the progress and determines whether the transaction will finish successfully (commit) or stop due to an issue (abort).

## Different Types of Transaction States in DBMS

![image](https://github.com/user-attachments/assets/2e0ce364-5934-49b6-9d89-8d079975aae9)

### Simplified Explanation of Transaction States

1. **Active State**  
   - The transaction starts, and its instructions (like insert, update, delete) are being executed.  
   - Changes are not yet saved to the database but kept temporarily in memory.

2. **Partially Committed**  
   - The transaction finishes all its operations but hasn't saved the changes permanently to the database yet.  
   - If successful, it moves to the "Committed" state; if something fails, it moves to the "Failed" state.

3. **Failed State**  
   - If an error occurs during the transaction, it stops, and the system ensures the database remains consistent.  
   - The database recovery system may attempt to fix the issue or terminate the transaction.

4. **Aborted State**  
   - If recovery from the failed state isn’t possible, the transaction is rolled back (undone) to restore the database to its original state.  
   - The transaction may either be canceled or restarted.

5. **Committed State**  
   - The transaction successfully saves its changes permanently to the database.  
   - This marks the completion of the transaction.

6. **Terminated State**  
   - After committing or rolling back, the transaction ends, and the system is ready for a new transaction.

---

### Example: Bank Transaction ($500 transfer from Account A to Account B)
1. **Active State:**  
   The transaction starts and reads Account A’s balance ($1000). It checks if there are sufficient funds.  

2. **Partially Committed:**  
   Deducts $500 from Account A ($1000 - $500 = $500) and temporarily updates Account B’s balance ($1500).

3. **Committed:**  
   Changes are saved permanently:  
   - Account A = $500  
   - Account B = $1500  

4. **Failed State:**  
   If something goes wrong (e.g., system crash), the transaction stops.  
   - Example: $500 is deducted from Account A but not added to Account B.

5. **Aborted State:**  
   The failed transaction is rolled back:  
   - Account A = $1000  
   - Account B = $1000  

6. **Terminated State:**  
   The transaction ends after committing or rolling back. The system is ready for a new transaction.  

This process ensures the database remains consistent, either completing the transaction fully or undoing changes in case of failure.



### What is Second Normal Form (2NF)?

Second Normal Form (2NF) is a step in database normalization that ensures the elimination of partial dependencies, which occur when non-key columns depend on only part of a composite primary key. 

To achieve 2NF, two rules must be satisfied:
1. **Be in 1NF**: The table must have a primary key, and all values must be atomic (no repeating groups or arrays).
2. **No Partial Dependencies**: All non-key columns must depend on the **entire primary key**, not just a part of it.

---

### Example of Moving to 2NF

#### 1NF Example: A Single Table

| MembershipID | MemberName | MovieRented | RentalDate |
|--------------|------------|-------------|------------|
| 101          | John Doe   | Matrix      | 2025-01-15 |
| 101          | John Doe   | Inception   | 2025-01-16 |
| 102          | Jane Roe   | Avatar      | 2025-01-15 |

- **Primary Key**: Composite key `(MembershipID, MovieRented)` uniquely identifies each row.
- Issue: Non-key column `MemberName` depends only on `MembershipID`, not the entire composite key. This is a **partial dependency**.

---

#### Why Is This Not in 2NF?

- `MemberName` depends **only on MembershipID** (part of the composite key).
- `MovieRented` and `RentalDate` depend on the entire composite key `(MembershipID, MovieRented)`.

This violates 2NF because not all non-key columns depend on the entire primary key.

---

#### Moving to 2NF: Splitting the Table

To remove the partial dependency, divide the table into two:

1. **Table 1: Members**
   | MembershipID | MemberName |
   |--------------|------------|
   | 101          | John Doe   |
   | 102          | Jane Roe   |

   - **Primary Key**: `MembershipID`
   - `MemberName` depends only on `MembershipID`.

2. **Table 2: Rentals**
   | MembershipID | MovieRented | RentalDate |
   |--------------|-------------|------------|
   | 101          | Matrix      | 2025-01-15 |
   | 101          | Inception   | 2025-01-16 |
   | 102          | Avatar      | 2025-01-15 |

   - **Primary Key**: Composite key `(MembershipID, MovieRented)`
   - `RentalDate` depends on the entire composite key.

---

### Foreign Key and Its Role

- In **Table 2**, `MembershipID` is now a **foreign key** that links back to **Table 1**.
- A **foreign key**:
  1. References the **primary key** of another table (here, `MembershipID` in **Table 1**).
  2. Helps establish relationships between tables (connects members to their rentals).
  3. Allows rows in one table to correspond to rows in another.

---

### Key Points About Foreign Keys
- A **foreign key** can have a name different from the primary key it references.
- **Foreign keys do not need to be unique**, as multiple rows in the child table can reference the same row in the parent table.
- Unlike primary keys, **foreign keys can contain NULL values**, meaning the reference can be optional.

---

### Benefits of 2NF and Foreign Keys
- Eliminates **partial dependencies**, reducing data redundancy.
- Organizes data into separate, related tables, improving database consistency.
- **Foreign keys** ensure relationships between tables remain valid, supporting data integrity.

### What is a Transitive Functional Dependency?

A **transitive functional dependency** happens when a non-key column in a database table indirectly depends on the primary key **through another non-key column**.

### Breaking It Down:

1. A **functional dependency** means one column's value determines another column's value.  
   Example: In a `Students` table, `StudentID → Name` means the `StudentID` determines the `Name`.

2. **Transitive dependency** means this happens indirectly.  
   - Column A (Primary Key) determines Column B.  
   - Column B determines Column C.  
   - Therefore, Column A indirectly determines Column C.  

This indirect dependency is a **transitive functional dependency**.

---

### Example:

Consider a `Students` table:

| StudentID | DepartmentID | DepartmentName |
|-----------|--------------|----------------|
| 101       | D01          | Computer Science |
| 102       | D02          | Mathematics      |

- **Primary Key**: `StudentID`  
- **Functional Dependency**:  
  - `StudentID → DepartmentID` (StudentID determines DepartmentID).  
  - `DepartmentID → DepartmentName` (DepartmentID determines DepartmentName).  
  - So, `StudentID → DepartmentName` is a transitive dependency because `StudentID` indirectly determines `DepartmentName` through `DepartmentID`.

---

#### Why Is This a Problem?
- Transitive dependencies can lead to **data redundancy** and **anomalies**.  
- Example: If the department name changes, you must update it everywhere it appears in the table.


#### Solution:

- **Normalize the table** to eliminate transitive dependencies by creating separate tables for related data.  
- For the example, split the table into two:  
  1. **Students Table**: `StudentID`, `DepartmentID`  
  2. **Departments Table**: `DepartmentID`, `DepartmentName`  

This ensures data consistency and avoids redundancy.


## What is an **index**, and how does it improve performance? What are the downsides of over-indexing?

### **What is an Index?**

An **index** in a database is a data structure that improves the speed of data retrieval operations on a table at the cost of additional storage and write performance. It works similarly to an index in a book, where you can quickly locate a topic without scanning every page. Indexes are created on columns in a database table to enable faster lookups for specific queries.

#### **How Does an Index Work?**

Indexes typically use data structures like:
- **B-trees**: Used by most relational databases (e.g., MySQL, PostgreSQL) for range queries and ordered data.
- **Hash tables**: Used for exact match lookups (e.g., some NoSQL databases like MongoDB for specific indexes).
- **Bitmap indexes**: Used for columns with low cardinality (e.g., gender, status).
- **Inverted indexes**: Common in full-text search engines (e.g., Elasticsearch).

When you query a table, the database checks if an appropriate index exists:
- If an index is available, the database performs a **direct lookup** instead of scanning the entire table.
- If no index exists, the database performs a **full table scan**, which is slower for large datasets.

---

### **Types of Indexes**

1. **Primary Index**: Automatically created on the primary key column(s) of a table.
2. **Unique Index**: Ensures no duplicate values in the indexed column(s).
3. **Composite Index**: Created on multiple columns to optimize queries involving those columns.
4. **Clustered Index**: Determines the physical storage order of data in a table (e.g., used in SQL Server).
5. **Non-Clustered Index**: Separate from the table data; contains pointers to the actual data.
6. **Full-Text Index**: Optimized for searching text-based data (e.g., searching keywords within a document).

---

### **How Does an Index Improve Performance?**

1. **Speeds Up Data Retrieval**:
   - Instead of scanning every row in the table (full table scan), the database uses the index to directly locate the relevant rows.
   - Example:
     ```sql
     SELECT * FROM employees WHERE last_name = 'Smith';
     ```
     If an index exists on the `last_name` column, the query engine uses it to fetch results faster.

2. **Optimizes Search Operations**:
   - Indexes are ideal for queries with conditions like `WHERE`, `ORDER BY`, `GROUP BY`, `LIMIT`, and `JOIN`.
   - Example:
     ```sql
     SELECT * FROM orders WHERE order_date BETWEEN '2025-01-01' AND '2025-01-10';
     ```

3. **Improves Sorting and Aggregation**:
   - Sorting and grouping operations are faster because indexes are often stored in an ordered manner.

4. **Enables Quick Joins**:
   - Indexes on foreign keys improve join performance significantly.
   - Example:
     ```sql
     SELECT * 
     FROM orders 
     JOIN customers ON orders.customer_id = customers.id;
     ```

---

### **Downsides of Over-Indexing**

While indexes improve read performance, over-indexing can introduce significant drawbacks:

#### **1. Increased Storage Requirements**
- Each index consumes additional disk space.
- Example: If you have 5 indexes on a table with millions of rows, the index files can grow significantly, impacting storage costs.

#### **2. Slower Write Performance**
- **Insert, Update, and Delete operations** become slower because the database needs to update all relevant indexes.
- Example: Adding a row to a table with multiple indexes requires the database to update the index structure for each indexed column.

#### **3. Maintenance Overhead**
- Indexes require maintenance, especially during bulk inserts or schema migrations.
- Fragmentation can occur over time, requiring reindexing for optimal performance.

#### **4. Query Optimization Pitfalls**
- If too many indexes exist, the query optimizer may choose a suboptimal index, resulting in inefficient query execution.

#### **5. Diminishing Returns for Low-Cardinality Columns**
- Indexes on low-cardinality columns (e.g., gender, boolean flags) are less effective because they don’t significantly reduce the number of rows scanned.
- Example: An index on a column with only two distinct values (`true/false`) is unlikely to improve performance.

#### **6. Risk of Redundant Indexes**
- Creating overlapping or redundant indexes can lead to unnecessary storage and maintenance overhead.
- Example:
  - Index 1: `(col1)`
  - Index 2: `(col1, col2)`
  - In many cases, Index 2 can cover queries optimized by Index 1, making Index 1 redundant.

---

### **Best Practices for Indexing**

1. **Use Indexes Strategically**:
   - Create indexes on columns frequently used in `WHERE`, `JOIN`, `ORDER BY`, or `GROUP BY` clauses.
   - Avoid indexing columns that are rarely queried or have low cardinality.

2. **Monitor Index Usage**:
   - Use tools like `EXPLAIN` (MySQL/PostgreSQL) or `EXPLAIN PLAN` (Oracle) to analyze query execution plans and determine which indexes are being used.
   - Identify unused indexes and drop them.

3. **Limit Composite Indexes**:
   - Only create composite indexes for queries that filter by multiple columns.
   - Example:
     ```sql
     SELECT * FROM orders WHERE customer_id = 5 AND order_date = '2025-01-10';
     ```
     A composite index on `(customer_id, order_date)` is more efficient than separate indexes.

4. **Rebuild and Reorganize Indexes**:
   - Periodically rebuild or reorganize indexes to prevent fragmentation in write-heavy workloads.

5. **Use Covering Indexes**:
   - Include all columns needed for the query in the index to avoid extra lookups.
   - Example:
     ```sql
     CREATE INDEX idx_orders_customer_date ON orders (customer_id, order_date, total_amount);
     ```

6. **Balance Read and Write Needs**:
   - Consider the trade-offs between read and write performance when designing indexes for write-heavy systems.

---

### **Example of Over-Indexing Impact**

#### **Scenario**:
A table `orders` has:
- 1 million rows.
- Columns: `id`, `customer_id`, `order_date`, `total_amount`.

#### **Indexes**:
- Index 1: `customer_id`
- Index 2: `order_date`
- Index 3: `total_amount`

#### **Impact**:
1. **Inserts**: Every time a new order is inserted, all three indexes need to be updated.
   - This increases the time required for each insert operation.
2. **Storage**: Each index takes additional space, significantly increasing storage requirements.
3. **Query Optimization Confusion**: If a query like this is run:
   ```sql
   SELECT * FROM orders WHERE order_date = '2025-01-10' AND total_amount > 1000;
   ```
   The query optimizer might choose an index suboptimally, resulting in slower performance.

#### **Solution**:
- Replace multiple single-column indexes with a composite index:
  ```sql
  CREATE INDEX idx_orders_date_amount ON orders (order_date, total_amount);
  ```

---

### **Conclusion**

Indexes are powerful tools for improving database performance, especially for read-heavy workloads. However, excessive or improper indexing can lead to increased storage, slower writes, and maintenance challenges. The key is to strike a balance between **read performance**, **write performance**, and **storage costs** by carefully analyzing query patterns and monitoring index usage. 




