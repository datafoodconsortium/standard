# Transport layer

HTTP has become the unquestioned standard to exchange data unidirectionally.
FTP is still relevant to exchange files containing data like ods or xlsx values but is not adapted to our needs.
HTTP can be improved with the REST logic. This good practice makes the best use of the HTTP standard to harmonize APIs.

In the semantic world, another standard complements the HTTP: Linked Data Platform. This standardizes the structure of the requested semantic content. It is based on 2 concepts: containers and resources. Containers contain resources. A resource can reference containers that contain data linked to it. A resource can itself be a container.
Some contradictions exist between REST and LDP. The header link for example is used to indicate the links that a resource has with another resource in REST while it is used to indicate what is the nature of the resource \(Resource, BasicContainer, DirectContainer ...\) in LDP.
JSON-LD is compatible with LDP provided that it complies with the Resource / Container logic.
LDP does not impose a form of url and OpenApi remains relevant to describe the API.

**Conclusion: use HTTP and LDP while following a REST logic as much as possible**
