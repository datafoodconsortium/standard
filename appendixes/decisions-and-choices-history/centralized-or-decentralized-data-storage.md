# Centralized or decentralized data storage

Do we want the data we store to be on multiple servers, or on only one?

* Federation, all actor must respect the SOLID standard, each having a SOLID in front, that integrated with his servers on the data to be shared. Advantage = as we all use the same standard, it simplifies the distributed storage of data on all SOLID servers in the federation.
* Syndication, a translator is needed between the technology of the actors and the common technology. This option respects more the identity of each one, the actors are not obliged to use the same storage technology. Both are complementary and compatible.

Our vision pushes us to favor decentralization, so to move towards a logic of federation, with a semantic server per actor. But we defend the freedom of choice of each actor, so the vision is a hybrid model of federation and syndication for those who do not wish to adopt a standardized semantic server. This also allows for a much easier migration from the existing state, with multiple different technologies, to a federated one where all use SOLID server.

**Conclusion: it seems that we need a centralized server for 3 uses :**

* ID repositories
  * Users
    * OIDC server of the commons
    * Depending on SOLID's technological evolutions, this repository could be decentralized thanks to web-id/OIDC
  * Products
    * Open Food Facts
  * Places
    * To define
* The semantic cache
  * To guarantee the performances and to be able to make queries \(SPARQL, GraphQL\) which cover the data of all the platforms
* The Open Food Facts Taxonomy Not Open-Data
  * For producers to map their catalog based on the Open Food Facts taxonomy. This mapping from the original taxonomy of the platforms to the Open Food Facts taxonomy has to be done once manually and can be automated after, which should facilitate the adoption of DFC.

