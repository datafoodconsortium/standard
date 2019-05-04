# Product ontology

This specification has been built since March 2017, through alternation between field interviews and modelization. We have been supported in that work by an ontologist, [Bernard Chabot](https://docs.google.com/document/d/1vLYI4pv-lqcy7WLoMN9XWROPh1FayXFU5g4zA5blmEQ/edit?usp=sharing).

Here is a simplified representation of the product ontology:

![](https://lh3.googleusercontent.com/B4uNamIvtaA6hfC5rofcnvSunb-U2tGNhMBMbO3rVK-dRd9JNzghKnbf5s1S-F3MSXw29uRs2Ei4QFR_L-Rh1uX8dtP9ij8qL7p37QsB8A3cJl_ltN7RAGRaq9ydnkDdY4y5mUB0)

[Click here to access a zoomable version of the document.](https://docs.google.com/presentation/d/157i0ySW3T89KviZHmderXl7X0ywuvtz0QunaHJcEF_Q/edit?usp=sharing)

The question of products identification is treated in the technical standard and won’t be discussed here. We came up with that model through various iterations, you can especially [check that spreadsheet](https://drive.google.com/open?id=1l0wCwerm1ZW6zkUF4uB_A8u6B-MRaY3DmKSZKK9Z0vc) illustrating the cases we used to illustrate and validate our last iteration. We chose to characterize products through various orthogonal facets that enable precise understanding, rich research and comparisons. This will be really useful for several use cases. They will enable platforms to order products received from automated data exchange in their own appropriate taxonomy. Also, actors will be able to make searches in a pull of products from various platforms using various custom taxonomies.  


So we choose the following “criteria” to uniquely identify products:

* Product type: this is a basic sales oriented taxonomy \(are you selling carrots or soap ?\).
* Then some facets help identify more precisely the products:
  * If the product has a unique “living/mineral” source, what is the source? For instance a steak has as source a living cow from a specific breed. If a product is composed it can be decomposed through the “transformation flow” mentioned above and each component can be described following the same process.
  * If the product has a unique “living/mineral” source, what part or product of this source is used? For instance honey comes from a unique living source which is a bee from a specific breed. The part of product of source concerned here is “honey”. If we talk about a carrot, the source will be the carrot variety, and the part concerned will be “the root”. If I talk about carrots seeds, the part concerned will be the seeds. If I sell carrots with the leaves it will be “whole plant”.
  * Every product sold will have gone through some sort of processes, at a minimum a tomato has been “harvested” from the living tomato plant. So even what we call “raw products” have undergone some basic process. Salt will have been “dried”, etc.

Each product has a geographical origin, it comes from somewhere. It can be a territorial origin \(ex: France, Nice, etc.\) or a general global origin \(ex: north west atlantic sea\) so this facet can have values from two taxonomies even though there is only one facet. A cocoa bean can be from the same producer \(big farmer owning lots of parcels for instance\), go through the same harvesting, fermenting, 



* * drying processes, come from the same variety of cocoa plant, BUT come from a different location.
  * Products can have certifications/labels
  * Products can have some physical characteristics \(soft, tender, etc.\)
  * Products can have some specific claims \(“gluten free”, “zero salt”, etc.\)
  * And products can have a specific brand
* And to finish a product is also identified with a dimension and unit, like potatoes are sold by 1 x kg, a jar of tomato sauce is sold by 1 x item, the item here being a “jar of 500 ml tomato sauce”.

Behind each of those facets, we need taxonomies, or it can be a free field but then it’s hard to make searches !

As we understand how complex it is to maintain taxonomies alone, we decided, just as we did for product identification \(see [technical specification / metadata repository](https://docs.google.com/document/d/1vLYI4pv-lqcy7WLoMN9XWROPh1FayXFU5g4zA5blmEQ/edit#heading=h.numqwivjctc8)\), to use Open Food Facts taxonomies. They are not totally aligned with the way we wanted to describe products so for the first real life test \(prototype\) we will ignore some of our facets. We are working hand in hand to make our complementary approaches converge.  


Depending on the use case, we might have to ask a user to fill in some unfilled info if they are needed by the integrated platform to process the data. For instance, if a product is in the category “apple” but no variety was filled in. And the data receiving platform has two categories “acidic apple” and “sugary apple”, how can the receiving platform know where to put the product ? So in that case, the UX will require to as the user to fill in missing data.

