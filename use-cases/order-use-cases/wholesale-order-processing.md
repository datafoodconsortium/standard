# Wholesale Order Processing

To manage wholesale ordering across platforms, we need to define a process to manage the stock as multiple orders come in from different traders, on different platforms, with different sales session cycles.

Traders will generate a single, consolidated Order for a producer, from multiple Customer orders within a Sales Session.

However, when a customer order is generated, we need to ensure stock is reserved/held, so that the Order can be fulfilled.

An example:

1. Supplier Jean is selling carrots through 2 hubs - Hub A & Hub B
2. Hub A has an order cycle that opens on Monday and closes on Friday
3. Hub B has an order cycle that opens on Tuesday and closes on Thursday
4. Jean has 10 kg of carrots to sell this week.
5. Customer Ali orders 1kg carrots through Hub A on Monday
6. Customer Paul orders 5 kg of carrots through Hub A on Tuesday
7. Customer David orders 3 kg of carrots through Hub B on Wednesday
8. Customer Michael orders 2 kg of carrots through Hub B on Thursday
9. When Hub B's order cycle closes on Thursday, it orders 5 kg of carrots from Jean.
10. When Hub A's order cycle closes on Friday, it orders 6 kg of carrots from Jean.
11. Jean is 1 kg of carrots short. If Hub A can only get 5 kg of carrots, Paul or Ali (who ordered earlier than David & Michael) won't get their order fulfilled.

To manage this, we need to ensure stock is reserved as the Customer Orders come in, but the Order is not completed until the Sales Session is finished.

This is managed (using Order & Fulfillment States) with the following flow:

**Trader Platform Process flow:**

1. When Customer initiates checkout: Create or Update an Order with the Producer to include all additional Products in Customer basket. Place Order in a `Held` `OrderState` (to reserve Producer stock), and with a `FulfilmentState` of `Held`, pending payment processing.
2. On successful completion of checkout of Customer Order: do nothing.
3. On unsuccessful completion of checkout of Customer Order: update Producer Order to remove additional Products required to fulfill that Order (or delete to newly created).
4. On completion of the Trader's Sales Session: submit (aggregate) the Producer Order by updating `OrderState` to `Complete` and the `FulfilmentState` to `Unfulfilled`.
