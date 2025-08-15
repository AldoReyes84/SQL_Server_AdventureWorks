Go back to [Aldo Reyes Portfolio](https://aldoreyes84.github.io/AldoReyes.github.io/)

Go back to [Data Analysis Project](https://github.com/AldoReyes84/Data-Analisys_For-AdventureWorksDW2022_SQL_PowerBI_Python_Excel/tree/main)

# SQL_Server_Data_Analysis

This analysis utilizes the AdventureWorks2022 sample database, installed in SQL Server Management Studio, as the primary data source for a broader business intelligence initiative.

### Focus: Reseller Sales Table

!Reseller Sales Table  

<img width="1897" height="918" alt="image" src="https://github.com/user-attachments/assets/c813db2c-9cfe-4fec-a918-48400a309367" />

Key metrics analyzed include:

- Quantity  
- Product Cost  
- Total Discount  
- Sales Amount  
- Gross Margin  
- Margin Percentage  

Data is aggregated by **month** and **year** to identify performance trends.


<img width="617" height="463" alt="image" src="https://github.com/user-attachments/assets/0adfd4c0-c34a-4bcf-97a6-64f9a1a80b07" />


SELECT 
    ISNULL(CAST(YEAR([DueDate]) AS VARCHAR), 'Total General') AS [Year],
    CASE 
        WHEN GROUPING(MONTH([DueDate])) = 1 THEN 'Subtotal Año'
        ELSE CAST(MONTH([DueDate]) AS VARCHAR)
    END AS [Month],
        
    FORMAT(SUM([OrderQuantity]), 'N0', 'es-MX') AS [TotalQuantity],
    FORMAT(SUM([TotalProductCost]), 'C', 'es-MX') AS [TotalCost],
    FORMAT(SUM([DiscountAmount]), 'C', 'es-MX') AS [TotalDiscount],
    FORMAT(SUM([SalesAmount]), 'C', 'es-MX') AS [TotalSales],

    FORMAT(SUM([SalesAmount]) - SUM([TotalProductCost]), 'C', 'es-MX') AS [GrossMargin],
    
    FORMAT(
        CASE 
            WHEN SUM([SalesAmount]) = 0 THEN 0
            ELSE (SUM([SalesAmount]) - SUM([TotalProductCost])) * 100.0 / SUM([SalesAmount])
        END, 'N2'
    ) + '%' AS [MarginPercentage]

FROM [AdventureWorksDW2022].[dbo].[FactResellerSales]

GROUP BY GROUPING SETS (
    (YEAR([DueDate]), MONTH([DueDate])),  -- Detalle por mes
    (YEAR([DueDate])),                    -- Subtotal por año
    ()                                    -- Total general
)

ORDER BY 
    GROUPING(YEAR([DueDate])), 
    YEAR([DueDate]), 
    GROUPING(MONTH([DueDate])), 
    MONTH([DueDate]);


<img width="702" height="815" alt="image" src="https://github.com/user-attachments/assets/67f7927f-ed9c-4c80-9660-9ec214d78c31" />

The interpretation of these results are addresserd in the [Data Analysis Project/FactResellersSales_Table](https://github.com/AldoReyes84/Data-Analisys_For-AdventureWorksDW2022_SQL_PowerBI_Python_Excel/tree/main#factresellerssales-table) 

