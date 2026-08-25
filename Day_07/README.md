# Day 07 - JOIN and Aggregation

## 📌 Problem:

One order, `order_total` ₹5,000 and contains 3 line items: a shirt, jeans, and a belt.

You join orders with order_items to bring in the item details and then calculate total revenue:
```
SELECT SUM(o.order_total)
FROM orders o
JOIN order_items i 
    ON i.order_id = o.order_id;
```
The result shows ₹15,000 instead of the actual ₹5,000.

The revenue has been inflated 3 times because the order appears once for each line item.

### ❓ The question is:

Why does adding a `JOIN` inflate the `SUM`, and how do you get the true total back?

**Bonus Question:**

After the join, which one correctly counts the actual number of orders — `COUNT(*)` or `COUNT(DISTINCT o.order_id)`?

## 💡 Solution:

A `JOIN` can multiply rows before the aggregation takes place.

The order has 3 line items, so after the join, the same order appears in 3 rows. The `order_total` of ₹5,000 is therefore repeated across all 3 rows.

When `SUM(order_total)` runs, it calculates:
```
₹5,000 + ₹5,000 + ₹5,000 = ₹15,000
```
Nothing is wrong with the `SUM()` function. The issue is the grain of the data after the join.

The fix is to aggregate at the correct grain.

One approach is to calculate the order value from the item-level data:
```
SUM(i.quantity * i.price)
```
Another approach is to aggregate `order_items` to one row per order first and then join it with orders.

**The bonus:**

After the join:

`COUNT(*)` counts all rows created by the join.

`COUNT(DISTINCT o.order_id)` counts the actual orders.

### 📝 Query:

**Query with the Issue**
```
SELECT SUM(o.order_total)
FROM orders o
JOIN order_items i 
    ON i.order_id = o.order_id;
```
**Correct Query**
```
SELECT SUM(i.quantity * i.price) AS total_revenue
FROM orders o
JOIN order_items i 
    ON i.order_id = o.order_id;
```
## ⭐ Key Takeaways:

- A `JOIN` can multiply rows before aggregation.
- Repeated values can cause `SUM()` to produce inflated results.
- Always check the grain of the data after a join.
- `COUNT(*)` counts rows in the joined result.
- `COUNT(DISTINCT order_id)` counts unique orders.
- Before aggregating after a join, ask: Is each row still one order, or has it become one line item?
