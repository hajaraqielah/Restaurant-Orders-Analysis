# 🧑‍🍳 Restaurant Orders Analysis

📌 This [dataset](https://mavenanalytics.io/data-playground/restaurant-orders) was retrieved from Maven Analytics's Data Playground.
This analysis will be divided into 3 parts namely as Objective. 

📝 Questions & Solutions 

Objective 1: Exploring the menu table

Q1. View menu_items and write a query to find number of items on the menu.

```sql
SELECT COUNT(*)
FROM menu_items
```

<img width="101" height="62" alt="image" src="https://github.com/user-attachments/assets/e20e812b-ddd8-4f97-bac4-5e665a49d31c" />

There are 32 items available on the menu.

Q2. What are the least and the most expensive items on the menu? 

```sql
SELECT *
FROM menu_items
ORDER BY price ASC;
```

<img width="495" height="60" alt="image" src="https://github.com/user-attachments/assets/52738af9-4e38-4e12-8399-3f9b546bfa26" />

```sql
SELECT *
FROM menu_items
ORDER BY price DESC;
```

<img width="496" height="52" alt="image" src="https://github.com/user-attachments/assets/0d182fc2-987e-4de7-842e-3df3f02e4222" />

The least expensive item is Edamame (5.00) while the most expensive is Shrimp Scampi (19.95).

Q3. How many Italian dishes are on the menu?

```sql
SELECT COUNT(*)
FROM menu_items
WHERE category = 'Italian';
```

<img width="97" height="53" alt="image" src="https://github.com/user-attachments/assets/73a33858-0c46-443a-aae4-ce144fd83075" />

There are 9 Italian dishes on the menu.

Q4. What are the least and the most expensive Italian items on the menu? 

```sql
SELECT *
FROM menu_items
WHERE category = 'Italian'
ORDER BY price ASC;
```

<img width="477" height="303" alt="image" src="https://github.com/user-attachments/assets/16005585-83bf-450d-9d1d-30aa83108fe8" />

The least expensive Italian dish is Spaghetti while the most expensive is Shrimp Scampi.

Q5. How many dishes are in each category?

```sql
SELECT category, COUNT(menu_item_id) AS num_dishes
FROM menu_items
GROUP BY category;
```

<img width="198" height="146" alt="image" src="https://github.com/user-attachments/assets/c220ded3-6224-4449-ad6d-679938b5f37d" />

Q6. What are the average prices of dishes in every categories?

```sql
SELECT category, AVG(price) AS avg_dishes_price
FROM menu_items
GROUP BY category;
```

<img width="270" height="147" alt="image" src="https://github.com/user-attachments/assets/6c78ab1d-b12c-407f-a708-23bd436b2fc8" />



Objective 2: Exploring Order Table

Q1. View the order_details table. What is the date range of the table?

```sql
SELECT MIN(order_date), MAX(order_date) 
FROM order_details;
```

<img width="277" height="53" alt="image" src="https://github.com/user-attachments/assets/39424565-d88f-4a56-b4fe-57c21136e012" />

Q2. How many orders were made within this date range? 

```sql
SELECT COUNT(DISTINCT order_id)
FROM order_details;
```

<img width="222" height="61" alt="image" src="https://github.com/user-attachments/assets/e5bfe652-3ed0-4006-804c-a2cf04953948" />

Q3. How many items were ordered within this date range?

```sql
SELECT COUNT(*)
FROM order_details;
```

<img width="97" height="53" alt="image" src="https://github.com/user-attachments/assets/0bc10901-8910-4f4f-aae6-9446fd5430bc" />

Q4. Which orders had the most number of items?

```sql
SELECT order_id, COUNT(item_id) AS num_of_items
FROM order_details
GROUP BY order_id
ORDER BY num_of_items DESC;
```

<img width="222" height="115" alt="image" src="https://github.com/user-attachments/assets/aebf75dd-035a-4dcf-ae60-022ec5f35925" />

Q5. How many orders had more than 12 items?

```sql
SELECT COUNT(*) FROM 

(SELECT order_id, COUNT(item_id) AS num_of_items
FROM order_details
GROUP BY order_id
HAVING num_of_items > 12) AS num_orders;
```

<img width="98" height="50" alt="image" src="https://github.com/user-attachments/assets/177ff74b-d35e-44c3-a5c1-a0bce9684f0f" />


Objective 3: Analyse Customer Behaviour

Q1. Combine the menu_items and order_details table into a single table.

```sql
SELECT * FROM menu_items;
SELECT * FROM order_details;

SELECT * 
FROM order_details 
LEFT JOIN menu_items 
ON order_details.item_id = menu_items.menu_item_id;
```

<img width="982" height="306" alt="image" src="https://github.com/user-attachments/assets/12bcb5fa-6f80-4b85-9013-c1cc500d5b95" />

Q2. What were the least and most ordered items? Which categories were they in? 

```sql
SELECT item_name, category,  COUNT(order_details_id) AS num_of_purchases 
FROM order_details 
LEFT JOIN menu_items 
ON order_details.item_id = menu_items.menu_item_id
GROUP BY item_name, category
ORDER BY num_of_purchases ASC;
```

<img width="473" height="83" alt="image" src="https://github.com/user-attachments/assets/645861e2-7564-41a3-ac6b-b8a4e18bc45d" />


```sql
SELECT item_name, category, COUNT(order_details_id) AS num_of_purchases 
FROM order_details 
LEFT JOIN menu_items 
ON order_details.item_id = menu_items.menu_item_id
GROUP BY item_name, category
ORDER BY num_of_purchases DESC;
```

<img width="467" height="87" alt="image" src="https://github.com/user-attachments/assets/2d31435d-5206-472b-8981-63420771b403" />
    
Q3. What were the top 5 orders that spent the most money?

```sql
SELECT order_id, SUM(price) AS total_spent
FROM order_details 
LEFT JOIN menu_items 
ON order_details.item_id = menu_items.menu_item_id
GROUP BY order_id
ORDER BY total_spent DESC
LIMIT 5;
```

<img width="201" height="186" alt="image" src="https://github.com/user-attachments/assets/c331320f-f6ed-40e0-8d90-c957680cab2d" />

Q4. View the details of the highest spend order. What insights could you gather from the results?

```sql
SELECT category, COUNT(item_id) AS num_items
FROM order_details 
LEFT JOIN menu_items 
ON order_details.item_id = menu_items.menu_item_id
WHERE order_id = 440
GROUP BY category;
```

<img width="191" height="148" alt="image" src="https://github.com/user-attachments/assets/649f3686-75ea-4c08-b968-51e02e2cc849" />

Q5.  View the details of the 5 highest spend order. What insights could you gather from the results?

```sql
SELECT category, COUNT(item_id) AS num_items
FROM order_details 
LEFT JOIN menu_items 
ON order_details.item_id = menu_items.menu_item_id
WHERE order_id IN (440, 2075, 1957, 330, 2675)
GROUP BY category;
```

<img width="193" height="147" alt="image" src="https://github.com/user-attachments/assets/2bd78880-7ac2-41f9-afd2-ee703c80c12a" />
