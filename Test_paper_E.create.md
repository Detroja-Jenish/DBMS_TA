```sql
CREATE DATABASE TestPaper_E;
```
```sql
USE TestPaper_E;
```
```sql
-- Table-1: Restaurants
CREATE TABLE Restaurants (
    RestaurantID INT PRIMARY KEY,
    RestaurantName VARCHAR(100),
    Cuisine VARCHAR(50),
    Rating DECIMAL(2,1)
);
```
```sql
-- Table-2: Customers
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    CustomerName VARCHAR(100),
    City VARCHAR(50),
    Membership VARCHAR(20)
);
```
```sql
-- Table-3: Orders
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    RestaurantID INT,
    OrderDate DATETIME,
    TotalAmount DECIMAL(10,2),
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID),
    FOREIGN KEY (RestaurantID) REFERENCES Restaurants(RestaurantID)
);
```
```sql
-- Table-4: MenuItems
CREATE TABLE MenuItems (
    MenuItemID INT PRIMARY KEY,
    RestaurantID INT,
    ItemName VARCHAR(100),
    Price DECIMAL(10,2),
    Category VARCHAR(50),
    FOREIGN KEY (RestaurantID) REFERENCES Restaurants(RestaurantID)
);
```
```sql
-- Insert Restaurants
INSERT INTO Restaurants VALUES
(1, 'Le Gourmet', 'French', 4.8),
(2, 'Sakura House', 'Japanese', 4.6),
(3, 'Pasta Palace', 'Italian', 4.2),
(4, 'Taco Fiesta', 'Mexican', 3.9),
(6, 'Spice Hub', 'Indian', 4.7),
(7, 'Dragon Wok', 'Chinese', 4.3),
(8, 'Green Garden', 'Vegan', 4.0),
(9, 'BBQ Nation', 'Korean', 4.5),
(10, 'The Deli Spot', 'Sandwiches', 3.8);
```
```sql
-- Insert Customers
INSERT INTO Customers VALUES
(1, 'Alice Johnson', 'New York', 'Gold'),
(2, 'Bob Smith', 'Bombay', 'Silver'),
(3, 'Charlie Brown', 'Los Angeles', 'Bronze'),
(4, 'Diana Prince', 'New York', 'Gold'),
(5, 'Ethan Hunt', 'Miami', 'Silver'),
(6, 'Fiona Carter', 'Boston', 'Bronze'),
(7, 'George Miller', 'San Francisco', 'Gold'),
(8, 'Hannah Lee', 'New York', 'Silver'),
(9, 'Ian Thomas', 'Chicago', 'Gold'),
(10, 'Julia Kim', 'Los Angeles', 'Bronze');
```
```sql
-- Insert Orders
INSERT INTO Orders VALUES
(101, 1, 1, '2024-05-01 18:30:00', 120.50),
(102, 2, 2, '2024-05-03 20:00:00', 60.00),   -- amount = 60 (for update query)
(103, 3, 3, '2024-06-10 12:15:00', 85.00),
(104, 1, 2, '2024-07-01 19:45:00', 150.00),
(106, 6, 6, '2024-08-05 13:20:00', 95.00),
(107, 7, 7, '2024-08-10 19:00:00', 70.00),
(108, 8, 6, '2024-09-01 18:00:00', 110.00),
(110, 10, 9, '2024-09-15 20:30:00', 130.00),
(111, 1, 10, '2024-09-20 11:30:00', 55.00),
(112, 5, 7, '2024-10-02 18:40:00', 200.00),
(113, 4, 6, '2024-10-05 21:15:00', 175.00),
(114, 2, 9, '2024-10-07 12:10:00', 90.00),
(115, 3, 8, '2024-10-08 16:30:00', 60.00);
```
```sql
-- Insert MenuItems
INSERT INTO MenuItems VALUES
(201, 1, 'Escargot', 25.00, 'Appetizer'),
(202, 2, 'Sushi Roll', 15.00, 'Sushi'),
(203, 3, 'Spaghetti Carbonara', 18.50, 'Main Course'),
(204, 4, 'Tacos', 12.00, 'Main Course'),
(206, 6, 'Chicken Curry', 20.00, 'Main Course'),
(207, 6, 'Naan Bread', 5.00, 'Appetizer'),
(208, 7, 'Kung Pao Chicken', 16.00, 'Main Course'),
(209, 7, 'Spring Rolls', 8.00, 'Appetizer'),
(210, 8, 'Vegan Burger', 14.00, 'Main Course'),
(211, 8, 'Avocado Salad', 12.00, 'Appetizer'),
(212, 9, 'Korean BBQ Platter', 35.00, 'Main Course'),
(213, 9, 'Kimchi Soup', 10.00, 'Soup'),
(214, 10, 'Turkey Sandwich', 9.50, 'Main Course'),
(215, 10, 'Club Sandwich', 11.00, 'Main Course');
```