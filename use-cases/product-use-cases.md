---
description: Use cases related to Products
---

# Product Use Cases

1. As a Producer I need to manage my product catalog across multiple sales platforms so that I can ensure I maximize my sales, without over-selling.
2. As a Distributor I need to constitute multiple catalogues of Products from multiple Producer Enterprises so that I can differentiate my sales by channel or brand&#x20;
3. As a Distributor I need to organize my Products so that I can sell as multiple price points.
4. As a Distributor, I need to organize my products to allow me to list products by seasonal availability so that I can ensure I have appropriate offers throughout the year.
5. &#x20;
6. Product Demand
   1. As a consumer, I want to be able to demand a Product from Enterprises.&#x20;
   2. As an Enterprise, I can propose a product to answer the need.&#x20;
   3. As another Enterprise, I can produce the Product proposed by the first enterprise that fulfills the need of the consumer.
7. Product Location/Tracking&#x20;
   1. As an enterprise, I can anticipate the location of the product.&#x20;
   2. As an enterprise, I know where my product is at a specific time.&#x20;
   3. As an Enterprise, I know the batch a Product came from.

### Product Transformations

1. As a Producer I need to be able to create Products by combining other Products so that I can create further Products
2. As a Producer I need to be able to track the source of composed Products so that I can fulfill traceability requirements
3. As a Producer I need to be able to plan my Product transformations so that I can ensure I have all the required source products available
4.

Some products are “composed products”, they are made of other products, processed in some way to make a new product, like a tomato sauce for instance. There is a theoretical transformation that plans a transformation process without any notion of where the products are located, the “recipe” that connects products with one another independently from where they could be. For instance as an artisan cookie maker I can tell that I put 100g flour, 20g chocolate, 2 eggs, etc. in my cookies. In that case I express the recipe in term of “functional products”, I need flour and chocolate. I can be more precise and tell which type exactly of flour and chocolate I use and talk more in term of “technical products”, like wheat flour T110. And I can even tell exactly the farmer who's flour I used in my recipe, so express the recipe in terms of other “supplied products”. This is what we call the “as planned transformation flow”.

Then this recipe becomes something more like a “production workflow” that adds some notion of location. I have to move the tomato from “Awesome farm” to “TheKitchen”, the onions from “The Other Farm” to “TheKitchen”, etc. When all my components are in “TheKitchen” I can start the production process, cook, mix, bottle, etc. And get some jar of tomato sauce as output, which are located in “TheKitchen”. But this is still only a plan, a production map, I still don’t have the products, I’m just organising and planning the operations. We realised when iterating that transforming the nature of a located product was exactly the same flow as transforming the place where this product is supposed to be located. As the localised product is a combination of an ID product and an ID place, one flow was transforming the ID product, the other the ID place. So we are treating transportation as a specific transformation flow. Input will be for instance 100 x potatoes 1kg located in “Awesome Farm” and output will be 100 x potatoes 1kg located in “The Great Town Shop”.

Then when physical products are concerned this flow becomes a “realized transformation flow”.

1.
