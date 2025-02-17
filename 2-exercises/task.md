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

select name, address from customers
where country = "United States";

2. Retrieve all the customers in ascending name sequence

select * from customers
order by name asc;

3. Retrieve all the products whose name contains the word `socks`

select * from products
where name like %'socks'%;

4. Retrieve all the products which cost more than 100 showing product id, name, unit price and supplier id.

select prod_id, products.product_name, unit_price, supp_id from product_availability p_a
inner join products p on p.id = p_a.prod_id
where p_a.unit_price > 100;

5. Retrieve the 5 most expensive products

select prod_id, products.product_name, unit_price, supp_id from product_availability p_a
inner join products p on p.id = p_a.prod_id
order by p_a.unit_price desc
limit 5;

6. Retrieve all the products with their corresponding suppliers. The result should only contain the columns `product_name`, `unit_price` and `supplier_name`

select products.product_name, unit_price, supplier.suppliers_name from product_availability p_a
inner join products p on p.id = p_a.prod_id
inner join suppliers s on s.id = p_a.supp_id
order by p.product_name;

7. Retrieve all the products sold by suppliers based in the United Kingdom. The result should only contain the columns `product_name` and `supplier_name`.

select product.product_name, supplier_name from suppliers s
where s.country = 'United Kingdom';

8. Retrieve all orders, including order items, from customer ID `1`. Include order id, reference, date and total cost (calculated as quantity * unit price).

select id, order_reference, order_date, order_items.quantity, product_availability.unit_price * order_items.quantity as 'total cost' from orders o
inner join order_items o_i on o_i.order_id = o.id
inner join product_availability p_o on p_o.prod_id = o_i.product_id
where o.customer_id = 1

9. Retrieve all orders, including order items, from customer named `Hope Crosby`

select * from orders, order_items
inner join order_items o_i on o_i.order_id = orders.id
inner join customers c on c.id = o_i.customer_id
where c.name = 'Hope Crosby';

10. Retrieve all the products in the order `ORD006`. The result should only contain the columns `product_name`, `unit_price` and `quantity`.
select products.name, product_availability.unit_price, order_items.quantity from products p
inner join product_availability p_a on p_a.prod_id = p.id
inner join order_items o_i on o_i.product_id = p.id
order by case when o.id > 'ORD006'

11. Retrieve all the products with their supplier for all orders of all customers. The result should only contain the columns `name` (from customer), `order_reference`, `order_date`, `product_name`, `supplier_name` and `quantity`.
select customers.name, orders.order_reference, orders.order_date, products.product_name, suppliers.supplier_name, quantity from order_items o_i
inner join customers c on c.id = orders.customer_id
inner join orders o on o.id = o_i.order_id
inner join products p on p.id = o_i.product_id
inner join suppliers s on s.id = o_i.supplier_id

12. Retrieve the names of all customers who bought a product from a supplier based in China.
select name from customers c
inner join orders o on o.id = c.id
inner join order_items o_i on o_i.order_id = orders.id
inner join suppliers s on s.id = o_i.supplier_id
where s.country = 'China';

13. List all orders giving customer name, order reference, order date and order total amount (quantity * unit price) in descending order of total.

select customers.name, orders.order_reference, orders.order_date, order_items.quantity * product_availability.unit_price as total from orders_items o_i
inner join customers c on c.id = orders.customer_id
inner join orders o on o.id = o_i.order_id
inner join product_availability p_a on p_a.id = o_i.product_id
order by total desc;

