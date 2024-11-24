# Join

## Northwind

* Wybierz nazwy i ceny produktów (baza northwind) o cenie jednostkowej pomiędzy 20.00 a 30.00, dla każdego produktu podaj dane adresowe dostawcy

```sql
select ProductName, Address, City, Country from Products p left outer join Suppliers s on p.SupplierID = s.SupplierID where UnitPrice BETWEEN 20 AND 30;
```

* Wybierz nazwy produktów oraz inf. o stanie magazynu dla produktów dostarczanych przez firmę ‘Tokyo Traders’

```sql
select ProductName, UnitsInStock from Products p join Suppliers s on p.SupplierID = s.SupplierID where s.CompanyName = 'Tokyo Traders';
```

* Czy są jacyś klienci którzy nie złożyli żadnego zamówienia w 1997 roku, jeśli tak to pokaż ich dane adresowe

```sql
select c.CustomerID, Address, City, PostalCode, Country from Customers c 
left outer join Orders o on c.CustomerID = o.CustomerID and YEAR(OrderDate) = 1997 where o.OrderID is NULL;
```

* Wybierz nazwy i numery telefonów dostawców, dostarczających produkty, których aktualnie nie ma w magazynie

```sql
select CompanyName, Phone from Suppliers s join Products p  on s.SupplierID =p.SupplierID where p.UnitsInStock = 0;
```

## Library

* Napisz polecenie, które wyświetla listę dzieci będących członkami biblioteki (baza library). Interesuje nas imię, nazwisko i data urodzenia dziecka.

```sql
select firstname, lastname, birth_date from juvenile j join member m on j.member_no = m.member_no;
```

* Napisz polecenie, które podaje tytuły aktualnie wypożyczonych książek

```sql
select DISTINCT title from loan l join title t on l.title_no = t.title_no;
```

* Podaj informacje o karach zapłaconych za przetrzymywanie książki o tytule ‘Tao Teh King’. Interesuje nas data oddania książki, ile dni była przetrzymywana i jaką zapłacono karę

```sql
select fine_paid, fine_assessed ,in_date, due_date, DATEDIFF(day, due_date, in_date) as kept_days
from loanhist l join title t on l.title_no = t.title_no where t.title = 'Tao Teh King' and DATEDIFF(day, due_date, in_date) > 0;
```

* Napisz polecenie które podaje listę książek (mumery ISBN) zarezerwowanych przez osobę o nazwisku: Stephen A. Graff

```sql
select isbn from reservation r join [member] m on r.member_no = m.member_no 
where firstname = 'Stephen' and middleinitial = 'A' and lastname = 'Graff';
```

* Napisz polecenie, wyświetlające CROSS JOIN między shippers i suppliers. użyteczne dla listowania wszystkich możliwych sposobów w jaki dostawcy mogą dostarczać swoje produkty

```sql
select suppliers.companyname, shippers.companyname from suppliers cross join shippers;
```

## Northwind v2

* Wybierz nazwy i ceny produktów (baza northwind) o cenie jednostkowej pomiędzy 20.00 a 30.00, dla każdego produktu podaj dane adresowe dostawcy, interesują nas tylko produkty z kategorii ‘Meat/Poultry’

```sql
select p.ProductName, p.UnitPrice from Products p
join Categories c on p.CategoryID = c.CategoryID
join Suppliers s on p.SupplierID  = s.SupplierID
where p.UnitPrice BETWEEN 20 AND 30 and c.CategoryName = 'Meat/Poultry';
```

* Wybierz nazwy i ceny produktów z kategorii ‘Confections’ dla każdego produktu podaj nazwę dostawcy.

```sql
select p.ProductName, p.UnitPrice, s.CompanyName as Supplier from Products p
join Categories c on p.CategoryID = c.CategoryID
join Suppliers s on p.SupplierID  = s.SupplierID
where c.CategoryName = 'Confections';
```

* Wybierz nazwy i numery telefonów klientów , którym w 1997 roku przesyłki dostarczała firma ‘United Package’

```sql
select distinct c.CompanyName, c.Phone from Orders o 
join Shippers s on o.ShipVia = s.ShipperID 
join Customers c on o.CustomerID = c.CustomerID 
where s.CompanyName = 'United Package' and YEAR(o.ShippedDate) = 1997;
```

* Wybierz nazwy i numery telefonów klientów, którzy kupowali produkty z kategorii ‘Confections’

```sql
select distinct c.CompanyName, c.Phone from Orders o
join Customers c on o.CustomerID = c.CustomerID 
join [Order Details] od on o.OrderID = od.OrderID 
join Products p on p.ProductID = od.ProductID 
join Categories c2 on c2.CategoryID = p.CategoryID
where c2.CategoryName = 'Confections'
```
