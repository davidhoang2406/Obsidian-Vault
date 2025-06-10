#### Laying the Groundwork

Whenever you’re fetching data from a database, it takes time and compute resources to do. While each request might only take milliseconds, system design is all about designing for scale, and with millions of users, this can lead to significant problems, such as:

- High Latency: Slow responses frustrate users and make your app feel sluggish
- Low Availability: A flood of requests can overwhelm your database or API, leading to reduced performance or even downtime
- Poor Efficiency: Fetching the same data repeatedly wastes valuable compute resources

One of the most common kinds of systems is one in which there is a high volume of reads, but the data doesn’t update very often - we call this a **high-read, low-write system**. Think of social media apps, where users spend the majority of their time reading other posts, and only make their own posts occasionally. Or e-commerce platforms, where products get viewed millions of times, but rarely have their details changed.

If you think it seems a bit redundant to be constantly fetching the same, unchanged data from a database, you’re right! Why do we need to go all the way to the database if we’ve already fetched something, can’t we store it closer to us for next time?

This is where **caches** come in.

#### Technical Explanation

At a high level, a cache works like this:

1. A request comes in. Before heading to the database or API, your system checks if the data is already in the cache.
2. If the data is in the cache, which we call a **cache hit**, it gets served immediately - no need to bother the database.
3. If the data isn’t in the cache, which we call a **cache miss**, your system fetches it from the database like usual… but this time, it also saves a copy in the cache for next time.

![cache-aside](https://systemdesignschool.io/concepts/caching/cache-aside.png)

By keeping frequently-used data close at hand, caches reduce the time it takes to respond to requests and lessen the load on your backend systems. This improves latency, boosts availability, and optimizes efficiency, making your app more scalable and responsive.

In an interview, you’ll also be expected to be able to explain the following concepts:

- **When to remove items from cache**: An eviction policy is a set of rules that determines which data to remove from the cache when it reaches its capacity. Some common eviction policies are:
	- Least Recently Used (LRU): Least recently used items are removed first.
	- First In, First Out (FIFO): Items are removed in the order they were added.
	- Least Frequently Used (LFU): Least frequently used items are removed first.
- **How to keep cache data up to date:** An invalidation strategy is a set of rules that determines how and when cached data is marked as stale or removed to keep it consistent with the source data. Some common invalidation strategies are:
	- Time-To-Live (TTL): Data is removed after a pre-set expiration time.
	- Event-Based Invalidation: The cache is cleared or updated when the underlying data is updated.
	- Manual Invalidation: Cache entries are explicitly removed or refreshed by the application.
- **How to store data in the cache**: A cache write strategy is a set of rules that determines how and when data is written to the cache to ensure it is available for future requests. Some common cache write strategies are:
	- Write-Through: Data is written to the cache and the underlying database simultaneously, keeping them in sync.
	- Write-Behind: Data is first written to the cache and then asynchronously written to the database, improving write performance.
	- Write-Around: Data is written directly to the database and added to the cache only when it is read, reducing cache pollution for infrequently accessed data.

#### Common Implementations

**1\. In-Memory vs. Disk-Based Caches**  
Caching systems can be categorized based on where the data is stored: either in memory or on disk.

**In-Memory Caches** store data in RAM, providing extremely fast access.

Key Features:

- Low latency (microseconds to milliseconds)
- Volatile storage: data is lost if the cache is restarted (unless persistence is enabled)

When to Use:

- For real-time applications requiring high-speed access, such as session management, leaderboards, or frequently accessed database queries

Examples: Redis, Memcached

**Disk-Based Caches** store data on persistent storage like hard drives or SSDs, offering slower but more durable caching.

Key Features:

- Data persists through restarts, making it ideal for long-term storage of cacheable data
- Slower than in-memory caches due to disk I/O latency

When to Use:

- For caching large datasets or static assets, where persistence is critical and access speed is less important

Examples: Varnish Cache, browser caches

**2\. Client-Side vs. Server-Side Caches**  
Caching systems can also be categorized based on where the cache is located within the architecture.

**Client-Side Caches** store data on the user’s device (e.g. in a browser or app).

Key Features:

- Reduces server communication by caching data locally
- Specific to individual users, enabling faster responses for their repeated requests

When to Use:

- For static assets (e.g., images, CSS, JavaScript) or user-specific data in offline-capable applications

Examples: Browser Cache, LocalStorage, IndexedDB

**Server-Side Caches** store data on or near the server, shared across all users and requests.

Key Features:

- Reduces load on backend systems like databases and APIs
- Optimizes performance for high-traffic applications by serving shared data quickly

When to Use:

- For frequently accessed data that is consistent across users, such as API responses, product pages, or popular posts

Examples: Redis, CDNs, NGINX Cache

#### CDN (Content Delivery Network)

##### Laying the Groundwork

Ever noticed how some websites load almost instantly, even when they have heavy images, videos, or other assets? That’s often thanks to a special type of server-side cache, known as a **CDN**, or, **content delivery network**. A CDN is a distributed network of servers that helps deliver content to users more quickly and efficiently by storing cached copies of static assets closer to the user’s location.

![cdn](https://systemdesignschool.io/fundamentals/caching/with-cdn.png)

The key challenges a CDN solves include:

- High Latency: When users are far from your server, it takes longer for requests and responses to travel back and forth, causing delays.
- Bandwidth Overload: A surge in traffic to your website can overwhelm your origin server, leading to slowdowns or crashes.
- Global Scalability: Hosting all your assets in a single location makes it hard to serve users worldwide without performance issues.

Static assets like images, videos, JavaScript files, and even API responses are cached across multiple servers globally, so users get their content from a server that’s physically closer to them. This reduces latency, distributes traffic more evenly, and keeps your origin servers from being overburdened.

#### Technical Explanation

You’ve already learned how caches work, and CDNs are essentially just a common cache implementation. But still, it’s good to go over how they work specifically:

- Content Distribution: When you deploy your app, static files (e.g. images, CSS, videos) are uploaded to your CDN provider. The CDN copies these files to its network of edge servers located around the globe
- Request Handling: When a user visits your site, their request is routed to the nearest CDN server
	- If the requested file is already cached there (cache hit), the server delivers it immediately
	- If the file isn’t cached (cache miss), the CDN fetches it from the origin server, caches it locally, and then serves it to the user
- Caching Strategy: CDNs use caching policies like TTL to determine how long files should be stored before refreshing from the origin server.

#### Common Implementations

**Cloudflare**  
Cloudflare is a popular CDN known for its ease of use and strong security features.

Key Features:

- Global network with low latency
- Built-in DDoS protection and Web Application Firewall
- Ideal for web applications needing speed and security with minimal configuration

**AWS CloudFront**  
AWS CloudFront is Amazon’s fully managed CDN integrated with AWS services.

Key Features:

- Seamless integration with AWS storage (S3) and compute (Lambda)
- Supports dynamic and static content delivery
- Ideal for applications already hosted in AWS or requiring custom logic at the edge

**Akamai**  
Akamai is one of the oldest and most robust CDNs, typically used by enterprise-level clients.

Key Features:

- Industry-leading global server network for ultra-low latency
- Advanced customization and analytics tools
- Ideal for enterprise applications with high traffic and advanced delivery requirements