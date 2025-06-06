
![banner rfm](https://github.com/user-attachments/assets/04e4c230-5608-4dff-ad47-0ebf3b8b9624)


# Python_RFM_project
# 📊 Project Title: Customer Segmentation for Retention & Revenue Growth | Python (RFM Model)  
Author: Linh Tran    
Tools Used: Python 

---

## 📑 Table of Contents  
1. [📌 Background & Overview](#-background--overview)  
2. [📂 Dataset Description & Data Structure](#-dataset-description--data-structure)
3. [⚒️ Main Process](#-main-process)
4. [🔎 Final Conclusion & Recommendations](#-final-conclusion--recommendations)

---

## 📌 Background & Overview  

### Objective:
### 📖 What is this project about? 
 
- A Python-based solution for customer segmentation using the RFM (Recency, Frequency, Monetary) model.
- Objective: Automate RFM analysis to identify high-value customers, improve retention strategies, and personalize marketing campaigns efficiently.

 > What is RFM model?  
**RFM (Recency – Frequency – Monetary)** is a part of **Marketing Analysis** used to assess **Customer Value**. It helps businesses segment their customer base into different groups, enabling targeted marketing campaigns and personalized customer care strategies. This analysis is conducted using the provided dataset.  
- **Recency (R):** How recently a customer made a purchase.  
- **Frequency (F):** How often a customer makes purchases.  
- **Monetary (M):** The total spending amount of a customer.

### 👤 Who is this project for?  

✔️ **Retail business owners & marketers**  
✔️ **Data analysts & business analysts**  
✔️ **E-commerce managers**  
✔️ **CRM & customer success teams**  
✔️ **Decision-makers & stakeholders**  

###  ❓Business Questions:  


✔️ **Customer Segmentation for Marketing Campaigns**: How can the Marketing department classify customer segments effectively to deploy tailored marketing campaigns for Christmas and New Year, appreciating loyal customers and attracting potential ones?  
✔️ **Implementing RFM Model**: How can the RFM (Recency, Frequency, Monetary) model be utilized to analyze and segment customers to enhance the effectiveness of marketing campaigns?  

### 🎯Project Outcome:  
 
- Segment customers into key groups using RFM analysis.  
- Identify high-value segments (Champions, Loyal, Potential Loyalists) for retention strategies.  
- Highlight at-risk and lost customers needing reactivation campaigns.  
- Provide data-driven insights to optimize marketing efforts for Christmas & New Year.  
- Recommend targeted promotions, loyalty programs, and personalized engagement strategies.   

---

## 📂 Dataset Description & Data Structure  

### 📌 Data Source  

- Source: Company database  
- Size: 8 columns, 541909 rows 
- Format: .csv

### 📊 Data Structure & Relationships  
<details>
  <summary>📌 This project used 2 tables:</summary>

  <details>
  <summary>Table 1: Transactions table</summary>

| Column Name | Data Type | Description |  
|-------------|----------|-------------|  
| InvoiceNo  | object      | Invoice number. Nominal, a 6-digit integral number uniquely assigned to each transaction. If this code starts with letter 'C', it indicates a cancellation. |  
| StockCode        | object     | Product (item) code. Nominal, a 5-digit integral number uniquely assigned to each distinct product. |
| Description    | object     | Product (item) name. Nominal. |  
| Quantity | int | The quantities of each product (item) per transaction. Numeric. |
| InvoiceDate       | object   | Invoice Date and time. Numeric, the day and time when each transaction was generated. |  
| UnitPrice | float | Unit price. Numeric, Product price per unit in sterling. |
| CustomerID | flotat| Customer number. Nominal, a 5-digit integral number uniquely assigned to each customer. |
| Country | object | Country name. Nominal, the name of the country where each customer resides. |

  </details>

  <details>
  <summary>Table 2: Segmentation table</summary>  

| Column Name | Data Type | Description |  
|-------------|----------|-------------| 
| Segment      | object   | Customer Segmentation Category    |
| RFM Score    | string   | RFM Score assigned to each segment |

</details>
</details> 


---

## ⚒️ Main Process

1️⃣  Exploratory Data Analysis (EDA)   



***1.1. Explore data***

![image](https://github.com/user-attachments/assets/3e887836-596d-4696-8c21-1122de6b888c)

 
 ```python
data.info()

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 541909 entries, 0 to 541908
Data columns (total 8 columns):
 #   Column       Non-Null Count   Dtype  
---  ------       --------------   -----  
 0   InvoiceNo    541909 non-null  object 
 1   StockCode    541909 non-null  object 
 2   Description  540455 non-null  object 
 3   Quantity     541909 non-null  int64  
 4   InvoiceDate  541909 non-null  object 
 5   UnitPrice    541909 non-null  float64
 6   CustomerID   406829 non-null  float64
 7   Country      541909 non-null  object 
dtypes: float64(2), int64(1), object(5)
memory usage: 33.1+ MB

```



![image](https://github.com/user-attachments/assets/84bea229-1db9-41cb-9b4e-f45c515d25ff)
![image](https://github.com/user-attachments/assets/9a5d9b8a-6569-4cdc-a9da-e7ee6abbc43d)
![image](https://github.com/user-attachments/assets/877b497a-53d8-402c-b5a7-6c8637a09709)
  

✅ "Description" and "CustomerID" columns have 1454 and 135080 missing values respectively.  

✅ InvoiceDate and CustomerID are in wrong datatype.  

➡️ Missing Data:  
- Removed null CustomerID rows.  
- Dropped Description column due to limited analytical value.  

➡️ Data type:  
- InvoiceDate => convert to Datetime type  
- CustomerID => convert to object type



```python
print('DataFrame dimension: ', data.shape)
     
DataFrame dimension:  (541909, 8)

print(data.describe())
     
            Quantity      UnitPrice     CustomerID
count  541909.000000  541909.000000  406829.000000
mean        9.552250       4.611114   15287.690570
std       218.081158      96.759853    1713.600303
min    -80995.000000  -11062.060000   12346.000000
25%         1.000000       1.250000   13953.000000
50%         3.000000       2.080000   15152.000000
75%        10.000000       4.130000   16791.000000
max     80995.000000   38970.000000   18287.000000
```

Detect why "Quantity" and "Price" have negative values (<0)

```
# subset dataframe to indicate rows that have negative values 
print('Row with negative Quantity (<0)')
data[data['Quantity'] < 0]

```

![image](https://github.com/user-attachments/assets/89ddb000-c1d1-4c05-946b-0e680d22043f)

```python
# The symbol "C" in "InvoiceNo" indicates a canceled order. Check if a negative Quantity always corresponds to a canceled order.  
# Create a column to check the cancellation status of the data.  
data['InvoiceNo'] = data['InvoiceNo'].astype(str)
data['check_cancel'] = data['InvoiceNo'].apply(lambda x: True if x[0] == 'C' else False)

# Check if the reason for Quantity < 0 is due to order cancellation.
data[(data['check_cancel'] == True) & (data['Quantity'] < 0)].sort_values('Quantity')
data[(data['check_cancel'] == True) & (data['Quantity'] <= 0)].sort_values('Quantity')
```

![image](https://github.com/user-attachments/assets/5df3a5f7-a8f1-41d3-b603-d4de1f5c4bf6)

```python
# Check orders without "C" (canceled) that still have a negative quantity
data[(data['check_cancel'] == False) & (data['Quantity'] < 0)].sort_values('Quantity')
data[(data['check_cancel'] == False) & (data['Quantity'] <= 0)].sort_values('Quantity')

data[(data['check_cancel'] == False) & (data['Quantity'] < 0)]['Description'].value_counts()
```

<p align="center">
  <img src="https://github.com/user-attachments/assets/c4a817d6-cf78-4f66-9190-6a8028fb2d6e" alt="Description">
</p>  

=> Orders with quantity < 0 that are not canceled are considered erroneous orders.

**Handling abnormal data values**   
➡️ Remove negative quantity values in df calculate RMF, keep in df data to get insight  
➡️ Remove excessively large quantity values  
➡️ UnitPrice < 0 => incorrect values => remove  

***1.2. Create a canceled orders table.***

*The project focuses on segmentation based only on successful orders, so I will analyze canceled orders separately.*

```python
# Invoices with a "C" at the beginning are canceled transactions.
cancelled = data['InvoiceNo'].astype(str).str.contains('C')
# Assign 0 to non-canceled orders and 1 to canceled orders.
cancelled.fillna(0, inplace=True)
cancelled = cancelled.astype(int)
cancelled.value_counts()
```

<p align="center">
  <img src="https://github.com/user-attachments/assets/5631a1e9-36fa-4623-bb41-3c9defa68160" alt="image">
</p>


*=> Number of canceled orders: 9288*  
*Calculate number of cancelled transactions and percentage:*  
```python
c1 = data['order_cancelled'].value_counts()[1]
c2 = data.shape[0]
print("Number of cancelled transactions: ", c1)
print('Percent of orders cancelled: {}/{} ({:.2f}%) '.format(c1, c2, c1/c2*100))
```  
*Number of cancelled transactions:  9288*
*Percent of orders cancelled: 9288/541909 (1.71%)*  
✅ The cancellation rate is relatively low, indicating a generally stable order fulfillment process.

***1.3. Clean data, create df_Transaction***


```python
# Create a copy of the 'data' dataframe
df_Transaction = data.copy()

# Remove canceled transactions
df_Transaction = df_Transaction[df_Transaction['order_cancelled'] == 0]

# Drop the 'Description' column
df_Transaction = df_Transaction.drop(columns=['Description'])

# Remove rows with missing CustomerID
df_Transaction = df_Transaction.dropna(subset=['CustomerID'])

# Check for duplicates
print('Duplicate entries: {}'.format(df_Transaction.duplicated().sum()))
print('{}% rows are duplicate.'.format(round((df_Transaction.duplicated().sum()/df_Transaction.shape[0])*100),2))

# Drop duplicate data
df_Transaction.drop_duplicates(inplace=True)

# Change data type for CustomerID and InvoiceDate
df_Transaction['CustomerID'] = df_Transaction['CustomerID'].astype(str)  # Convert CustomerID to object type
data['InvoiceDate'] = pd.to_datetime(data['InvoiceDate'], format='%d/%m/%Y %H:%M')  # Convert InvoiceDate to datetime

# Remove negative UnitPrice and Quantity
df_Transaction = df_Transaction[(df_Transaction['UnitPrice'] > 0) & (df_Transaction['Quantity'] > 0)]

# Check EDA results
print(df_Transaction.head())
print(df_Transaction.describe())
print(df_Transaction.dtypes)
```

![image](https://github.com/user-attachments/assets/4293ced4-5879-46b3-b8d1-92583f314937)

2️⃣ **2. Calculate RFM**  

***2.1. Calculate RFM***


```python
# Create Revenue column
df_Transaction['Revenue'] = df_Transaction['UnitPrice'] * df_Transaction['Quantity']
```

⭐ In RFM, Recency (R) measures the time elapsed since a customer's last transaction until the present.  
➡️  Since the dataset only includes transactions up to the latest recorded date, we set last_date as the following day to avoid a zero gap when calculating Recency.   

```python
# To display only the date in InvoiceDate
from datetime import datetime
df_Transaction["InvoiceDate"] = df_Transaction["InvoiceDate"].dt.date

import datetime as dt
last_date = max(df_Transaction.InvoiceDate) + dt.timedelta(days=1)
```


```python
df_rfm = df_Transaction.groupby('CustomerID').agg({'InvoiceDate': lambda x: (last_date - x.max()).days, 'InvoiceNo': lambda x: len(x), 'Revenue': lambda x: x.sum()}).reset_index()
df_rfm['InvoiceDate'] = df_rfm['InvoiceDate'].astype(int)

# Rename columns
df_rfm.rename(columns={'InvoiceDate': 'Recency',
                         'InvoiceNo': 'Frequency',
                         'Revenue': 'Monetary'}, inplace=True)
df_rfm.head()
```

<p align="center">
  <img src="https://github.com/user-attachments/assets/81fd94b8-4185-4028-bda5-9e956040046b" alt="image">
</p>


***2.2. Calculate RFM Score***
```python
# Calculate Recency (R), Frequency (F), and Monetary (M) scores
df_rmf['R_Score'] = pd.qcut(df_rmf['Recency'], 5, labels=[5, 4, 3, 2, 1])
df_rmf['F_Score'] = pd.qcut(df_rmf['Frequency'], 5, labels=[1, 2, 3, 4, 5])
df_rmf['M_Score'] = pd.qcut(df_rmf['Monetary'], 5, labels=[1, 2, 3, 4, 5])

# Combine R, F, and M scores into a single string
df_rmf['RFM_Score'] = df_rmf['R_Score'].astype(str) + df_rmf['F_Score'].astype(str) + df_rmf['M_Score'].astype(str)

# The result is a DataFrame containing columns R_Score, F_Score, M_Score, and RFM_Score
print(df_rmf[['CustomerID', 'Recency', 'Frequency', 'Monetary', 'RFM_Score']])
```
<p align="center">
  <img src="https://github.com/user-attachments/assets/db0f3f2b-6c63-4a93-a697-91f6d84d77c3" alt="image">
</p>


# **3. Segmentation**
```python
# Calculate the number of customers in each segment.
df_segment_data = df_user.groupby('Segment').agg({'CustomerID': lambda x: len(x),
                                                   'Recency': lambda x: x.mean(),
                                                   'Frequency': lambda x: x.sum(),
                                                   'Monetary': lambda x: x.sum()}).reset_index()

df_segment_data.rename(columns={'CustomerID': 'Count'}, inplace=True)
df_segment_data['percent'] = (df_segment_data['Count'] / df_segment_data['Count'].sum()) * 100
df_segment_data['percent'] = df_segment_data['percent'].round(1)

# Rename columns after calculating the aggregations
df_segment_data.rename(columns={'Recency': 'Avg Recency', 
                                 'Frequency': 'Orders', 
                                 'Monetary': 'Sales'}, inplace=True)

df_segment_data['Avg Recency'] = df_segment_data['Avg Recency'].round(1)
df_segment_data['Avg orders'] = (df_segment_data['Orders'] / df_segment_data['Count']).round(1) # Use 'Count' instead of 'Number of Customers'
df_segment_data['Avg sales'] = (df_segment_data['Sales'] / df_segment_data['Count']).round(1)  # Use 'Count' instead of 'Number of Customers'

print(df_segment_data)
```

<p align="center">
  <img src="https://github.com/user-attachments/assets/f50c0866-f6d0-4308-bd63-8933caf2894e" alt="image">
</p>

| **Segment**             | **Characteristics**                                                                           | **Recommendations**                                                                                     |
|-------------------------|--------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| **Champions**           | Recently purchased, frequent buyers, and highest spenders.                               | Offer gifts, special promotions, and free trials of new products. Encourage them to promote the brand. |
| **Loyal**               | Spend significantly and purchase frequently. Respond well to promotions.                  | Recommend higher-value products. Collect product reviews. Maintain regular engagement.                 |
| **Potential Loyalist**  | New customers who have made multiple purchases.                                          | Introduce loyalty programs or memberships and suggest other relevant products.                         |
| **New Customers**       | Recently made a purchase but not frequently.                                             | Provide initial support to ensure a smooth first-time shopping experience.                             |
| **Promising**           | Recently made a purchase but with low spending.                                          | Build brand awareness and offer free trials.                                                           |
| **Need Attention**      | Recency, Frequency, and Monetary (RFM) values are above average but not consistent.      | Provide time-limited offers based on past purchases. Encourage repeat purchases.                       |
| **About to Sleep**      | Below-average RFM values, at risk of becoming inactive.                                  | Recommend popular products or special discounts. Reconnect with them.                                  |
| **At Risk**             | Previously spent a lot and purchased frequently but haven’t bought in a while.          | Send personalized emails, messages, or promotions to re-engage them.                                   |
| **Cannot Lose Them**    | Used to make large and frequent purchases but have not returned for a long time.        | Re-attract them with new product offerings or renewal options to prevent losing them to competitors.    |
| **Hibernating Customers** | Haven't purchased in a long time, low spending, and few orders.                      | Suggest relevant products with special offers. Reinforce brand value.                                  |
| **Lost Customers**      | Lowest RFM values, least engaged customers.                                             | Attempt re-engagement campaigns, but consider deprioritizing if they remain inactive.                  |

3️⃣ **Visualization**   
**Customers share by Segment**  

<p align="center">
  <img src="https://github.com/user-attachments/assets/9c9582cb-633e-4635-b0dc-1cb397de7780" alt="image">
</p>

**Sales share by Segment**  
<p align="center">
  <img src="https://github.com/user-attachments/assets/fb5d3852-f3aa-4b2a-8c98-96d5b3046a11" alt="image">
</p>

**Average orders per customer by Segment**  

<p align="center">
  <img src="https://github.com/user-attachments/assets/72ae4976-63a8-42d9-b8c1-4b65ca607040" alt="image">
</p>

**Average sales per customer by Segment**  


<p align="center">
  <img src="https://github.com/user-attachments/assets/5faa69dd-1eca-4f39-8446-f4189b0842cf" alt="image">
</p>  

👉 More focus on: 


- **Champions & Loyal Customers**: These segments place the **highest number of orders per customer**, proving their strong buying habits. Retaining them is **more cost-effective** than acquiring new customers.    
-  **Potential Loyalists**: They purchase frequently but haven't yet reached Champions' level. Targeting them can increase customer lifetime value.    
-  **At Risk & Lost Customers**: These customers used to contribute revenue but are now disengaged. Losing them permanently represents **lost potential revenue**.    
-   **Hibernating Customers**: They represent a **large portion (18.49%)** of the customer base but **contribute very little to sales**.    


📍 **Recency by Segment**  
<p align="center">
  <img src="https://github.com/user-attachments/assets/40b9d255-3758-435f-98e3-a6c6c8f95bcf" alt="image">
</p>  


📌 **Analysis:** 

- **Champions**: Recently active, highly engaged. Maintain loyalty with VIP programs, early access to new products, and personalized thank-you emails.  
- **At Risk**: Haven’t purchased in a while. Reactivate with reminder emails, time-limited discounts, and win-back campaigns highlighting past purchases.  
- **Loyal**: Moderate recency, still engaged. Strengthen retention with exclusive deals, reward programs, and personalized product recommendations.  
- **Hibernating Customers**: Long inactivity. Re-engage with reactivation emails, special comeback discounts, and surveys to understand why they left.  
- **Lost Customers**: Extremely high recency. Try last-chance offers, nostalgic messaging about past purchases, and bundle deals to entice them back.  
- **Potential Loyalists**: Recently active but inconsistent. Nurture with follow-up emails, personalized promotions, and incentives for repeat purchases.

📍 **Frequency by Segment**

<p align="center">
  <img src="https://github.com/user-attachments/assets/ce9a6a3d-9352-442b-a4c9-9c919135802b" alt="image">
</p>

📌 **Analysis:**     

- **Champions**: Most frequent buyers with high consistency. Reward them with VIP perks, early product access, and exclusive discounts to keep engagement strong.  
- **At Risk**: Purchase frequency has declined. Reignite interest with targeted win-back campaigns, personalized recommendations, and time-sensitive promotions.  
- **Loyal**: Consistently engaged but not as frequent as Champions. Strengthen ties through loyalty programs, referral incentives, and personalized emails.  
- **Hibernating Customers**: Infrequent shoppers with long gaps between purchases. Use email reminders, re-engagement ads, and special "welcome back" deals.  
- **Lost Customers**: Very low purchase frequency, nearly inactive. Send final attempt offers, nostalgia-driven messages, and exclusive reactivation discounts.  
- **Potential Loyalists**: Show promising buying behavior but not yet fully committed. Encourage regular purchases with tailored incentives, follow-ups, and bundled deals.

📍 **Monetary by Segment**  
<p align="center">
  <img src="https://github.com/user-attachments/assets/e2f11d82-ae04-4a55-83b7-adb1adce1807" alt="image">
</p>  

📌 **Analysis:**     

- **Champions**: Highest spending customers. Maintain exclusivity with VIP memberships, premium offerings, and early access to products.  
- **At Risk**: Previously high spenders but declining engagement. Offer personalized discounts, limited-time promotions, and one-on-one consultations.  
- **Loyal**: Consistent spenders but with room to grow. Upsell and cross-sell using tailored recommendations, subscription options, and exclusive bundles.  
- **Hibernating Customers**: Low spending with sporadic engagement. Use budget-friendly offers, special reactivation discounts, and engaging reminders.  
- **Lost Customers**: Minimal spending, nearly inactive. Last-chance deals, "win back" offers, and emotional storytelling may help revive interest.  
- **Potential Loyalists**: Growing spending potential. Nurture them with personalized recommendations, milestone-based rewards, and targeted follow-ups.
---

## 🔎 Final Conclusion & Recommendations  

✅ **Champions & Loyal Customers**: Retain with VIP programs, early access, and exclusive holiday offers.  
✅ **Potential Loyalists**: Strengthen engagement through personalized recommendations and milestone rewards.  
✅ **At-Risk & Hibernating Customers**: Reactivate with limited-time discounts, email campaigns, and reminders.  
✅ **Lost Customers**: Last-chance deals and emotional messaging to re-engage.  
✅ **Overall Strategy**: Tailor holiday promotions to each segment, leveraging email marketing, personalized ads, and loyalty incentives for maximum ROI.


---
