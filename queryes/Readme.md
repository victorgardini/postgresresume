### Querying a Table
To retrieve data from a table, the table is queried. An SQL SELECT statement is used to do this. Where * is a shorthand for “all columns”
```
SELECT * FROM weather;
```
The same result would be had with:
```
SELECT city, temp_lo, temp_hi, prcp, date FROM weather;
```

#### WHERE, AND, OR, NOT
```
SELECT column1, column2, ...
    FROM table_name
    WHERE condition1 AND condition2 AND condition3 ...;

SELECT column1, column2, ...
    FROM table_name
    WHERE condition1 OR condition2 OR condition3 ...; 

SELECT column1, column2, ...
    FROM table_name
    WHERE NOT condition; 

SELECT * FROM Customers
    WHERE Country='Germany' AND (City='Berlin' OR City='München');
```

#### AVG, COUNT e SUM


#### AS
```
SELECT fqdn AS teste FROM website_domain WHERE id < 10;
```
#### ORDER BY
```
SELECT * FROM weather
    ORDER BY city;

SELECT * FROM weather
    ORDER BY city, temp_lo;
```
#### DISTINCT
You can request that duplicate rows be removed from the result of a query:
```
SELECT DISTINCT city
    FROM weather;
```

Here again, the result row ordering might vary. You can ensure consistent results by using DISTINCT and ORDER BY together:
```
SELECT DISTINCT city
    FROM weather
    ORDER BY city;
```