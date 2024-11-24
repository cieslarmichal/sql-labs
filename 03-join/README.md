# Join

## Northwind

* Wybierz nazwy i ceny produktów (baza northwind) o cenie jednostkowej pomiędzy 20.00 a 30.00, dla każdego produktu podaj dane adresowe dostawcy

```sql
select ProductName, Address, City, Country from Products p left outer join Suppliers s on p.SupplierID = s.SupplierID where UnitPrice BETWEEN 20 AND 30;
```

* Wybierz nazwy produktów oraz inf. o stanie magazynu dla produktów dostarczanych przez firmę 'Tokyo Traders'

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
where s.CompanyName = 'United Package' and YEAR(o.ShippedDate) = 1997;
```

* Który ze spedytorów był najaktywniejszy w 1997 roku, podaj nazwę tego spedytora
  
```sql
```

* Dla każdego zamówienia podaj wartość zamówionych produktów. Zbiór wynikowy
powinien zawierać nr zamówienia, datę zamówienia, nazwę klienta oraz wartość
zamówionych produktów

```sql
```

* Dla każdego zamówienia podaj jego pełną wartość (wliczając opłatę za przesyłkę).
Zbiór wynikowy powinien zawierać nr zamówienia, datę zamówienia, nazwę klienta
oraz pełną wartość zamówienia

```sql
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

* Wybierz nazwy i numery telefonów klientów, którzy nie kupowali produktów z kategorii ‘Confectionsʼ
  
```sql
select c.customerid, companyname
from  orders o inner join [order details] od on od.orderid = o.orderid
inner join products p on od.productid = p.productid
inner join categories ca on p.categoryid = ca.categoryid and categoryname = 'confections'
right join Customers c on c.customerid = o.customerid
where o.orderid is null
  ```

```sql
select  c.customerid, companyname
from  customers c left join
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
```

* Dla każdego klienta podaj liczbe zlożonych przez niego zamówień w 1997

```sql
select c.CustomerID, c.CompanyName, COUNT(o.OrderID) as orders
from Customers c
left join Orders o on c.CustomerID = o.CustomerID and YEAR(o.OrderDate) = 1997
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
select a.member_no, max(m.firstname), max(m.lastname), COUNT(*) from juvenile j 
join adult a on j.adult_member_no = a.member_no
join [member] m on a.member_no = m.member_no
where a.state = 'AZ'
group by a.member_no
having COUNT(*) > 2;
```

* Podaj listę członków biblioteki mieszkających w Arizonie (AZ) którzy mają więcej niż dwoje dzieci zapisanych do biblioteki oraz takich którzy mieszkają w Kaliforni (CA) i mają więcej niż troje dzieci zapisanych do biblioteki
  
```sql
select a.member_no, min(m.firstname), min(m.lastname), COUNT(*) as children from juvenile j 
join adult a on j.adult_member_no = a.member_no
join [member] m on a.member_no = m.member_no
where a.state = 'AZ'
group by a.member_no
having COUNT(*) > 2
union
select a.member_no, min(m.firstname), min(m.lastname), COUNT(*) as children from juvenile j 
join adult a on j.adult_member_no = a.member_no
join [member] m on a.member_no = m.member_no
where a.state = 'CA'
group by a.member_no
having COUNT(*) > 3;
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
select chef.EmployeeID as SupervisorID, chef.LastName as Supervisor, emp.EmployeeID, emp.LastName as Employee
from Employees chef
join Employees emp on emp.ReportsTo = chef.EmployeeID
```

* Napisz polecenie, które wyświetla pracowników, którzy nie mają podwładnych
  
```sql
select chef.EmployeeID as SupervisorID, chef.LastName as Supervisor, emp.EmployeeID, emp.LastName as Employee
from Employees chef
left join Employees emp on emp.ReportsTo = chef.EmployeeID
where emp.EmployeeID is null;
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
