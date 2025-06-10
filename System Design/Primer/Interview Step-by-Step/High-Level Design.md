High-level design is the bulk of the interview, and is where you’ll combine the work from the first few parts with your knowledge of system design. This should take around 15-20 minutes, but varies based on your level (juniors can expect to spend more time on this, whereas seniors will likely be expected to move to deep dives more quickly).

High-level design is what stumps the most people, because they often don’t know where to even begin. Again, luckily for you, we have a tried and tested framework for easily building up to a working system.

You should start by addressing your functional requirements in the most basic way possible, and then after you have a feasible solution, start addressing the non-functional requirements by making it more robust.

#### Functional Requirements

To start with the functional requirements, you’re just going to take your API endpoints and map the data flow with some services. Your goal is to show that your thinking is very methodical and structured, so it’s best to start simple without trying to add any complicated parts yet - this way, if you’ve made a mistake, the interviewer can correct it early on.

- Wondering why microservices is the best approach? Check out this: {here}

So, take a look at how we do this for the Twitter example:

Users need to be able to post tweets:

![High-Level Design Diagram 1](https://systemdesignschool.io/primer-high-level-design/hld1.jpeg)

Users need to be able to view individual tweets:

![High-Level Design Diagram 2](https://systemdesignschool.io/primer-high-level-design/hld2.jpeg)

Users need to be able to follow other users:

![High-Level Design Diagram 3](https://systemdesignschool.io/primer-high-level-design/hld3.jpeg)

Users need to be able to view feed:

![High-Level Design Diagram 4](https://systemdesignschool.io/primer-high-level-design/hld4.jpeg)

Users need to be able to like tweets

![High-Level Design Diagram 5](https://systemdesignschool.io/primer-high-level-design/hld5.jpeg)

Users need to be able to comment on tweets:

![High-Level Design Diagram 6](https://systemdesignschool.io/primer-high-level-design/hld6.jpeg)

However, this is a lot of services, and even though a separation of concerns is important, we don’t want to go too overboard and turn everything into a microservice. One thing we want to think of is, is there any way we can combine some services? In this case, I’d argue that likes and comments are logically very similar - both are ways of engaging with tweets, both are a type of counter on a tweet, they just contain different data. Well, this makes them very similar, and in this case, we should consider merging them into a single Engagement Service and Engagement Database.

![High-Level Design Diagram 7](https://systemdesignschool.io/primer-high-level-design/hld7.jpeg)

Now lastly, with multiple services, it’s almost always a good rule of thumb to add in an API gateway (make sure to go back to the above section, Main Components, if you’re unsure what that is).

![High-Level Design Diagram 8](https://systemdesignschool.io/primer-high-level-design/hld8.jpeg)
#### Non-Functional Requirements

Great, now we have a structured system that is at least feasible for solving the functional requirements! However, this system clearly has significant challenges at scale, and doesn’t really show any of our advanced system design knowledge. Now let’s move into part 2 of high-level design: building for the non-functional requirements.

Let’s address each non-functional requirement one step at a time. The biggest mistake candidates make here is not tying these recommendations back to the non-functional requirements they called out earlier in the interview.

We’ll start with **scalability**, as it’s a relatively straightforward component to address in most cases. As outlined in the non-functional requirements section, our goal is to support horizontal scaling to handle growth in the user base and increased traffic seamlessly. Horizontal scaling allows us to add more instances of our services to handle higher loads, ensuring consistent performance as demand increases.

![High-Level Design Diagram 9](https://systemdesignschool.io/primer-high-level-design/hld9.jpeg)

To implement horizontal scaling, we will deploy multiple instances of each service - Tweet Service, Feed Service, Follow Service, and Engagement Service. These instances will operate independently, handling requests in parallel. However, to distribute traffic efficiently across these instances, we need a load balancer.

![High-Level Design Diagram 10](https://systemdesignschool.io/primer-high-level-design/hld10.jpeg)

A load balancer ensures incoming requests are evenly distributed across available service instances. It also performs health checks to monitor the status of each instance and reroutes traffic away from unhealthy instances, ensuring high availability. This is actually one of our other non-functional requirements that we’ll get to in a moment, and is a nice bonus we get from this load balancer. By incorporating a load balancer, we can scale each service dynamically based on traffic patterns. For example, during peak hours, more instances of the Feed Service can be spun up to handle the surge in requests, and these can scale back down during periods of lower activity to optimize resource usage.

Now let's move on to another core non-functional requirement: low latency. When we have only a few users posting tweets, our servers should run blazing-fast, as each user's feed only fetches a handful of tweets. But as we start to scale to millions of users and billions of tweets, accessing the database for every feed request will start to become exponentially slow with our current system. It seems a bit redundant for users to write tweets to the database, and then for the feed to re-pull these from the database each time, no?

This is where a cache comes to the rescue!

![High-Level Design Diagram 14](https://systemdesignschool.io/primer-high-level-design/hld14.jpeg)

A cache is a high-speed data storage layer that stores frequently accessed data closer to the application. In our system, the Feed Service could leverage a distributed caching system like Redis or Memcached to store the most recent tweets for each user. Here’s how it works:

1. Feed Precomputation:
	- When a user posts a tweet, the Feed Service doesn’t just update the followers’ feeds in the database. It also pushes the new tweet to a cache, storing it as part of the precomputed feeds for the user’s followers.
	- This way, when followers log in, the Feed Service can fetch the feed data directly from the cache instead of querying the database or relying on real-time aggregation.
2. Hot Data Access:
	- Caches are ideal for storing "hot" data - data that is accessed frequently, such as the latest tweets for a user's feed. Since caches operate in memory, they can deliver this data in milliseconds, reducing response times and improving the user experience.
3. Reducing Database Load:
	- By offloading repeated reads to the cache, we reduce the load on the database. This makes the system more scalable and ensures that the database is available for other critical write operations.
4. Cache Expiry and Consistency:
	- To ensure the cache stays fresh, we can set an expiry time for cached items or use an event-driven update model. For example, when a new tweet is posted, an event triggers the cache to update, ensuring followers see the latest tweets without unnecessary delays.

Now, when adding in the load balancer earlier, we touched on how they have an additional benefit, which is that the health checks help us maintain high availability. But what else can we do to have **high availability**? Well, let me present you with a situation where we might not have availability with the current system. What happens if a user posts a tweet at 1:02pm, but at 1:03pm, our tweet service goes down, and at 1:04pm, their followers log onto the app. Are their followers going to see this tweet in their feed? As it stands now, NO! Now here's another scenario. What happens when we have millions of active users publishing tweets all at the same time? If we tried to process them all simultaneously, we would overload our servers! Hmm, if only there was a solution to this… wait a minute, this is exactly why we have message queues!

![High-Level Design Diagram 15](https://systemdesignschool.io/primer-high-level-design/hld15.jpeg)

A message queue acts as a buffer between services, decoupling their dependencies and ensuring that messages (like new tweets) are not lost, even if one of the services experiences downtime. Here's how it works: when a user posts a tweet, the Tweet Service doesn’t directly communicate with the Feed Service. Instead, it places the tweet in a queue, which is then processed by a consumer and added to both the database and cache. The Feed Service, which processes tweets to update users' feeds, then reads from this cache. This way, even if the Tweet Service goes offline, the messages (tweets) are safely stored in the queue and processed by the consumers, and the Feed Service can still update the feeds of the user’s followers when they log in.

By incorporating a message queue, we ensure eventual consistency and high availability even during partial system failures. In our scenario, followers would still see the tweet in their feed, thanks to the queue ensuring that no message is lost. This decoupling of services also helps out with scalability, as the queue can handle varying workloads and traffic spikes without overwhelming downstream services. Message queues like RabbitMQ, Kafka, or AWS SQS are built for durability and reliability, making them a perfect fit for our use case.

While we’ve addressed a lot of the service non-functional requirements, there is still one last critical requirement to tackle: data durability. In a system that handles billions of tweets, likes, and follows, ensuring that data is never lost is essential, because once lost, we cannot get it back. How do we prevent this from happening? This is where we take our databases a step further and use a distributed databases model.

![High-Level Design Diagram 16](https://systemdesignschool.io/primer-high-level-design/hld16.jpeg)

A distributed database is designed to replicate and store data across multiple nodes in a cluster. Distributed databases like Amazon DynamoDB, Google Cloud Spanner, or Cassandra automatically replicate data across multiple nodes. This means that even if one node goes down, the data is still accessible from other replicas. Additionally, distributed databases provide built-in mechanisms for point-in-time recovery and automated backups. In this case, regular snapshots of the Tweet DB, Follow DB, and Engagement DB can be taken and stored in a separate backup system. And, if we ever have a complete failure, the data can be restored to its last consistent state.
