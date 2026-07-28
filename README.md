

# 💳 Credit Card Analytics Dashboard in Power BI

A **comprehensive end-to-end Power BI project** analyzing **credit card operations, customer behavior, and transactions**.
This dashboard provides **real-time insights** into key performance metrics, helping stakeholders monitor revenue, spending patterns, and customer trends effectively.

---

## 📌 Table of Contents

1. [🎯 Project Objective](#-project-objective)
2. [📂 Dataset Description](#-dataset-description)
3. [⚙️ Data Preparation](#️-data-preparation)
4. [📊 Visualizations](#-visualizations)
5. [🔎 Project Insights](#-project-insights)

---

## 🎯 Project Objective

Build a **weekly credit card dashboard** that tracks:

* Revenue trends 💰
* Customer demographics 👥
* Transaction behaviors 📈
* Performance by card type, geography, and more 🌍

---

## 📂 Dataset Description

We used **four datasets** containing 2023–2024 data:

1. **`credit_card`** 🏦

   * Rows: **10,109** | Columns: **18**
   * Contains weekly credit card details like `Client_Num`, `Card_Category`, `Annual_Fees`, `Activation_30_Days`, `Credit_Limit`, `Total_Trans_Amt`, `Interest_Earned`, etc.
   * Covers **52 weeks** (Jan 1, 2023 – Dec 24, 2023).

2. **`customer`** 👨‍👩‍👧

   * Rows: **10,109** | Columns: **15**
   * Includes customer info like `Age`, `Gender`, `Education_Level`, `Marital_Status`, `Income`, `Cust_Satisfaction_Score`.

3. **`cc_add`** ➕

   * Rows: **186** | Columns: **18**
   * Extension of the `credit_card` dataset (Post-week 52).

4. **`cust_add`** ➕

   * Rows: **186** | Columns: **15**
   * Extension of the `customer` dataset.

---

## ⚙️ Data Preparation

Steps followed to **clean and enrich the data** before visualization:

1. 📥 **Loaded Excel data** into **PostgreSQL** using SQL queries.
2. 🔗 Connected **PostgreSQL to Power BI Desktop** and imported datasets.
3. 🧮 Created calculated **columns & measures** in Power BI:

   ```DAX
   -- Revenue Calculation
   Revenue = 'public cc_detail'[annual_fees] 
           + 'public cc_detail'[total_trans_amt] 
           + 'public cc_detail'[interest_earned]
   ```

   ```DAX
   -- Week Number
   week_num2 = WEEKNUM('public cc_detail'[week_start_date])
   ```

   ```DAX
   -- Current Week Revenue
   Current_week_revenue =
   CALCULATE(
       SUM('public cc_detail'[Revenue]),
       FILTER(ALL('public cc_detail'),
       'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2]))
   )
   ```

   ```DAX
   -- Previous Week Revenue
   Previous_week_revenue =
   CALCULATE(
       SUM('public cc_detail'[Revenue]),
       FILTER(ALL('public cc_detail'),
       'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2]) - 1)
   )
   ```

   ```DAX
   -- Week-on-Week Revenue Change
   WOW_revenue =
   DIVIDE(([Current_week_revenue] - [Previous_week_revenue]), [Previous_week_revenue])
   ```

   ```DAX
   -- Age Group Buckets
   AgeGroup = SWITCH(
       TRUE(),
       'public cust_detail'[customer_age] < 30, "20-30",
       'public cust_detail'[customer_age] >= 30 && 'public cust_detail'[customer_age] < 40, "30-40",
       'public cust_detail'[customer_age] >= 40 && 'public cust_detail'[customer_age] < 50, "40-50",
       'public cust_detail'[customer_age] >= 50 && 'public cust_detail'[customer_age] < 60, "50-60",
       'public cust_detail'[customer_age] >= 60, "60+",
       "unknown"
   )
   ```

   ```DAX
   -- Income Group Buckets
   IncomeGroup = SWITCH(
       TRUE(),
       'public cust_detail'[income] < 35000, "Low",
       'public cust_detail'[income] >= 35000 && 'public cust_detail'[income] < 70000, "Medium",
       'public cust_detail'[income] >= 70000, "High",
       "unknown"
   )
   ```

---

## 📊 Visualizations

Two interactive **Power BI reports** were built:

### 🔹 Credit Card Transaction Report

📷 ![Credit Card Transaction Report](https://github.com/kuldeepmishra92/Credit-Card-Analytics-Dashboard-in-Power-BI/blob/main/Credit%20Card%20Transactions%20Report_67351301_1.jpg)

### 🔹 Credit Card Customer Report

📷 ![Credit Card Customer Report](https://github.com/kuldeepmishra92/Credit-Card-Analytics-Dashboard-in-Power-BI/blob/main/Credit%20Card%20Customer%20Report_67351359_1.jpg)

---

## 🔎 Project Insights

### 📅 Week-on-Week (WOW) Change:

* 📈 Revenue ↑ **28.8%**
* 💳 Total Transaction Amount ↑ **35.03%**
* 🔢 Transaction Count ↑ **3.3%**
* 👥 Customer Count ↑ **12.8%**

### 📊 Year-to-Date (YTD) Overview:

* 💰 Overall Revenue: **57M**
* 🏦 Total Interest Earned: **8M**
* 💳 Total Transaction Amount: **46M**
* 👨 Male Customers: **31M revenue** | 👩 Female Customers: **26M revenue**
* 🎴 Blue & Silver Cards = **93% of total transactions**
* 🌍 Top States: **TX, NY, CA = 68% of total revenue**
* ✅ Activation Rate: **57.5%**
* ⚠️ Delinquency Rate: **6.06%**
