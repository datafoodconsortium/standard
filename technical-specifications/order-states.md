# Order States

Within the DFC Standard is it necessary to indicate what the status of an Order is. This status can be broken down into 3 categories:

1. The overall state of the Order (Order State)
2. The status of any/all payments related to the Order (Payment State)
3. How far the Order is through processing (Fulfilment State)

## Order States

| DFC Class |  DFC State	|	Description |  Socleo Equivalent | OFN Equivalent | Shopify Equivalent |
| :---------- | :--------- | :------------------------------ | :--------------: | :----------------: | :--------------: |
| Order States | Draft | Order is in a draft state - generally before it is ready for completion - customer can still amend the order. Stock is not reserved. | draft |	cart <br>address <br>delivery <br>confirmation <br>payment <br>resumed | Order/Open |
|  | Held | Order is held - no activity should be carried out, but order cannot be amended by the customer. Could be due to a variety of conditions (e.g. payment rejected, out of stock errors) | invoiced | | Order/Open |
| | Complete | Order is finalied and will be ready for fulfilment (although that may not be yet, see Fulfilment States) | validated <br>closed <br>awaiting_payment | complete | Order/Archived |
| | Cancelled | Order has been cancelled - no further activity should be carried out on the order | canceled <br>retracted <br>refused | canceled | Order/Cancelled |
| Fulfilment States | Held | Order is Complete, but cannot be fulfilled at this time (e.g. order is a scheduled future order) |  | shipment/pending | Fulfilment/On Hold <br>Fulfilment/Scheduled |
|  | Unfulfilled | Order is ready for fulfilment/shipment | In preparation <br>prepared | shipment/ready | Fulfilment/Unfilfillled |
|  | 	Fulfillled | Order has been fulfilled/shipped | Shipped <br>Delivered | shipment/shipped | Fulfilment/Fulfilled |
|  | Cancelled | Order is cancelled, no further fulfilment activity should be carried out |  |  | shipment/canceled | Order/Cancelled |
| Payment States | Unpaid | Full or partial payment is still required on the order |  | payment/checkout <br>payment/balance_due <br>payment/processing <br>payment/requires_authorization <br>payment/failed | Payment/Unpaid (including: <br>Payment/Pending <br>Payment/Overdue <br>Payment/Authorised <br>Payment/PartiallyPaid ) |
|  | Paid | Full payment has been successfully received on the order |  | payment/credit_owed <br>payment/completed | Payment/Paid |
|  | Cancelled	Payment has been cancelled, Order State should be Held or Cancelled & no further activity should be carried out until Payment issues are resolved. |  | payment/void | Payment/Voided |
