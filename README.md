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

#### Orders table: 6 issues were found and cleaned before analysis
                                                          

| # | Problem | Fix |
|---|---------|-----|
| **1** | 42 exact duplicate rows | Remove duplicates on `Order_ID` |
| **2** | Status spelled many ways, e.g. `Deliverd`, `DELIVERED`, `Canceled`, `In Processing` | Standardise to 4 values: **Delivered, Shipped, Processing, Cancelled** |
| **3** | 20+ payment variants, e.g. `GPay`, `COD`, `Pay Pal`, plus 120 blanks | Standardise to 6 payment methods; replace blanks with **Unknown** |
| **4** | `Order_Date` and `Actual_Delivery_Date` mix real dates with text formats such as `16/01/2024` and `02.01.2024` (day first) | Parse text as **UK dates (DD/MM/YYYY)**, then set the data type to **Date** |
| **5** | `Shipping_Cost` contains text values such as `£3.46`, 14 negative values, and 166 blanks | Remove the `£` symbol, investigate negative values, and determine how blanks should be handled |
| **6** | 515 non-delivered orders have a delivery date, while 91 delivered orders have no delivery date | Per the data dictionary, blank the delivery date unless the status is **Delivered**; exclude the 91 missing delivery dates from the **On-Time Delivery %** calculation |


Data Cleaning Log  Exclusion: the 91 delivered orders with no delivery date are left out of the on-time % calculation.

#### Order_Items table: 6 issues cleaned before analysis

| # | Problem | Fix |
|---|---------|-----|
| **1** | 118 duplicate lines | Remove duplicates on `Order_Item_ID` |
| **2** | `Returned` appears in multiple formats: `Yes`, `Y`, `True`, `yes`, `No`, `N`, `False`, `no`, plus 442 blanks | Standardise to **Yes/No**; treat blanks as **No** and document this assumption |
| **3** | `Return_Reason` contains `-`, case variants, and `Size/Specification Issue`; 48 returned lines have no reason | Replace `-` with null; standardise values to the data dictionary; replace missing reasons with **Not Recorded**; determine how to handle **Changed Mind**, which exists in the data but not in the dictionary |
| **4** | `Discount` is mostly between 0–0.60, but ~120 rows contain whole numbers such as `5`, `10`, and `50` | If the value is greater than `1`, divide it by `100` |
| **5** | `Quantity` contains negative and zero values (28 lines) and extreme values up to 897 | Flag and exclude invalid or extreme quantities from the analysis and document the exclusion |
| **6** | 55 `Unit_Price` values are missing | Fill missing values using the product's `List_Price`, which is typically within 1–2% of `Unit_Price` |  

Data Cleaning Log Assumption Logged: blank Returned=No; 28 bad-quantity lines flagged and excluded.

#### Customers: 4 issues cleaned before analysis

| # | Problem | Fix |
|---|---------|-----|
| **1** | `Segment` contains inconsistent variants such as `Repeat Customer` and `NEW CUSTOMER` | Standardise to **New / Returning / Premium** |
| **2** | `Region` contains typos and inconsistent formatting such as `Wset`, `Sotuh`, and `NORTH` | Standardise to **5 regions** |
| **3** | `Age_Group` contains `-`, `Unknown`, and 84 blank values | Combine `-`, `Unknown`, and blanks into **Unknown** |
| **4** | 15 duplicate customer IDs, including one with conflicting details | Remove duplicate IDs and manually inspect the conflicting record |

Data Cleaning Log Manual check: the one duplicate ID with conflicting details is reviewed by hand before deduping.

#### Products: 4 issues cleaned before analysis
| # | Problem | Fix |
|---|---------|-----|
| **1** | `Category` contains inconsistent casing, such as `fashion` | Standardise category casing |
| **2** | 5 duplicate `SKU` values | Remove duplicate SKUs |
| **3** | 6 missing `Unit_Cost` values | Flag records with missing `Unit_Cost` for review |
| **4** | 3 products have `Unit_Cost` greater than or equal to their selling price | Flag these products for review |

Data Cleaning Log Flagged, not deleted: the 6 missing Unit_Cost and 3 cost >= price products are marked for review   

## Data Model Overview  
The Power BI data model was built using two fact tables and three dimension tables

Fact Tables: Orders, Order_Items  
Dimension Tables: Customers, Products, Date

<img width="853" height="399" alt="image" src="https://github.com/user-attachments/assets/a5fa6ab1-e4dd-482d-b975-476675854d4d" />  

All data was transformed using Power Query, and core business calculations were implemented in DAX.  

### Analytical Approach
## Executive Summary  
Healthy revenue, but profit is leaking in three places

<img width="659" height="269" alt="image" src="https://github.com/user-attachments/assets/91025e79-52e2-4227-8c4d-35a3408088f4" />

Problems Identified  
 Which categories and subcategories generate the most revenue? The most profit?
<img width="653" height="318" alt="image" src="https://github.com/user-attachments/assets/a6a0bc8c-9386-4cb5-9652-5286b794bff8" />







