# Join

## Northwind

* Wybierz nazwy i ceny produktów (baza northwind) o cenie jednostkowej pomiędzy 20.00 a 30.00, dla każdego produktu podaj dane adresowe dostawcy

```sql
select ProductName, Address, City, Country from Products p left outer join Suppliers s on p.SupplierID = s.SupplierID where UnitPrice between 20 and 30;
```

* Wybierz nazwy produktów oraz inf. o stanie magazynu dla produktów dostarczanych przez firmę 'Tokyo Traders'

```sql
select ProductName, UnitsInStock from Products p join Suppliers s on p.SupplierID = s.SupplierID where s.CompanyName = 'Tokyo Traders';
```

* Czy są jacyś klienci którzy nie złożyli żadnego zamówienia w 1997 roku, jeśli tak to pokaż ich dane adresowe

```sql
select c.CustomerID, Address, City, PostalCode, Country from Customers c 
left outer join Orders o on c.CustomerID = o.CustomerID and year(OrderDate) = 1997 where o.OrderID is NULL;
```

* Wybierz nazwy i numery telefonów dostawców, dostarczających produkty, których aktualnie nie ma w magazynie

```sql
select CompanyName, Phone from Suppliers s join Products p  on s.SupplierID = p.SupplierID where p.UnitsInStock = 0;
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
select fine_paid, fine_assessed ,in_date, due_date, datediff(day, due_date, in_date) as kept_days
from loanhist l join title t on l.title_no = t.title_no where t.title = 'Tao Teh King' and datediff(day, due_date, in_date) > 0;
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
where p.UnitPrice between 20 and 30 and c.CategoryName = 'Meat/Poultry';
```

* Wybierz nazwy i ceny produktów z kategorii 'Confections' dla każdego produktu podaj nazwę dostawcy.

```sql
select p.ProductName, p.UnitPrice, s.CompanyName as Supplier from Products p
join Categories c on p.CategoryID = c.CategoryID
join Suppliers s on p.SupplierID  = s.SupplierID
where c.CategoryName = 'Confections';
```

* Wybierz nazwy i numery telefonów klientów, którym w 1997 roku przesyłki dostarczała firma 'United Package'

```sql
select distinct c.CompanyName, c.Phone from Orders o 
join Shippers s on o.ShipVia = s.ShipperID 
join Customers c on o.CustomerID = c.CustomerID 
where s.CompanyName = 'United Package' and year(o.ShippedDate) = 1997;
```

* Który ze spedytorów był najaktywniejszy w 1997 roku, podaj nazwę tego spedytora
  
```sql
select top 1 s.CompanyName as Shipper, count(*) as orders
from Orders o 
join Shippers s on o.ShipVia = s.ShipperID
group by s.ShipperID, s.CompanyName
order by 2 desc;
```

* Dla każdego zamówienia podaj wartość zamówionych produktów. Zbiór wynikowy
powinien zawierać nr zamówienia, datę zamówienia, nazwę klienta oraz wartość
zamówionych produktów

```sql
select o.OrderID, c.CompanyName as Customer, o.OrderDate, sum(UnitPrice * Quantity * (1-Discount)) as [order value]
from [Order Details] od
join Orders o on od.OrderID = o.OrderID
join Customers c on o.CustomerID = c.CustomerID
group by o.OrderID, c.CompanyName, o.OrderDate
order by 3;
```

* Dla każdego zamówienia podaj jego pełną wartość (wliczając opłatę za przesyłkę).
Zbiór wynikowy powinien zawierać nr zamówienia, datę zamówienia, nazwę klienta
oraz pełną wartość zamówienia

```sql
select o.OrderID, c.CompanyName as Customer, o.OrderDate, sum(UnitPrice * Quantity * (1-Discount)) + min(Freight) as [order value with freight]
from [Order Details] od
join Orders o on od.OrderID = o.OrderID
join Customers c on o.CustomerID = c.CustomerID
group by o.OrderID, c.CompanyName, o.OrderDate
order by 4 desc;
```

* Wybierz nazwy i numery telefonów klientów, którzy kupowali produkty z kategorii 'Confections'

```sql
select distinct companyname
from customers c inner join orders o on c.customerid = o.customerid
inner join [order details] od on od.orderid = o.orderid
inner join products p on od.productid = p.productid
inner join categories ca on p.categoryid = ca.categoryid
where categoryname = 'confections'
```

* Wybierz nazwy i numery telefonów klientów, którzy nie kupowali produktów z kategorii 'Confections'
  
```sql
select c.customerid, companyname
from orders o inner join [order details] od on od.orderid = o.orderid
inner join products p on od.productid = p.productid
inner join categories ca on p.categoryid = ca.categoryid and categoryname = 'confections'
right join Customers c on c.customerid = o.customerid
where o.orderid is null
  ```

```sql
select  c.customerid, companyname
from customers c left join
(
orders o inner join [order details] od on od.orderid = o.orderid
inner join products p on od.productid = p.productid
inner join categories ca on p.categoryid = ca.categoryid
and categoryname = 'confections'
) on c.customerid = o.customerid
where o.orderid is null
```

* Wybierz nazwy i numery telefonów klientów, którzy w 1997r nie kupowali produktów z
kategorii 'Confections'

```sql
select c.CustomerID, c.CompanyName, c.Phone  
from orders o join [order details] od on od.orderid = o.orderid and year(o.OrderDate) = 1997
join products p on od.productid = p.productid
join categories ca on p.categoryid = ca.categoryid and categoryname = 'confections'
right join Customers c on c.customerid = o.customerid
where o.OrderID is null
```

* Dla każdego klienta podaj liczbe zlożonych przez niego zamówień w 1997

```sql
select c.CustomerID, c.CompanyName, count(o.OrderID) as orders
from Customers c
left join Orders o on c.CustomerID = o.CustomerID and year(o.OrderDate) = 1997
group by c.CustomerID, c.CompanyName;
```

## Library v2

* Napisz polecenie, które wyświetla listę dzieci będących członkami biblioteki. Interesuje nas imię, nazwisko, data urodzenia dziecka, adres zamieszkania dziecka oraz imię i nazwisko rodzica.

```sql
select m.member_no,
m.firstname + ' ' + m.lastname as childName,
j.birth_date as childBirthdate,
p.firstname + ' ' + p.lastname as parentName,
a.city
from juvenile j
join [member] m on j.member_no  = m.member_no
join [member] p on j.adult_member_no = p.member_no
join adult a on j.adult_member_no = a.member_no;
```

* Podaj listę członków biblioteki mieszkających w Arizonie (AZ) mają więcej niż dwoje dzieci zapisanych do biblioteki
  
```sql
select a.member_no, max(m.firstname), max(m.lastname), count(*) from juvenile j 
join adult a on j.adult_member_no = a.member_no
join [member] m on a.member_no = m.member_no
where a.state = 'AZ'
group by a.member_no
having count(*) > 2;
```

* Podaj listę członków biblioteki mieszkających w Arizonie (AZ) którzy mają więcej niż dwoje dzieci zapisanych do biblioteki oraz takich którzy mieszkają w Kaliforni (CA) i mają więcej niż troje dzieci zapisanych do biblioteki
  
```sql
select a.member_no, min(m.firstname), min(m.lastname), count(*) as children from juvenile j 
join adult a on j.adult_member_no = a.member_no
join [member] m on a.member_no = m.member_no
where a.state = 'AZ'
group by a.member_no
having count(*) > 2
union
select a.member_no, min(m.firstname), min(m.lastname), count(*) as children from juvenile j 
join adult a on j.adult_member_no = a.member_no
join [member] m on a.member_no = m.member_no
where a.state = 'CA'
group by a.member_no
having count(*) > 3;
```

```sql
select m.member_no, m.lastname, m.firstname, count(*), min(a.state)
from member m
join adult a on m.member_no = a.member_no
join juvenile j on m.member_no = j.adult_member_no
group by m.member_no, state, m.lastname, m.firstname
having count(*) > 2 and state = 'AZ' or count(*) > 3 and state = 'CA';
```

## Northwind v3

* Napisz polecenie, które wyświetla pracowników oraz ich podwładnych

```sql
select s.FirstName + ' ' + s.LastName as Supervisor,  e.FirstName + ' ' + e.LastName as Employee
from Employees s
join Employees e on e.ReportsTo = s.EmployeeID
```

* Napisz polecenie, które wyświetla pracowników, którzy nie mają podwładnych
  
```sql
select s.FirstName + ' ' + s.LastName as Superior
from Employees s
left join Employees e on e.ReportsTo = s.EmployeeID
where e.EmployeeID is null;
```

* Łączna lista pracowników i klientów

```sql
select firstname + ' ' + lastname as name,city, postalcode
from employees
union
select companyname, city, postalcode
from customers;
```

* Państwa z których mamy zarówno klientów jak i dostawców
  
```sql
select country from customers
intersect
select country from suppliers
```

* Klienci którzy złożyli zamówienia w 1997 r. a nie złożyli zamówień w roku poprzednim

```sql
select customerid from orders where year(orderdate) = 1997
except
select customerid from orders where year(orderdate) = 1996
```

* Dla każdego zamówienia podaj łączną wartość tego zamówienia (wartość zamówienia wraz z opłatą za przesyłkę), nazwę klienta oraz imie i nazwisko pracownika obsługującego zamówienie.

```sql
select o.OrderID, c.CompanyName as Customer, e.FirstName + ' ' + e.LastName as Employee, sum((1 - od.Discount) * od.UnitPrice * od.Quantity) + min(o.Freight) as payment
from [Order Details] od
join Orders o on od.OrderID = o.OrderID 
join Customers c on o.CustomerID = c.CustomerID
join Employees e on o.EmployeeID = e.EmployeeID
group by o.OrderID, c.CustomerID, c.CompanyName, e.FirstName, e.LastName
```

* Podaj nazwy przewoźników, którzy w marcu 1998 przewozili produkty z kategorii 'Meat/Poultry'

```sql
select distinct s.CompanyName from Orders o 
join [Order Details] od on o.OrderID = od.OrderID and year(o.ShippedDate) = 1998 and month(o.ShippedDate) = 3
join Products p on od.ProductID = p.ProductID 
join Categories c on p.CategoryID = c.CategoryID and c.CategoryName = 'Meat/Poultry'
join Shippers s on o.ShipVia = s.ShipperID
order by 1;
```

* Podaj nazwy przewoźników, którzy w marcu 1997r nie przewozili produktów z kategorii 'Meat/Poultry'

```sql
select s.CompanyName from Orders o 
join [Order Details] od on o.OrderID = od.OrderID and year(o.ShippedDate) = 1997 and month(o.ShippedDate) = 3
join Products p on od.ProductID = p.ProductID 
join Categories c on p.CategoryID = c.CategoryID and c.CategoryName = 'Meat/Poultry'
right join Shippers s on o.ShipVia = s.ShipperID
where o.OrderID is null;
```

* Dla każdego przewoźnika podaj wartość produktów z kategorii 'Meat/Poultry' które ten przewoźnik przewiózł w marcu 1997

```sql
select s.CompanyName, sum(od.UnitPrice * od.Quantity) as [shipped value] from Orders o 
join [Order Details] od on o.OrderID = od.OrderID and year(o.ShippedDate) = 1997 and month(o.ShippedDate) = 3
join Products p on od.ProductID = p.ProductID 
join Categories c on p.CategoryID = c.CategoryID and c.CategoryName = 'Meat/Poultry'
right join Shippers s on o.ShipVia = s.ShipperID
group by s.ShipperID, s.CompanyName 
order by 2 desc;
```

* Dla każdej kategorii produktu, podaj łączną liczbę zamówionych przez klientów jednostek towarów z tej kategorii.
  
```sql
select c.CategoryName, sum(od.Quantity) as quantity from Categories c 
join Products p on c.CategoryID = p.CategoryID
join [Order Details] od on p.ProductID = od.ProductID
group by c.CategoryID, c.CategoryName
order by 2 desc;
```

* Dla każdej kategorii produktu, podaj łączną liczbę zamówionych w 1997r jednostek towarów z tej kategorii.

```sql
select c.CategoryName, sum(od.Quantity) as quantity from Categories c 
join Products p on c.CategoryID = p.CategoryID
join [Order Details] od on p.ProductID = od.ProductID
join Orders o on od.OrderID = o.OrderID and year(o.OrderDate) = 1997
group by c.CategoryID, c.CategoryName
order by 2 desc;
```

* Dla każdej kategorii produktu, podaj łączną wartość zamówionych towarów z tej kategorii

```sql
select c.CategoryName, sum(od.Quantity * od.UnitPrice) as value from Categories c 
join Products p on c.CategoryID = p.CategoryID
join [Order Details] od on p.ProductID = od.ProductID
join Orders o on od.OrderID = o.OrderID
group by c.CategoryID, c.CategoryName
order by 2 desc;
```

* Dla każdego przewoźnika podaj liczbę zamówień które przewieźli w 1997r

```sql
select s.CompanyName, count(*) as orders from Shippers s
join Orders o on s.ShipperID = o.ShipVia
group by s.ShipperID, s.CompanyName
order by 2 desc;
```

* Który z przewoźników był najaktywniejszy (przewiózł największą liczbę zamówień) w 1997r, podaj nazwę tego przewoźnika

```sql
select top 1 s.CompanyName, count(*) as orders from Shippers s
join Orders o on s.ShipperID = o.ShipVia and year(o.ShippedDate) = 1997
group by s.ShipperID, s.CompanyName
order by 2 desc;
```

* Dla każdego przewoźnika podaj łączną wartość "opłat za przesyłkę" przewożonych przez niego zamówień od '1998-05-03' do '1998-05-29'

```sql
select s.CompanyName, sum(o.Freight) as [freight sum] from Shippers s
left join Orders o on s.ShipperID = o.ShipVia and OrderDate between '1998-05-03' and '1998-05-29'
group by s.ShipperID, s.CompanyName
order by 2 desc;
```

* Dla każdego pracownika (imię i nazwisko) podaj łączną wartość zamówień obsłużonych przez tego pracownika w maju 1996

```sql
select e.FirstName, e.LastName, count(o.OrderID) as orders from Employees e
left join Orders o on e.EmployeeID = o.EmployeeID and year(o.OrderDate) = 1996 and month(o.OrderDate) = 5
group by e.EmployeeID, e.FirstName, e.LastName
order by 3 desc;
```

* Który z pracowników obsłużył największą liczbę zamówień w 1996r, podaj imię i nazwisko takiego pracownika

```sql
select top 1 e.FirstName, e.LastName, count(*) as orders from Employees e
join Orders o on e.EmployeeID = o.EmployeeID and year(o.OrderDate) = 1996
group by e.EmployeeID, e.FirstName, e.LastName
order by 3 desc;
```

* Który z pracowników był najaktywniejszy (obsłużył zamówienia o największej wartości) w 1996r, podaj imię i nazwisko takiego pracownika

```sql
select top 1 e.FirstName, e.LastName, sum((1-od.Discount) * od.UnitPrice * od.Quantity) + min(o.Freight) from Employees e
join Orders o on e.EmployeeID = o.EmployeeID and year(o.OrderDate) = 1996
join [Order Details] od on o.OrderID = od.OrderID
group by e.EmployeeID, e.FirstName, e.LastName
order by 3 desc;
```

* Dla każdego pracownika, który ma podwładnych podaj łączną wartość obsłużonych zamówień

```sql
select distinct s.FirstName + ' ' + s.LastName as Superviser, sum((1-od.Discount)*od.UnitPrice*od.Quantity) as [orders value]
from Orders o
join [Order Details] od on o.OrderID = od.OrderID
join Employees s on o.EmployeeID = s.EmployeeID
join Employees e on s.EmployeeID = e.ReportsTo
group by s.EmployeeID, s.FirstName, s.LastName, e.EmployeeID
```

* Dla każdego pracownika, który nie ma podwładnych podaj łączną wartość obsłużonych zamówień

```sql
select s.FirstName + ' ' + s.LastName as Superviser, sum((1-od.Discount)*od.UnitPrice*od.Quantity) as [orders value]
from Orders o
join [Order Details] od on o.OrderID = od.OrderID
join Employees s on o.EmployeeID = s.EmployeeID
left join Employees e on s.EmployeeID = e.ReportsTo
where e.EmployeeID is null
group by s.EmployeeID, s.FirstName, s.LastName
```

* Napisz polecenie, które wyświetla klientów z Francji którzy w 1998r złożyli więcej niż dwa zamówienia
oraz klientów z Niemiec którzy w 1997r złożyli więcej niż trzy zamówienia

```sql
select c.CustomerID, c.CompanyName, count(*) as orders from Orders o
join Customers c on o.CustomerID = c.CustomerID
where c.Country = 'France' and year(o.OrderDate) = 1998 or c.Country = 'Germany' and year(o.OrderDate) = 1997
group by c.CustomerID, c.CompanyName, c.Country
having count(*) > 2 and c.Country = 'France' or count(*) > 3 and c.Country = 'Germany'
```

```sql
select c.CustomerID, c.CompanyName, count(*) as orders from Orders o
join Customers c on o.CustomerID = c.CustomerID
where Country = 'France' and year(o.OrderDate) = 1998
group by c.CustomerID, c.CompanyName
having count(*) > 2
union
select c.CustomerID, c.CompanyName, count(*) as orders from Orders o
join Customers c on o.CustomerID = c.CustomerID
where Country = 'Germany' and year(o.OrderDate) = 1997
group by c.CustomerID, c.CompanyName
having count(*) > 3
```
