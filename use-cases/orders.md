---
description: >-
  This section expands on what Orders are within the DFC Standard and how we
  represent them.
---

# Orders

Orders are patterns for processing the transfer of goods between actors. They can be processed and transferred immediately (e.g. a sale at a market stall) or there can be a delay whilst goods are prepared, packed and/or dispatched (e.g. an online veg box order).

All Orders happen within a Sales Session (the Farmers Market, or an Order Cycle for an online platform). The Order requires a Customer (an Actor linked by the `dfc-b:OrderedBy` relationship)

The status of the Order is defined by 3 States - `OrderState`, `PaymentState` & `FulfillmentState`. These are further enumerated & explained in the relevant section.

#### Example Order JSON-LD

```
"@context": https://www.datafoodconsortium.org
"@id": http://test.host/api/dfc/Enterprises/10000/Orders/10001
"@type": dfc-b:Order
dfc-b:belongsTo: http://test.host/api/dfc/Enterprises/10000/SaleSessions/10002
dfc-b:orderNumber: "MYORDERNUM:12345"
dfc-b:hasOrderStatus: dfc-v:Draft
dfc-b:hasFulfilmentStatus: dfc-v:Held
dfc-b:hasPaymentState: dfc-v:Unpaid
dfc-b:hasPaymentMethod:
  "@type": dfc-b:PaymentMethod
  dfc-b:paymentMethodType: Example Card PaymentMethod
  dfc-b:paymentMethodProvider: Stripe
  dfc-b:hasPrice:
    "@type": dfc-b:QuantitativeValue
    dfc-b:hasUnit: GBP
    dfc-b:value: £0.27
    dfc-b:VATrate: 0.00
dfc-b:discount: 1.55    # any overall discount applied to the Order (individual Product discounts can be applied to Offers)
dfc-b:OrderedBy: http://test.host/api/dfc/Persons/10000
dfc-b:selects:
  "@type": dfc-b:ShippingOption
  dfc-b:optionOf: http://test.host/api/dfc/Enterprises/10000/SaleSessions/10002
  dfc-b:fee: 1.50
dfc-b:uses:
  "@type": dfc-b:PickupOption
  dfc-b:pickedUpAt: Our Fantastic Farm Gate
dfc-b:soldBy: http://test.host/api/dfc/Enterprises/10000
dfc-b:hasPart:
- "@id": http://test.host/api/dfc/Enterprises/10000/Orders/10001/orderlines/10001-01
  "@type": dfc-b:OrderLine
  dfc-b:concerns: http://test.host/api/dfc/Enterprises/10000/SuppliedProducts/10001
  dfc-b:hasQuantity:
    "@type": dfc-b:QuantitativeValue
    dfc-b:hasUnit: Packet
    dfc-b:hasValue: 5.0
  dfc-b:Price:              # the total price for the Order Line
    "@type": dfc-b:QuantitativeValue
    dfc-b:hasUnit: GBP
    dfc-b:value: 19.95
    dfc-b:VATrate: 0.0
  dfc-b:discount: []

```
