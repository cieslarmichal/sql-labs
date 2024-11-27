# Group by

* Podaj liczbę produktów o cenach mniejszych niż 10 lub większych niż 20

```sql
select count(*) from Products where UnitPrice not between 10 and 20;
```

* Podaj maksymalną cenę produktu dla produktów o cenach poniżej 20

```sql
select max(UnitPrice) from Products where UnitPrice < 20;
```

* Podaj maksymalną i minimalną i średnią cenę produktu dla produktów o produktach sprzedawanych w butelkach (‘bottleʼ)

```sql
select min(UnitPrice), max(UnitPrice), avg(UnitPrice) from Products where Products.QuantityPerUnit like '%bottle%'
```

* Wypisz informację o wszystkich produktach o cenie powyżej średniej
  
```sql
select * from Products where UnitPrice > (select avg(UnitPrice) from Products);
```

* Podaj sumę/wartość zamówienia o numerze 10250
  
```sql
select sum(UnitPrice * Quantity * (1-Discount)) from [Order Details] where OrderID = 10250;
```

* Podaj maksymalną cenę zamawianego produktu dla każdego zamówienia, posortuj zamówienia wg maksymalnej ceny produktu

```sql
select OrderID, max(UnitPrice) as max from [Order Details] group by OrderID order by max;
```

* Podaj maksymalną i minimalną cenę zamawianego produktu dla każdego zamówienia

```sql
select OrderID,min(UnitPrice) as min, max(UnitPrice) as max from [Order Details] group by OrderID;
```

* Podaj liczbę zamówień dostarczanych przez poszczególnych spedytorów (przewoźników)

```sql
select ShipVia, count(*) from Orders group by ShipVia;
```

* Który ze spedytorów był najaktywniejszy w 1997 roku

```sql
select ShipVia, count(*) as count from Orders where year(ShippedDate) = 1997 group by ShipVia order by count desc;
```

* Wyświetl zamówienia dla których liczba pozycji zamówienia jest większa niż 5

```sql
select OrderID, count(ProductID) from [Order Details] group by OrderID gaving count(ProductID) > 5;
```

* Wyświetl klientów dla których w 1998 roku zrealizowano więcej niż 8 zamówień (wyniki posortuj malejąco wg łącznej kwoty za dostarczenie zamówień dla każdego z klientów)

```sql
select CustomerID, count(OrderID), sum(Freight) from Orders where year(Orders.ShippedDate) = 1998
group by CustomerID having count(OrderID) > 8 order by sum(Freight) desc;
```

* Napisz polecenie, które oblicza wartość sprzedaży dla każdego zamówienia w tablicy
order details i zwraca wynik posortowany w malejącej kolejności (wg wartości
sprzedaży).

```sql
select OrderID, sum(UnitPrice * Quantity * (1-Discount)) from [Order Details]
group by OrderID order by 2 desc;
```

* Zmodyfikuj zapytanie z poprzedniego punktu, tak aby zwracało pierwszych 10
wierszy

```sql
select top 10 OrderID, sum(UnitPrice * Quantity * (1-Discount)) from [Order Details]
group by OrderID order by 2 desc;
```

* Podaj liczbę zamówionych jednostek produktów dla wszystkich produktów

```sql
select productid, sum(Quantity) from [Order Details] group by ProductID 
```

* Podaj nr zamówienia oraz wartość zamówienia, dla zamówień, dla których łączna
liczba zamawianych jednostek produktów jest większa niż 250

```sql
select OrderID, sum(UnitPrice * Quantity * (1-Discount)) as wartosc from [Order Details] group by OrderID having sum(Quantity) > 250;
```

* Dla każdego pracownika podaj liczbę obsługiwanych przez niego zamówień

```sql
select EmployeeID, count(OrderID) from Orders group by EmployeeID order by 2 desc;
```

* Dla każdego spedytora/przewoźnika podaj łączną wartość "opłat za przesyłkę" dla
przewożonych przez niego zamówień

```sql
select ShipVia, sum(Freight) from Orders group by ShipVia order by 2 desc;
```

* Dla każdego spedytora/przewoźnika podaj łączną wartość "opłat za przesyłkę" przewożonych przez niego zamówień w latach o 1996 do 1997

```sql
select ShipVia, sum(Freight) from Orders where year(ShippedDate) between 1996 and 1997 group by ShipVia order by 2 desc;
```

* Dla każdego pracownika podaj liczbę obsługiwanych przez niego zamówień z podziałem na lata i miesiące

```sql
select EmployeeID, year(OrderDate) as year, month(OrderDate) as month, count(OrderID) as orders from Orders
group by EmployeeID, year(OrderDate), month(OrderDate) order by 1, 2, 3;
```

* Dla każdej kategorii podaj maksymalną i minimalną cenę produktu w tej kategorii

```sql
select CategoryID, min(UnitPrice) as minprice, max(UnitPrice) as maxprice from Products group by CategoryID order by 3 desc;
```
