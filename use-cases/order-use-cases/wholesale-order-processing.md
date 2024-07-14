# Wholesale Order Processing

To manage wholesale ordering across platforms, we need to define a process to manage the stock as multiple orders come in across different sales sessions.

**Trader Platform Process flow:**

1. Place a temporary hold on Producer stock, pending payment processing
2. Complete payment process
3. Place Order in  reserve Producer stock
4. Submit (aggregate) order & decrement stock level once sales cycle closes.

We have agreed previously that hubs will generate a single, consolidated order for a supplier, from multiple customer order.

However, when a customer order is generated, we need a method to ensure stock is reserved/held, so that the order can be fulfilled.

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

To fix this, we need a way to track stock levels as orders come in.
