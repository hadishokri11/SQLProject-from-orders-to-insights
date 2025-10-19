# SQLProject-from-orders-to-insights 🍽️
A hands-on SQL project for practicing data cleaning, joins, and analysis using restaurant order data.

## 🧠 The Situation  
You've just been hired as a **Data Analyst** for **Taste of the World Café**, a restaurant known for its diverse menu offerings and generous portions.  
The café recently rolled out a brand-new menu at the start of the year — and management wants to know how things are going.  

My job? To dig into the data and uncover what’s working, what’s not, and what customers really love most.  

---

## 📋 The Assignment  
The Taste of the World Café debuted a new menu, and now it’s time to measure its impact.  
I’ve been asked to dive into the customer order data to answer key questions like:  

- Which menu items are performing well (and which are not)?  
- Who are the top customers?  
- What are their favorite dishes?  

---

## 🎯 The Objectives  
1. **Explore the `menu_items` table** to get an overview of what’s on the new menu.  
2. **Explore the `order_details` table** to understand what kind of data has been collected.  
3. **Combine both tables** to see how customers are reacting to the new menu items — what’s selling, what’s not, and who’s ordering what.  

---

## 🧩 Tools Used  
- **SQL** for querying and analysis  
- **MySQL**
- **YouTube Project Reference:** *Data Analyst Portfolio Project – Restaurant Order Analysis in SQL (with Solutions)*  

---

## 🧹 Data Cleaning Process  

When I first connected to MySQL, I created a new connection named **`restaurant_db`** and imported three CSV files:
- `menu_items`
- `order_details`
- `restaurant_db_data_dictionary`

I used the **Table Data Import Wizard** for this process.  

As I began exploring each table, I noticed something odd in the `order_details` table —  
the **date** and **time** columns were imported as **text** instead of proper `DATE` and `TIME` data types.  

At first, I thought of deleting the table and re-importing it.  
But then I realized this was a great chance to **practice some SQL data cleaning** 🧠.  

Here’s how I went about it:

---

### 🔍 Step 1: Explore the data  

```sql
SELECT *
FROM order_details;
```
---

### 🏗️ Step 2: Add a new column

I created a new column called new_order_date with the correct data type:
```sql
ALTER TABLE order_details
ADD COLUMN new_order_date DATE;
```
---

### 🔄 Step 3: Convert and update the data

Then I filled this column by converting the existing order_date text into a proper date format:
```sql
UPDATE order_details
SET new_order_date = STR_TO_DATE(order_date, '%c/%e/%y');
```
---

### ✅ Step 4: Verify the conversion
I did a few random checks comparing the old and new columns to make sure the conversion worked correctly.
---

### 🧹 Step 5: Drop and rename columns

Once everything looked good, I replaced the old column with the cleaned one:
```sql
ALTER TABLE order_details
DROP COLUMN order_date;

ALTER TABLE order_details
RENAME COLUMN new_order_date TO order_date;
```
This small mistake turned into a valuable data cleaning exercise —
A reminder that not every dataset comes clean, and sometimes the best learning happens when you fix the mess yourself

---

