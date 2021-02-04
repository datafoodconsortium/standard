# Serialization

In our case data serialization is the process of converting the semantic data to a format that allows sharing it in a form that then allows recovery of its original structure. Historically the standard used was RDF \(XML\), which has evolved to TTL which simplifies the syntax. The last evolution is JSON-LD, which is like JSON but expresses a context which allows an equivalence with RDF or TTL, but which is not serialized on files. TTL simplifies RDF, while JSON-LD offers another radically different way of doing things using the most common standard \(JSON\). When we enter the semantic world, we can have trouble reading RDF or TTL, while JSON-LD is easy to read. JSON-LD is the standard that has been chosen by Google and Facebook in their journey towards the semantic web.

**Conclusion: the consortium's vision is therefore that we must adopt the JSON-LD standard.**

The consortium, however, anticipates that this choice will require a bit of thinking around the server implementing, because standard SOLID servers return semantic data in RDF or TTL. We could also contribute to the global common good so that SOLID servers can provide JSON-LD.

