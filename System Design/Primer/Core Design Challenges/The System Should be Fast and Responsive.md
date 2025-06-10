Most user-facing apps must be quick. The response time should be less than 500ms. If it goes longer than 1 second, the user will have a poor experience.

Reading is usually fast after we have replication. Read requests are usually implemented as a query to an in-memory key-value dictionary beside HTTP protocols. Therefore, for many simple apps, the latency is mostly network round time.

Writing is where the challenge lies. Because most typical writing processes involve many data queries and updates, they last far longer than the 1-second limit. The solution is asynchrony: the write request is returned immediately after our server receives its data and puts the data in a queue. In the meantime, the actual processing continues in the back end. After receiving the response from the server, the client-side logic has the wiggle room for a speedy user experience. For example, it can show some UI before redirecting the user to read the result. This will usually take 1~2 seconds and is enough for the backend processing of the actual write request.

This is implemented by a message queue like Kafka.

![challenge 3](https://systemdesignschool.io/fundamentals/core-challenges-in-web-scale-app/Untitled%203.png)
