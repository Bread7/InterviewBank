# Mysql and redis

## Database Types of MySQL and Redis

MySQL is a relational database primarily used for storing persistent data, storing data on the hard disk, which has slower read speeds.

Redis is a NoSQL, non-relational database, also a caching database, storing data in memory. The cached data can be accessed much faster, greatly improving efficiency, but the data has a limited storage time.

## MySQL's Operating Mechanism

MySQL, as a relational database for persistent storage, has a relatively weak point in that every time a request accesses the database, **there are I/O operations**. If the database is accessed repeatedly and frequently, it will: First, **spend a lot of time repeatedly connecting to the database**, leading to **slow performance**; second, repeated database access will also **result in high database load**. Therefore, the concept of caching emerges.

### Caching

Caching is a buffer area (cache) for data exchange. When a browser makes a request, it first searches in the cache. If it exists, it retrieves it; otherwise, it accesses the database.

The benefit of caching is that it speeds up read access.

## Redis Database

The Redis database is a caching database used to store frequently used data, reducing the number of times the database is accessed and improving efficiency.

### Summary of Differences Between Redis and MySQL

#### Type

In terms of type, MySQL is a relational database, while Redis is a caching database.

#### Functionality

MySQL is used for persistently storing data on the hard disk, which is powerful but slow in speed. Redis is used for storing frequently used data in cache, with fast read speeds.

#### Requirements

MySQL and Redis are generally used together due to their different requirements.

#### Storage Location

MySQL stores data on the disk, while Redis stores data in memory.

#### Suitable Data Types for Storage

Redis is suitable for storing some frequently used, hot data. Because it is stored in memory, both read and write speeds are very fast. It is generally applied in scenarios such as leaderboards, counters, message queue pushes, friend followings, and followers.

## Can All Data Be Stored Directly in Redis?

Firstly, it should be understood that MySQL stores data on disk, while Redis stores data in memory. Redis can be used for persistent storage or caching. Currently, most companies use MySQL + Redis, with MySQL as the main storage and Redis as auxiliary storage for caching, speeding up access and improving performance.

Redis stores data in memory. If storing large amounts of data in memory, the storage capacity will be much less than that of disk storage. To store a large amount of data, you would need to spend more money to purchase memory, which can be relatively wasteful in areas that do not require high performance. Therefore, MySQL (main) + Redis (auxiliary) is commonly used, using Redis in areas that require performance and MySQL in areas that do not need high performance, to use resources efficiently.

MySQL supports SQL queries, which can be used for some associated queries and statistics.

Redis requires a high memory requirement and cannot store all data in Redis under limited conditions.

MySQL is more inclined to store data, while Redis is more inclined to quickly retrieve data. However, when Redis queries complex table relationships, it is not as good as MySQL, so you can put popular data in Redis and basic data in MySQL.

## Author

[Ik0nw](https://github.com/Ik0nw)

## Interview Question

1. For a high-traffic e-commerce platform, why might you choose to implement Redis alongside MySQL? What specific benefits does Redis provide in terms of performance and scalability, and how does it complement MySQL's capabilities?
2. Can you compare and contrast the data storage mechanisms of Redis and MySQL? Explain how Redis's in-memory storage differs from MySQL's disk-based storage, and discuss the implications of these differences on performance, data durability, and scalability.
3. In a scenario where real-time data processing and low-latency access are critical requirements, would you recommend using Redis or MySQL as the primary database solution? Provide examples of use cases where each database excels, and explain your rationale for choosing one over the other in such scenarios.
