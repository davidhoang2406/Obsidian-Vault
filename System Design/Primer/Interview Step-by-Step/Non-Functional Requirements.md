After defining functional requirements, we will move on to non-functional requirements. This stage also usually only takes a few minutes.

While functional requirements outline what the user can do, non-functional requirements outline what the system should do/have in order to support those functional requirements.

Common considerations include:

- Performance: How fast does the system need to respond to user actions or process requests? This often includes defining acceptable latency thresholds for key operations like loading a user’s feed or posting a tweet.
- Availability: How consistently should the system be accessible to users? This could mean ensuring near 100% uptime through redundancy, failover mechanisms, and proactive monitoring.
- Scalability: Can the system handle increasing numbers of users, data, or traffic without a degradation in performance? Scalability can be both vertical (upgrading resources on a single server) and horizontal (adding more servers or instances). For large-scale systems, horizontal scalability is often preferred.
- Reliability: How well does the system handle failures? This includes designing for fault tolerance so that the system can recover from crashes or unexpected errors without losing functionality or data.
- Consistency: How accurately does the system reflect the same data across all users and operations? In systems with distributed databases, maintaining strong consistency can be challenging but critical for certain operations.
- Durability: How safely is data stored? This involves ensuring that data, once written, isn’t lost due to hardware failures, power outages, or other disruptions.
- Security: How well does the system protect against unauthorized access or attacks? This includes safeguarding sensitive user data and preventing exploits such as SQL injection or distributed denial-of-service (DDoS) attacks.

Let’s take a look again at our Twitter example. We know that our functional requirements are these:

- Users need to be able to post tweets
- Users need to be able to view individual tweets
- Users need to be able to view feed
- Users need to be able to follow other users
- Users need to be able to like tweets
- Users need to be able to comment on tweets

So, what characteristics should our system have? Well, here are the basic ones we came up with:

- Low Latency: Ensure users can post tweets and view their feed within 200 milliseconds to provide a smooth experience
- High Availability: Maintain as close to 100% uptime to ensure the platform is always accessible to users
- Scalability: Support horizontal scaling to handle growth in the user base and increased traffic seamlessly
- Data Durability: Store tweets, likes, and comments in a distributed, fault-tolerant system to prevent data loss