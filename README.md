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

**Fusion of two tables with union all**
``` sql
SELECT 'client' AS table_name, `id`, null AS `title`, null AS `description`
FROM client
UNION ALL
SELECT 'product' AS table_name, `id`, null AS `name`, null AS `theme`
FROM product
ORDER BY `id` DESC
```


**INTERSECT : return the records that two SELECT statements have in common**
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

**ORDER BY COUNT : Return the most repeated foreign key client_id in table product**

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
