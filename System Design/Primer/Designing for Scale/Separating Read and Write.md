A system is read-heavy or write-heavy depends on the business requirements. For example, a social media platform is read-heavy because users read more than they write. On the other hand, an IOT system is write-heavy because users write more than they read. This is why we want to separate read and write operations to treat them differently.

Read and write separation typically involves two main strategies. First, replication implements a **leader-follower architecture** where writes occur on the leader, and **followers provide read replicas**.

![Separating Read and Write with Leader-Replica](https://systemdesignschool.io/fundamentals/scaling/replication.png)

Second, the so-called **CQRS (Command Query Responsibility Segregation) pattern** takes read-write separation further by using completely different models for reading and writing data. In CQRS, the system is split into two parts:

- **Command Side (write side)**: Handles all write operations (create, update, delete) using a data model optimized for writes
- **Query Side (read side)**: Handles all read operations using a denormalized data model optimized for reads

Changes from the command side are **asynchronously propagated to the query side**.

![CQRS](https://systemdesignschool.io/fundamentals/scaling/cqrs.png)

For example, a system might use MySQL as the source-of-truth database while employing Elasticsearch for full-text search or analytical queries, and asynchronously sync changes from MySQL to Elasticsearch using MySQL binlog **Change Data Capture (CDC)**.