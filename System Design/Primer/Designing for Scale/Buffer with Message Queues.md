High-concurrency scenarios often encounter write-intensive operations. Frequent database writes can overload the system due to disk I/O bottlenecks. Message queues can buffer write requests, changing synchronous operations into asynchronous ones, thereby limiting database write requests to manageable levels and preventing system crashes.

![Buffer Requests with Message Queue](https://systemdesignschool.io/fundamentals/scaling/message-queue.png)

We will cover message queues in great detail in the [Message Queues](https://systemdesignschool.io/fundamentals/message-queue) section.