# Power BI Project - Sales and Departures Analysis

## Overview
This project is a Power BI dashboard that provides an analysis of transactions and departures over time, segmented by different dimensions. The goal was to create a dynamic representation of sales data,  to understand transaction trends, departure timings, and filter the insights based on multiple criteria such as country, product, and date.

## Data Model
In this project, we used data from multiple sources, including sales, products, and user information. Key steps in building the data model included:

1. **Data Import and Transformation**:
   - Imported datasets are: `Products.xlsx`, `Sales_1.csv`, `Sales_2.csv`, and `Users.csv`.
   - Combined `Sales_1` and `Sales_2` into a unified dataset called **CombinedSales** to simplify analysis.
   - UNIX timestamps were converted into `Date/Time` format.

2. **Date Table**:
   - Created a **DateTable** using DAX to ensure a continuous date range from the earliest transaction to the latest departure.
     
3. **Relationships**:
   - Established relationships between **CombinedSales** and **Products** via `ProductID`, and between **CombinedSales** and **Users** via `User`. Relationships were set as **one-to-many** with cross-filter direction as **both** to allow seamless filtering across tables.
   - Linked the **CombinedSales** table to the **DateTable** twice using `TransactionDateConverted` and `DepartureDateConverted`. One relationship was active, and the other was activated using the `USERELATIONSHIP` function where necessary.

## DAX Measures
To support detailed analysis and dynamic visualization, several DAX measures were implemented:

1. **Transaction Count**:
   ```DAX
   TransactionCount = COUNTROWS(CombinedSales)
```
```
2. **Departure Count**:
   ```DAX
   DepartureCount = CALCULATE(
       COUNTROWS(CombinedSales), 
       USERELATIONSHIP(CombinedSales[DepartureDateConverted], DateTable[Date])
   )
   ```
   - This measure provides a count of departures using the **DepartureDateConverted** field, using also the `USERELATIONSHIP` to work with the inactive relationship.
3. **Revenue**:
   ```DAX
   Revenue = SUMX(
       Sales, 
       Sales[Units] * Sales[Price] * (1 - Sales[Discount])
   )
   ```
   - This measure calculates the total revenue by multiplying the number of units sold by the price per unit and applying the discount.

4. **Revenue % of Category**:
   ```DAX
   Revenue % of Category = 
   VAR CategoryRevenue = 
       CALCULATE(
           [Revenue], 
           REMOVEFILTERS(Products[Product])
       )
   RETURN 
       DIVIDE([Revenue], CategoryRevenue, 0)
   ```
   - This measure calculates the revenue as a percentage of the category revenue. It first calculates the total category revenue without filtering for individual products, and then returns the percentage of individual revenue to the category revenue.


## Visual Design
- **Filters**: Three key filters were integrated into the dashboard: **Country**, **Product**, and **Date**, allowing users to refine their insights dynamically.
- **Visual Styling**:
  - The dashboard utilizes **Ryanair colors** (blue and yellow) to create a visually impactful interface, which helps provide a professional and consistent look.
  - **Arial font** was chosen for its clarity and readability, ensuring the information presented is easily accessible to all users.


## Tooltip and Drill pages
- This two hidden pages are created for the point "Country Interaction: When you hover over a country, it should display revenue by last name. Right-clicking should allow you to drill down to a details page showing email, last name, revenue, and transaction date."



Feel free to explore the dashboard !

   
