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
```

* Dla każdego klienta podaj łączną wartość jego zamówień (uwzględnij opłatę za przesyłkę) z 1996r

```sql
```

* Dla każdego klienta podaj maksymalną wartość zamówienia złożonego przez tego klienta w 1997

```sql
```

* Czy są jacyś klienci którzy nie złożyli żadnego zamówienia w 1997 roku, jeśli tak to
pokaż ich dane adresowe

```sql
```

* Wybierz nazwy i numery telefonów klientów , którym w 1997 roku przesyłki dostarczała firma United Package.

```sql
```

* Wybierz nazwy i numery telefonów klientów , którym w 1997 roku przesyłek nie dostarczała firma United Package.

```sql
```

* Wybierz nazwy i numery telefonów klientów, którzy kupowali produkty z kategorii Confections.

```sql
```

* Wybierz nazwy i numery telefonów klientów, którzy nie kupowali produktów z kategorii Confections.

```sql
```

* Wybierz nazwy i numery telefonów klientów, którzy w 1997r nie kupowali produktów z kategorii Confections.

```sql
```

* Podaj wszystkie produkty których cena jest mniejsza niż średnia cena produktu

```sql
```

* Podaj wszystkie produkty których cena jest mniejsza niż średnia cena produktu danej kategorii

```sql
```

* Dla każdego produktu podaj jego nazwę, cenę, średnią cenę wszystkich produktów oraz różnicę między ceną produktu a średnią ceną wszystkich produktów

```sql
```

* Dla każdego produktu podaj jego nazwę kategorii, nazwę produktu, cenę, średnią cenę wszystkich produktów danej kategorii
oraz różnicę między ceną produktu a średnią ceną wszystkich produktów danej kategorii

```sql
```

* Podaj produkty kupowane przez więcej niż jednego klienta

```sql
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
