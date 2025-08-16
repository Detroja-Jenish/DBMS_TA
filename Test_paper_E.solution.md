```sql
-- 1. Display unique city of customers who have 'gold' membership.
SELECT
    DISTINCT Customers.City
FROM Customers
WHERE Customers.Membership = 'gold'
```
|City|
|:---|
|Chicago|
|New York|
|San Francisco|

```sql
-- 2. Display top 2 rating with restaurant names.
SELECT
    TOP 2
    Restaurants.RestaurantName,
    Restaurants.Rating
FROM Restaurants
ORDER BY Restaurants.Rating DESC
```
|RestaurantName|Rating|
|:-----|-----:|
|Le Gourmet|4.8|
|Spice Hub|4.7|

```sql
-- 3. Insert new restaurant in restaurant table. (5, 'Burger Junction', 'American', 4.1)
INSERT INTO Restaurants 
    (Restaurants.RestaurantID, Restaurants.RestaurantName, Restaurants.Cuisine, Restaurants.Rating)
    VALUES 
    (5, 'Burger Junction', 'American', 4.1);
```

```sql
-- 4. Update customer id to 4 in orders table where amount is 60.
UPDATE Orders
SET Orders.CustomerID = 4
WHERE Orders.TotalAmount = 60;
```

```sql
-- 5. Remove the costumer who belongs to Chicago city.
BEGIN TRANSACTION;
DELETE FROM Customers
WHERE Customers.City = 'Chicago';
ROLLBACK TRANSACTION;
```

```sql
-- 6. Change column name Total Amount to Amount in Orders table.
EXEC sp_rename 'Orders.TotalAmount',  'Amount', 'COLUMN';
```

```sql
-- 7. Delete Menu Items table.
BEGIN TRANSACTION;
DROP TABLE MenuItems;
ROLLBACK TRANSACTION;
```

```sql
-- 8. Display name and city of those customers whose membership contains 4 letters.
SELECT 
    Customers.CustomerName, 
    Customers.City
FROM Customers
WHERE LEN(Customers.Membership) = 4;
```
|CustomerName|City|
|:--------|:--------|
|Alice Johnson|New York|
|Diana Prince|New York|
|George Miller|San Francisco|
|Ian Thomas|Chicago|

```sql
-- 9. Display 3rd to 7th character of restaurant name from restaurants table.
SELECT 
    SUBSTRING(Restaurants.RestaurantName, 3, 5) AS [3rd to 7th character of restaurant name]
FROM Restaurants;
```
|3rd to 7th character of restaurant name|
|:--------------------------------------|
| Gour|
|kura |
|sta P|
|co Fi|
|rger |
|ice H|
|agon |
|een G|
|Q Nat|
|e Del|

```sql
-- 10. Write a query to subtract 1 year from current date.
SELECT DATEADD(YEAR, -1,GETDATE())
```

```
-- 11. Find max amount of all orders.
SELECT 
    MAX(Orders.Amount) AS MaxAmount
FROM Orders;
```

|MaxAmount|
|:--------|
|200.00|

```sql
-- 12. Display city with the total number of customers.
SELECT 
    Customers.City,
    COUNT(Customers.CustomerID) AS TotalCustomers
FROM Customers
GROUP BY Customers.City;
```
|City|TotalCustomers|
|:---|:-------------|
|Bombay|1|
|Boston|1|
|Chicago|1|
|Los Angeles|2|
|Miami|1|
|New York|3|
|San Francisco|1|

```sql
-- 13. display restaurant names with average rating greater than 4.5.
SELECT Restaurants.RestaurantName, Restaurants.Rating
FROM Restaurants
WHERE Restaurants.Rating > 4.5;
```
|RestaurantName|Rating|
|:------|------:|
|Le Gourmet|4.8|
|Sakura House|4.6|
|Spice Hub|4.7|

```sql
-- 14. Find the highest-rated restaurant and its details.
SELECT 
    Restaurants.RestaurantID,
    Restaurants.RestaurantName,
    Restaurants.Cuisine,
    Restaurants.Rating
FROM Restaurants
WHERE Restaurants.Rating = (
    SELECT 
        MAX(Restaurants.Rating)
    FROM Restaurants
);
```
|RestaurantID|RestaurantName|Cuisine|Rating|
|:---------:|:---------|:---------|---------:|
|1|Le Gourmet|French|4.8|

```sql
-- 15. Subquery to get restaurants that are visited by customers from 'New York'.
SELECT 
    Restaurants.RestaurantID,
    Restaurants.RestaurantName,
    Restaurants.Cuisine,
    Restaurants.Rating
FROM Restaurants
WHERE Restaurants.RestaurantID IN (
    SELECT Orders.RestaurantID
    FROM Orders
    JOIN Customers
    ON Orders.CustomerID = Customers.CustomerID
    WHERE Customers.City = 'New York'
);
```
|RestaurantID|RestaurantName|Cuisine|Rating|
|:---------:|:---------|:---------|---------:|
|1|Le Gourmet|French|4.8|
|2|Sakura House|Japanese|4.6|
|6|Spice Hub|Indian|4.7|
|8|Green Garden|Vegan|4.0|
|10|The Deli Spot|Sandwiches|3.8|

```sql
-- 16. Create a View to list customers and their cities.
CREATE VIEW CustomerCityView AS
SELECT 
    Customers.CustomerName,
    Customers.City
FROM Customers;
```

```sql
-- 17. Get all customers and their orders (including customers without order)
SELECT Customers.CustomerID,
       Customers.CustomerName,
       Orders.OrderID,
       Orders.RestaurantID,
       Orders.Amount
FROM Customers
LEFT JOIN Orders
    ON Customers.CustomerID = Orders.CustomerID;
```
|CustomerID|CustomerName|OrderID|RestaurantID|Amount|
|:-------:|:-----------|:-------:|:-------:|-------:|
|1|Alice Johnson|101|1|120.50|
|1|Alice Johnson|104|2|150.00|
|1|Alice Johnson|111|10|55.00|
|2|Bob Smith|114|9|90.00|
|3|Charlie Brown|103|3|85.00|
|4|Diana Prince|102|2|60.00|
|4|Diana Prince|113|6|175.00|
|4|Diana Prince|115|8|60.00|
|5|Ethan Hunt|112|7|200.00|
|6|Fiona Carter|106|6|95.00|
|7|George Miller|107|7|70.00|
|8|Hannah Lee|108|6|110.00|
|9|Ian Thomas|NULL|NULL|NULL|
|10|Julia Kim|110|9|130.00|

```sql
-- 18. Generate a combination of every customer with every restaurant.
SELECT Customers.CustomerID,
       Customers.CustomerName,
       Restaurants.RestaurantID,
       Restaurants.RestaurantName
FROM Customers
CROSS JOIN Restaurants;
```

```sql
-- 19. List all restaurants and the corresponding orders.
SELECT 
    Restaurants.RestaurantID,
    Restaurants.RestaurantName,
    Orders.OrderID,
    Orders.CustomerID,
    Orders.Amount,
    Orders.OrderDate
FROM Restaurants
LEFT JOIN Orders
ON Restaurants.RestaurantID = Orders.RestaurantID;
```
|RestaurantID|RestaurantName|OrderID|CustomerID|Amount|OrderDate|
|:-------:|:-------|:-------:|:-------:|-------:|:-------|
|1|Le Gourmet|101|1120.50|2024-05-01 18:30:00.000|
|2|Sakura House|102|460.00|2024-05-03 20:00:00.000|
|2|Sakura House|104|1150.00|2024-07-01 19:45:00.000|
|3|Pasta Palace|103|385.00|2024-06-10 12:15:00.000|
|4|Taco Fiesta|NULL|NULL|NULL|NULL|
|5|Burger Junction|NULL|NULLNULL|NULL|
|6|Spice Hub|106|695.00|2024-08-05 13:20:00.000|
|6|Spice Hub|108|8110.00|2024-09-01 18:00:00.000|
|6|Spice Hub|113|4175.00|2024-10-05 21:15:00.000|
|7|Dragon Wok|107|770.00|2024-08-10 19:00:00.000|
|7|Dragon Wok|112|5200.00|2024-10-02 18:40:00.000|
|8|Green Garden|115|460.00|2024-10-08 16:30:00.000|
|9|BBQ Nation|110|10130.00|2024-09-15 20:30:00.000|
|9|BBQ Nation|114|290.00|2024-10-07 12:10:00.000|
|10|The Deli Spot|111|155.00|2024-09-20 11:30:00.000|

```sql
-- 20. Get the total amount spent by each customer at each restaurant, along with the customer and
-- restaurant names. Include customers who have not ordered from certain restaurants, showing
-- NULL for those cases.
SELECT
    Customers.CustomerName,
    Restaurants.RestaurantName,
    SUM(Orders.Amount) as TotalSpent
FROM Customers
LEFT JOIN Orders
ON Orders.CustomerID = Customers.CustomerID
LEFT JOIN Restaurants
ON Restaurants.RestaurantID = Orders.RestaurantID
GROUP BY Customers.CustomerName,
   Restaurants.RestaurantName;
```
|CustomerName|RestaurantName|TotalSpent|
|:-----------|:-----------|-----------:|
|Ian Thomas|NULL|NULL|
|Bob Smith|BBQ Nation|90.00|
|Julia Kim|BBQ Nation|130.00|
|Ethan Hunt|Dragon Wok|200.00|
|George Miller|Dragon Wok|70.00|
|Diana Prince|Green Garden|60.00|
|Alice Johnson|Le Gourmet|120.50|
|Charlie Brown|Pasta Palace|85.00|
|Alice Johnson|Sakura House|150.00|
|Diana Prince|Sakura House|60.00|
|Diana Prince|Spice Hub|175.00|
|Fiona Carter|Spice Hub|95.00|
|Hannah Lee|Spice Hub|110.00|
|Alice Johnson|he Deli Spot|55.00|