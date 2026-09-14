## 1. Business Performance Summary
```sql
select 
  sum(nett_sales) as total_nett_sales,
  sum(nett_profit) as total_nett_profit,
  count(transaction_id) as total_transaction,
  sum(nett_sales) / count(transaction_id) AS avg_transaction_value,
  avg(branch_rating) as avg_branch_rating,
  avg(transaction_rating) as avg_transaction_rating,
  count(distinct customer_name) AS unique_customers
from `rakamin-kf-analytics-508303.kimia_farma.kf_table_analysis`
```
Result
|total_nett_sales|	total_nett_profit|	total_transaction	|avg_transaction_value|	avg_branch_rating	|avg_transaction_rating|	unique_customers|
|---|---|--|--|--|--|--|
|321,171,190,319.0|	91,214,988,059.850449|	672,458	|477,607.80646374938|	4.44798351718632|	4.0000330132143089|	264,601|

## 2. Nett Sales and Profit YoY
```sql
select 
  date_trunc(date, year) as year,
  sum(nett_sales) as total_nett_sales,
  sum(nett_profit) as total_nett_profit
from `rakamin-kf-analytics-508303.kimia_farma.kf_table_analysis`
group by year
order by year
```
Result
|year| total_nett_sales|	total_nett_profit|
|--|--|--|
|2020-01-01|	80,437,605,040.0|	22,842,355,149.649975|
|2021-01-01|	80,037,846,824.0|	22,731,171,469.500187|
|2022-01-01|	80,578,445,844.0|	22,883,598,882.799908|
|2023-01-01|	80,117,292,611.0|	22,757,862,557.899982|

## 3. Monthly Nett Sales
```sql
select 
  date_trunc(date, month) as month,
  sum(nett_sales) as total_nett_sales
from `rakamin-kf-analytics-508303.kimia_farma.kf_table_analysis`
group by month
```
Result
|month|	total_nett_sales|
|--|--|
|2023-01-01|	6,842,843,272.0|
|2022-01-01|	6,869,281,246.0|
|2020-02-01|	6,414,736,842.0|
|2020-07-01|	6,760,784,461.0|
|2022-06-01|	6,651,015,860.0|
|:|:|

_** hasil hanya menampilkan 5 baris pertama_

## 4. Profit by Province
```sql
select 
  province,
  sum(nett_profit) as total_profit
from `rakamin-kf-analytics-508303.kimia_farma.kf_table_analysis`
group by province
order by total_profit desc
```
Result
|province|	total_profit|
|--|--|
|Jawa Barat|	26,940,703,195.150703|
|Sumatera Utara|	6,517,187,359.849968|
|Jawa Tengah|	6,318,005,348.2499571|
|Jawa Timur|	4,722,764,504.199955|
|Sulawesi Utara|	4,516,325,323.6000013|
|:|:|

_** hasil hanya menampilkan 5 baris pertama_

## 5. Top Rated Branch with Low Transactions Ratings
```sql
select 
  branch_id as branch_id,
  avg(branch_rating) as avg_branch_rating,
  avg(transaction_rating) as avg_transaction_rating,
  avg(branch_rating) - avg(transaction_rating) as gap_rating
from `rakamin-kf-analytics-508303.kimia_farma.kf_table_analysis`
group by branch_id
order by gap_rating desc
limit 5
```
Result
|branch_id|	avg_branch_rating|	avg_transaction_rating|	gap_rating|
|--|--|--|--|
|82157	|5.0|	3.9051470588235322|	1.0948529411764678|
|44567|	5.0|	3.9306024096385537|	1.0693975903614463|
|13775|	5.0	|3.931638418079098|	1.068361581920902|
|31872|	5.0|	3.9349593495934938|	1.0650406504065062|
|62707|	5.0|	3.9572916666666655|	1.0427083333333345|
|:|:|

_** hasil hanya menampilkan 5 baris pertama_

## 6. Top 10 Province by Nett Sales
```sql
SELECT
  province as province,
  branch_name as branch_type,
  SUM(nett_sales) AS total_nett_sales,
  SUM(SUM(nett_sales)) OVER (
    PARTITION BY province
  ) AS total_nett_sales_province
from `rakamin-kf-analytics-508303.kimia_farma.kf_table_analysis`
group by province, branch_type
order by total_nett_sales desc
limit 10
```
Result
|province|	branch_type|	total_nett_sales|	total_nett_sales_province	|
|--|--|--|--|
|Jawa Barat|	Kimia Farma - Apotek|	34,196,247,476.0|	94,869,594,875.0|
|Jawa Barat|	Kimia Farma - Klinik-Apotek-Laboratorium|	33,291,979,665.0|	94,869,594,875.0|
|Jawa Barat|	Kimia Farma - Klinik & Apotek|	27,381,367,734.0|	94,869,594,875.0|
|Sumatera Utara	|Kimia Farma - Klinik-Apotek-Laboratorium|	8,372,000,917.0	|22,952,159,897.0|
|Sumatera Utara	|Kimia Farma - Apotek	|8,047,112,187.0|	22,952,159,897.0|
|Sumatera Utara	|Kimia Farma - Klinik & Apotek|	6,533,046,793.0|	22,952,159,897.0|
|:|:|

_** hasil hanya menampilkan 2 provinsi pertama dengan total nett sales tertinggi_

## 7. Top 10 Province by Total Transactions
```sql
SELECT
  province as province,
  branch_name as branch_type,
  count(transaction_id) AS total_transaction,
  SUM(count(transaction_id)) OVER (
    PARTITION BY province
  ) AS total_transactions_province
from `rakamin-kf-analytics-508303.kimia_farma.kf_table_analysis`
group by province, branch_type
order by total_transaction desc
limit 10
```
Result
|province|	branch_type|	total_transactions|	total_transactions_province	|
|--|--|--|--|
|Jawa Barat	|Kimia Farma - Apotek	|71,508	|198,723|
|Jawa Barat|	Kimia Farma - Klinik-Apotek-Laboratorium|	69,874|	198,723|
|Jawa Barat|	Kimia Farma - Klinik & Apotek|	57,341	|198,723|
|Sumatera Utara|	Kimia Farma - Klinik-Apotek-Laboratorium|	17,561	|48,178|
|Sumatera Utara|	Kimia Farma - Apotek|	16,903|	48,178|
|Sumatera Utara|	Kimia Farma - Klinik & Apotek|	13,714	|48,178|
|:|:|

_** hasil hanya menampilkan 2 provinsi pertama dengan total transactions tertinggi_

## 8. Transactions Distribution by Branch Type
```sql
select
  branch_name as branch_type,
  COUNT(DISTINCT transaction_id) AS total_transactions
from `rakamin-kf-analytics-508303.kimia_farma.kf_table_analysis`
group by branch_type
order by total_transactions desc
```
Result
|branch_type|	total_transactions|
|--|--|
|Kimia Farma - Apotek	|227,677|
|Kimia Farma - Klinik & Apotek|	222,718|
|Kimia Farma - Klinik-Apotek-Laboratorium|	222,063|

## 9. Top 10 Products by Total Transactions
```sql
select  
  product_id as product_id,
  count(transaction_id) as total_transactions
from `rakamin-kf-analytics-508303.kimia_farma.kf_table_analysis`
group by product_id
order by total_transactions desc
limit 5
```
Result
|product_id|	total_transactions|
|--|--|
|KF519|	4608|
|KF262|	4590|
|KF346|	4587|
|KF576|	4582|
|KF943|	4582|

## 10. Top 10 Products by Nett Sales
```sql
select  
  product_id as product_id,
  sum(nett_sales) as total_nett_sales
from `rakamin-kf-analytics-508303.kimia_farma.kf_table_analysis`
group by product_id
order by total_nett_sales desc
limit 5
```
Result
|product_id|	total_transactions|
|--|--|
|KF953|	4106577825.0|
|KF633|	4031493928.0|
|KF977|	3963848400.0|
|KF710|	3947534800.0|
|KF881|	3938881372.0|

## 11. Customer Transaction Frequency
```sql
select  
  transaction_frequency_group as customer_segment,
  count(customer_name) as total_customer
from `rakamin-kf-analytics-508303.kimia_farma.kf_table_analysis`
group by customer_segment
order by total_customer desc
```
Result
|customer_segment|	total_customer|
|--|--|
|Repeat Customer|	518956|
|One-time Customer|	153502|
