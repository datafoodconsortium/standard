---
description: Use cases related to Products
---

# Product Use Cases

1.  As a Producer I need to manage my product catalog across multiple sales platforms so that I can ensure I maximize my sales, without over-selling.

    &#x20;\
    DFC Product Catalog's allow multiple `Catalog Item` listings of a `Supplied Product,` with different stock limits (if required), all pulling from the same `Theoretical Stock` pool (on the `Supplied Product`. \

2. As a Distributor I need to constitute multiple catalogues of Products from multiple Producer Enterprises so that I can differentiate my sales by channel or brand \
   \
   DFC `Catalogs` are managed by an `Enterprise`, that can be different from the `Enterprise` supplying the `Defined Product` \
   \
   \

3. As a Distributor I need to organize my Products so that I can sell as multiple price points.\
   \
   `Offer` functionality allows multiple `Price` offers to be made to different `Customer Category'`s (e.g. member vs non-member price)\

4. As a Distributor, I need to organize my products to allow me to list products by seasonal availability so that I can ensure I have appropriate offers throughout the year.\
   \
   `Catalog` functionality within the standard provides the option to list `Supplied Products` multiple times, for multiple different markets... so there may be some (private) catalogs for wholesale buyers, local restaurants and food hubs; and public catalogs for retail customers. These catalogs may overlap but not fully match (e.g. some products may not have the volume to warrant wholesale listing). Multiple catalogs can be maintained to match seasonal Product availability.\

5. Product Demand
   1. As a consumer, I want to be able to demand a Product from Enterprises. \
      \
      The `Defined Product` structure denotes a `Functional Product` can be requested by an `Actor`. This gives anyone (Individual or Organization) the ability to place a requirement for a Product into the marketplace.\

   2. As an Enterprise, I can propose a product to answer the need. \
      \
      A `Supplied Product` can be proposed to match the requirement defined by the `Functional Product`. This then links to one or more `Catalog Items` to allow sale of the `Product`.\

   3. As another Enterprise, I can produce the Product proposed by the first enterprise that fulfills the need of the consumer.\
      \
      \

6. Product Location/Tracking&#x20;
   1. As an enterprise, I can anticipate the location of the product so that I can map inventory to predicted demand.&#x20;
   2. As an enterprise, I know where my product is at a specific time so that I can control inventory against actual demand.&#x20;
   3. As an Enterprise, I know the batch a Product came from so that I have product traceability through the supply chain.

### Product Transformations

1. As a Producer I need to be able to create Products by combining other Products so that I can create further Products
2. As a Producer I need to be able to track the source of composed Products so that I can fulfill traceability requirements
3. As a Producer I need to be able to plan my Product transformations so that I can ensure I have all the required source products available
4. As a Producer I need to be able to request Products from suppliers in advance so that I can ensure my requirements are met.
5. As a Supplier I need to be able to propose, in advance, a Product to meet a requested need so that I can match buyer requirements to my planned output.
6. As a Supplier I need to be able to link my proposed Product to a Physical Product to ensure traceability of orders to deliveries/product batches.

Some products are “composed products”, they are made of other products, processed in some way to make a new product, like a tomato sauce for instance. There is a theoretical transformation that plans a transformation process without any notion of where the products are located, the “recipe” that connects products with one another independently from where they could be. For instance as an artisan cookie maker I can tell that I put 100g flour, 20g chocolate, 2 eggs, etc. in my cookies. In that case I express the recipe in term of “functional products”, I need flour and chocolate. I can be more precise and tell which type exactly of flour and chocolate I use and talk more in term of “technical products”, like wheat flour T110. And I can even tell exactly the farmer who's flour I used in my recipe, so express the recipe in terms of other “supplied products”. This is what we call the “as planned transformation flow”.

Then this recipe becomes something more like a “production workflow” that adds some notion of location. I have to move the tomato from “Awesome farm” to “TheKitchen”, the onions from “The Other Farm” to “TheKitchen”, etc. When all my components are in “TheKitchen” I can start the production process, cook, mix, bottle, etc. And get some jar of tomato sauce as output, which are located in “TheKitchen”. But this is still only a plan, a production map, I still don’t have the products, I’m just organising and planning the operations. We realised when iterating that transforming the nature of a located product was exactly the same flow as transforming the place where this product is supposed to be located. As the localised product is a combination of an ID product and an ID place, one flow was transforming the ID product, the other the ID place. So we are treating transportation as a specific transformation flow. Input will be for instance 100 x potatoes 1kg located in “Awesome Farm” and output will be 100 x potatoes 1kg located in “The Great Town Shop”.

Then when physical products are concerned this flow becomes a “realized transformation flow”.

1.
