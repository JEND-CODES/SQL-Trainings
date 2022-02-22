# SQL-Trainings
Examples and solutions for SQL exercising

### Database structure
```
CLIENT
└─── id
└─── name

PRODUCT
└─── id
└─── title
└─── description
└─── price
└─── createdAt
```

**INNER JOIN : returns records that have matching value in both tables**

``` sql
SELECT *
FROM client
INNER JOIN product
WHERE client.id = product.id
```

``` sql
```
