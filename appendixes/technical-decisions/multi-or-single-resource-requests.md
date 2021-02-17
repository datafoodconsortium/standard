# Multi- or single-resource requests?

The ability to make multi-resource requests \(a query that looks for information across multiple files\) is related to using a TripleStore to store information, not just files. The TripleStore is then the aggregation of all the triplets of all the files. We can not make sparql multi-resource requests today in the SOLID server implemented by the MIT \(node ​​solid server\) but it is possible on a JENA server. JENA does not implement authentication and rights management solutions, and these are the points that SOLID offers a solution for. In the SOLID standard, SPARQL multi-resources is planned, but not yet implemented.

The need for multi-resource queries will influence the choice of server \(next point\).

In our vision, and in the continuity of the architectural vision described above, to be able to carry out research on a set of resources \(eg a search on all the catalogs or all the logistical needs\) we need to ability to make multi-resource SPARQL queries. We do not yet know how to put it in place, but we know in our vision that we need to be able to make multi-resource requests.

**Conclusion: we need to be able to do multi-resources requests, but it is unclear how to achieve this.**

