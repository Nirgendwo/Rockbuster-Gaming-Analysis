###Checking for duplicates
```SQL
SELECT title,
       release_year,
       language_id,
       rental_duration,
       COUNT(*)
FROM film
GROUP BY title,
         release_year,
         language_id,
         rental_duration
HAVING COUNT(*) >1;
``` 
### checking for incorrect/missing data
```SQL
SELECT language_id,
FROM film
GROUP BY language_id;

```
