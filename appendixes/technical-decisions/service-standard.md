# Service standard

An old standard exists to describe API, coming from the XML world but today applicable to most data format: WSDL. This norm is complex as it is very generic and is able to describe almost any kind of API, not only HTTP, and not only synchronous or unidirectional. Working hand in hand with WSDL, and nearly as complex, the UDDI standard allowed for registration and discovery of services.

OpenAPI on the other hand focuses on HTTP and on the service usage from a resource point of view, which makes it much simpler. **It allows to document et specify the API (the endpoint, the URLs structure, the serialisations...).**

The capability of being able to specify by a given standard, the form of the url, the ability to work in a Query logic, the serialization, the use of a model and its visibility with each request is summarized in this table:

|                                  | <p>Spec</p><p>OpenAPI</p> | URL | Query         | Sérialisation                  | Data model accessible per API | <p>Data model in the API response<br></p> |
| -------------------------------- | ------------------------- | --- | ------------- | ------------------------------ | ----------------------------- | ----------------------------------------- |
| REST + OpenAPI                   | Yes                       | No  | No            | Yes (json)                     | Yes but OpenAPI specific      | Yes but OpenAPI specific                  |
| REST resource driven semantic    | Yes                       | No  | No            | Yes (rdf/ttl/json-ld)          | Yes but OpenAPI specific      | Yes (owl)                                 |
| REST application driven (SPARQL) | Partial                   | Yes | Yes (SAPRQL)  | Yes (rdf/ttl/json-ld)          | Yes (owl)                     | Yes                                       |
| GraphQL                          | Partial                   | Yes | Yes (GraphQL) | Yes (json))                    | Yes but OpenAPI specific      | No                                        |
| hyperGraphQL                     | Partial                   | Yes | Yes (GraphQL) | Yes (json-ld in data property) | Yes (owl)                     | <p>Yes (owl)<br></p>                      |

## URL structure

### Resource driven (REST logic)

A URL is a unique address pointing to a single resource. This resource can be a container that contains several atomic resources.

| <p>1: http://serveur/idContainer/idRessource1</p><p>2: http://serveur/idContainer/idRessource1/attribut1/</p><p>3: http://serveur/idContainer/idRessource1/attribut1/attribut2/</p><p>4: http://serveur/idContainer/idRessource1/attribut1/idRessource2/attribut2/...</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

A URL may also contain parameters, using the special characters ‘?' to mark the start of the parameters and '&' to separate each parameter. Previous examples translated with parameters:

| <p>1: http://serveur/idContainer?idRessource=idRessource1</p><p>2: http://serveur/idContainer?idRessource=idRessource1&#x26;attribut=attribut1</p><p>3: http://serveur/idContainer?idRessource=idRessource1&#x26;attribut=attribut1.attribut2</p><p>4: diffic<strong>ult to express see</strong> 6</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

With pagination parameters to load data by package rather than all at once:

| 5: [http://serveur/idContainer?idRessource=idRessource1\&start=0\&length=100](http://serveur/idContainer?idRessource=idRessource1\&start=0\&length=100) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- |

### **Application driven**

The more parameters an API can handle, the more generic the API becomes that encompass the entire application. This type of URL changes the design of an API since it no longer creates an API by type of resource but a generic API capable of meeting the different needs of the application.

The tendency is to make parameters that contain json objects which makes APIs much more generic (search, projection (choice of fields), sorting ...). The syntax use is usually based on the MongoDB NoSql database “standard”.

| 6 : [http://serveur/idContainer?search={attribut1:{$gte:value1\}}\&projection={attribut1:1,attribut2:1,attribut3:0}\&sort={attribut1:1,attribut2:2}\&start=0\&length=100](http://serveur/idContainer?search={attribut1:{$gte:value1\}}\&projection={attribut1:1,attribut2:1,attribut3:0}\&sort={attribut1:1,attribut2:2}\&start=0\&length=100) |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

The request above means retrieving the resources related to the container idContainer :

* Whose attribute1 is greater than value1
* Only retrieve attribute1, attribute2 but not attribute3,
* Sorted according to attribute1, then attribute2
* Starting with the first document found and returning 100 documents

The more complex parameters the API contains, the less it is possible to use the OpenAPI specification because the result and structure of the query are variable. If the strategy is moving towards this type of url, it is probably better to take the step towards using queries.

### **Query**

The ultimate use of advanced parameters is called Query. This is a normalized query passed in a single parameter with all the information needed for server using this standard to execute the request. In the semantic world it is SPARQL. GraphQL offers another standardization specific to this technology for queries.

The disadvantage of SPARQL queries is the inherent complexity of the semantic web. GraphQL can be an alternative, but it will require to transform the OWL ontology into GraphQL types. The drawback with the graphQL approach is that the model consisting of types is not exposed with the data returned.

HyperGraphQL is an implementation of GraphQL dedicated to the semantic web in order to connect to a tripleStore. The API provides data in their json-ld form with context in owl schemas.

## **Gateways between protocols**

Ideally Both protocols (de facto standard and semantic standard) should be accepted by the platforms. The API in de facto standard could then be based on the semantic API to avoid duplicated maintenance (putting SPARQL in front). This represents a significant cost to setup and configure.

The following table shows where it is possible to go from one standard to another (if the de facto standard is used in phase 1 and the semantic one in phase 2):

| Source               | Destination          | Comment                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| -------------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| SPARQL               | GraphQL              | <p>- It is possible to <a href="https://www.npmjs.com/package/graphql-to-sparql">convert a GraphQL query into a SPARQL query</a>.</p><p>- It seems possible to <a href="https://comunica.github.io/Article-ISWC2018-Demo-GraphQlLD/">overlay graphQL on a SPARQL API</a> but it's quite experimental.</p><p>- There is HyperGraphQL also which allows to connect directly to a Jena semantic base. the SPARQL and GraphQL API can then coexist based on the same database.</p> |
| SPARQL               | REST resource driven | <p>REST APIs can be based quite easily on SPARQL APIs to simplify their use, through the semantic bus for example.<br></p>                                                                                                                                                                                                                                                                                                                                                     |
| GraphQL              | REST resource driven | The REST APIs can be based quite easily on GraphQL APIs to simplify their use, through the semantic bus for example                                                                                                                                                                                                                                                                                                                                                            |
| GraphQL              | SPARQL               | A SPARQL API can only be based on a semantic database (jena or rdf for example)                                                                                                                                                                                                                                                                                                                                                                                                |
| REST resource driven | SPARQL               | A SPARQL API can only be based on a semantic database (jena or rdf for example)                                                                                                                                                                                                                                                                                                                                                                                                |
| REST resource driven | GraphQL              | GraphQL is designed to connect to resource-driven APIs by adding resolvers to the server. It also allows dynamic aggregations / searches on several APIs of several actors.                                                                                                                                                                                                                                                                                                    |

To build the prototype, we need a user-friendly protocol that is easy to set up quickly while offering the possibility of making a more professional API available. For the sake of maintainability of it all, the user-friendly API should be pluggable into the professional API.

**Conclusion: the actors of the consortium agree on a shared vision on the need to have 2 APIs:**

* **a "professional" API using SPARQL**
* **A more user friendly API in JSON-LD only on "read" data permissions.**

## De facto standards or semantic web ?

Do we want to respect the standards of the semantic web, or develop a simpler API, potentially more intuitive, enjoyable, and easy to use?

The standard syntax for querying semantic data is SPARQL (W3C). The standard for semantic APIs takes this query standard and is based on an HTTP url to which is added a parameter (usually endpoint or search) that contains the request. From this, the server request will be done via HTTP (LDP at the margin) and SPARQL, and the answer will be given in semantic data (json-ld or rf or ttl) with a granularity that depends on the request SPARQL but in a generally high granularity service logic.

In the non-semantic world and low granularity services, still predominant, the most used API standards today are

* OpenAPI (managed by a large consortium) It is not compatible with json-ld as it is and probably will not be, because the concepts are competing especially in terms of model. It respects the logic of the common good with a broad consortium and shared governance.
* Graphql (pushed by Facebook while trying to build a community) . Very centralized by Facebook without trying to make a consortium.

They do not respect the semantic web standards and are not managed by the W3C.

There is here a fine balance to be found between potential for interoperability, coverage of a wide variety of use cases, etc. and usability and ease of usage.

Here we find ourselves in a situation where we can choose to develop an API using web-semantic standards (http + json-ld + sparql) standardized by the W3C but complex and not or more accessible technologies (OpenApi, graphQL ) which are de facto standards.

**Conclusion:**

All the actors in the consortium agree that compared to our goal, our willingness to allow flexible cooperation between actors on all issues where it makes sense, we want to move towards the use of the API standard SPARQL to ensure the flexibility of use cases and to use the power of the semantic web. The development in addition of a second API based on OpenAPI would make it easier to reuse and guarantees easy access to the standard. This vision is of course coupled with the vision of a high granularity services architecture, necessary to use the power of the semantic web.

**They warn nevertheless about the complexity of implementation of high granularity services and the risk of slowing the project if we started on this option for the prototype.**

**They are also realistic about a low skill level by the current teams of the SPARQL API standard.**

**The API of the DFC standard will be "REST resource driven semantics". This means HTTP requests (a URL per usage / resource) documented by writings and in OpenAPI that return the Json-LD. This contains the OWL semantic model and not the model under the Open-API standard. The API can return multiple nested resources without respecting the high granularity service logic.**

**Platforms can use the implementation they want including solid servers but must expose the API of the standard.**

**Eventually, a cache server will increase search performance and use SPARQL and / or GraphQL for this. This cache will also expose the API of the standard.**
