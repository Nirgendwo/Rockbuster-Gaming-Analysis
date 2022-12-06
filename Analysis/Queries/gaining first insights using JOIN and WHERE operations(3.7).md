### Getting the customer count for top ten countrys
```SQL
SELECT  D.country as country_name,
                COUNT(A.customer_id) as customer_count
FROM customer A
INNER JOIN address B ON A.address_id = B.address_id
INNER JOIN city C ON B.city_id = C.city_id
INNER JOIN country D ON C.country_id = D.country_id
GROUP BY D.country
ORDER BY COUNT(A.*) DESC
LIMIT 10;
```
### Getting the customer count for top ten citys located in the top ten countrys
```SQL
SELECT  C.city,
        COUNT(A.customer_id)
FROM customer A
INNER JOIN address B ON A.address_id = B.address_id
INNER JOIN city C ON B.city_id = C.city_id
INNER JOIN country D ON C.country_id = D.country_id
WHERE country IN ('India', 'China', 'United States', 'Japan', 'Mexico', 'Brazil', 'Russian Federation', 'Philippines', 'Turkey', 'Indonesia')
GROUP BY city
ORDER BY count DESC
LIMIT 10;
```
### Getting customer information for top five customer based on the amount they paid(sum of all payments ever made) locaed in top ten citys
```SQL
SELECT  
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
LIMIT 5;

```
