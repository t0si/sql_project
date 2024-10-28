What are your risk areas? Identify and describe them.

 * Data Loss from Column Deletion: By deleting columns deemed unnecessary, there's a risk of losing potentially valuable information that could be useful for further analysis or answering different questions in the future. This also limits the flexibility to explore alternative data perspectives.

 * Inconsistent Data Filtering: Filters applied to exclude missing or irrelevant values (e.g., WHERE product_sku IS NOT NULL) might unintentionally exclude data that could provide useful context. Over-filtering could lead to gaps in the analysis or a misrepresentation of the data.

 * Inadequate Data Transformation: Due to time constraints or prioritization challenges, some conversions or transformations might be incomplete or inconsistently applied across tables. This could result in mismatches or inaccuracies during data merging and aggregation.

 * Syntax Errors and Debugging Challenges: Frequent syntax errors can consume time and lead to delays in completing the analysis. Without careful debugging, these errors could impact the accuracy and completeness of the results.

 * Limited Version Control: Not using version control for the data and scripts could make it challenging to recover specific changes, especially when columns or data are removed. This could complicate the process of revisiting or modifying the analysis later.



QA Process:
Describe your QA process and include the SQL queries used to execute it.

 * This query helps identify inconsistencies in city names.

SELECT LOWER(city) AS city_normalized,
       COUNT(*)    AS quantity
FROM all_sessions
GROUP BY LOWER(city)
ORDER BY quantity DESC;
    
 * This query identifies negative revenue values, which could be due to returns or incorrect data entries in the table.

    
SELECT COUNT(*) AS negative_revenue_count
FROM all_sessions
WHERE total_transactions_revenue < 0
   OR total_transactions_revenue IS NULL; 
    
    
 * Checking for outliers in total transaction revenue by city reveals that "not available in the dataset" appears frequently. This limits our ability to fully assess revenue by city. While we can get an idea of revenue distribution based on available data, the missing information prevents a complete analysis.
 
SELECT product_sku,
       city,
       total_transactions_revenue
FROM all_sessions
WHERE total_transactions_revenue < 0
   OR total_transactions_revenue > 100000
ORDER BY total_transactions_revenue 
 
SELECT COUNT(*)                                                    AS negative_revenue_count,
       COUNT(*) FILTER (WHERE total_transactions_revenue > 100000) AS high_revenue_count
FROM all_sessions
WHERE total_transactions_revenue < 0
   OR total_transactions_revenue IS NULL; 
 

    
