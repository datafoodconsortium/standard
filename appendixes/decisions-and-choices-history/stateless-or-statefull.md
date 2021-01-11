# Stateless or statefull?

When several requests are made, should they be independant or can they rely on each other?

* In a “stateless” setting, no state information is retained on the server itself. If we do 2 requests, the second one does not need to know the results of the first one, but it will have to contain all the required information for the server to be able to return an answer from scratch.
* In a “stateful” setting, state information \(session\) is retained on the server. The 2nd request will be done on the basis of the input and output of the 1st request .

Some of the main advantages of each approach impact performance and scaling:

* A stateless architecture makes it much easier to scale horizontally, which means to deploy additional servers \(often identical\) hosting the same application on which the load is spread. Whereas if stateful, the servers store a state and in order to perform horizontal scaling, either this state will need to be synchronized between servers or the same user will always have to be directed to the same server - which makes it harder and less efficient to implement.
* In a stateful architecture each request does not need to repeat steps that were already performed or provide the same information again. The most common use of this is for identification/authentication, which then only needs to be done on the first request saving significant processing time.

The common best practice today is to use stateless services, making each request independant. It makes for easier debugging, maintenance and scaling but needs additional development for identification and authentication especially.

**Conclusion: implement stateless service**