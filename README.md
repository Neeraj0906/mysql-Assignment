# mysql-Assignment

**Create the database and tables**

CREATE DATABASE ecommerce; -- Create the ecommerce database

USE ecommerce;  -- Use the ecommerce database

-- Create the customers table
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255),
    email VARCHAR(255),
    address VARCHAR(255)
);

-- Create the orders table
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    total_amount DECIMAL(10, 2),
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);

-- Create the products table
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255),
    price DECIMAL(10, 2),
    description TEXT
);

-- Insert data into customers table
INSERT INTO customers (name, email, address)
VALUES ('John Doe', 'john@example.com', '123 Main St'),
       ('Jane Smith', 'jane@example.com', '456 Maple Ave'),
       ('Alice Brown', 'alice@example.com', '789 Oak Rd');

-- Insert data into orders table
INSERT INTO orders (customer_id, order_date, total_amount)
VALUES (1, '2024-09-15', 100.00),
       (2, '2024-09-18', 150.00),
       (1, '2024-10-01', 200.00);

-- Insert data into products table
INSERT INTO products (name, price, description)
VALUES ('Product A', 50.00, 'Description for Product A'),
       ('Product B', 30.00, 'Description for Product B'),
       ('Product C', 40.00, 'Description for Product C');


-- Select customers who have placed orders in the last 30 days
SELECT DISTINCT c.*
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.order_date >= CURDATE() - INTERVAL 30 DAY;

-- Sum the total order amount for each customer
SELECT c.name, SUM(o.total_amount) AS total_spent
FROM customers c
JOIN orders o ON c.id = o.customer_id
GROUP BY c.name;

-- Update price of Product C
UPDATE products
SET price = 45.00
WHERE name = 'Product C';

-- Add a discount column to the products table
ALTER TABLE products
ADD COLUMN discount DECIMAL(5, 2) DEFAULT 0;

-- Select the top 3 products by price
SELECT * 
FROM products
ORDER BY price DESC
LIMIT 3;

-- Select customers who have ordered Product A
SELECT DISTINCT c.name
FROM customers c
JOIN orders o ON c.id = o.customer_id
JOIN order_items oi ON o.id = oi.order_id
JOIN products p ON oi.product_id = p.id
WHERE p.name = 'Product A';

-- Join orders and customers to get customer name and order date
SELECT c.name, o.order_date
FROM orders o
JOIN customers c ON o.customer_id = c.id;

-- Select orders where the total amount exceeds 150
SELECT * 
FROM orders
WHERE total_amount > 150.00;

-- Create order_items table to normalize the database
CREATE TABLE order_items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT,
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);

-- Calculate the average total amount for all orders
SELECT AVG(total_amount) AS avg_order_total
FROM orders;
