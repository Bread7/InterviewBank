# How MySQL executes query

MySQL, as one of the most popular relational databases, is particularly favored by interviewers. Today, we bring you the first classic question in the MySQL interview series: **What happens when MySQL executes an SQL query?**

\
We can consider the query requirements that MySQL needs to support: multiple clients connecting to MySQL simultaneously, using SQL statements to create, read, update, and delete data. For query scenarios, MySQL must ensure that results are returned to the client as quickly as possible.

Having understood these requirement scenarios, we might design MySQL as follows:

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

### **1) Connector**

Manages client connections and is responsible for authorization verification. When a user sends an SQL query statement via the client, first, the MySQL server checks the client's connection information (such as username and password) and establishes a connection. After a successful connection, the server processes the SQL query statement.

MySQL also offers a range of parameter settings such as maximum number of connections and connection timeout to control client connection behavior.

### **2) Query Cache**

If a query has been executed before, MySQL will store the result in the query cache. If the same statement is queried again, the result will be returned directly. However, the query cache can become ineffective due to frequent updates, as any update to a table clears the cache for that table.

This makes the feature somewhat useless, so we can disable the query cache by setting the `query_cache_type` parameter to `DEMAND`.

It should be noted that MySQL 8.0 completely removed the query cache feature, so it no longer exists from version 8.0 onwards.

### **3) SQL Parser**

The SQL parser analyzes the syntax structure of the SQL statement, performing lexical and syntactic analysis, and checks whether it conforms to SQL grammar specifications. If not, it returns an error message. Afterwards, a syntax tree is constructed.

Example:

```sql
mysql> selec * from t where ID=1;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'selec * from t where ID=1' at line 1
```

### **4) Preprocessor**

The preprocessor further checks whether the table names and column names in the SQL statement exist, replaces `select *` with specific columns, and checks if the user has the necessary access rights.

### **5) Optimizer**

The optimizer selects the most optimal execution plan based on information like the data distribution of the table and indexes. This step is crucial for improving query efficiency.

The optimizer may rewrite the query, such as changing the order of joins or selecting the most suitable index.

### **6) Executor**

The query execution engine, following the optimized execution plan, calls the storage engine's API to perform actual data read or modification operations, including primary key index queries and index pushdown.

If the query involves multiple tables, MySQL will use appropriate algorithms (such as nested loops, hash join) to perform table join operations.

Afterwards, the query results are collected and formatted, then returned to the client over the network. If the query results are too large, they may be returned in batches.

## Author

[Ik0nw](https://github.com/Ik0nw)

## Reference

[https://mp.weixin.qq.com/s/oN9Qbt2gVDaMph604S6Czg](https://mp.weixin.qq.com/s/oN9Qbt2gVDaMph604S6Czg)
