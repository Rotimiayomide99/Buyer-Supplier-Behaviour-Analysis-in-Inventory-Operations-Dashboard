Buyer & Supplier Behaviour Analysis in Inventory Operations

This project analyzes procurement transaction data to evaluate organizational spending patterns, supplier concentration risk, buyer purchasing behavior, and category-level cost drivers.

The dataset contains 500 procurement transactions across six categories: Electronics, Software, Furniture, Accessories, Office Supplies, and Stationery.

The objective of this project was to transform raw transactional data into actionable insights that support cost optimization, supplier risk mitigation, and improved procurement governance.

Tools Used:

SQL Server (Data exploration and KPI calculation)

Power BI (Data modeling, DAX, and dashboard visualization)

📁 Dataset Structure

Table: dbo.spend_analysis

Columns:

TransactionID

ItemName

Category

Quantity

UnitPrice

TotalCost

PurchaseDate

Supplier

Buyer

📌 Business Objectives

Identify major spending categories

Measure supplier concentration and dependency risk

Evaluate buyer purchasing patterns

Track monthly spending trends

Detect potential cost optimization opportunities

🧮 SQL Analysis & KPI Queries
1️⃣ Total Spend
SELECT CAST(SUM(TotalCost) AS DECIMAL(10,2)) AS TotalSpend 
FROM dbo.spend_analysis;

2️⃣ Average Transaction Value
SELECT CAST(SUM(TotalCost)/COUNT(TransactionID) AS DECIMAL(10,2)) 
AS average_transaction_value 
FROM dbo.spend_analysis;

3️⃣ Average Units Purchased per Category
SELECT Category, AVG(Quantity) AS avg_unit_per_category 
FROM dbo.spend_analysis 
GROUP BY Category;

4️⃣ Spend per Category (%)
SELECT Category, 
       CAST(SUM(TotalCost) AS DECIMAL(10,2)) AS spend_per_category, 
       CAST(SUM(TotalCost) * 100.0 / 
            (SELECT SUM(TotalCost) FROM dbo.spend_analysis) 
            AS DECIMAL(10,2)) 
       AS spend_per_category_percentage
FROM dbo.spend_analysis 
GROUP BY Category;

5️⃣ Spend per Supplier (%)
SELECT Supplier, 
       SUM(TotalCost) AS spend_per_supplier, 
       CAST(SUM(TotalCost) * 100.0 / 
            (SELECT SUM(TotalCost) FROM dbo.spend_analysis) 
            AS DECIMAL(10,2)) 
       AS spend_per_supplier_percentage
FROM dbo.spend_analysis 
GROUP BY Supplier;

6️⃣ Total Quantity Purchased
SELECT SUM(Quantity) AS total_quantities_purchased 
FROM dbo.spend_analysis;

7️⃣ Number of Transactions per Buyer
SELECT Buyer, COUNT(TransactionID) AS number_of_transactions 
FROM dbo.spend_analysis 
GROUP BY Buyer;

8️⃣ Cost per Transaction per Buyer
SELECT Buyer, 
       CAST(SUM(TotalCost) AS DECIMAL(10,2)) AS total_cost_per_buyer, 
       CAST(SUM(TotalCost) / COUNT(TransactionID) AS DECIMAL(10,2)) 
       AS cost_per_transaction_per_buyer
FROM dbo.spend_analysis 
GROUP BY Buyer;

9️⃣ Supplier Concentration Ratio (Top 3 Suppliers)
WITH Top3 AS (
    SELECT 
        Supplier,
        SUM(TotalCost) AS TotalSpend
    FROM dbo.spend_analysis
    GROUP BY Supplier
    ORDER BY TotalSpend DESC
    OFFSET 0 ROWS FETCH NEXT 3 ROWS ONLY
),
AllSpend AS (
    SELECT SUM(TotalCost) AS GrandTotal 
    FROM dbo.spend_analysis
)
SELECT 
    t.Supplier,
    FORMAT(t.TotalSpend, 'N2') AS TotalSpend,
    FORMAT((t.TotalSpend * 1.0 / a.GrandTotal), 'P2') 
    AS PctOfTotalSpend
FROM Top3 t
CROSS JOIN AllSpend a;

🔟 Monthly Spend Trend
SELECT 
    YEAR(PurchaseDate) AS Year,
    MONTH(PurchaseDate) AS Month,
    CAST(SUM(TotalCost) AS DECIMAL(10,2)) AS MonthlySpend
FROM dbo.spend_analysis
GROUP BY YEAR(PurchaseDate), MONTH(PurchaseDate)
ORDER BY YEAR(PurchaseDate), MONTH(PurchaseDate);

📊 Key Insights

Electronics accounts for 56.25% of total spend

Software accounts for 27.09%

Combined, technology-related purchases represent over 83% of procurement expenditure

The top 3 suppliers account for approximately 67.52% of total spend, indicating high supplier concentration risk

Significant variation exists in buyer transaction values, suggesting potential need for purchasing standardization

Monthly spending shows peaks in March, September, and October, indicating possible seasonality or budget release cycles

💡 Recommendations

Diversify supplier base in high-spend categories

Renegotiate contracts in Electronics and Software segments

Introduce procurement approval thresholds for high-value transactions

Implement quarterly supplier performance reviews

Develop forecasting models to smooth monthly spending fluctuations

📈 Dashboard Deliverables

The Power BI dashboard includes:

Executive KPI Overview

Category Spend Distribution

Supplier Pareto Analysis

Buyer Purchasing Behavior Analysis

Monthly Spend Trend Visualization

🎯 Business Impact

This project demonstrates how structured spend analysis can:

Reduce procurement risk

Improve supplier negotiation power

Enhance cost visibility

Support data-driven procurement strategy
