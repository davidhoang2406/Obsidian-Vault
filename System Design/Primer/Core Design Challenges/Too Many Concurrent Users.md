While a large user-base introduces many problems, the most common and intuitive one is that a single machine/database has a RPS/QPS limit. In all single-server demo apps you would see in a web dev tutorial, the server’s performance will degenerate fast once the limit is exceeded.

The solution is also intuitive: repetition. We just repeat the same assets of our app and assign the users randomly to each replication. When the replicated assets are server logic, it’s called load balancing. When the replicated asserts are data, it’s usually called database replicas.

![challenge 1.1](https://systemdesignschool.io/fundamentals/core-challenges-in-web-scale-app/Untitled.png)



![challenge 1.2](https://systemdesignschool.io/fundamentals/core-challenges-in-web-scale-app/Untitled%201.png)

