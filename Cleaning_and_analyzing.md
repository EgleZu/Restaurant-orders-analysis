> ### CHECKING AND CLEANING DATA
>
> Data is consistent except from missing values in order_details table.
> 
> 1. Checking for null values in each column from order_details table:
```sql
SELECT 
    COUNT(*) AS total_rows,
    COUNT(order_details_id) AS order_details_id_non_null,
    COUNT(order_id) AS order_id_non_null,
    COUNT(order_date) AS order_date_non_null,
    COUNT(order_time) AS order_time_non_null,
    COUNT(item_id) AS item_id_non_null
FROM
    restaurant_db.order_details;
```
> ![image](https://github.com/user-attachments/assets/96d92f09-b209-4306-8eaa-415649fbfa81)
>
> Only item_id column has null values.
>
> 2. Checking how many rows are missing values:
```sql
SELECT 
    COUNT(*) AS rows_with_null_value
FROM
    restaurant_db.order_details
WHERE
    item_id IS NULL;
```
> ![image](https://github.com/user-attachments/assets/0dc2901a-0c1b-44d0-85fc-4838f735bba5)
>
> 3. Finding rows with missing values:
```sql
SELECT *
FROM
    restaurant_db.order_details
WHERE
    item_id IS NULL;
```
> ![image](https://github.com/user-attachments/assets/c80ea871-41ed-48b6-b6e0-f1c6ccabbef3)
> <br>... <br>
> Rows with missing values in item_id column will not be relevant to this specific project.
> 
> 4. Creating a temporary table 
> (to keep original data, if further investigation on missing data will be performed):
```sql
DROP TABLE IF EXISTS orders;
CREATE TEMPORARY TABLE orders 
SELECT * FROM restaurant_db.order_details 
WHERE item_id IS NOT NULL;
```
> 5. Checking data period:
```sql
SELECT 
    MIN(order_date) AS beginning, MAX(order_date) AS end
FROM
    restaurant_db.orders;
```
> ![image](https://github.com/user-attachments/assets/cf20219c-b0e6-4f6c-a1bd-d7ff8457bce8)
>
> 6. Calculating total number of orders:
```sql
SELECT 
    COUNT(DISTINCT order_id) AS total_orders
FROM
    restaurant_db.orders;
```
> ![image](https://github.com/user-attachments/assets/681a5e1c-9b20-4586-9962-4bdbc29faf1f)
>
> 7. Calculating total number of items ordered:
```sql
SELECT 
    COUNT(*) AS total_items_ordered
FROM
    restaurant_db.orders;
```
> ![image](https://github.com/user-attachments/assets/c1f13478-a7d7-4b9f-a63c-e31f0a759b72)
>
> 8. Checking how many items are in the menu:
```sql
SELECT 
    COUNT(*) AS menu_items
FROM
    restaurant_db.menu_items;
```
> ![image](https://github.com/user-attachments/assets/2aba9234-52c5-41c3-a245-3b8dfc496b20)
>
> 9. Checking how many categories are in the menu;
> calculating how many meals are in each menu category:
```sql
SELECT 
    category, COUNT(item_name) AS menu_items
FROM
    restaurant_db.menu_items
GROUP BY category;
```
> ![image](https://github.com/user-attachments/assets/13353493-025c-4704-a3c0-fad9d7f33043)
>
>


> ### ANALYSING DATA
> 1. Categories by orders:
```sql
SELECT 
    t2.category, COUNT(t1.item_id) AS times_ordered
FROM
    restaurant_db.orders t1
        LEFT JOIN
    restaurant_db.menu_items t2 ON t2.menu_item_id = t1.item_id
GROUP BY t2.category
ORDER BY times_ordered DESC;
```
> ![image](https://github.com/user-attachments/assets/6b45201e-4006-46e6-be4f-54c7ba3578db)
>
> 2. Categories by revenue:
```sql
SELECT 
    t2.category, SUM(price) AS revenue
FROM
    restaurant_db.orders t1
        LEFT JOIN
    restaurant_db.menu_items t2 ON t2.menu_item_id = t1.item_id
GROUP BY t2.category
ORDER BY revenue DESC;
```
> ![image](https://github.com/user-attachments/assets/0c19bd02-bd7c-4bd6-b27f-3128221e8913)
> 
> 3. Average meal price by category:
```sql
SELECT 
    category, ROUND(AVG(price), 2) AS average_price
FROM
    restaurant_db.menu_items
GROUP BY category
ORDER BY average_price DESC;
```
> ![image](https://github.com/user-attachments/assets/5374fa97-cd6e-46d9-98a6-f6ec0c68eca4)
>
> 4. Total average price and price range:
```sql
SELECT 
    ROUND(AVG(price), 2) AS average_price,
    MIN(price) AS lowest_price,
    MAX(price) AS highest_price
FROM
    restaurant_db.menu_items;
```
> ![image](https://github.com/user-attachments/assets/19bcb075-5327-423c-bc24-86738b88118b)
> 
> 5. Menu items in category by price range:
```sql
SELECT 
    category,
    COUNT(item_name) AS menu_items,
    CASE
        WHEN price > 15 THEN 'higher'
        WHEN price < 10 THEN 'lower'
        ELSE 'mid-range'
    END AS price_range
FROM
    restaurant_db.menu_items
GROUP BY category , price_range
ORDER BY category , menu_items;
```
> ![image](https://github.com/user-attachments/assets/04f57f23-f36f-4c35-acaa-9075451f9e46)
> 
> 6. Top 5 meals by revenue:
```sql
SELECT 
    t2.item_name, t2.category, SUM(price) AS revenue
FROM
    restaurant_db.orders t1
        LEFT JOIN
    restaurant_db.menu_items t2 ON t2.menu_item_id = t1.item_id
GROUP BY t2.item_name , t2.category
ORDER BY revenue DESC
LIMIT 5;
```
> ![image](https://github.com/user-attachments/assets/412b560a-37cc-4f88-9a3d-7c72014a2f41)
>
> 7. Top 5 meals by orders:
```sql
SELECT 
    t2.item_name,
    t2.category,
    COUNT(t1.item_id) AS times_ordered
FROM
    restaurant_db.orders t1
        LEFT JOIN
    restaurant_db.menu_items t2 ON t2.menu_item_id = t1.item_id
GROUP BY t2.item_name , t2.category
ORDER BY times_ordered DESC
LIMIT 5;
```
> ![image](https://github.com/user-attachments/assets/41abfb09-d0e3-48cf-941e-7d019ab2b6bf)
>
> 8. Bottom 5 meals by revenue:
```sql
SELECT 
    t2.item_name, t2.category, SUM(price) AS revenue
FROM
    restaurant_db.orders t1
        LEFT JOIN
    restaurant_db.menu_items t2 ON t2.menu_item_id = t1.item_id
GROUP BY t2.item_name , t2.category
ORDER BY revenue
LIMIT 5;
```
> ![image](https://github.com/user-attachments/assets/4e14233d-a7cd-440f-8c73-46cb3de52d4b)
>
> 9. Bottom 5 meals by orders:
```sql
SELECT 
    t2.item_name,
    t2.category,
    COUNT(t1.item_id) AS times_ordered
FROM
    restaurant_db.orders t1
        LEFT JOIN
    restaurant_db.menu_items t2 ON t2.menu_item_id = t1.item_id
GROUP BY t2.item_name , t2.category
ORDER BY times_ordered
LIMIT 5;
```
> ![image](https://github.com/user-attachments/assets/a54bbb93-7f0e-44ac-847b-ff2ae558ff39)
>
> 10. Burgers and bowls in the menu:
```sql
SELECT 
    item_name
FROM
    restaurant_db.menu_items
WHERE
    item_name LIKE '%bowl%'
        OR item_name LIKE '%burger%';
```
> ![image](https://github.com/user-attachments/assets/840252e1-eef4-4ffa-ac2e-c66798c61162)
>
> 11. Lowest, highest and average order value:
```sql
SELECT 
    MIN(order_value) AS min_order_value,
    MAX(order_value) AS max_order_value,
    ROUND(SUM(order_value) / COUNT(order_id), 2) AS avg_order_value
FROM
    (SELECT 
        t1.order_id, SUM(t2.price) AS order_value
    FROM
        restaurant_db.orders t1
    LEFT JOIN restaurant_db.menu_items t2 ON t2.menu_item_id = t1.item_id
    GROUP BY t1.order_id) AS order_totals;
```
> ![image](https://github.com/user-attachments/assets/aa8e2fe9-82f2-4b7e-9fc4-a9965ba5531f)
>
> 12. Highest spend orders:
```sql
SELECT 
    t1.order_id,
    SUM(t2.price) AS order_value,
    COUNT(t1.order_details_id) AS items_ordered
FROM
    restaurant_db.orders t1
        LEFT JOIN
    restaurant_db.menu_items t2 ON t2.menu_item_id = t1.item_id
GROUP BY t1.order_id
HAVING order_value > 170
ORDER BY order_value DESC;
```
> ![image](https://github.com/user-attachments/assets/0f8b126a-dc7a-430d-8cfe-1e7536f72970)
>
> 13. Categories by orders of highest spend orders (440, 2075, 1957, 330, 2675):
```sql
SELECT 
    t2.category, COUNT(t1.item_id) AS items_ordered
FROM
    restaurant_db.orders t1
        LEFT JOIN
    restaurant_db.menu_items t2 ON t2.menu_item_id = t1.item_id
WHERE
    t1.order_id IN (440, 2075, 1957, 330, 2675)
GROUP BY t2.category
ORDER BY items_ordered DESC;
```
> ![image](https://github.com/user-attachments/assets/d66eff80-4c73-449b-8dad-121ce0eadbd6)
>
> 14. From most to least popular hour to order:
```sql
SELECT 
    HOUR(order_time) AS order_hour,
    COUNT(order_id) AS total_orders
FROM
    restaurant_db.orders
GROUP BY order_hour
ORDER BY total_orders DESC;
```
> ![image](https://github.com/user-attachments/assets/2c8869c5-e7fa-4ba3-90e5-a57b1d105c6e)
>
> 15. Top 5 meals during peak hours (12 and 13):
```sql
SELECT 
    t2.item_name, COUNT(t1.item_id) AS total_orders
FROM
    restaurant_db.orders t1
        LEFT JOIN
    restaurant_db.menu_items t2 ON t2.menu_item_id = t1.item_id
WHERE
    HOUR(t1.order_time) IN (12, 13)
GROUP BY t2.item_name
ORDER BY total_orders DESC
LIMIT 5;
```
> ![image](https://github.com/user-attachments/assets/3c1011a2-f45c-4b2c-b6fb-62d053f64f9d)
>
 
