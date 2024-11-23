# Select

## Northwind

* Wybierz nazwy i adresy wszystkich klientów
* Wybierz nazwiska i numery telefonów pracowników
* Wybierz nazwy i ceny produktów
* Pokaż wszystkie kategorie produktów (nazwy i opisy)
* Pokaż nazwy i adresy stron www dostawców
* Wybierz nazwy i adresy wszystkich klientów mających siedziby w Londynie
* Wybierz nazwy i adresy wszystkich klientów mających siedziby we Francji lub w Hiszpanii
* Wybierz nazwy i ceny produktów o cenie jednostkowej pomiędzy 20.00 a 30.00
* Wybierz nazwy i ceny produktów z kategorii 'Meat/Poultry'
* Wybierz nazwy produktów oraz inf. o stanie magazynu dla produktów dostarczanych przez firmę ‘Tokyo Tradersʼ
* Wybierz nazwy produktów których nie ma w magazynie
* Szukamy informacji o produktach sprzedawanych w butelkach (‘bottleʼ)
* Wyszukaj informacje o stanowisku pracowników, których nazwiska zaczynają się na literę z zakresu od B do L
* Wyszukaj informacje o stanowisku pracowników, których nazwiska zaczynają się na literę B lub L
* Znajdź nazwy kategorii, które w opisie zawierają przecinek
* Znajdź klientów, którzy w swojej nazwie mają w którymś miejscu słowo 'Store'
* Szukamy informacji o produktach o cenach mniejszych niż 10 lub większych niż 20
* Wybierz nazwy i ceny produktów o cenie jednostkowej pomiędzy 20.00 a 30.00
* Wybierz zamówienia złożone w 1997 roku
* Napisz instrukcję select tak aby wybrać numer zlecenia, datę zamówienia, numer klienta dla wszystkich niezrealizowanych jeszcze zleceń, dla których krajem odbiorcy jest Argentyna
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
select * from Orders where YEAR(OrderDate) = 1997 order by MONTH(OrderDate) desc, Freight;
```

* Napisz polecenie, które oblicza wartość każdej pozycji zamówienia o numerze 10250

```sql
select ProductID, UnitPrice*Quantity*(1- Discount) from [Order Details] od where OrderID = 10250;
```

* Napisz polecenie które dla każdego dostawcy (supplier) pokaże pojedynczą kolumnę
zawierającą nr telefonu i nr faksu w formacie (numer telefonu i faksu mają być
oddzielone przecinkiem)

```sql
SELECT CompanyName, Phone + isnull(', ' + Fax, '') as Kontakt from Suppliers;
```

## Library
