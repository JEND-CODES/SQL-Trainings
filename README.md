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
**UNION: set operator used to combine the result-set of two or more SELECT statements**

``` sql
SELECT `name` FROM client
UNION ALL
SELECT `title` FROM product
```
