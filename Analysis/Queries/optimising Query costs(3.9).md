# I rewrote the Queries from the previus step
### This document contains Queries using WITH
```SQL
WITH top_five_customer (customer_id, first_name, last_name, country, city, total_amount_paid) AS (SELECT  
        A.customer_id As customer_id,
        A.first_name AS first_name,
        A.last_name AS last_name,
        D.country AS country,
        C.city AS city,
        SUM(E.amount) AS total_amount_paid
FROM customer A
INNER JOIN address B ON A.address_id = B.address_id
INNER JOIN city C ON B.city_id = C.city_id
INNER JOIN country D ON C.country_id = D.country_id
INNER JOIN payment E ON A.customer_id = E.customer_id
WHERE city IN ('Aurora', 'Tokat', 'Tarsus', 'Altixco', 'Emeishan', 'Pontainak', 'Shimoga', 'Aparaceida de Goiania', 'Zalantun', 'Taguig')
GROUP BY    
A.customer_id,
       	D.country, 
       	C.city
ORDER BY total_amount_paid DESC
LIMIT 5)

SELECT AVG(total_amount_paid) AS average FROM top_five_customer;
```
```SQL
WITH top_five_customer (customer_id, total_amount_paid) AS (
    SELECT  
        A.customer_id as customer_id,
        SUM(E.amount) as total_amount_paid
        FROM customer A
        INNER JOIN address B ON A.address_id = B.address_id
        INNER JOIN city C ON B.city_id = C.city_id
        INNER JOIN country D ON C.country_id = D.country_id
        INNER JOIN payment E ON A.customer_id = E.customer_id
        WHERE city IN ('Aurora', 'Tokat', 'Tarsus', 'Altixco', 'Emeishan', 'Pontainak', 'Shimoga', 'Aparaceida de Goiania', 'Zalantun', 'Taguig')
        GROUP BY    
        A.customer_id,
                D.country, 
                C.city
        ORDER BY total_amount_paid DESC
    LIMIT 5)


SELECT country.country, COUNT(customer) AS all_customer_count, COUNT(top_five_customer) AS top_customer_count 
    FROM customer 
INNER JOIN address 
    ON customer.address_id = address.address_id
INNER JOIN city
    ON address.city_id = city.city_id
INNER JOIN country
    ON country.country_id = city.country_id
LEFT JOIN top_five_customer
ON customer.customer_id = top_five_customer.customer_id
GROUP BY country.country
ORDER BY top_customer_count DESC, country.country;
```
