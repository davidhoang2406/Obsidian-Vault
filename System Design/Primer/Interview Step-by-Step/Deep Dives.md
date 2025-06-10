Deep dives are the last step of the system design interview, and usually focus on addressing higher-level, more specific challenges or edge cases in your system. They go beyond the high-level architecture to test your understanding of advanced features, domain-specific scenarios, and trade-offs.

#### 1\. How would you handle the Celebrity Problem?

The "celebrity problem" arises when a user with millions of followers posts a tweet, creating a massive fan-out as their tweet needs to be added to millions of follower feeds. This can overwhelm the system and lead to latency and high write amplification.

**Solution:** We can modify our existing architecture to handle this more efficiently:

- Fan-Out-on-Write for Normal Users: For users with a manageable number of followers, we continue with the standard fan-out-on-write model. The tweet is pushed to their followers’ feeds as soon as it’s posted.
- Fan-Out-on-Read for Celebrities: For users with a large follower count (e.g., more than 10,000), we switch to a fan-out-on-read model. The celebrity's tweets are stored in the Tweet Database and Tweet Cache but not precomputed into individual follower feeds. When a follower opens their feed, the Feed Service dynamically fetches the celebrity's latest tweets from the cache or database and merges them into the user’s timeline.
- Dynamic Switching: Implement a threshold (e.g., follower count or engagement volume) to dynamically decide whether to use fan-out-on-write or fan-out-on-read for a given user.

#### 2\. How would you efficiently support Trends and Hashtags?

Twitter trends and hashtags involve aggregating data across billions of tweets in real-time to identify popular topics. How can we compute and update trends efficiently?

**Solution:** We enhance the existing architecture as follows:

- Distributed Trend Computation: Each region or data center computes local trends by aggregating hashtags and keywords using a sliding window algorithm (e.g., the past 15 minutes). Local results are sent to a global aggregation service, which combines them to generate global trends.
- Hashtag Indexing: Modify the Tweet Service to index hashtags upon tweet creation: Maintain an inverted index where hashtags are keys, and associated tweet IDs are values. Use a distributed search engine like Elasticsearch or Solr to store and query the hashtag index efficiently.
- Caching Trends: Trends are calculated periodically (e.g., every minute) and cached in a distributed cache like Redis for low-latency access. A TTL (time-to-live) ensures trends are refreshed frequently without overwhelming the system.

#### 3\. How would you handle Tweet Search at Scale?

Search is a core feature of Twitter, allowing users to search tweets, hashtags, and profiles. How can we support a scalable, real-time search system?

**Solution:** We incorporate a distributed search architecture:

- Real-Time Indexing: Modify the Tweet Service to send newly created tweets to a search indexing service via a message queue. The indexing service processes tweets and updates the search index in a distributed search engine like Elasticsearch or Apache Solr.
- Sharded Indexing: Partition the search index by time (e.g., daily indices) or hashtags to distribute the load across multiple nodes. Older indices can be stored on slower storage systems to save costs while keeping recent indices on faster nodes.
- Query Optimization: Use inverted indexing to allow efficient keyword and hashtag search. Employ a ranking algorithm (e.g., BM25 or ML-based) to surface the most relevant tweets based on user engagement, recency, or other factors.
- Search Cache: Cache popular search queries and their results to reduce the load on the search engine.
