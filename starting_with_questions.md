Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

SELECT country, SUM(total_transactions_revenue) AS total_transactions_revenue
FROM all_sessions
where total_transactions_revenue IS NOT NULL
GROUP BY country
ORDER BY total_transactions_revenue DESC;

SELECT city, SUM(total_transactions_revenue) AS total_transactions_revenue
FROM all_sessions
WHERE total_transactions_revenue IS NOT NULL
  AND city != 'not available in demo dataset'
GROUP BY city
ORDER BY total_transactions_revenue DESC
LIMIT 5;



Answer: 
San Francisco,4692960000
Sunnyvale,2976690000
Atlanta,2563320000
Palo Alto,1824000000
Tel Aviv-Yafo,1806000000



United States,39462510000
Israel,1806000000
Australia,1074000000
Canada,450450000
Switzerland,50970000




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries: 

SELECT AVG(product_quantity) AS avg_products_ordered_per,
       city,
       country

FROM all_sessions
WHERE product_quantity IS NOT NULL
  AND city != 'not available in demo dataset'
GROUP BY city, country
ORDER BY avg_products_ordered_per DESC;



Answer:

10,Madrid,Spain
8,Salem,United States
4,Atlanta,United States
2,Houston,United States
1.1666666666666667,New York,United States
1,Detroit,United States
1,Dublin,Ireland
1,Los Angeles,United States
1,Mountain View,United States
1,(not set),United States
1,Palo Alto,United States
1,San Francisco,United States
1,San Jose,United States
1,Seattle,United States
1,Ann Arbor,United States
1,Sunnyvale,United States
1,Bengaluru,India
1,Chicago,United States
1,Columbus,United States
1,Dallas,United States






**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

SELECT country,
       city,
       v2_product_category,
       COUNT(*) AS total_orders
FROM all_sessions
WHERE v2_product_category IS NOT NULL
  AND city != 'not available in demo dataset'

GROUP BY city, country, v2_product_category
ORDER BY total_orders DESC, country, city;



Answer: Yes, the city with most number of larger orders is Mountain View in the United States.




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

SELECT v2_product_category,
       city,
       country,
       product_sku,
       total_revenue
FROM (SELECT v2_product_category,
             city,
             country,
             product_sku,
             SUM(total_transactions_revenue)                                                        AS total_revenue,
             RANK() OVER (PARTITION BY city, country ORDER BY SUM(total_transactions_revenue) DESC) AS sales_rank
      FROM all_sessions
      WHERE product_sku IS NOT NULL
        AND city != 'not available in demo dataset'
        AND v2_product_category != '(not set)'
      GROUP BY city, country, product_sku, v2_product_category
      HAVING SUM(total_transactions_revenue) > 0) AS ranked_sales
WHERE sales_rank = 1
ORDER BY total_revenue DESC;



Answer: From the results we can see that Nest is a big revenue generator. 





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

SELECT city,
       country,
       COUNT(country)                                                                           AS transaction_count,
       SUM(total_transactions_revenue)                                                          AS total_revenue,
       (SUM(total_transactions_revenue) / (SELECT SUM(total_transactions_revenue)
                                           FROM all_sessions
                                           WHERE city IS NOT NULL
                                             AND country IS NOT NULL
                                             AND city != 'not available in demo dataset'
                                             AND total_transactions_revenue IS NOT NULL) * 100) AS revenue_percentage
FROM all_sessions
WHERE city IS NOT NULL
  AND country IS NOT NULL
  AND city != 'not available in demo dataset'
  AND total_transactions_revenue IS NOT NULL
GROUP BY city, country
ORDER BY total_revenue DESC;




Answer: Most of the cities that generate most revenue, are in the United States.







