# Transition strategy from current to ideal

**Basic principle: Improve the legacy rather than migrate to new technology**

## High vs Low granularity services

The reality of the platforms around the consortium today is that they are absolutely not designed for high granularity services. At best, as some low granularity services connected to each other via API. If each platform were to implement a standard based on a high granularity services architecture, each platform would have to re-assemble these high granularity services in low-granularity corresponding to their own low granularity services to be able to share their services. data via API ... Re-develop in their API a kind of "connector" between their services and high granularity services of the standard.

So the approach for the prototype is rather not to go to a 100% high granularity service state, but rather only on some relevant perimeter, without pushing the high granularity service logic to the extreme, but granular enough so that the API is simple to use. Depending on the potential of reusability of the service by others. For example, separating the product and inventory information is quite clear.
To try to go as granular as possible, we can use as soon as necessary an aggregation bus \(like the [semantic bus](https://github.com/assemblee-virtuelle/Semantic-Bus) developed by Simon\), for example to switch from several high granularity services to a low granularity service, according to the respective configurations of each platform.

## Semantic web

A semantic approach in high granularity service would also require platforms to transform their database by a BDD TripleStore for example and that it is not the purpose of DFC to impose it. Also, developers do not master SPARQL today

**Conclusion: the actors of the consortium finally preferred to start by first developing a user friendly API using OpenAPI, in JSON-LD.**

Of course, we keep in mind the goal of moving towards a fully semantic and professional API when the DFC project budget has been secured. This approach being a little less experimental, it will allow us to connect 2 data sources to the prototype. This will require us to code existing API transformations into "REST resource driven semantic" APIs that respect the DFC ontology.
