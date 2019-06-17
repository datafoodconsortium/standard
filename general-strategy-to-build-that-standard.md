# General strategy to build that standard

### Remain independent

We could have setup a standardization group within GS1. We chose not to as GS1 were asking us to sign a form to tell we commit to use the GS1 standards. How can we commit to use a specific solution when we have not yet investigated our problem, the potential solutions available, etc. ? We are pretty happy we made that decision, as we remained free to work with Open Food Facts for products identification, which wouldn’t have been possible in the context of a GS1 standardization group.

### Collaborate with Open Food Facts for product identifiers and ontologies

We need a general common universal product id base to be able to match products between platform, and know if we talk about the same product, or another. In the agro-industry and retail industry agents use the EAN, GS1 product id codes. In the context of local and alternative food systems, short food chains, very few producers have subscribed to GS1 services, they don’t have EAN. And probably most won’t want to subscribe and pay the 85€ min yearly fee. So we have chosen to collaborate with [Open Food Facts](https://world.openfoodfacts.org/who-we-are) and use their identification system as our universal products identifiers. When there is an EAN, Open Food Facts use it as product identifier. When there is not they attribute one randomly, for free. We can call those ids “pseudo EAN”.

As we don’t want to force all actors to open their data \(this should remain the agent decision\), we will have our own “DFC universal product” catalog but we will use Open Food Facts API to generate ids for products which don’t have EAN. We also have chosen to use the same taxonomies as Open Food Facts to describe products, so a product file in the DFC catalog will be super easy to open and duplicate into Open Food Facts anytime the agent want to open their data.

### Collaborate with “The Commons” for shared data storage

We also choose a “neutral” agent to host the shared data of the consortium : this DFC universal products catalog. That avoid any conflict of interest among the consortium partners.

### Iterative process and prototype base development

When building both the semantic and technical specifications of the standard, we need real life cases to test if what we imagine is adapted, works well, enable to do what we want to do. So we are developing a prototype to not only make a “proof of concept” and show the potential of the standard to solve the problems mentioned above, but also to build and improve step by step the semantic and technical standard.

