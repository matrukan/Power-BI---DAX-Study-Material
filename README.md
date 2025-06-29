# Power-BI
## DAX-Study-Material

This Material is a chapter wise study material of DAX formulas ,with explainations along with the use cases

#### Kaggle Dataset Introduction
For our examples, we'll use the "Superstore Sales Dataset" from Kaggle, which contains sales data for a fictional retail store with information about orders, products, customers, and regions.
Dataset link: [Superstore Sales Dataset](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)

###  Data Model Overview
The dataset contains tables:
- Orders (fact table with sales transactions)
- People (sales representatives)
- Returns (returned orders)
# Comprehensive Power BI DAX Study Material with Kaggle Dataset Examples

## Chapter 1: Introduction to DAX and Data Modeling

### 1.1 What is DAX?
DAX (Data Analysis Expressions) is a formula language used in Power BI, Excel Power Pivot, and SQL Server Analysis Services for data modeling and analysis. It combines functions, operators, and values to perform advanced calculations and queries on your data.

## Chapter 2: Basic DAX Functions

### 2.1 Aggregation Functions

#### SUM()
**Description**: Adds all the numbers in a column.

**Example**:
```dax
Total Sales = SUM(Orders[Sales])
```

**Use Case**: Calculate total revenue from all sales.

**Additional Examples**:
1. `Total Quantity = SUM(Orders[Quantity])`
2. `Total Profit = SUM(Orders[Profit])`
3. `Total Discount = SUM(Orders[Discount])`
4. `Sum of Shipping Cost = SUM(Orders[Shipping Cost])`
5. `Total Product Cost = SUM(Orders[Sales]) - SUM(Orders[Profit])`

#### AVERAGE()
**Description**: Calculates the arithmetic mean of values in a column.

**Example**:
```dax
Avg Sale Amount = AVERAGE(Orders[Sales])
```

**Use Case**: Find the average transaction value.

**Additional Examples**:
1. `Avg Discount = AVERAGE(Orders[Discount])`
2. `Avg Profit per Order = AVERAGE(Orders[Profit])`
3. `Avg Shipping Cost = AVERAGE(Orders[Shipping Cost])`
4. `Avg Quantity per Order = AVERAGE(Orders[Quantity])`
5. `Avg Days to Ship = AVERAGE(Orders[Ship Date] - Orders[Order Date])`

#### MIN() and MAX()
**Description**: Returns the smallest/largest value in a column.

**Examples**:
```dax
Largest Order = MAX(Orders[Sales])
Smallest Order = MIN(Orders[Sales])
```

**Use Case**: Identify outlier transactions.

**Additional Examples**:
1. `Highest Discount = MAX(Orders[Discount])`
2. `Lowest Profit = MIN(Orders[Profit])`
3. `Earliest Order Date = MIN(Orders[Order Date])`
4. `Latest Ship Date = MAX(Orders[Ship Date])`
5. `Most Items in Order = MAX(Orders[Quantity])`

### 2.2 Counting Functions

#### COUNT()
**Description**: Counts the number of cells in a column that contain numbers.

**Example**:
```dax
Number of Orders = COUNT(Orders[Order ID])
```

**Use Case**: Count how many transactions occurred.

**Additional Examples**:
1. `Count of Products = COUNT(Orders[Product ID])`
2. `Count of Customers = COUNT(Orders[Customer ID])`
3. `Count of Returned Items = COUNT(Returns[Order ID])`
4. `Count of Sales People = COUNT(People[Person])`
5. `Count of Ship Modes = COUNT(Orders[Ship Mode])`

#### DISTINCTCOUNT()
**Description**: Counts the number of distinct values in a column.

**Example**:
```dax
Unique Customers = DISTINCTCOUNT(Orders[Customer ID])
```

**Use Case**: Measure customer reach.

**Additional Examples**:
1. `Unique Products Sold = DISTINCTCOUNT(Orders[Product ID])`
2. `Unique Categories = DISTINCTCOUNT(Orders[Category])`
3. `Unique Regions = DISTINCTCOUNT(Orders[Region])`
4. `Unique Cities = DISTINCTCOUNT(Orders[City])`
5. `Unique Order Dates = DISTINCTCOUNT(Orders[Order Date])`

## Chapter 3: Filter Context and CALCULATE Function

### 3.1 Understanding Filter Context
Filter context is the set of filters applied to a calculation, coming from:
- Visual filters
- Slicers
- Row/column headers in matrices
- FILTER functions

### 3.2 CALCULATE Function

#### CALCULATE()
**Description**: Modifies filter context for an expression.

**Example**:
```dax
West Region Sales = CALCULATE(SUM(Orders[Sales]), Orders[Region] = "West")
```

**Use Case**: Create region-specific measures.

**Additional Examples**:
1. `Furniture Sales = CALCULATE(SUM(Orders[Sales]), Orders[Category] = "Furniture")`
2. `High Value Orders = CALCULATE(COUNTROWS(Orders), Orders[Sales] > 1000)`
3. `Q1 Sales = CALCULATE(SUM(Orders[Sales]), DATESBETWEEN(Orders[Order Date], DATE(2023,1,1), DATE(2023,3,31)))`
4. `Corporate Segment Sales = CALCULATE(SUM(Orders[Sales]), Orders[Segment] = "Corporate")`
5. `Same Day Shipping = CALCULATE(COUNTROWS(Orders), Orders[Ship Date] = Orders[Order Date])`

### 3.3 Filter Modifiers

#### ALL()
**Description**: Removes filters from a table or column.

**Example**:
```dax
% of Total Sales = DIVIDE(SUM(Orders[Sales]), CALCULATE(SUM(Orders[Sales]), ALL(Orders)))
```

**Use Case**: Calculate percentage of total.

**Additional Examples**:
1. `% of Category Sales = DIVIDE(SUM(Orders[Sales]), CALCULATE(SUM(Orders[Sales]), ALL(Orders[Category])))`
2. `Rank All Products = RANKX(ALL(Orders[Product Name]), SUM(Orders[Sales]))`
3. `Avg Sales All Regions = CALCULATE(AVERAGE(Orders[Sales]), ALL(Orders[Region]))`
4. `% Difference from Avg = DIVIDE(SUM(Orders[Sales]), [Avg Sales All Regions]) - 1`
5. `Total Customers All Time = CALCULATE(DISTINCTCOUNT(Orders[Customer ID]), ALL(Orders[Order Date]))`

## Chapter 4: Time Intelligence Functions

### 4.1 Basic Time Intelligence

#### TOTALYTD()
**Description**: Calculates year-to-date total.

**Example**:
```dax
Sales YTD = TOTALYTD(SUM(Orders[Sales]), Orders[Order Date])
```

**Use Case**: Track annual running total.

**Additional Examples**:
1. `Profit YTD = TOTALYTD(SUM(Orders[Profit]), Orders[Order Date])`
2. `Quantity YTD = TOTALYTD(SUM(Orders[Quantity]), Orders[Order Date])`
3. `YTD Growth = DIVIDE([Sales YTD], TOTALYTD(SUM(Orders[Sales]), SAMEPERIODLASTYEAR(Orders[Order Date]))) - 1`
4. `YTD Avg Order Value = DIVIDE([Sales YTD], TOTALYTD(COUNTROWS(Orders), Orders[Order Date]))`
5. `YTD Customer Count = TOTALYTD(DISTINCTCOUNT(Orders[Customer ID]), Orders[Order Date])`

#### SAMEPERIODLASTYEAR()
**Description**: Returns dates from prior year.

**Example**:
```dax
PY Sales = CALCULATE(SUM(Orders[Sales]), SAMEPERIODLASTYEAR(Orders[Order Date]))
```

**Use Case**: Year-over-year comparison.

**Additional Examples**:
1. `PY Profit = CALCULATE(SUM(Orders[Profit]), SAMEPERIODLASTYEAR(Orders[Order Date]))`
2. `YoY Growth = DIVIDE(SUM(Orders[Sales]), [PY Sales]) - 1`
3. `PY Customers = CALCULATE(DISTINCTCOUNT(Orders[Customer ID]), SAMEPERIODLASTYEAR(Orders[Order Date]))`
4. `PY Avg Discount = CALCULATE(AVERAGE(Orders[Discount]), SAMEPERIODLASTYEAR(Orders[Order Date]))`
5. `PY Quantity = CALCULATE(SUM(Orders[Quantity]), SAMEPERIODLASTYEAR(Orders[Order Date]))`

### 4.2 Advanced Time Intelligence

#### DATEADD()
**Description**: Shifts dates by specified interval.

**Example**:
```dax
Sales Last Quarter = CALCULATE(SUM(Orders[Sales]), DATEADD(Orders[Order Date], -1, QUARTER))
```

**Use Case**: Quarter-over-quarter comparison.

**Additional Examples**:
1. `Sales Next Month = CALCULATE(SUM(Orders[Sales]), DATEADD(Orders[Order Date], 1, MONTH))`
2. `3 Month Moving Avg = AVERAGEX(DATEADD(Orders[Order Date], -2, MONTH), SUM(Orders[Sales]))`
3. `6 Month Growth = DIVIDE(SUM(Orders[Sales]), CALCULATE(SUM(Orders[Sales]), DATEADD(Orders[Order Date], -6, MONTH))) - 1`
4. `Weekly Comparison = CALCULATE(SUM(Orders[Sales]), DATEADD(Orders[Order Date], -7, DAY))`
5. `Bi-Annual Comparison = CALCULATE(SUM(Orders[Sales]), DATEADD(Orders[Order Date], -6, MONTH))`

## Chapter 5: Advanced DAX Patterns

### 5.1 Iterator Functions

#### SUMX()
**Description**: Sums an expression evaluated for each row.

**Example**:
```dax
Total Profit Margin = SUMX(Orders, Orders[Sales] * (1 - Orders[Discount]) - Orders[Product Cost])
```

**Use Case**: Row-by-row calculation.

**Additional Examples**:
1. `Weighted Avg Discount = SUMX(Orders, Orders[Discount] * Orders[Sales]) / SUM(Orders[Sales])`
2. `Total Shipping per Unit = SUMX(Orders, Orders[Shipping Cost] / Orders[Quantity])`
3. `Net Sales = SUMX(Orders, Orders[Sales] * (1 - Orders[Discount]))`
4. `Profit per Unit = SUMX(Orders, Orders[Profit] / Orders[Quantity])`
5. `Days to Ship = SUMX(Orders, Orders[Ship Date] - Orders[Order Date]) / COUNTROWS(Orders)`

#### RANKX()
**Description**: Ranks values in a column.

**Example**:
```dax
Product Rank = RANKX(ALL(Orders[Product Name]), SUM(Orders[Sales]))
```

**Use Case**: Identify top-performing products.

**Additional Examples**:
1. `Customer Rank = RANKX(ALL(Orders[Customer ID]), SUM(Orders[Sales]))`
2. `Region Rank = RANKX(ALL(Orders[Region]), SUM(Orders[Profit]))`
3. `Category Rank by Quantity = RANKX(ALL(Orders[Category]), SUM(Orders[Quantity]))`
4. `City Rank by Profit Margin = RANKX(ALL(Orders[City]), SUM(Orders[Profit])/SUM(Orders[Sales]))`
5. `Sales Person Rank = RANKX(ALL(People[Person]), CALCULATE(SUM(Orders[Sales]))))`

### 5.2 Advanced Filtering

#### FILTER()
**Description**: Returns a filtered table.

**Example**:
```dax
High Value Customers = CALCULATE(DISTINCTCOUNT(Orders[Customer ID]), 
    FILTER(Orders, Orders[Sales] > 1000))
```

**Use Case**: Segment analysis.

**Additional Examples**:
1. `Low Profit Products = CALCULATE(COUNTROWS(Orders), FILTER(Orders, Orders[Profit] < 0))`
2. `Fast Shipping Orders = CALCULATE(COUNTROWS(Orders), FILTER(Orders, Orders[Ship Date] - Orders[Order Date] <= 2))`
3. `Corporate Segment in West = CALCULATE(SUM(Orders[Sales]), FILTER(Orders, Orders[Segment] = "Corporate" && Orders[Region] = "West"))`
4. `Repeat Customers = CALCULATE(DISTINCTCOUNT(Orders[Customer ID]), FILTER(Orders, COUNTROWS(FILTER(Orders, EARLIER(Orders[Customer ID]) = Orders[Customer ID])) > 1))`
5. `High Quantity Low Profit = CALCULATE(SUM(Orders[Quantity]), FILTER(Orders, Orders[Quantity] > 5 && Orders[Profit] < 10))`

## Chapter 6: DAX Best Practices and Performance

### 6.1 Optimizing DAX
- Use variables (VAR) for complex calculations
- Avoid unnecessary calculations in iterators
- Use CALCULATE efficiently
- Prefer measures over calculated columns

### 6.2 Variables in DAX

#### VAR/RETURN
**Description**: Stores intermediate results.

**Example**:
```dax
Profit Margin = 
VAR TotalSales = SUM(Orders[Sales])
VAR TotalProfit = SUM(Orders[Profit])
RETURN DIVIDE(TotalProfit, TotalSales)
```

**Use Case**: Improve readability and performance.

**Additional Examples**:
1. ```
   Discount Impact = 
   VAR SalesWithDiscount = SUMX(Orders, Orders[Sales] * (1 - Orders[Discount]))
   VAR SalesWithoutDiscount = SUM(Orders[Sales])
   RETURN SalesWithoutDiscount - SalesWithDiscount
   ```
2. ```
   Customer Value = 
   VAR CustomerSales = SUM(Orders[Sales])
   VAR CustomerOrders = COUNTROWS(Orders)
   RETURN DIVIDE(CustomerSales, CustomerOrders)
   ```
3. ```
   Shipping Efficiency = 
   VAR AvgShippingTime = AVERAGEX(Orders, Orders[Ship Date] - Orders[Order Date])
   VAR IdealShippingTime = 3
   RETURN AvgShippingTime - IdealShippingTime
   ```
4. ```
   Regional Performance = 
   VAR RegionSales = SUM(Orders[Sales])
   VAR TotalSales = CALCULATE(SUM(Orders[Sales]), ALL(Orders[Region]))
   RETURN DIVIDE(RegionSales, TotalSales)
   ```
5. ```
   Product Mix = 
   VAR CategorySales = SUM(Orders[Sales])
   VAR FurnitureSales = CALCULATE(SUM(Orders[Sales]), Orders[Category] = "Furniture")
   RETURN DIVIDE(FurnitureSales, CategorySales)
   ```

## Chapter 7: Practical Applications and Case Studies

### 7.1 Building Key Metrics

#### Customer Lifetime Value
```dax
CLV = 
VAR AvgPurchaseValue = DIVIDE(SUM(Orders[Sales]), DISTINCTCOUNT(Orders[Customer ID]))
VAR AvgPurchaseFreq = DIVIDE(COUNTROWS(Orders), DISTINCTCOUNT(Orders[Customer ID]))
VAR CustomerLifespan = 365 // 1 year assumption
RETURN AvgPurchaseValue * AvgPurchaseFreq * CustomerLifespan
```

#### Inventory Turnover
```dax
Inventory Turnover = 
VAR COGS = SUM(Orders[Sales]) - SUM(Orders[Profit])
VAR AvgInventory = 50000 // Placeholder for actual inventory data
RETURN DIVIDE(COGS, AvgInventory)
```

### 7.2 Advanced Analytics

#### Cohort Analysis
```dax
Cohort Retention = 
VAR SelectedCohort = MIN(Orders[Order Date])
VAR CohortCustomers = CALCULATE(DISTINCTCOUNT(Orders[Customer ID]), 
    FILTER(ALL(Orders), Orders[Order Date] = SelectedCohort))
VAR ReturningCustomers = CALCULATE(DISTINCTCOUNT(Orders[Customer ID]), 
    FILTER(ALL(Orders), 
        Orders[Order Date] > SelectedCohort && 
        Orders[Order Date] <= SelectedCohort + 30 &&
        Orders[Customer ID] IN VALUES(Orders[Customer ID])))
RETURN DIVIDE(ReturningCustomers, CohortCustomers)
```

This comprehensive guide covers foundational to advanced DAX concepts with practical examples using the Superstore dataset. Each concept includes multiple examples to reinforce learning and demonstrate real-world applications.
