# Difference between cache and buffer

#### Role of Cache

Cache is used for caching, aiming to **speed up reading**.

The reason for using cache is generally due to a significant speed difference between two devices, causing the resources of the high-speed device to be severely wasted, or one side expects the other side to be as fast as possible. By using an intermediate cache layer, some data that might be used can be stored temporarily, thereby reducing inefficiency and waste caused by the speed difference.

For example, the CPU's high-speed cache stores a portion of memory data, reducing the CPU waiting time caused by the speed difference between the CPU and memory. The CPU's L1, L2, and L3 caches are added due to the speed differences between layers.

Another example is caching database query results in Redis, so that the client doesn't have to retrieve this data from the slower database layer when needed.

#### Role of Buffer

A buffer is used to temporarily store data, accumulating a portion of continuous operations' data and then performing a complete read or write operation on the accumulated data all at once.

For example, a TCP socket has two buffer spaces: send buffer and receive buffer. When it needs to send data, it first writes the data to the send buffer to buffer it for a while. Once a certain amount of data is accumulated (or an immediate send is required), it sends out the data in the send buffer. The sent data is then buffered in the recipient's receive buffer, and the recipient monitors the receive buffer for incoming data and reads it from there.

Buffers are more efficient because the cost of multiple read or write operations is higher than the cost of completing a large read or write operation all at once.

For instance, if you use a small bucket to fetch water from a well to fill a large water tank, and the tank is far from the well, the efficiency is low because a lot of time is wasted traveling back and forth. Every trip wastes this time. In this case, using an intermediate small tank to buffer some water between the well and the large tank can improve efficiency. Once the small tank is full, you can transport the water to the large tank all at once. However, if the large tank is right next to the well, it would be more efficient to pour the water directly into the large tank each time, and adding a buffer layer would be inefficient.

For buffers, the concern is not about persistence but about when to send the data out, such as when to flush, whether to set it to sync mode, and the inefficiencies brought by flush or sync mode.

#### Summary

* **Cache** is designed to increase the speed of reading from I/O devices and RAM by storing frequently accessed data closer to the CPU.
* **Buffer** is designed to increase the efficiency of data transfer between RAM and I/O devices (like hard disks) by accumulating data and reducing the frequency of read/write operations.



