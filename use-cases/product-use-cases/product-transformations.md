# Product Transformations

Some products are “composed products”, they are made of other products, processed in some way to make a new product, like a tomato sauce for instance.&#x20;

There is a theoretical transformation that plans a transformation process without any notion of where the products are located, the “recipe” that connects products with one another independently from where they could be. For instance as an artisan cookie maker I can tell that I put 100g flour, 20g chocolate, 2 eggs, etc. in my cookies. In that case I express the recipe in term of “functional products”, I need flour and chocolate.&#x20;

I can be more precise and tell which type exactly of flour and chocolate I use and talk more in term of “technical products”, like wheat flour T110. And I can even tell exactly the farmer who's flour I used in my recipe, so express the recipe in terms of other “supplied products”. This is what we call the “as planned transformation flow”.

Then this recipe becomes something more like a “production workflow” that adds some notion of location. I have to move the tomato from “Awesome farm” to “TheKitchen”, the onions from “The Other Farm” to “TheKitchen”, etc. When all my components are in “TheKitchen” I can start the production process, cook, mix, bottle, etc. And get some jar of tomato sauce as output, which are located in “TheKitchen”.&#x20;

This is still only a plan, a production map, I still don’t have the products, I’m just organising and planning the operations. &#x20;

Then when physical products are concerned this flow becomes a “realized transformation flow”.

<div data-full-width="true">

<figure><img src="../../.gitbook/assets/DFC Ontologie-TransformationFlow.png" alt=""><figcaption><p>Transformation Flow Portion of the Ontology</p></figcaption></figure>

</div>

### Transportation as a Transformation Flow

We realized when iterating that transforming the nature of a located product was exactly the same flow as transforming the place where this product is supposed to be located. As the Localized Product is a combination of an Product ID  and an Place ID, one flow was transforming the Product ID, the other the Place ID. **So we are treating transportation as a specific transformation flow.** Input will be for instance 100 x potatoes 1kg located in “Awesome Farm” and output will be 100 x potatoes 1kg located in “The Great Town Shop”.

### Product Transformation Use Cases

1. As a Producer I need to be able to create Products by combining other Products so that I can create further Products\
   \
   \

2. As a Producer I need to be able to track the source of composed Products so that I can fulfill traceability requirements\
   \
   \

3. As a Producer I need to be able to plan my Product transformations so that I can ensure I have all the required source products available\
   \
   \

4. As a Producer I need to be able to request Products from suppliers in advance so that I can ensure my requirements are met.\
   \
   \

5. As a Supplier I need to be able to propose, in advance, a Product to meet a requested need so that I can match buyer requirements to my planned output.\
   \
   \

6. As a Supplier I need to be able to link my proposed Product to a Physical Product to ensure traceability of orders to deliveries/product batches.\
   \


