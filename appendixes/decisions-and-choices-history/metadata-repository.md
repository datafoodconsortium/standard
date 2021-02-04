# Metadata repository

## Users

To interoperate data between platforms, we need to know that user X on platform 1 is user Y on platform 2, that they are the same agent. For that we need either a unique identifier for users on top of a way to authenticate them. The user repository is essential here. It usually includes for each user a login, a password \(hashed of course\), an ID, and information about the user.

### Centralized repository

_**One ID per user**_

![](https://lh3.googleusercontent.com/R3qflqSINbyTah5m4fLWFcD9xuYufUMD09BrinU2rRW0TlPR1crVn_t_-2Ixvf-mtcKVhSnrf9aNX7nD4_wHaGPGYc0vngGJ7_0YvqRzIUXzfyMDou9Kke-YQ6ANztPW3EQGq3SB)

Principle: All platforms allow connection with the OIDC server of the commons that users perceive like the DFC server. A platform user must have associated their account with an OIDC common account to take advantage of DFC features.

* Advantage: One OIDC account for the user. Simplicity architecture and implementation for DFC. Users can continue without the DFC OIDC account but will not be able to enjoy the associated features.
* Disadvantage: Platforms need to perform developments to be able to recognize the OIDC server of the commons as a means of identification / authentication and allow existing users to associate their existing account with a DFC OIDC account

_**One ID per platform**_

![](https://lh5.googleusercontent.com/HdvWJ_hzxaoLw_7o-NXj6bHY02JzPRC51wPb7gQNJUzikY8xJzU0jiSBgKOQOBsl0SphiNmoh6Fk01z8dwNIWMjTzypiIchUFz8i3dKg4zZZ3b3WMIL5au8jpKQ4Wj8x3Ntcjk2y)

Principle: The platforms \(including DFC\) each have an identifier on the oidc server of DFC.

* Advantage: Simple architecture and implementation for DFC. No need to ask users to associate their account with the OIDC DFC server. Platforms can communicate with each other without DFC. transparent for users.
* Disadvantage: It is not possible to differentiate rights by incoming users. A platform 1 grants a set of rights to the platform 2 for example without user distinctions. It is possible, however, that the user specifies on platform B what is entitled to see the platform A. The platforms need to realize developments to be able to validate the identification / authentication OIDC

### Decentralized repository

_**One ID per user**_

![](https://lh6.googleusercontent.com/Vdi82ZngTIlBcxhbOp_VUVpSz4GP2sSEvxtnssNvUhKruIlQQ9Q4ctHH6RuRq-Noi3XVVARgMUMTVlA0Uz5br5k_7JBTqcXmGFr_03olx5EwRKht3IM39NfhZeSdMPQNZRCKRZ42)

Principle: Each platform and DFC have an authentication server. The user must inform DFC about his access to his own data \(user, password token ...\). DFC can then mediate and platform 1 can request data from platform 2 through DFC as it is the only place where the mapping between the 2 accounts is specified. The identification and authentication protocols may be different between platforms but this will increase the complexity of the DFC code.

* Advantage: Platforms do not need to reimplement / improve their authentication / authentication
* Disadvantage: multiple accounts for the user. Complex code DFC side

_**One ID per platform**_

![](https://lh5.googleusercontent.com/Nt4QSohFKuENPQUYkiZ4a1EHSliugRlD2edZAZKcQuxQhpgHytV-U4Ryr9qkk6kTxTqGuemnrsv4e0ehk0Tjc9cFod8vxPMv74l89Iq4VO9ZkIQOeLMPOFhulO-9xz6Tn-Gagct_)

Principle: The platforms \(including DFC\) each have an identifier on each platform

* Benefit: No need to ask users to associate their account with the OIDC DFC server. The platforms can communicate with each other without DFC but with a technical cost \(see disadvantage\). transparent for users.
* Disadvantage: It is not possible to differentiate rights by incoming users. A platform 1 grants a set of rights to the platform 2 for example without user distinctions. However, it is possible for the user to specify on platform B what is entitled to see platform A. Code relatively complex DFC side but less than solution 1. In the case of direct access \(without DFC\), if a new platform arrives in the ecosystem: the new platform will have to configure all the protocols / token of the others and the others will have to add the protocol / token of the new one.

**Conclusion:**

* **Short term :** The platforms are ready to make an effort to integrate an OIDC authentication into their platform and allow their users to complete their platform-specific authentication via OIDC. **So we chose a centralized user repository with one user ID.** The chosen OIDC server is that of the collective "Les Communs \(the commons\)". This group is a legitimate leader in strategic thinking for the commons and the provision of IT tools to enable the commons to organize. This server will then be our unique source of user universal identifiers. To start with and identify matching users on Open Food Facts, as Open Food Facts is not going to complete their platform authentication via OIDC, we will use SIRET to identify the corresponding users and user their data. It is possible because all data of Open Food Fact are Open-Data and don’t need authentication.
* **Long term**: We hope that web-id / OIDC or other DID protocol will mature and that we will be able to rely on it for decentralized authentication. That would enable us to manage those unique universal identifiers in a decentralized way. It would also enable to manage agent characterization information, that are not managed through OIDC \(only basic info like login and password are currently managed\). If we need later on to manage conflicts between agent description facets and decide which info is right, we might need also some shared database to store those universal conflict-free / trusted information.

&gt;&gt;&gt; [Access the source for the drawings](https://www.draw.io/?state=%7B%22ids%22:%5B%221Zia2iwl-GkYc77qowCU0kHNzVNgQqa9V%22%5D,%22action%22:%22open%22,%22userId%22:%22115151052281975084839%22%7D#G1Zia2iwl-GkYc77qowCU0kHNzVNgQqa9V)

## Products

To be able to determine that two products on two platforms are the same, it is essential to have a common identifier.

### Decentralized repository

If several servers are able to manage product identifiers, this means that the product must only be created on one platform. If this product is to be used by a second platform, it must refer to the identifier created on the first platform. This requires an infrastructure capable of crowdsourcing different sources and is difficult to do without a semantic web. The semantic web is based on the uri that contains the address of the server on which the initial resource was created. Other servers can manage information about this product but the identifier remains linked to the original server.

It is also possible to issue ID ranges per platform. This is the logic of GS1 but it means that it is not possible for a platform to create an identifier without having previously requested its range from a centralized entity that realizes the assignment of the identification ranges. It is therefore a false decentralization because there is a centralized entity from which all the entities of the consortium depend.

#### Centralized repository

It is easier to entrust to a single entity the creation of product identifiers. This does not prevent each platform from managing its own internal identifiers, but platform products will not be interoperable with other platforms unless they are linked to a central repository identifier. This means that there is a need for a simple and ergonomic solution so that platform users can easily link their products that they manage on a platform with a centralized identifier. When choosing this trusted third party, it is important to make an alliance with a third party that already has a legitimate leading position and is able to generate identifiers recognized by all market players. Open Food Fact is able to reference GS1 managed EANs for packaged products and is also able to generate identifiers in a reserved range \(let’s call them “pseudo EAN”\). They have a healthy governance and in line with the values ​​of DFC. They have a leading position in the field of factual information on food products.

**Conclusion:**

* **Short term**: given the pre-existence of several platforms that already have their product identifiers, it seems complex and inconsistent to start with decentralized identifiers. The most consistent strategy is to use Open Food Fact IDs and generate one for products that do not have an API. We pay great attention to ergonomics so that platform users have the least possible difficulty in associating the identifier of their platform with an Open Food Facts ID.
* **Long term**: moving to completely decentralized management of product identifiers would be possible thanks to the Semantic Web and Solid but it requires a complex migration work and a difficult strategy to identify. It is therefore a solution that remains in the vision but still requires a lot of research and development.

**Operational implication :** DFC would have a specific universal products catalog, in a separate database from Open Food Facts, hosted by a trusted third party \(Les Communs in France\). This will enable DFC to not force users to open their products data, as any info on Open Food Facts is open data. When a product _\*\*_is imported on DFC catalog, we would check \(using enterprise ID SIRET first, then hopefully only OIDC\) if the user already have corresponding products in Open Food Facts, and in that case, match the product and use the corresponding existing id \(EAN or pseudo EAN\). If no matching product is found, and the product doesn’t have an EAN, we would use the Open Food Facts API to generate a new pseudo EAN. Products on the DFC catalog will be easy to push to Open Food Facts whenever a user wants to open their data. On a first step the mapping between products will happen through the prototype, which will send back universal id information to the original platform. On a second step, each platform might want to integrate on their own interface a module to link /create a given product to it’s universal DFC entry.

## Places

Platforms reference places, but there can be cases where a same place is not referenced the same way in two platforms, so mapping them to GPS coordinates might not give the same coordinates even if it’s the same place. As for users and products, we need a way to know that a place in platform A is the same place in platform B. Especially when we move on in the development of the prototype, step 2 and 3, we will need to be able to identify places, so we can search places within x km distance of departure or arrival.

We could at some point, like users and products, have ids for places. Open Street Map could be a good candidate to manage universal location ids, but it seems it is not the case for the moment, as they reallocate ids when places are delete. Also some addresses are not recognized today by Open Street Map. GS1 did work on the topic as well, we would need to investigate better their solution.

This problem has not fully been investigated yet.

