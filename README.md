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

#### 🔍 Step 1: Explore the data  

```sql
SELECT *
FROM order_details;
```
---

#### 🏗️ Step 2: Add a new column

I created a new column called new_order_date with the correct data type:
```sql
ALTER TABLE order_details
ADD COLUMN new_order_date DATE;
```
---

#### 🔄 Step 3: Convert and update the data

Then I filled this column by converting the existing order_date text into a proper date format:
```sql
UPDATE order_details
SET new_order_date = STR_TO_DATE(order_date, '%c/%e/%y');
```
---


#### ✅ Step 4: Verify the conversion 

I did a few random checks comparing the old and new columns to make sure the conversion worked correctly

---


#### 🧹 Step 5: Drop and rename columns

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

#### 🔄 Step 6: Repeat with order_time column

I did the same thing with order_time column:
```sql
ALTER TABLE order_details
ADD COLUMN new_order_time TIME;

UPDATE order_details
SET new_order_time = str_to_date(order_time, '%h:%i:%s %p');

ALTER TABLE order_details
DROP COLUMN order_time;

ALTER TABLE order_details
RENAME COLUMN new_order_time TO order_time;
```

![Clean data](https://github.com/hadishokri11/SQLProject-from-orders-to-insights/blob/main/1st%20Clean%20data.PNG?raw=true)
---


## Objective 1: 🍽️ Explore the `menu_items` table - Data Exploration – Understanding the Menu

After cleaning the order_details table, I moved on to exploring what’s on the new menu.
The menu_items table contains all the dishes offered at Taste of the World Café — including their categories, cuisines, and prices.

```sql
-- 1. View the menu_tem table.
SELECT * from menu_items;

-- 2. Find the number of items on the menu.
SELECT COUNT(*) from menu_items AS num_menu_items;
-- 32 menu items

-- 3. What are the TOP 3 least and most expensive items on the menu?
SELECT * from menu_items
ORDER BY price
LIMIT 3;
-- Edamame, Mac & Cheese, French Fries

SELECT * from menu_items
ORDER BY price DESC
LIMIT 3;
-- Shrimp Scampi, Korean Beef Bowl, Pork Ramen

-- 4. How many Italian dishes are on the menu?
SELECT category, COUNT(*) AS num_item
FROM menu_items
WHERE category = "Italian";
-- 9 dishes

-- 5. What are the TOP 3 least and most expensive Italian dishes on the menu?
-- least expansive
SELECT * from menu_items
WHERE category = "Italian"
ORDER BY price
LIMIT 3;
-- Spaghetti, Fettuccine Alfredo, Cheese Lasagna

-- most expensive
SELECT * from menu_items
WHERE category = "Italian"
ORDER BY price DESC
LIMIT 3;
-- Shrimp Scampi, Spaghetti & Meatballs, Meat Lasagna

-- 6. How many dishes are in each category?
SELECT category, count(item_name) AS num_dishes
FROM menu_items
GROUP BY category;
-- American: 6, Asian: 8, Mexican: 9,Italian: 9

-- 7. What is the average dish price within each category?
SELECT category, AVG(price) AS avg_price 
FROM menu_items
GROUP BY category
ORDER BY avg_price;
-- American: 10.07, Mexican: 11.8, Asian: 13.48, Italian: 16.75
```
💡 These queries give a quick but powerful overview of the restaurant’s new menu — highlighting pricing patterns, cuisine variety, and category-level insights.

---

## Objective 2: Explore the `order_details` table - understand what kind of data has been collected

```sql
-- 1. View the order_details table.
SELECT * 
FROM order_details
LIMIT 10;

-- 2. What is the date range of the table?
SELECT MIN(order_date), MAX(order_date)
FROM order_details;

-- 3. How many orders were made within this date range?
SELECT COUNT(DISTINCT order_id)
FROM order_details;

-- 4. How many items were ordered within this date range?
SELECT COUNT(*)
FROM order_details;

-- 5. Which orders had the most number of items?
SELECT order_id, COUNT(*) AS num_items
FROM order_details
GROUP BY order_id
ORDER BY num_items DESC;

-- 6. How many orders had more than 12 items?
SELECT COUNT(*) AS orders FROM

(SELECT order_id, COUNT(*) AS num_items
FROM order_details
GROUP BY order_id
HAVING num_items > 12
ORDER BY num_items DESC) AS num_orders;
```

After all the questions have been aswered, I tinker around the data to find out if I can summarize the data into a quick summary table:
```sql
SELECT 
COUNT(DISTINCT order_id) AS total_orders,
COUNT(*) AS total_items,
MIN(order_date) AS start_date,
MAX(order_date) AS end_date
FROM order_details;
```

I also want to find out the trend of customers by date or over time:
```sql
SELECT order_date, COUNT(DISTINCT order_id) AS total_orders
FROM order_details
GROUP BY order_date
ORDER BY order_date;
```

