This challenge is a result of solving Challenge 1 and Challenge 2. With data replication and asynchronous data update, the read requests can easily see inconsistent data. Inconsistency usually means outdated: the user won’t see any random wrong data, but old versions or deleted data.

The solution is more on the application level than on the system level. Because the outdated read resulted from replication and asynchronous updates will eventually disappear when the servers catch up, we build the user experience so that seeing outdated data for a short period of time is OK. This is called eventual consistency.

Most Apps tolerate eventual consistency well. Especially compared with the alternatives: Losing data forever or being very slow. The exceptions are banking or payment-related apps. Any inconsistency is unacceptable, so the apps must wait for all processing to finish before returning anything to the users. That’s why such apps feel much slower than, say, Google Search.

![challenge 4](https://systemdesignschool.io/fundamentals/core-challenges-in-web-scale-app/Untitled%204.png)