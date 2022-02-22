# SQL-Trainings
Examples and solutions for SQL exercising

### Database structure
```
CLIENT
└─── id
└─── name
└─── theme_id

PRODUCT
└─── id
└─── title
└─── description
└─── price
└─── createdAt
└─── client_id

THEME
└─── id
└─── name
```

**INNER JOIN : returns records that have matching value in both tables**
``` sql
SELECT *
FROM client
INNER JOIN product
WHERE client.id = product.id
```

``` sql
SELECT *
FROM client
INNER JOIN product
ON client.id = product.id
INNER JOIN theme
ON client.theme_id = theme.id
```

**UNION : operator used to combine the result of two or more SELECT statements**
``` sql
SELECT `name` FROM client
UNION ALL
SELECT `title` FROM product
```

``` sql
SELECT `id`, `name` FROM client
WHERE `name` = 'Client1'
UNION ALL
SELECT `id`, `title` FROM product
WHERE `title` = 'Product1'
```

**Order alphabetically**
``` sql
SELECT `name` FROM client
UNION ALL
SELECT `title` FROM product
ORDER BY `name` ASC
```

**Fusion of two tables with UNION ALL**
``` sql
SELECT 'client' AS table_name, `id`, null AS `title`, null AS `description`
FROM client
UNION ALL
SELECT 'product' AS table_name, `id`, null AS `name`, null AS `theme`
FROM product
ORDER BY `id` DESC
```


**INTERSECT : returns the records that two SELECT statements have in common**
``` sql
SELECT DISTINCT `id` 
FROM `client`
WHERE `id` IN (
  SELECT `id` 
  FROM `product`
  )
ORDER BY `id` ASC
```


**IN / NOT IN : allows to specify multiple values in a WHERE clause**
``` sql
SELECT DISTINCT `id` FROM `product`
WHERE `id` NOT IN (
  SELECT `id` 
  FROM `client`
  )
ORDER BY `id` ASC
```

**ORDER BY COUNT : returns the most repeated foreign key client_id in the product table**
``` sql
SELECT client.id, client.name 
FROM client 
WHERE client.id = (
    SELECT client_id 
    FROM product 
    GROUP BY product.client_id 
    ORDER BY COUNT(*) DESC 
    LIMIT 1
    )
```

**Returns the least repeated foreign key client_id in the product table**
``` sql
SELECT client.id, client.name 
FROM client 
WHERE client.id = (
    SELECT client_id 
    FROM product 
    GROUP BY product.client_id 
    ORDER BY COUNT(*) ASC 
    LIMIT 1
    )
```

**Returns the list of clients who have not yet purchased a product**
``` sql
SELECT client.name
FROM client
WHERE id NOT IN (
    SELECT product.client_id 
    FROM product
    )
```

**Client who bought the most products**
``` sql
SELECT product.client_id, client.name, COUNT(*) AS 'spentmost'
FROM product 
JOIN client
ON product.client_id = client.id
GROUP BY product.client_id
LIMIT 1
```

**HAVING allows filtering using functions such as SUM, COUNT, AVG, MIN or MAX**
``` sql
SELECT 
    client.id, 
    client.name,
    COUNT(product.client_id) AS 'spentmost'
FROM client 
INNER JOIN product ON client.id = product.client_id 
GROUP BY client.id, client.name
HAVING spentmost =  (       
    SELECT COUNT(product.client_id) AS spentmost
    FROM product
    GROUP BY client_id
    ORDER BY spentmost DESC
    LIMIT 1
)
```

**Two clients who bought the most products**
``` sql
SELECT product.client_id, client.name, COUNT(*) AS 'spentmost'
FROM product 
JOIN client
ON product.client_id = client.id
GROUP BY product.client_id
LIMIT 2
```

**SUM, AVG**
``` sql
SELECT 
SUM(price) AS price_sum,
SUM(id) AS id_sum,
SUM(client_id) AS client_id_sum
FROM product
```

``` sql
SELECT 
SUM(price) AS price_sum,
SUM(id) AS id_sum,
SUM(client_id) AS client_id_sum,
(SUM(price) + SUM(id) + SUM(client_id)) as 'Total'
FROM product
```

``` sql
SELECT `title`, `client_id`,
AVG(price) AS price_avg
FROM product
INNER JOIN client
WHERE product.client_id = client.id
```

**Spend of each client for each product**
``` sql
SELECT 
    product.price AS spent,
    client.name AS buyer
FROM 
    product
INNER JOIN 
    client
WHERE 
    product.client_id = client.id
GROUP BY 
    product.id
```

**Total spend made by each client**
``` sql
SELECT 
    SUM(product.price) AS spent_sum,
    client.name AS client_name
FROM 
    product
INNER JOIN 
    client
WHERE 
    product.client_id = client.id
GROUP BY 
    product.client_id
```

**Average spend made by each client**
``` sql
SELECT 
    AVG(product.price) AS spent_avg,
    client.name AS client_name
FROM 
    product
INNER JOIN 
    client
WHERE 
    product.client_id = client.id
GROUP BY 
    product.client_id
```

**BETWEEN : operator selects values within a given range inclusive**
``` sql
SELECT product.title, product.price 
FROM product 
WHERE product.price BETWEEN :param1 AND :param2
```

``` sql
SELECT product.title, product.price 
FROM product 
WHERE product.price >= :param1
AND product.price <= :param2
```

**Check IN array of values**
``` sql
SELECT product.title 
FROM product 
WHERE product.price IN (" . $values . ")
```

**Average price of all products**
``` sql
SELECT sum(price) / count(price) 
AS 'medium_price'
FROM product
```

**Lowest priced product**
``` sql
SELECT product.title, product.price 
FROM product 
WHERE product.price = (
    SELECT MIN(product.price)
    FROM product
    )
```

``` sql
SELECT product.title, product.price 
FROM product 
ORDER BY product.price ASC
LIMIT 1
```

**Product with the highest price**
``` sql
SELECT product.title, product.price 
FROM product 
WHERE product.price = (
    SELECT MAX(product.price)
    FROM product
    )
```

**Three products with the highest prices**
``` sql
SELECT product.title, product.price 
FROM product 
ORDER BY product.price DESC
LIMIT 3
```

**Products whose title contains a specific letter**
``` sql
SELECT * 
FROM product 
WHERE product.title 
LIKE '%P%'
```

**Products whose prices are higher than the average price of all products**
``` sql
SELECT * 
FROM product 
WHERE product.price > (
    SELECT sum(product.price)/count(product.price)
    FROM product
    ) 
```
