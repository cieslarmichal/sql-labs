# Select

## Northwind

* Wybierz nazwy i adresy wszystkich klientów mających siedziby w Londynie

```sql
select CompanyName, Address from Customers where City = 'London';
```

* Wybierz nazwy i adresy wszystkich klientów mających siedziby we Francji lub w Hiszpanii

```sql
select CompanyName, Address from Customers where Country in ('France', 'Spain');
```

* Wybierz nazwy i ceny produktów o cenie jednostkowej pomiędzy 20.00 a 30.00

```sql
select CompanyName, Address from Customers where Country in ('France', 'Spain');
```

* Wybierz nazwy i ceny produktów z kategorii 'Meat/Poultry'

```sql
select ProductName, UnitPrice from Products p join Categories c on p.CategoryID = c.CategoryID 
where c.CategoryName = 'Meat/Poultry' order by 2;
```

* Wybierz nazwy produktów oraz inf. o stanie magazynu dla produktów dostarczanych przez firmę ‘Tokyo Tradersʼ

```sql
select ProductName, UnitsInStock from Products p join Suppliers s on p.SupplierID = s.SupplierID
where s.CompanyName = 'Tokyo Traders' order by 2;
```

* Wybierz nazwy produktów których nie ma w magazynie

```sql
select ProductName from Products where UnitsInStock = 0;
```

* Szukamy informacji o produktach sprzedawanych w butelkach (‘bottleʼ)
  
```sql
select ProductName, QuantityPerUnit from Products where QuantityPerUnit like '%bottle%';
```

* Wyszukaj informacje o stanowisku pracowników, których nazwiska zaczynają się na literę z zakresu od B do L

```sql
select LastName, Title from Employees where LastName between 'B' and 'L';
```

* Wyszukaj informacje o stanowisku pracowników, których nazwiska zaczynają się na literę B lub L

```sql
select LastName, Title from Employees where LastName like 'B%' or LastName like 'L%';
```

* Znajdź nazwy kategorii, które w opisie zawierają przecinek

```sql
select CategoryName, Description from Categories where Description like '%,%';
```

* Znajdź klientów, którzy w swojej nazwie mają w którymś miejscu słowo 'Store'

```sql
select CompanyName from Customers where CompanyName like '%Store%';
```

* Szukamy informacji o produktach o cenach mniejszych niż 10 lub większych niż 20

```sql
select ProductName, UnitPrice from Products where UnitPrice < 10 or UnitPrice > 20;
```

* Wybierz nazwy i ceny produktów o cenie jednostkowej pomiędzy 20.00 a 30.00
  
```sql
select ProductName, UnitPrice from Products where UnitPrice between 10 and 20 order by 2;
```

* Wybierz zamówienia złożone w 1997 roku
  
```sql
select * from Orders where year(OrderDate) = 1997;
```

* Napisz instrukcję select tak aby wybrać numer zlecenia, datę zamówienia, numer klienta dla wszystkich niezrealizowanych jeszcze zleceń, dla których krajem odbiorcy jest Argentyna
  
```sql
select OrderID, OrderDate, CustomerID from Orders where ShipCountry = 'Argentina' and ShippedDate is null;
```

* Wybierz nazwy i kraje wszystkich klientów, wyniki posortuj według kraju, w ramach danego kraju nazwy firm posortuj alfabetycznie

```sql
select CompanyName, Country from Customers order by Country, CompanyName;
```

* Wybierz nazwy i kraje wszystkich klientów mających siedziby we Francji lub w
Hiszpanii, wyniki posortuj według kraju, w ramach danego kraju nazwy firm posortuj
alfabetycznie

```sql
select CompanyName, Country from Customers where Country in ('Spain', 'France') order by Country, CompanyName;
```

* Wybierz zamówienia złożone w 1997 r. Wynik po sortuj malejąco wg numeru
miesiąca, a w ramach danego miesiąca rosnąco według ceny za przesyłkę

```sql
select * from Orders where year(OrderDate) = 1997 order by month(OrderDate) desc, Freight;
```

* Napisz polecenie, które oblicza wartość każdej pozycji zamówienia o numerze 10250

```sql
select ProductID, UnitPrice*Quantity*(1- Discount) from [Order Details] od where OrderID = 10250;
```

* Napisz polecenie które dla każdego dostawcy (supplier) pokaże pojedynczą kolumnę
zawierającą nr telefonu i nr faksu w formacie (numer telefonu i faksu mają być
oddzielone przecinkiem)

```sql
select CompanyName, Phone + isnull(', ' + Fax, '') as Kontakt from Suppliers;
```

## Library

* Napisz polecenie, które wybiera tytuł o numerze 10

```sql
select title from title where title_no=10;
```

* Napisz polecenie select, za pomocą którego uzyskasz numer książki (nr tyułu) i autora dla wszystkich książek, których autorem jest Charles Dickens lub Jane Austen

```sql
select title_no, author from title where author in ('Charles Dickens', 'Jane Austen');
```

* Napisz polecenie, które wybiera numer tytułu i tytuł dla wszystkich książek, których tytuły zawierających słowo 'adventure'

```sql
select title_no, title from title where title like '%adventure%';
```

* Napisz polecenie, które wybiera numer czytelnika, oraz zapłaconą karę dla wszystkich książek, tore zostały zwrócone w listopadzie 2001

```sql
select member_no, fine_paid from loanhist where year(in_date) = 2001 and month (in_date) = 11 and fine_paid is not null;
```

* Napisz polecenie, które wybiera wszystkie unikalne pary miast i stanów z tablicy adult.

```sql
select DISTINCT city, state from adult;
```

* Napisz polecenie, które wybiera wszystkie tytuły z tablicy title i wyświetla je w porządku alfabetycznym.

```sql
select title from title order by title;
```

* Napisz polecenie, które wybiera numer członka biblioteki, isbn książki i wartość naliczonej kary dla wszystkich wypożyczeń, dla których naliczono karę
stwórz kolumnę wyliczeniową (double_fine) zawierającą podwojoną wartość kolumny fine_assessed
stwórz kolumnę o nazwie diff, zawierającą różnicę wartości w kolumnach double_fine i fine_assessed wybierz wiersze dla których wartość w kolumnie diff jest większa niż 3

```sql
select member_no, isbn,fine_assessed, fine_assessed * 2 as double_fine, fine_assessed * 2  - fine_assessed as diff from loanhist
where fine_assessed * 2  - fine_assessed > 3
```

* Napisz polecenie, które generuje pojedynczą kolumnę: first_name + middel_initial + dwie litery z last_name dla nazwiska Anderson

```sql
select LOWER(firstname + middleinitial + SUBSTRING(lastname, 0, 3)) as email_name from [member] where lastname = 'Anderson';
```

* Napisz polecenie, które wybiera title i title_no z tablicy title w formacie: The title is: title, title_no

```sql
select 'The title is: ' + title +', title number ' + CAST(title_no as varchar) from title;
```
