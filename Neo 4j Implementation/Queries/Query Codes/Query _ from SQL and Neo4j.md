SQL Queries



1\) select sf.Order\_Number,sf.CustomerKey,sf.Total\_Amount,sto.Country 

from Sales\_Fact sf right join

Stores\_Dim sto on sf.StoreKey = sf.StoreKey



2\) select ROUND( Max(Total\_Amount),2) maximum\_total\_Amount,ROUND( MIN(Total\_Amount),2) minimum\_total\_Amount,ROUND( AVG(Total\_Amount),2 )average\_total\_Amount

from Sales\_Fact 





3\) select  \*  from Products\_Dim where Category in (5,2)





4\) select sf.Order\_Number,pd.ProductKey , pd.Product\_Name,pd.Category,sf.Total\_Amount  from Sales\_Fact sf 

inner join Products\_Dim pd on sf.ProductKey = pd.ProductKey where sf.Total\_Amount > 500 

and pd.Category in (5,2) 





5\) select Top 5  Product\_Name,Order\_Number , Total\_Amount from  Sales\_Fact sf

right join Products\_Dim pd on pd.ProductKey = sf.ProductKey order by Total\_Amount desc



6\) select Top 5  cd.Name,Order\_Number , Total\_Amount from  Sales\_Fact sf

right join Customers\_Dim cd on cd.CustomerKey = sf.CustomerKey order by Total\_Amount desc





7\) select \*  from Sales\_Fact where Quantity >=5







\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_







Neo4J Queries



Query 1: Get orders with store country



MATCH (sto:Store)

OPTIONAL MATCH (c:Customer)-\[:PLACED]->(o:Order)-\[:ORDERED\_FROM]->(sto)

RETURN o.order\_id AS Order\_Number,

&nbsp;      c.customer\_id AS CustomerKey,

&nbsp;      o.amount AS Total\_Amount,

&nbsp;      sto.country AS Country;



Query 2: Aggregated order amounts



MATCH (o:Order)

RETURN 

&nbsp; ROUND(MAX(o.amount), 2) AS MaxAmount,

&nbsp; ROUND(MIN(o.amount), 2) AS MinAmount,

&nbsp; ROUND(AVG(o.amount), 2) AS AvgAmount;





Query 3: Products with category 5 or 2



MATCH (p:Product)

WHERE p.category IN \['5', '2']

RETURN p.product\_id, p.name, p.category;



Query 4: High value orders for category 5 or 2 products



MATCH (o:Order)-\[:CONTAINS\_PRODUCT]->(p:Product)

WHERE o.amount > 500 AND p.category IN \['5','2']

RETURN o.order\_id, p.product\_id, p.name, p.category, o.amount;



Query 5: Top 5 most expensive orders (with product info)



MATCH (o:Order)-\[:CONTAINS\_PRODUCT]->(p:Product)

RETURN p.name, o.order\_id, o.amount

ORDER BY o.amount DESC

LIMIT 5;





Query 6: Top 5 customers by order amount



MATCH (c:Customer)-\[:PLACED]->(o:Order)

RETURN c.name, o.order\_id, o.amount

ORDER BY o.amount DESC

LIMIT 5;









Query 7: Orders with quantity >= 5



MATCH (o:Order)

WHERE o.quantity >= 5

RETURN o.order\_id, o.amount, o.quantity;





--------------------------------------------------------------------





Nodes \& Relationships



Customer Node



LOAD CSV WITH HEADERS FROM 'file:///customers.csv' AS row

CREATE (:Customer {

&nbsp; customer\_id: toInteger(row.CustomerKey),

&nbsp; name: row.Name,

&nbsp; gender: row.Gender,

&nbsp; country: row.Country,

&nbsp; state: row.State,

&nbsp; city: row.City,

&nbsp; continent: row.Continent

});



Product Node



LOAD CSV WITH HEADERS FROM 'file:///products.csv' AS row

CREATE (:Product {

&nbsp; product\_id: toInteger(row.ProductKey),

&nbsp; name: row.Product\_Name,

&nbsp; brand: row.Brand,

&nbsp; category: row.Category,

&nbsp; price: toFloat(row.Unit\_Price\_USD)

});



Store Node



LOAD CSV WITH HEADERS FROM 'file:///stores.csv' AS row

CREATE (:Store {

&nbsp; store\_id: toInteger(row.StoreKey),

&nbsp; country: row.Country,

&nbsp; state: row.State,

&nbsp; square\_meters: toInteger(row.Square\_Meters),

&nbsp; open\_date: row.Open\_Date

});



Date Node



LOAD CSV WITH HEADERS FROM 'file:///dates.csv' AS row

CREATE (:Date {

&nbsp; date\_id: toInteger(row.date\_key),

&nbsp; full\_date: row.FullDate,

&nbsp; month: row.Month,

&nbsp; quarter: row.Quarter\_,

&nbsp; year: row.Year

});





Order Node and Relationships



1\)



LOAD CSV WITH HEADERS FROM 'file:///orders.csv' AS row

MERGE (c:Customer {customer\_id: toInteger(row.CustomerKey)})

MERGE (p:Product {product\_id: toInteger(row.ProductKey)})

MERGE (s:Store {store\_id: toInteger(row.StoreKey)})

MERGE (d:Date {date\_id: toInteger(row.DateKey)})

CREATE (o:Order {

&nbsp; order\_id: row.Order\_Number,

&nbsp; amount: toFloat(row.Total\_Amount),

&nbsp; quantity: toInteger(row.Quantity)

})

MERGE (c)-\[:PLACED]->(o)

MERGE (o)-\[:CONTAINS\_PRODUCT]->(p)

MERGE (o)-\[:ORDERED\_FROM]->(s)

MERGE (o)-\[:ON\_DATE]->(d);





2\)



LOAD CSV WITH HEADERS FROM 'file:///products.csv' AS row

MERGE (cat:category {category: row.Category})

WITH cat,row

MATCH (p:product {product\_id: toInteger(row.ProductKey)})

MERGE (p)-\[:BELONGS\_TO]->(cat)





3\)



LOAD CSV WITH HEADERS FROM 'file:///customers.csv' AS row

MERGE (city:City {name: row.City})

WITH city,row

MATCH (c:Customer {customer\_id: toInteger(row.CustomerKey)})

MERGE (c)-\[:LIVES\_IN]->(city)





4\)



LOAD CSV WITH HEADERS FROM 'file:///customers.csv' AS row

MERGE (country:Country {name: row.Country})

WITH country,row

MATCH (s:Store {store\_id: toInteger(row.StoreKey)})

MERGE (s)-\[:LOCATED\_IN]->(country)





