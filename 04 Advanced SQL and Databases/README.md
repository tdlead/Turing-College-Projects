### Task T1.1
**Problem:**
You’ve been tasked to create a detailed overview of all individual customers (these are defined by customerType = ‘I’ and/or stored in an individual table).<br>

**Solution:**
https://console.cloud.google.com/bigquery?sq=147855269776:c57ac0eac1a24c13ba209efdf2ed1f25

### Task T1.2
**Problem:**
Business finds the original query valuable to analyze customers and now want to get the data from the first query for the top 200 customers with the highest total amount (with tax) who have not ordered for the last 365 days.<br>

**Solution:**
https://console.cloud.google.com/bigquery?sq=147855269776:c20c0d25826343cd8a6c5f28ebbc03f9

### Task T1.3
**Problem:**
Enrich your original 1.1 SELECT by creating a new column in the view that marks active & inactive customers based on whether they have ordered anything during the last 365 days.<br>

**Solution:**
https://console.cloud.google.com/bigquery?sq=147855269776:6cb8ae64796b4cb4b52c915a1e561e07

### Task T1.4
**Problem:**
Business would like to extract data on all active customers from North America. Only customers that have either ordered 2500 in total amount (with Tax) or ordered 5 + times should be presented.<br>

**Solution:**
https://console.cloud.google.com/bigquery?sq=147855269776:61648b830eeb4de597423ef5c43220d4

### Task T2.1
**Problem:**
Create a query of monthly sales numbers in each Country & region.  <br>
**Solution:**
https://console.cloud.google.com/bigquery?sq=147855269776:97d74d2a47ab41a9a6bcf5273299d330

### Task T2.2
**Problem:**
Enrich 2.1 query with the cumulative_sum of the total amount with tax earned per country & region.<br>

**Solution:**
https://console.cloud.google.com/bigquery?sq=147855269776:3059e02bb39a40fb899ed0f350129322

### Task T2.3
**Problem:**
Enrich 2.2 query by adding ‘sales_rank’ column that ranks rows from best to worst for each country based on total amount with tax earned each month.
**Solution:**
https://console.cloud.google.com/bigquery?sq=147855269776:1cce25052d9b40409588e834b6255d25

### Task T2.4
**Problem:**
Enrich 2.3 query by adding taxes on a country level
**Solution:**
https://console.cloud.google.com/bigquery?sq=147855269776:ba0a744686f744a19c4a4021fa1ac7b0
