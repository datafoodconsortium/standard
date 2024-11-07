---
description: Use cases related to Products
---

# Product Use Cases

1.  As a Producer I need to manage my product catalog across multiple sales platforms so that I can ensure I maximize my sales, without over-selling.

    &#x20;\
    DFC Product Catalog's allow multiple `Catalog Item` listings of a `Supplied Product,` with different stock limits (if required), all pulling from the same `Theoretical Stock` pool (on the `Supplied Product`. \

2. As a Distributor I need to constitute multiple catalogues of Products from multiple Producer Enterprises so that I can differentiate my sales by channel or brand.\
   \
   DFC `Catalogs` are managed by an `Enterprise`, that can be different from the `Enterprise` supplying the `Defined Product` \

3. As a Producer I need to link multiple Variants of a Product, so that I can easily group Variants together for a better Customer experience.\
   \
   _**TBC**_\

4. As a Distributor I need to organize my Products so that I can sell as multiple price points.\
   \
   `Offer` functionality allows multiple `Price` offers to be made to different `Customer Category'`s (e.g. member vs non-member price)\

5. As a Distributor, I need to organize my products to allow me to list products by seasonal availability so that I can ensure I have appropriate offers throughout the year.\
   \
   `Catalog` functionality within the standard provides the option to list `Supplied Products` multiple times, for multiple different markets... so there may be some (private) catalogs for wholesale buyers, local restaurants and food hubs; and public catalogs for retail customers. These catalogs may overlap but not fully match (e.g. some products may not have the volume to warrant wholesale listing). Multiple catalogs can be maintained to match seasonal Product availability.\

6. Product Demand
   1. As a consumer, I want to be able to demand a Product from Enterprises. \
      \
      The `Defined Product` structure denotes a `Functional Product` can be requested by an `Actor`. This gives anyone (Individual or Organization) the ability to place a requirement for a Product into the marketplace.\

   2. As an Enterprise, I can propose a product to answer the need. \
      \
      A `Supplied Product` can be proposed to match the requirement defined by the `Functional Product`. This then links to one or more `Catalog Items` to allow sale of the `Product`.\

   3. As another Enterprise, I can produce the Product proposed by the first enterprise that fulfills the need of the consumer.\
      \
      A `Localized Product` is constituted to reference a `Supplied Product`. This provides a reference representation of a `Physical Product`. \

7. Product Location/Tracking&#x20;
   1. As an Enterprise, I can anticipate the location of the product so that I can map inventory to predicted demand. \
      \
      \

   2. As an Enterprise, I know where my product is at a specific time so that I can control inventory against actual demand. \
      \

   3. As an Enterprise, I know the batch a Product came from so that I have product traceability through the supply chain.\
      \
      The `OrderLine` `isFulfilledBy` a PhysicalProduct, which can be traced to a `ProductBatch` containing traceability data.
8.
