# DAX Measures (Core)

-- Revenue & Returns
Total Revenue = 
SUMX (
    'Transaction_Data',
    'Transaction_Data'[quantity] * RELATED ( Products[product_retail_price] )
)

Total Returns = 
COUNTROWS('Return_Data')

Return Rate = 
DIVIDE ( [Quantity Returned], [Quantity Sold], 0 )


-- Profit & Volume
Total Profit = 
[Total Revenue] - [Total Cost]

Total Transactions = 
COUNTROWS( 'Transaction_Data' )

-- Customers and Recency
Customer Count = 
DISTINCTCOUNT ( 'Transaction_Data'[customer_id] )

Last Purchase Date = 
CALCULATE (
    MAX ( 'Transaction_Data'[transaction_date] ),
    ALLEXCEPT ( 'Transaction_Data', 'Transaction_Data'[customer_id] )
)

-- Active customers (last 90 days)
Active Customers (90d) = 
VAR Anchor =
    MAXX ( ALL ( 'Transaction_Data'[transaction_date] ), 'Transaction_Data'[transaction_date] )
VAR Val =
    CALCULATE (
        DISTINCTCOUNT ( 'Transaction_Data'[customer_id] ),
        FILTER (
            VALUES ( 'Transaction_Data'[customer_id] ),
            DATEDIFF ( [Last Purchase Date], Anchor, DAY ) <= 90
        )
    )
RETURN
COALESCE ( Val, 0 )


-- Date intelligence
Avg Revenue / Customer = 
DIVIDE ( [Total Revenue], [Customer Count] )

YTD Revenue = 
TOTALYTD(
    [Total Revenue],
    'Calendar'[Date]
)


-- Notes
-- Report also includes additional measures (e.g., Return Rate (LM), Recency (days), Repeat Rate),
-- which are documented on the Notes page and within the PBIX.
