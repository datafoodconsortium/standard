# Business model and ontology

This specification has been built since March 2017, through alternation between field interviews and modelization. We have been supported in that work by an ontologist, [Bernard Chabot](https://docs.google.com/document/d/1vLYI4pv-lqcy7WLoMN9XWROPh1FayXFU5g4zA5blmEQ/edit?usp=sharing).

The human readable modelization can be represented as follow:

![](https://lh5.googleusercontent.com/KzmDJ63oH6uLhA7z9qgknOEwdEGWQRWIVkJFvFOtTGSEwyf9XjCo44evKK1a4prG4pbUkaPCKwJQAGNaG_9SkYLgd_dABOvkXetf9xJNo4kXtjliAZT28WlL8VwBi8nndUA_VWUc)

[Click here to access a zoomable version of the document.](https://docs.google.com/presentation/d/157i0ySW3T89KviZHmderXl7X0ywuvtz0QunaHJcEF_Q/edit?usp=sharing)

### Products

* We have identified the different concepts of products we manipulate. We distinguished the product “need” \(what I want as a customer\), from the product “answer” \(what I propose as a distributor to satisfy your need\), from the product “supply” \(what I propose as a producer that enable distributors to meet their promises to customers\), all those products being manipulated without any “location” notion.
* Then a producer identifies a location where their products are supposed to be when they become real product. We call them “localized products”, which is a combination between an ID product \(what product it is\) and an ID place \(where it is\). For instance, the potatoes of “Awesome farm” are in theory localized in the farm itself.
* When the potatoes get harvested they become “physical products”, real products that you can hold in your hand. A physical product is also always located somewhere, and belongs to a product batch.

### Transformation

* Some products are “composed products”, they are made of other products, processed in some way to make a new product, like a tomato sauce for instance. There is a theoretical transformation that plans a transformation process without any notion of where the products are located, the “recipe” that connects products with one another independently from where they could be. For instance as an artisan cookies maker I can tell that I put 100g flour, 20g chocolate, 2 eggs, etc. in my cookies. In that case I express the recipe in term of “functional products”, I need flour and chocolate. I can be more precise and tell which type exactly of flour and chocolate I use and talk more in term of “technical products”, like wheat flour T110. And I can even tell exactly the flour from which farmer I used in my recipe, so express the recipe in terms of other “supplied products”. This is what we call the “as planned transformation flow”.
* Then this recipe becomes something more like a “production workflow” that adds some notion of location. I have to move the tomato from “Awesome farm” to “TheKitchen”, the onions from “The Other Farm” to “TheKitchen”, etc. When all my components are in “TheKitchen” I can start the production process, cook, mix, bottle, etc. And get some jar of tomato sauce as output, which are located in “TheKitchen”. But this is still only a plan, a production map, I still don’t have the products, I’m just organizing and planning the operations. We realized when iterating that transforming the nature of a located product was exactly the same flow as transforming the place where this product is supposed to be located. As the localized product is a combination of an ID product and an ID place, one flow was transforming the ID product, the other the ID place. So we are treating transportation as a specific transformation flow. Input will be for instance 100 x potatoes 1kg located in “Awesome Farm” and output will be 100 x potatoes 1kg located in “The Great Town Shop”.
* Then when physical products are concerned this flow becomes a “realized transformation flow”.

### Sales operations

* A distributor \(an enterprise\) constitutes its catalog, made of “catalog items”, and build offers for them given their customer categories. The product offered can be, depending on the sales and marketing strategy of the distributor, a functional product \(ex: tomatoes to stuff\), a technical product \(ex: beefsteak tomatoes\) or a supplied product \(ex: beefsteak tomatoes from Awesome Farm\). A given agent can have a catalog on various repositories \(i.e. platforms\) so a same enterprise can have multiple catalog items for the same product, if they use multiple platforms for instance. We will be able to match them using the unique product identifier.
* A sale session aggregates offers under certain shopping conditions \(opening / closing dates, etc.\)
* The customer makes an order with various order lines in a specific sale session and choose a shipping option - that can be delivery \(they are delivered to their home or business address\) or pick-up \(they need to collect the product in a location defined by the distributor\) - and a payment option among those defined by the distributor.
* The sale happens in a place that can be physical \(a physical store\) or virtual \(an online store\). Note that this works as well for a physical store: technically each day the store opens and close at a define time, and each day can be considered as a specific sale session. In the case of physical store sales, the shipping option is implicitly “collect on site”.

### Transaction

When an agent has ordered products and products has been delivered through a last transformation flow \(transport\), the ownership of the product changes hands. The transaction is then officially happening and the previous product owner can invoice the customer.

