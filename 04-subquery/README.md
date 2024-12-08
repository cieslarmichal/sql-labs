# Subquery

* Podaj łączną wartość zamówienia o numerze 10250 (uwzględnij cenę za przesyłkę)

```sql
select sum(od.UnitPrice* od.Quantity * (1-od.Discount)) + min(o.Freight)
from [Order Details] od 
join Orders o on od.OrderID = o.OrderID
where od.OrderId =10250;
```

```sql
select sum(od.UnitPrice* od.Quantity * (1-od.Discount)) + (select Freight from Orders o where o.OrderID = 10250)
from [Order Details] od 
where od.OrderId =10250;
```

* Podaj łączną wartość każdego zamówienia (uwzględnij cenę za przesyłkę)

```sql
select o.OrderID, sum(od.UnitPrice* od.Quantity * (1-od.Discount)) + min(o.Freight) as 
from [Order Details] od 
join Orders o on od.OrderID = o.OrderID
group by o.OrderID;
```

```sql
select o.OrderID, 
(select sum(od.UnitPrice* od.Quantity * (1-od.Discount)) from [Order Details] od where od.OrderID = o.OrderID) + o.Freight
from Orders o;
```

* Dla każdego produktu podaj maksymalną wartość zakupu tego produktu
  
```sql
```

* Dla każdego produktu podaj maksymalną wartość zakupu tego produktu w 1997r

```sql
```
