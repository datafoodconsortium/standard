# Directionality

One key factors to differentiate between protocols is their directionality:

* **Unidirectional** = a client requests from a server and the server responds. The server cannot initiate the communication.
* **Bi-directional** = the client and the server can send messages without being requested.

The advantage of bidirectionality is that one can also adopt a push mode, where the server sends data to the client. For example regarding inventory: a product on platform A is purchased, but this product is also sold on platform B, platform A could "push" the information that the available stock has changed to platform B.
**The need to have uni- or bi-directionality is therefore related to the need to have data synchronization and notifications in real time**. An alternative in the example above using unidirectionality could be that servers send pull requests, every minute for example, to know if the stock has changed. Is it sufficient ? Do we want real time or near real time to 1 or 5 minutes?

So far the protocol used in the semantic web is rather unidirectional, HTTP or LDP. It can be a simple request about a resource \(file: LDP or base of triplestore: HTTP\) or more complete queries with Sparql.

There is today very little R&D on bidirectionality in the semantic world, it is still very experimental \(AMQP + SPARQL, XMPP + SPARQL, HTTP2 + SPARQL, WebSocket\). The terrain is slippery, not yet mature. The theoretical ideal would of course be bi-directional, **but the rational and practical solution of the consortium is rather to use unidirectionality.** We prefer to focus our vision towards new and proven standards and technologies. We will see later when technologies will mature if it makes sense to switch to bidirectional.

**Conclusion: implement a unidirectional solution**
