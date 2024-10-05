# Python-Data_Wrangling-E_Wallet_Company
Using Python to do EDA and Data Wrangling tasks from an E-Wallet Company dataset

## Table of Contents:
1. [Introduction](#data)
2. [Dataset](#cau4)
3. [Exploratory Data Analysis](#cau5)
4. [Data Wrangling](#cau6)

<div id='data'/>

## 1. Introduction

This project focuses on analyzing key datasets from an e-wallet company to extract valuable business insights. The main objective is to examine payment volumes, product details, and transaction trends using the following datasets:

- payment_report.csv: Contains monthly payment volumes for various products, providing insights into product performance over time.

- product.csv: Includes detailed information about products, such as categories, prices, and descriptions, enabling a comprehensive analysis of product segments.

- transactions.csv: Records transaction details, including timestamps, amounts, and product IDs, offering a deep dive into customer behavior and transaction trends.

Part I: Exploratory Data Analysis (EDA) involves merging and cleaning the payment and product datasets, identifying missing data, duplicates, incorrect data types, and ensuring data quality.

Part II: Data Wrangling aims to uncover key insights, such as identifying top-performing products, analyzing team performance, and categorizing different types of transactions, including refunds and transfers. The objective is to derive actionable insights from the datasets for better decision-making.


<div id='cau4'/>
  
## 2. Dataset

### Dataframe Payment_Report

| report_month | payment_group | product_id | source_id | volume      |
|--------------|---------------|------------|-----------|-------------|
| 2023-01      | payment        | 12         | 45        | 624110375   |
| 2023-01      | payment        | 17         | 45        | 335715113   |
| 2023-01      | payment        | 18         | 45        | 737784466   |
| 2023-01      | payment        | 19         | 45        | 120963069   |
| 2023-01      | payment        | 20         | 45        | 319653158   |
| ...          | ...            | ...        | ...       | ...         |
| 2023-04      | payment        | 15067      | 45        | 1504000     |
| 2023-04      | refund         | 1976       | 37        | 3542271587  |
| 2023-04      | refund         | 1976       | 38        | 13831708189 |
| 2023-04      | refund         | 1976       | 39        | 1905435543  |
| 2023-04      | refund         | 1976       | 39        | 3679922071  |

This is a sample from the dataset, showing the structure of the payment and refund transactions, including details like report_month, payment_group, product_id, source_id, and volume. The dataset continues with similar data for multiple months.

### Dataframe Product

| product_id | category | team_own |
|------------|----------|----------|
| 17         | PXXXXXB  | ASD      |
| 18         | PXXXXXB  | ASD      |
| 20         | PXXXXXB  | ASD      |
| 287        | PXXXXXB  | ASD      |
| 372        | PXXXXXB  | ASD      |
| ...        | ...      | ...      |
| 321        | PXXXXXV  | ASD      |
| 322        | PXXXXXV  | ASD      |
| 341        | PXXXXXV  | ASD      |
| 342        | PXXXXXV  | ASD      |
| 367        | PXXXXXV  | ASD      |

This dataset contains information about products, including product_id, category, and the team that owns the product (team_own). It shows various products and their assigned categories under team ASD. The table continues with more entries.

### Dataframe Transactions

| | transaction_id | merchant_id | volume  | transType | transStatus | sender_id  | receiver_id | extra_info | timeStamp      |
|-----|----------------|-------------|---------|-----------|-------------|------------|-------------|------------|----------------|
| 1   | 3002692434     | 5           | 100000  | 24        | 1           | 10199794.0 | 199794.0    | NaN        | 1682932054455  |
| 2   | 3002692437     | 305         | 20000   | 2         | 1           | 14022211.0 | 14022211.0  | NaN        | 1682932054912  |
| 3   | 3001960110     | 7255        | 48605   | 22        | 1           | NaN        | 10530940.0  | NaN        | 1682932055000  |
| 4   | 3002680710     | 2270        | 1500000 | 2         | 1           | 10059206.0 | 59206.0     | NaN        | 1682932055622  |
| 5   | 3002680713     | 2275        | 90000   | 2         | 1           | 10004711.0 | 4711.0      | NaN        | 1682932056197  |
| ... | ...            | ...         | ...     | ...       | ...         | ...        | ...         | ...        | ...            |
| 1323998 | 3003723033 | 2270        | 100000  | 2         | 1           | 10277242.0 | 277242.0    | NaN        | 1683035672876  |
| 1323999 | 3003723036 | 2270        | 100000  | 2         | 1           | 10144599.0 | 144599.0    | NaN        | 1683035672892  |
| 1324000 | 3003723039 | 5           | 400     | 22        | 1           | 10028007.0 | 21013762.0  | NaN        | 1683035672896  |
| 1324001 | 3003602967 | 2250        | 1       | 8         | 1           | 38559843.0 | 24501638.0  | NaN        | 1683035673053  |


<div id='cau5'/>

## 3. EDA 

### Merge Payment_report and Product

~~~~python

payment_enriched = pd.merge(payment_report,product, on = 'product_id', how = 'left')

~~~~

|  | report_month | payment_group | product_id | source_id | volume      | category  | team_own |
|-----|--------------|---------------|------------|-----------|-------------|-----------|----------|
| 1   | 2023-01      | payment       | 12         | 45        | 624110375   | PXXXXXT   | ASD      |
| 2   | 2023-01      | payment       | 17         | 45        | 335715113   | PXXXXXB   | ASD      |
| 3   | 2023-01      | payment       | 18         | 45        | 737784466   | PXXXXXB   | ASD      |
| 4   | 2023-01      | payment       | 19         | 45        | 120963069   | PXXXXXM2  | ASD      |
| 5   | 2023-01      | payment       | 20         | 45        | 319653158   | PXXXXXB   | ASD      |
| ... | ...          | ...           | ...        | ...       | ...         | ...       | ...      |
| 914 | 2023-04      | payment       | 15067      | 45        | 1504000     | PXXXXXR   | ASL      |
| 915 | 2023-04      | refund        | 1976       | 37        | 3542271587  | NaN       | NaN      |
| 916 | 2023-04      | refund        | 1976       | 38        | 13831708189 | NaN       | NaN      |
| 917 | 2023-04      | refund        | 1976       | 39        | 1905435543  | NaN       | NaN      |
| 918 | 2023-04      | refund        | 1976       | 39        | 3679922071  | NaN       | NaN      |


### Checking Missing Data

~~~~python

payment_enriched.isnull().sum()
transactions.isnull().sum()
~~~~

| transaction_id | merchant_id | volume | transType | transStatus | sender_id | receiver_id | extra_info | timeStamp |
|----------------|-------------|--------|-----------|-------------|-----------|-------------|------------|-----------|
| 0              | 0           | 0      | 0         | 0           | 49059     | 164795      | 1317907    | 0         |


**Results:**
- 0 NaN from payment_enriched
- 49059 NaN from transaction[sender_id]
- 164795 NaN from transaction[receiver_id]
- 1317907 NaN from transaction[extra_info]

**Next Step:**
- Can Deny NaN from extra_info
- Check Impact of NaN on the whole DataFrame. Delete if both sender_id and receiver_id missing.
- Check the other missing values impact

### Checking Incorrect Duplicates

~~~~python
duplicates_by_columns_trans = transactions[transactions.duplicated(subset=['transaction_id', 'sender_id', 'receiver_id'])]
transactions_cleaned = transactions.drop_duplicates(subset=['transaction_id', 'sender_id', 'receiver_id'])
~~~~

<div id='cau6'/>

## 4. Data Wrangling 

### Using payment_report.csv & product.csv

**1. Top 3 product_ids with the highest volume.**

product_id | volume
-----------|---------------
1976       | 61797583647
429        | 14667676567
372        | 13713658515


**2. Given that 1 product_id is only owed by 1 team, are there any abnormal products against this rule?**

No.    | product_id | team_own
-----|------------|---------
0    | 3          | 0
279  | 1976       | 0
308  | 10033      | 0


**3. Find the team has had the lowest performance (lowest volume) since Q2.2023. Find the category that contributes the least to that team.**

~~~~python
#Edit Datetime
payment_enriched["report_month"] = pd.to_datetime(payment_enriched["report_month"])
payment_enriched["report_year"] = payment_enriched["report_month"].dt.year
payment_enriched["report_quarter"] = payment_enriched["report_month"].dt.quarter
from_q2_2023 = payment_enriched[(payment_enriched['report_year'] >= 2023) & (payment_enriched['report_quarter'] >= 2)]
team_performance = from_q2_2023.groupby('team_own')['volume'].sum()
lowest_performance_team = team_performance.idxmin()
print(f"Team has had lowest performance since Q2.2023: {lowest_performance_team}")

#Find Category from lowest performance team
team_data = from_q2_2023[from_q2_2023['team_own'] == lowest_performance_team]
category_contribution = team_data.groupby('category')['volume'].sum()
least_contributing_category = category_contribution.idxmin()
print(f"Category that contributes the least to {lowest_performance_team}: {least_contributing_category}")

~~~~

Team has had lowest performance since Q2.2023: APS
Category that contributes the least to APS: PXXXXXE

**4. Find the contribution of source_ids of refund transactions (payment_group = ‘refund’), what is the source_id with the highest contribution?**

~~~~python
refund_transactions = payment_enriched[payment_enriched['payment_group'] == 'refund']
source_id_contribution = refund_transactions.groupby('source_id')['volume'].sum()
highest_contribution_source_id = source_id_contribution.idxmax()
print(f"Source ID with the highest contribution: {highest_contribution_source_id}")
~~~~
Source ID with the highest contribution: 38

### Using transactions.csv


**1. Define type of transactions (‘transaction_type’) for each row, given:**
  transType = 2 & merchant_id = 1205: Bank Transfer Transaction
  transType = 2 & merchant_id = 2260: Withdraw Money Transaction
  transType = 2 & merchant_id = 2270: Top Up Money Transaction
  transType = 2 & others merchant_id: Payment Transaction
  transType = 8, merchant_id = 2250: Transfer Money Transaction
  transType = 8 & others merchant_id: Split Bill Transaction
  Remained cases are invalid transactions

~~~~python

def determine_transaction_type(row):
    if row['transType'] == 2 and row['merchant_id'] == 1205:
        return 'Bank Transfer Transaction'
    elif row['transType'] == 2 and row['merchant_id'] == 2260:
        return 'Withdraw Money Transaction'
    elif row['transType'] == 2 and row['merchant_id'] == 2270:
        return 'Top Up Money Transaction'
    elif row['transType'] == 2:
        return 'Payment Transaction'
    elif row['transType'] == 8 and row['merchant_id'] == 2250:
        return 'Transfer Money Transaction'
    elif row['transType'] == 8:
        return 'Split Bill Transaction'
    else:
        return 'Invalid Transaction'

transactions['transaction_type'] = transactions.apply(determine_transaction_type, axis=1)
~~~~

transaction_id | merchant_id | volume | transType | transStatus | sender_id | receiver_id | extra_info | timeStamp      | transaction_type
---------------|--------------|--------|-----------|-------------|-----------|-------------|------------|----------------|---------------------
3002692434     | 5            | 100000 | 24        | 1           | 10199794.0| 199794.0    | NaN        | 1682932054455  | Invalid Transaction
3002692437     | 305          | 20000  | 2         | 1           | 14022211.0| 14022211.0  | NaN        | 1682932054912  | Payment Transaction
3001960110     | 7255         | 48605  | 22        | 1           | -1.0      | 10530940.0  | NaN        | 1682932055000  | Invalid Transaction
3002680710     | 2270         | 1500000| 2         | 1           | 10059206.0| 59206.0     | NaN        | 1682932055622  | Top Up Money Transaction
3002680713     | 2275         | 90000  | 2         | 1           | 10004711.0| 4711.0      | NaN        | 1682932056197  | Payment Transaction
...            | ...          | ...    | ...       | ...         | ...       | ...         | ...        | ...            | ...
3003723030     | 305          | 20000  | 2         | 1           | 24524311.0| -1.0        | NaN        | 1683035672634  | Payment Transaction
3003723033     | 2270         | 100000 | 2         | 1           | 10277242.0| 277242.0    | NaN        | 1683035672876  | Top Up Money Transaction
3003723036     | 2270         | 100000 | 2         | 1           | 10144599.0| 144599.0    | NaN        | 1683035672892  | Top Up Money Transaction
3003723039     | 5            | 400    | 22        | 1           | 10028007.0| 21013762.0  | NaN        | 1683035672896  | Invalid Transaction
3003602967     | 2250         | 1      | 8         | 1           | 38559843.0| 24501638.0  | NaN        | 1683035673053  | Transfer Money Transaction

**2. Of each transaction type (excluding invalid transactions): find the number of transactions, volume, senders and receivers.**

~~~~python
valid_transactions = transactions[transactions['transaction_type'] != 'Invalid Transaction']
transaction_summary = valid_transactions.groupby('transaction_type').agg(
    num_transactions=('transaction_type', 'count'),
    total_volume=('volume', 'sum'),
    num_senders=('sender_id', 'nunique'),
    num_receivers=('receiver_id', 'nunique')
).reset_index()
transaction_summary
~~~~

transaction_type            | num_transactions | total_volume    | num_senders | num_receivers
----------------------------|------------------|-----------------|-------------|---------------
Bank Transfer Transaction    | 37879            | 50605806190     | 23156       | 9272
Payment Transaction          | 398677           | 71851515181     | 139583      | 113299
Split Bill Transaction       | 1376             | 4901464         | 1323        | 572
Top Up Money Transaction     | 290502           | 108606478829    | 110409      | 110409
Transfer Money Transaction   | 341177           | 37033171492     | 39021       | 34585
Withdraw Money Transaction   | 33725            | 23418181420     | 24814       | 24814


**Insights**

- Top-Up Money Transactions are the most significant in terms of volume, with over 108 billion, followed by Payment Transactions at 71.85 billion. This suggests that users are frequently topping up their accounts.

- Payment Transactions lead in terms of number of transactions (398,677) and the number of senders and receivers. This shows that payments are the most frequent type of transaction, reflecting high activity in day-to-day purchases or services.

- Transfer Money Transactions show substantial activity as well, with 341,177 transactions and a volume of over 37 billion, indicating that a large number of users are using the e-wallet for peer-to-peer transfers.

- Withdraw Money Transactions have a lower transaction count (33,725) but still hold a significant volume, indicating users cash out occasionally but in large amounts.

- Split Bill Transactions, though small in volume and transaction count, show that the feature is being utilized, likely in social or group payment settings.

- These insights highlight how different transaction types serve unique purposes in the e-wallet ecosystem, with heavy usage in top-ups and payments.
