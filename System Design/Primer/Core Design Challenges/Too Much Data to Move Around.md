The twin challenge of too many users is the issue of too much data. The data becomes 'big' when it's no longer possible to hold everything on one machine. Some common examples: Google index, all the tweets posted on Twitter, all movies on Netflix.

The solution is called sharding: partitioning the data by some logic. The sharding logic groups some data together, for instance, if we shard by user\_id in Twitter, then all tweets from one user will be stored in the same machine.

![challenge 2](https://systemdesignschool.io/fundamentals/core-challenges-in-web-scale-app/Untitled%202.png)


