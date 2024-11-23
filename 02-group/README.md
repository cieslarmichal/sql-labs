# Group by

* Podaj liczbę produktów o cenach mniejszych niż 10 lub większych niż 20

```sql
select COUNT(*) from Products where UnitPrice NOT BETWEEN 10 AND 20;
```

* Podaj maksymalną cenę produktu dla produktów o cenach poniżej 20

```sql
select MAX(UnitPrice) from Products where UnitPrice < 20;
```

* Podaj maksymalną i minimalną i średnią cenę produktu dla produktów o produktach sprzedawanych w butelkach (‘bottleʼ)

```sql
select MIN(UnitPrice), MAX(UnitPrice), AVG(UnitPrice) from Products where Products.QuantityPerUnit like '%bottle%'
```

* Wypisz informację o wszystkich produktach o cenie powyżej średniej
  
```sql
select * from Products where UnitPrice > (select AVG(UnitPrice) from Products);
```

* Podaj sumę/wartość zamówienia o numerze 10250
  
```sql
select SUM(UnitPrice * Quantity * (1-Discount)) from [Order Details] WHERE OrderID = 10250;
```

* Podaj maksymalną cenę zamawianego produktu dla każdego zamówienia, posortuj zamówienia wg maksymalnej ceny produktu

```sql
select OrderID, MAX(UnitPrice) as max from [Order Details] group by OrderID order by max;
```

* Podaj maksymalną i minimalną cenę zamawianego produktu dla każdego zamówienia

```sql
select OrderID,MIN(UnitPrice) as min, MAX(UnitPrice) as max from [Order Details] group by OrderID;
```

* Podaj liczbę zamówień dostarczanych przez poszczególnych spedytorów (przewoźników)

```sql
select ShipVia, COUNT(*) from Orders group by ShipVia;
```

* Który ze spedytorów był najaktywniejszy w 1997 roku

```sql
select ShipVia, COUNT(*) as count from Orders where YEAR(ShippedDate) = 1997 group by ShipVia order by count desc;
```

* Wyświetl zamówienia dla których liczba pozycji zamówienia jest większa niż 5

```sql
select OrderID, COUNT(ProductID) from [Order Details] group by OrderID HAVING COUNT(ProductID) > 5;
```

* Wyświetl klientów dla których w 1998 roku zrealizowano więcej niż 8 zamówień (wyniki posortuj malejąco wg łącznej kwoty za dostarczenie zamówień dla każdego z klientów)

```sql
select CustomerID, COUNT(OrderID), SUM(Freight) from Orders WHERE YEAR(Orders.ShippedDate) = 1998
group by CustomerID having COUNT(OrderID) > 8 order by SUM(Freight) desc;
```

* Napisz polecenie, które oblicza wartość sprzedaży dla każdego zamówienia w tablicy
order details i zwraca wynik posortowany w malejącej kolejności (wg wartości
sprzedaży).

```sql
select OrderID, SUM(UnitPrice * Quantity * (1-Discount)) from [Order Details]
group by OrderID order by 2 desc;
```

* Zmodyfikuj zapytanie z poprzedniego punktu, tak aby zwracało pierwszych 10
wierszy

```sql
select OrderID, SUM(UnitPrice * Quantity * (1-Discount)) from [Order Details]
group by OrderID order by 2 desc limit 10;
```

* Podaj liczbę zamówionych jednostek produktów dla wszystkich produktów

```sql
select productid, SUM(Quantity) from [Order Details] group by ProductID 
```

* Podaj nr zamówienia oraz wartość zamówienia, dla zamówień, dla których łączna
liczba zamawianych jednostek produktów jest większa niż 250

```sql
select OrderID, SUM(UnitPrice * Quantity * (1-Discount)) as wartosc from [Order Details] group by OrderID having SUM(Quantity) > 250;
```

* Dla każdego pracownika podaj liczbę obsługiwanych przez niego zamówień
* Dla każdego spedytora/przewoźnika podaj łączną wartość "opłat za przesyłkę" dla
przewożonych przez niego zamówień
* Dla każdego spedytora/przewoźnika podaj łączną wartość "opłat za przesyłkę"
przewożonych przez niego zamówień w latach o 1996 do 1997
* Dla każdego pracownika podaj liczbę obsługiwanych przez niego zamówień z
podziałem na lata i miesiące
* Dla każdej kategorii podaj maksymalną i minimalną cenę produktu w tej kategorii
