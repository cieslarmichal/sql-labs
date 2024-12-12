# Subquery

## Northwind

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
select od.ProductID, p.ProductName, max(od.unitprice * od.quantity * (1 - od.discount)) as [max product order value]
from [Order Details] od
join Products p on od.ProductID = p.ProductID
group by od.ProductID, p.ProductName 
order by od.ProductID;
```

* Dla każdego produktu podaj maksymalną wartość zakupu tego produktu w 1997r

```sql
select od.ProductID, p.ProductName, max(od.unitprice * od.quantity * (1 - od.discount)) as [max product order value]
from [Order Details] od
join Products p on od.ProductID = p.ProductID
join Orders o on od.OrderID = o.OrderID and year(o.OrderDate) = 1997
group by od.ProductID, p.ProductName 
order by od.ProductID;
```

* Dla każdego klienta podaj łączną wartość jego zamówień (bez opłaty za przesyłkę) z 1996r
  
```sql
select c.CustomerID, c.CompanyName, sum(od.unitprice * od.quantity * (1 - od.discount)) as [orders value] 
from Customers c 
left join Orders o on o.CustomerID = c.CustomerID and year(o.OrderDate) = 1996 
left join [Order Details] od on od.OrderID = o.OrderID 
group by c.CustomerID, c.CompanyName 
order by 3 desc;
```

```sql
select c.CustomerID, c.CompanyName, (select sum(od.unitprice * od.quantity * (1 - od.discount))
from Orders o
join [Order Details] od on od.OrderID = o.OrderID
where o.CustomerID = c.CustomerID and year(o.OrderDate) = 1996 
) as [orders value] 
from Customers c
order by 3 desc;
```

* Dla każdego klienta podaj łączną wartość jego zamówień (uwzględnij opłatę za przesyłkę) z 1996r

```sql
select c.CustomerID, c.CompanyName,
       sum(od.unitprice * od.quantity * (1 - od.discount)) +
  (select sum(o2.Freight)
   from Orders o2
   where o2.CustomerID = c.CustomerID
     and year(o2.OrderDate) = 1996) as [orders value]
from Customers c
left join Orders o on o.CustomerID = c.CustomerID
and year(o.OrderDate) = 1996
left join [Order Details] od on od.OrderID = o.OrderID
group by c.CustomerID,
         c.CompanyName
order by 3 desc
```

```sql
select c.CustomerID, c.CompanyName,
  (select sum(od.unitprice * od.quantity * (1 - od.discount))
   from Orders o
   join [Order Details] od on od.OrderID = o.OrderID
   where o.CustomerID = c.CustomerID
     and year(o.OrderDate) = 1996) +
  (select sum(o2.Freight)
   from Orders o2
   where o2.CustomerID = c.CustomerID
     and year(o2.OrderDate) = 1996) as [orders value]
from Customers c
order by 3 desc;
```

* Dla każdego klienta podaj maksymalną wartość zamówienia złożonego przez tego klienta w 1997

```sql
select t.CustomerID, max(t.OrderValue) from (select c.CustomerID, o.OrderID, sum(od.unitprice * od.quantity * (1 - od.discount)) as OrderValue
from Customers c 
left join Orders o on c.CustomerID = o.CustomerID and year(o.OrderDate) = 1997
left join [Order Details] od on o.OrderID = od.OrderID
group by c.CustomerID, o.OrderID) as t
group by t.CustomerID
order by 1;
```

* Klienci którzy nie złożyli żadnego zamówienia w 1997 roku

```sql
select c.CustomerID, c.CompanyName from Customers c 
where c.CustomerID not in (select o.CustomerID from Orders o where year(o.OrderDate) = 1997)
```

```sql
select c.CustomerID, c.CompanyName from Customers c 
where not exists (select * from Orders o where c.CustomerID = o.CustomerID and year(o.OrderDate) = 1997)
```

```sql

select c.CustomerID, c.CompanyName from Customers c 
left join Orders o on c.CustomerID = o.CustomerID and year(o.OrderDate) = 1997
where o.OrderID is null
```

* Wybierz nazwy i numery telefonów klientów , którym w 1997 roku przesyłki dostarczała firma United Package.

```sql
select c.CompanyName, c.Phone from Customers c 
where c.CustomerID  in 
(select o.CustomerID from Orders o join Shippers s on o.ShipVia = s.ShipperID and s.CompanyName = 'United Package' and year(o.OrderDate) = 1997)

```

* Wybierz nazwy i numery telefonów klientów , którym w 1997 roku przesyłek nie dostarczała firma United Package.

```sql
select c.CompanyName, c.Phone from Customers c 
where c.CustomerID not in 
(select o.CustomerID from Orders o join Shippers s on o.ShipVia = s.ShipperID and s.CompanyName = 'United Package' and year(o.OrderDate) = 1997)
```

* Wybierz nazwy i numery telefonów klientów, którzy kupowali produkty z kategorii Confections.

```sql
select c.CompanyName, c.Phone from Customers c 
where c.CustomerID in 
(
select o.CustomerID from Orders o
join [Order Details] od on o.OrderID = od.OrderID
join Products p on od.ProductID = p.ProductID
join Categories cat on cat.CategoryID = p.CategoryID and cat.CategoryName = 'Confections'
);
```

* Wybierz nazwy i numery telefonów klientów, którzy nie kupowali produktów z kategorii Confections.

```sql
select c.CompanyName, c.Phone from Customers c 
where c.CustomerID not in 
(
select o.CustomerID from Orders o
join [Order Details] od on o.OrderID = od.OrderID
join Products p on od.ProductID = p.ProductID
join Categories cat on cat.CategoryID = p.CategoryID and cat.CategoryName = 'Confections'
);
```

* Wybierz nazwy i numery telefonów klientów, którzy w 1997r nie kupowali produktów z kategorii Confections.

```sql
select c.CompanyName, c.Phone from Customers c 
where c.CustomerID not in 
(
select o.CustomerID from Orders o
join [Order Details] od on o.OrderID = od.OrderID
join Products p on od.ProductID = p.ProductID
join Categories cat on cat.CategoryID = p.CategoryID and cat.CategoryName = 'Confections'
where year(o.OrderDate) = 1997
);
```

* Podaj wszystkie produkty których cena jest mniejsza niż średnia cena produktu

```sql
select p.ProductID, p.ProductName, p.UnitPrice
from Products p
where p.UnitPrice < (select avg(p2.UnitPrice) from Products p2);
```

* Podaj wszystkie produkty których cena jest mniejsza niż średnia cena produktu danej kategorii

```sql
select p.ProductID, p.CategoryID, p.ProductName, p.UnitPrice
from Products p
where p.UnitPrice < (select avg(p2.UnitPrice) from Products p2 where p2.CategoryID = p.CategoryID);
```

* Dla każdego produktu podaj jego nazwę, cenę, średnią cenę wszystkich produktów oraz różnicę między ceną produktu a średnią ceną wszystkich produktów

```sql
with av as (
  select avg(p2.UnitPrice) as avg from Products p2
)

select p.ProductID, p.ProductName, p.UnitPrice, av.avg as avg, p.UnitPrice - av.avg as diff
from Products p, av;

```

* Dla każdego produktu podaj jego nazwę kategorii, nazwę produktu, cenę, średnią cenę wszystkich produktów danej kategorii
oraz różnicę między ceną produktu a średnią ceną wszystkich produktów danej kategorii

```sql
with avc as (
 select c.CategoryID, c.CategoryName, avg(p2.UnitPrice) as avg
 from Products p2
 join Categories c on p2.CategoryID = c.CategoryID
 group by c.CategoryID, c.CategoryName
)

select p.ProductName, avc.CategoryName, p.UnitPrice, avc.avg as avg, p.UnitPrice - avc.avg as diff
from Products p
join avc on p.CategoryID = avc.CategoryID
```

* Podaj produkty kupowane przez więcej niż jednego klienta

```sql
select ProductName, count(*) as boughtby from (select distinct p.ProductID, p.ProductName, o.CustomerID from Products p 
join [Order Details] od on p.ProductID = od.ProductID
join Orders o on o.OrderID = od.OrderID) cp
group by ProductID, ProductName
having count(*) > 1
order by 2
```

* Podaj produkty kupowane w 1997r przez więcej niż jednego klienta

```sql
```

* Podaj nazwy klientów którzy w 1997r kupili co najmniej dwa różne produkty z kategorii 'Confections'

```sql
```

* Dla każdego pracownika (imię i nazwisko) podaj łączną wartość zamówień obsłużonych przez tego pracownika (z ceną za przesyłkę)

```sql
```

* Który z pracowników był najaktywniejszy (obsłużył zamówienia o największej wartości) w 1997r, podaj imię i nazwisko takiego pracownika
oraz datę ostatnio obsłużonego zamówienia

```sql
```

* Najaktywniejszy pracownik który ma podwładnych

```sql
```

* Najaktywniejszy pracownik który nie ma podwładnych

```sql
```

## Library

* Dla każdego dorosłego członka biblioteki podaj jego imię, nazwisko oraz liczbę jego dzieci.

```sql
```

* Dla każdego dorosłego członka biblioteki podaj jego imię, nazwisko, liczbę jego dzieci, liczbę zarezerwowanych książek oraz liczbę wypożyczonych książek.

```sql
```

* Dla każdego dorosłego członka biblioteki podaj jego imię, nazwisko, liczbę jego dzieci, oraz liczbę książek zarezerwowanych i wypożyczonych przez niego i jego dzieci.

```sql
```

* Dla każdego tytułu książki podaj ile razy ten tytuł był wypożyczany w 2001r

```sql
```

* Dla każdego tytułu książki podaj ile razy ten tytuł był wypożyczany w 2002r

```sql
```
