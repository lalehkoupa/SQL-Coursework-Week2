# E-Commerce Database

In this homework, you are going to work with an ecommerce database. In this database, you have `products` that `consumers` can buy from different `suppliers`. Customers can create an `order` and several products can be added in one order.

## Submission

Below you will find a set of tasks for you to complete to set up a database for an e-commerce app.

To submit this homework write the correct commands for each question here:

```sql


```

When you have finished all of the questions - open a pull request with your answers to the `Databases-Homework` repository.

## Setup

To prepare your environment for this homework, open a terminal and create a new database called `cyf_ecommerce`:

```sql
createdb cyf_ecommerce
```

Import the file [`cyf_ecommerce.sql`](./cyf_ecommerce.sql) in your newly created database:

```sql
psql -d cyf_ecommerce -f cyf_ecommerce.sql
```

Open the file `cyf_ecommerce.sql` in VSCode and examine the SQL code. Take a piece of paper and draw the database with the different relationships between tables (as defined by the REFERENCES keyword in the CREATE TABLE commands). Identify the foreign keys and make sure you understand the full database schema.

## Task

Once you understand the database that you are going to work with, solve the following challenge by writing SQL queries using everything you learned about SQL:

1. Retrieve all the customers' names and addresses who live in the United States

```sql
SELECT name,address
FROM customers
WHERE country='United States' ;
```

2. Retrieve all the customers in ascending name sequence

```sql
SELECT *
FROM customers
ORDER BY name asc;
```

3. Retrieve all the products whose name contains the word `socks`

```sql
SELECT *
FROM products
Where name Like '%socks%';
```

4. Retrieve all the products which cost more than 100 showing product id, name, unit price and supplier id.

```sql
SELECT pa.prod_id,pa.supp_id,pa.unit_price,products.product_name
FROM product_availability as pa
INNER JOIN products ON pa.prod_id=products.id
WHERE pa.unit_price>100;
```

5. Retrieve the 5 most expensive products

```sql
SELECT pa.prod_id,pa.unit_price,products.product_name
FROM product_availability as pa
INNER JOIN products ON pa.prod_id=products.id
ORDER BY unit_price Desc
LIMIT 5;
```

6. Retrieve all the products with their corresponding suppliers. The result should only contain the columns `product_name`, `unit_price` and `supplier_name`

```sql
SELECT products.product_name,pa.unit_price,suppliers.supplier_name
FROM products
INNER JOIN product_availability as pa ON pa.prod_id=products.id
INNER JOIN suppliers ON suppliers.id=pa.supp_id;
```

7. Retrieve all the products sold by suppliers based in the United Kingdom. The result should only contain the columns `product_name` and `supplier_name`.

```sql
SELECT products.product_name,suppliers.supplier_name
FROM products
INNER JOIN product_availability as pa ON products.id= pa.prod_id
INNER JOIN suppliers ON pa.supp_id=suppliers.id
WHERE suppliers.country='United Kingdom';
```

8. Retrieve all orders, including order items, from customer ID `1`. Include order id, reference, date and total cost (calculated as quantity \* unit price).

```sql
SELECT order_items.order_id,orders.order_reference,orders.order_date,order_items.quantity*pa.unit_price as total_cost
FROM product_availability as pa I
NNER JOIN order_items ON pa.prod_id=order_items.product_id AND pa.supp_id=order_items.supplier_id
INNER JOIN orders ON order_items.order_id=orders.id
INNER JOIN customers ON customers.id=orders.customer_id
WHERE customers.id=1;
```

9. Retrieve all orders, including order items, from customer named `Hope Crosby`

```sql
SELECT orders.id,orders.order_date,orders.order_reference,customers.name
FROM order_items
INNER JOIN orders ON orders.id=order_items.order_id
INNER JOIN customers ON orders.customer_id=customers.id
WHERE customers.name='Hope Crosby';
```

10. Retrieve all the products in the order `ORD006`. The result should only contain the columns `product_name`, `unit_price` and `quantity`.

```sql
SELECT product_name,unit_price, quantity ,order_date
FROM products
INNER JOIN product_availability ON products.id = product_availability.prod_id
INNER JOIN order_items ON order_items.product_id = products.id
INNER JOIN orders ON order_items.order_id = orders.id
WHERE orders.order_reference = 'ORD006';
```

11. Retrieve all the products with their supplier for all orders of all customers. The result should only contain the columns `name` (from customer), `order_reference`, `order_date`, `product_name`, `supplier_name` and `quantity`.

```sql
SELECT customers.name,orders.order_reference,orders.order_date,products.product_name,suppliers.supplier_name
FROM customers
INNER JOIN orders ON orders.customer_id=customers.id
INNER JOIN order_items ON order_items.order_id = orders.id
INNER JOIN products  ON order_items.product_id=products.id
INNER JOIN suppliers ON order_items.supplier_id=suppliers.id;
```

12. Retrieve the names of all customers who bought a product from a supplier based in China.

```sql
SELECT customers.name
FROM customers
INNER JOIN orders ON orders.customer_id = customers.id
INNER JOIN order_items ON order_items.order_id = orders.id
INNER JOIN product_availability as pa ON order_items.product_id = pa.prod_id AND order_items.supplier_id = pa.supp_id
INNER JOIN suppliers ON pa.supp_id = suppliers.id
WHERE suppliers.country = 'China';
```

13. List all orders giving customer name, order reference, order date and order total amount (quantity \* unit price) in descending order of total.

```sql
SELECT customers.name, orders.order_reference, orders.order_date, pa.unit_price * order_items.quantity AS total_cost FROM customers
INNER JOIN orders ON orders.customer_id = customers.id
INNER JOIN order_items ON order_items.order_id = orders.id
INNER JOIN product_availability as pa ON order_items.product_id = pa.prod_id AND order_items.supplier_id = pa.supp_id
 ORDER BY total_cost DESC;
```
