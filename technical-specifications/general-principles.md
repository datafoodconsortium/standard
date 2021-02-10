# General principles





Une plateforme peut interagir avec une autre platorme pour lire et écrire des données   
- Interopérabilité base sur le web sémantique normalisées W3C ce qui inclue protocole Sémantique et Ontologies -&gt; Protocole Spécification  
- Nous ne constituons pas de référentiel d'identifiant mais mettons en lien les identifiants des platforme en se plaçant comme intermédiaire d' inter-médiation.   
- Cette interaction doi être sécurisé \(la platforme\) -&gt; Authentification  
- DFC met à disposition un prototype pour mettre en exemple le protocole et la fonction d'inter-mediation indispensable à l'interopérabilité -&gt; Architecture, ...



## Federation vs syndication

The nuance between syndication and federation applies to all the notions where several servers jointly concur to achieve an objective.

* Federation = all servers respect the same protocol. This facilitates interaction between the actors of the federation. But if all aspects of the protocol are federated \(including identification and authentication and admit a single reference system for business subject\). It can make access to the federation more implacable because to enter it,  All must implement the same protocol and often migrate to common servers \(authentification, identification référence system\).
* Syndication = each actor has his own protocol, which is translated into the common protocol to allow interaction. Moving the complexity to the integration layer.
* Mixed = The architecture may also be an hybrid : using common protocole \(syntaxe, API, ontology\) and centralized authentification  but not centralized référence system.

## Libraries to develop in semantic

A question was raised about the existence of mature libraries in different programming languages ​​to manage semantic data in SPARQL. Are there open-source libraries and code sample in the different languages ​​to query and process web-semantics? As it can exist for OpenAPI where there are libraries that can generate code samples.

There would be libraries in almost all languages, but these are more or less mature. In javascript in particular, the standardization is taking place, RDF-JS.. The situation is that with SPARQL it is more difficult that with OpenAPI or Graphql, who invest a lot of money for the community, and thus also finance the libraries that will interact with these standards. The libraries around SPARQL are more "fresh paint", less documented, more difficult to setup.

**There is a risk, and to check by each of the actors, so that it does not generate too much pain.**

## Transition strategy from current to ideal

**Basic principle: Improve the legacy rather than migrate to new technology**

### High vs Low granularity services

The reality of the platforms around the consortium today is that they are absolutely not designed for high granularity services. At best, as some low granularity services connected to each other via API. If each platform were to implement a standard based on a high granularity services architecture, each platform would have to re-assemble these high granularity services in low-granularity corresponding to their own low granularity services to be able to share their services. data via API ... Re-develop in their API a kind of "connector" between their services and high granularity services of the standard.

So the approach for the prototype is rather not to go to a 100% high granularity service state, but rather only on some relevant perimeter, without pushing the high granularity service logic to the extreme, but granular enough so that the API is simple to use. Depending on the potential of reusability of the service by others. For example, separating the product and inventory information is quite clear.  
To try to go as granular as possible, we can use as soon as necessary an aggregation bus \(like the [semantic bus](https://github.com/assemblee-virtuelle/Semantic-Bus) developed by Simon\), for example to switch from several high granularity services to a low granularity service, according to the respective configurations of each platform.

### Semantic web

A semantic approach in high granularity service would also require platforms to transform their database by a BDD TripleStore for example and that it is not the purpose of DFC to impose it. Also, developers do not master SPARQL today

**Conclusion: the actors of the consortium finally preferred to start by first developing a user friendly API using OpenAPI, in JSON-LD.**

Of course, we keep in mind the goal of moving towards a fully semantic and professional API when the DFC project budget has been secured. This approach being a little less experimental, it will allow us to connect 2 data sources to the prototype. This will require us to code existing API transformations into "REST resource driven semantic" APIs that respect the DFC ontology.

