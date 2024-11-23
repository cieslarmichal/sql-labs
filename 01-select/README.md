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
select member_no, fine_paid from loanhist where YEAR(in_date) = 2001 and MONTH (in_date) = 11 and fine_paid is not null;
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
WHERE fine_assessed * 2  - fine_assessed > 3
```

* Napisz polecenie, które generuje pojedynczą kolumnę: first_name + middel_initial + dwie litery z last_name dla nazwiska Anderson

```sql
select LOWER(firstname + middleinitial + SUBSTRING(lastname, 0, 3)) as email_name from [member] WHERE lastname = 'Anderson';
```

* Napisz polecenie, które wybiera title i title_no z tablicy title w formacie: The title is: title, title_no

```sql
select 'The title is: ' + title +', title number ' + CAST(title_no as varchar) from title;
```
