# Sql_sales_inventory_project
This project simulates a simple retail business database system using SQL. It is designed to efficiently manage customers, products, orders, and inventory. The primary goal is to provide a structured way to store and retrieve business data, generate reports, and perform analytical queries that help in business decision-making.
-- Customers Table
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    phone VARCHAR(15),
    city VARCHAR(50)
);

-- Products Table
CREATE TABLE Products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10,2),
    stock_quantity INT
);

-- Orders Table
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    total_amount DECIMAL(10,2),
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);

-- OrderDetails Table
CREATE TABLE OrderDetails (
    order_detail_id INT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT,
    subtotal DECIMAL(10,2),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);

INSERT INTO Customers VALUES (1, 'John Doe', 'john@example.com', '1234567890', 'New York');
INSERT INTO Products VALUES (101, 'Laptop', 'Electronics', 65000.00, 20);
INSERT INTO Orders VALUES (5001, 1, '2025-04-20', 65000.00);
INSERT INTO OrderDetails VALUES (1, 5001, 101, 1, 65000.00);
-- More Customers
INSERT INTO Customers VALUES 
(2, 'Alice Smith', 'alice@example.com', '9990001111', 'Chicago'),
(3, 'Bob Johnson', 'bob@example.com', '8881112222', 'Los Angeles'),
(4, 'Nina Brown', 'nina@example.com', '7773334444', 'Houston');

-- More Products
INSERT INTO Products VALUES
(102, 'Smartphone', 'Electronics', 25000.00, 50),
(103, 'Office Chair', 'Furniture', 4000.00, 35),
(104, 'Wireless Mouse', 'Electronics', 700.00, 100),
(105, 'Desk Lamp', 'Furniture', 1200.00, 80);

-- More Orders
INSERT INTO Orders VALUES
(5002, 2, '2025-04-22', 29000.00),
(5003, 3, '2025-04-23', 4700.00),
(5004, 4, '2025-04-24', 1900.00);

-- Order Details
INSERT INTO OrderDetails VALUES
(2, 5002, 102, 1, 25000.00),
(3, 5002, 104, 2, 1400.00),
(4, 5003, 103, 1, 4000.00),
(5, 5003, 104, 1, 700.00),
(6, 5004, 105, 1, 1200.00),
(7, 5004, 104, 1, 700.00);


-- Total sales by product
SELECT p.product_name, SUM(od.subtotal) AS total_sales
FROM Products p
JOIN OrderDetails od ON p.product_id = od.product_id
GROUP BY p.product_name;

-- Top customers by total amount spent
SELECT c.name, SUM(o.total_amount) AS total_spent
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.name
ORDER BY total_spent DESC;

-- Current inventory status
SELECT product_name, stock_quantity
FROM Products
ORDER BY stock_quantity ASC;

--Most Sold Products
SELECT p.product_name, SUM(od.quantity) AS total_quantity_sold
FROM Products p
JOIN OrderDetails od ON p.product_id = od.product_id
GROUP BY p.product_name
ORDER BY total_quantity_sold DESC;


--Monthly sales Report

SELECT DATE_FORMAT(order_date, '%Y-%m') AS month, SUM(total_amount) AS total_sales
FROM Orders
GROUP BY month
ORDER BY month;

--City wise Customer count

SELECT city, COUNT(*) AS number_of_customers
FROM Customers
GROUP BY city;

