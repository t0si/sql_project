What issues will you address by cleaning the data?

By cleaning the data, I aimed to filter out missing or null values, remove duplicates to prevent redundancy, standardize column and table names, and check for any inconsistent dates. I also planned to convert certain values to ensure consistency across all tables.





Queries:
Below, provide the SQL queries you used to clean your data.

* In pgAdmin, I deleted unnecessary columns (possibly more than needed) using the GUI and used specific conditions in my queries to exclude missing values. For instance, I applied filters like:

sql

WHERE product_sku IS NOT NULL
  AND city != 'not available in demo dataset'
  AND v2_product_category != '(not set)'



WHERE total_transactions_revenue IS NOT NULL.


