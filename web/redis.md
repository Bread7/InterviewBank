# REDIS

## What exactly is Redis?

An abbreviation for **RE**mote **DI**ctionary **S**erver (REDIS). The server is used for its in-memory key-value database, for caching and message brokering.

<figure><img src="../.gitbook/assets/Pasted image 20240704135642.png" alt=""><figcaption><p>Image of how REDIS is typically used</p></figcaption></figure>

The default port of REDIS is 6379.

Unlike NoSQL databases such as MongoDB and PostgreSQL, REDIS keeps its data in memory for normal operations.

NoSQL (“Not Only SQL”) describes databases that, unlike SQL, are non-relational, and cannot be organized in tables, among other things. Therefore, these approaches can also be distributed across different computer systems and are highly scalable. Suitable for most Big Data applications.

Since REDIS holds data in memory, it is good for very fast reads and writes. This means good performance in speed-reliant services such as [`tokens`](jwt/) management, one-time pin storages, and lightweight checks.

## So means cannot use hard disk?

Wrong. REDIS can persist in a hard disk if required by the use of RDB (Redis DataBase) snapshots and AOF (Append-Only File). &#x20;



<figure><img src="../.gitbook/assets/Pasted image 20240704154758 (3).png" alt=""><figcaption><p>Some different ways to setup REDIS persistence</p></figcaption></figure>

**RDB:**

A snapshot of the data in memory is taken and saved to disk at specified intervals. The snapshot can be configured to be automatic, manual, or both. RDB is a good option when using Redis as a cache, as it allows for quick recovery of data in the event of a crash.

**AOF:**

Logs every write operation to a log file persistently. These log files contain a sequence of write operations that can be used to reconstruct the dataset. AOF persistence is more durable than RDB snapshots because it logs every write operation, but it can be less efficient in terms of disk space usage and performance.



**Now we know what REDIS is, what are some attack vectors to look out for when securing REDIS?**

**Exposure of REDIS to the Public Internet:**

One of the most significant risks is exposing Redis directly to the internet without proper access controls. Ensure that Redis instances are deployed within a trusted network and not accessible from the public internet. Use security groups and IP whitelisting to restrict access to only necessary services​​.

**Weak Authentication and Access Controls:**

Some REDIS authentication mechanisms are disabled by users when configuring user access controls. This includes the use of weak passwords, default user accounts, and a lack of role-based access controls (RBAC). This usually means that users could make changes to the database without a password or using an easy password to make changes that affect files within the server.

The lack of access controls could result in file overwrite that could potentially append SSH keys/malicious binaries etc. to the machine, providing attackers with remote code execution when pulled off successfully.

**Command Injection via Slave:**

If an attacker gains control over a slave instance, they could potentially execute arbitrary commands on the master by sending specially crafted replication commands. This is a critical risk if the slave instance is compromised​.

**Existing RCEs for attacking REDIS:**

There are several RCEs that exist for REDIS that are exposed to the public carelessly, with the right configurations, they could be exploited by known scripts that target older versions of REDIS (<=5.0.5). A number of GitHub repos exist with such exploits.

**REDIS SAVE Command abuse:**

In most production environments, you rarely want to let a user use the save command, most of the REDIS exploits out there are targeting this command, as it performs a **synchronous** save of the dataset producing a snapshot of all the data inside the Redis instance, in the form of an RDB file.

This is unideal for production due to the reason that it will block all other clients during this process to save the data in the memory to a snapshot. For security, this is dangerous for the production side as attackers can use this save function to save files to the server. If poor access controls are implemented, LFI could incur resulting in RCE.

Questions to ponder:

* What is Redis and what are its primary use cases?
* What are the differences between Redis and traditional databases like MySQL?
* How does Redis handle data eviction when it runs out of memory? (Can do a quick search)
* What security measures should be taken when deploying Redis?

References

* [https://www.ibm.com/topics/redis](https://www.ibm.com/topics/redis)
* [https://databasecamp.de/en/data/redis-en](https://databasecamp.de/en/data/redis-en)
* [https://redis.io/docs/latest/commands/save/](https://redis.io/docs/latest/commands/save/)
* [https://www.quora.com/Can-Redis-permanently-store-data-on-the-hard-disk](https://www.quora.com/Can-Redis-permanently-store-data-on-the-hard-disk)
* [https://book.hacktricks.xyz/network-services-pentesting/6379-pentesting-redis](https://book.hacktricks.xyz/network-services-pentesting/6379-pentesting-redis)
* [https://backendless.com/redis-what-it-is-what-it-does-and-why-you-should-care/](https://backendless.com/redis-what-it-is-what-it-does-and-why-you-should-care/)
