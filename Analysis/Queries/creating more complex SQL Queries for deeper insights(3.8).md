### Getting the average amount paid(sum of all payments) by the top five customers(defined in previus step)
```SQL
SELECT AVG(total_amount_paid) AS average FROM
(SELECT  
        A.customer_id,
        A.first_name,
        A.last_name,
        D.country as country_name,
        C.city,
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
LIMIT 5) AS all_customer_count
```
### Getting the location(country) and sum of customers in that country of the top five customers
```SQL
SELECT country.country, COUNT(customer) AS all_customer_count, COUNT(pop_buyer) AS top_customer_count 
    FROM customer 
INNER JOIN address 
    ON customer.address_id = address.address_id
INNER JOIN city
    ON address.city_id = city.city_id
INNER JOIN country
    ON country.country_id = city.country_id
LEFT JOIN (
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
    LIMIT 5) AS pop_buyer
ON customer.customer_id = pop_buyer.customer_id
GROUP BY country.country
ORDER BY top_customer_count DESC, country.country;
```
