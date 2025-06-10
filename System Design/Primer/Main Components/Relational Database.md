#### Laying the Groundwork

At the heart of most applications lies data, and in many cases, that data needs to be structured in a way that’s easy to understand, query, and maintain. Enter the **relational database**, a workhorse for handling structured data in countless systems.

![Basic Structure Example](https://systemdesignschool.io/fundamentals/sql-database/basic-structure-example.png)

A relational database organizes data into tables, which look a lot like spreadsheets. Each table contains rows (records) and columns (fields). This structure makes it straightforward to link different pieces of data together, thanks to something called relationships. For example, in an e-commerce system, you might have one table for customers, another for orders, and a relationship between them to keep track of who bought what.

But why would you choose a relational database in a system design interview (or real life)? It’s all about consistency and structure. If your app deals with data that must stay accurate and interrelated, like financial transactions, user profiles, or inventory levels, a relational database is often the right choice. It ensures:

- Data Integrity: No weird duplicates or orphaned records thanks to constraints like primary and foreign keys
- Powerful Querying: SQL allows you to filter, join, and analyze data with ease
- ACID Properties: Transactions are guaranteed to be Atomic, Consistent, Isolated, and Durable - essential for systems where accuracy matters

If your interviewer gives you a problem where data needs to be consistent, relational databases are usually your best bet.

#### Technical Explanation

Relational databases are built around a simple but powerful idea: data is organized into tables, and relationships between those tables are defined through keys.

Here’s a quick rundown of the main components in a relational database:

- Tables: Think of tables as the basic building blocks. Each table represents an entity, like Users or Orders. Columns define what kind of data is stored (e.g., name, email, order\_date), and rows hold the actual data.
- Primary Key: A unique identifier for each row. For example, every user might have a unique user\_id.
- Foreign Key: A reference to a primary key in another table, creating relationships. For instance, an order\_id in an Orders table might reference a user\_id in the Users table.

There are also a few kinds of relationships you should know about:

- One-to-One: Each record in Table A links to exactly one record in Table B (e.g., user profiles and user settings)
- One-to-Many: One record in Table A links to many records in Table B (e.g., a single user can place multiple orders)
- Many-to-Many: Many records in Table A link to many in Table B, often using a third table to manage the relationship (e.g., students and classes)

In a system design interview, you may be asked questions about relational databases such as the relationships between entities you define, as well as asked about the schema, which includes info about the tables and keys. For a more in-depth look, check out our course (course here)

#### Common Implementations

**PostgreSQL**  
Postgres is an open-source relational database known for its robustness and advanced features, and is one of the industry standards for relational databases.

Key Features:

- Supports complex queries and custom extensions
- Excellent for analytical workloads alongside traditional transactional uses
- Great for projects where you need flexibility, reliability, and open-source licensing

**MySQL**  
MySQL is another widely-used open-source database, and is also an industry standard.

Key Features:

- Fast and efficient for read-heavy workloads
- Well-supported by many hosting platforms
- Reliable and easy-to-deploy database

**SQLite**  
SQLite is a lightweight, serverless relational database, but is typically not used for large-scale applications.

Key Features:

- Embedded directly into applications
- Zero setup, as it is just a single file
- Ideal for mobile apps, small projects, or prototypes where simplicity is key