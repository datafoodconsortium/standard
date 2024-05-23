# Order States

Within the DFC Standard is it necessary to indicate what the status of an Order is. This status can be broken down into 3 categories:

1. The overall state of the Order (Order State)
2. The status of any/all payments related to the Order (Payment State)
3. How far the Order is through processing (Fulfilment State)

## Order States

| DFC Class |  States	|	Description |
| ----------- | ---------- | ------------------------------- |
| Order States | Draft | Order is in a draft state - generally before it is ready for completion - customer can still amend the order. Stock is not reserved. |
|  | Held | Order is held - no activity should be carried out, but order cannot be amended by the customer. Could be due to a variety of conditions (e.g. payment rejected, out of stock errors) |
| | Complete | Order is finalied and will be ready for fulfilment (although that may not be yet, see Fulfilment States)
	Cancelled	Order has been cancelled - no further activity should be carried out on the order |
| Fulfilment States | Held | Order is Complete, but cannot be fulfilled at this time (e.g. order is a scheduled future order)
|  | Unfulfilled | Order is ready for fulfilment/shipment |
|  | 	Fulfillled | Order has been fulfilled/shipped |
|  | Cancelled | Order is cancelled, no further fulfilment activity should be carried out |
| Payment States | Unpaid | Full or partial payment is still required on the order |
|  | Paid | Full payment has been successfully received on the order |
|  | Cancelled	Payment has been cancelled, Order State should be Held or Cancelled & no further activity should be carried out until Payment issues are resolved. |
