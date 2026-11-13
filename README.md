# Shopora
Shopora is a fictional online shop that sells everyday products in ten groups, from electronics and home goods to fashion, beauty, food and garden supplies. It has just over 300 products. Shopora buys them from suppliers and sells them straight to customers on its own website and app, not through other sellers.
Shopora delivers to five regions —North, South, East, West and Central —and every customer is given a promised delivery datewhen they order. Customers are grouped into three types: new, returning, and Premium members who pay a monthly fee for free, faster delivery. People can pay by card, digital wallet, bank transfer, or cash when the order arrives.
Shopora tries to win customers with low prices and a wide choice. Price cuts run all year, the November and December rush is built on big discounts, and each product group manager decides their own discount levels. Between January 2024 and December 2025, Shopora handled about 15,000 orders from around 4,200 customers.

## The Business Problem
Shopora is selling more every year, but keeping less and less of the money it makes. Big discounts, too many returns and late deliveries are all eating into what is left. If nothing
changes, Shopora could keep growing its sales and still lose money. Someone needs to look at the data, find out what is really going wrong, and say clearly what to fix.


## Data Source
4 tables were used for this analysis
- Orders
- Order-Items
- Customers
- Products

## Data Preparation
For the Orders table, 6 issues were found and cleaned before analysis
                                                          
| #  | Problem    |  Fix
|---------------------------------------------------
|  1  | 42 exact duplicate rows  | Remove duplicates on Order_ID
|  2  | Status spelled many 


## Data Quality Issues and Fixes

| # | Problem | Fix |
|---|---------|-----|
| **1** | 42 exact duplicate rows | Remove duplicates on `Order_ID` |
| **2** | Status spelled many ways, e.g. `Deliverd`, `DELIVERED`, `Canceled`, `In Processing` | Standardise to 4 values: **Delivered, Shipped, Processing, Cancelled** |
| **3** | 20+ payment variants, e.g. `GPay`, `COD`, `Pay Pal`, plus 120 blanks | Standardise to 6 payment methods; replace blanks with **Unknown** |
| **4** | `Order_Date` and `Actual_Delivery_Date` mix real dates with text formats such as `16/01/2024` and `02.01.2024` (day first) | Parse text as **UK dates (DD/MM/YYYY)**, then set the data type to **Date** |
| **5** | `Shipping_Cost` contains text values such as `£3.46`, 14 negative values, and 166 blanks | Remove the `£` symbol, investigate negative values, and determine how blanks should be handled |
| **6** | 515 non-delivered orders have a delivery date, while 91 delivered orders have no delivery date | Per the data dictionary, blank the delivery date unless the status is **Delivered**; exclude the 91 missing delivery dates from the **On-Time Delivery %** calculation |



