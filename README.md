SQL
==============================================


**Structured Query Language (SQL)** — _декларативный_ язык структурированных запросов, с помощью которого пишутся специальные запросы (_скрипты, инструкции_) к базе данных для получения данных и манипулирования ими.

Язык SQL представляет собой набор операторов, которые делятся на определенные группы со своими назначениями. В сокращенном виде эти группы называются **DDL**, **DML**, **DCL** и **TCL**.

## DDL – Data Definition Language

**Data Definition Language (DDL)** – это группа операторов **определения** данных. Другими словами, с помощью операторов, входящих в эту группы, мы определяем структуру базы данных и работаем с объектами этой базы, т.е. создаем, изменяем и удаляем их.

В эту группу входят следующие операторы:

* **CREATE** – используется для создания объектов базы данных;
* **ALTER** – используется для изменения объектов базы данных;
* **DROP** – используется для удаления объектов базы данных.

## DML – Data Manipulation Language

**Data Manipulation Language (DML)** – это группа операторов для манипуляции данными. С помощью этих операторов мы можем добавлять, изменять, удалять и выгружать данные из базы, т.е. манипулировать ими.

В эту группу входят самые распространённые операторы языка SQL:

* **SELECT** – осуществляет выборку данных;
* **INSERT** – добавляет новые данные;
* **UPDATE** – изменяет существующие данные;
* **DELETE** – удаляет данные.

## DCL – Data Control Language

**Data Control Language (DCL)** – группа операторов определения доступа к данным. Иными словами, это операторы для управления разрешениями, с помощью них мы можем разрешать или запрещать выполнение определенных операций над объектами базы данных.

## TCL – Transaction Control Language

**Transaction Control Language (TCL)** – группа операторов для управления транзакциями. Транзакция – это команда или блок команд (инструкций), которые успешно завершаются как единое целое, при этом в базе данных все внесенные изменения фиксируются на постоянной основе или отменяются, т.е. все изменения, внесенные любой командой, входящей в транзакцию, будут отменены.

-------------------------------------------

## Базовый синтаксис SQL команды SELECT

Одна из основных функций SQL — получение данных из СУБД. Для построения всевозможных запросов к базе данных используется оператор **SELECT**. Он позволяет выполнять сложные проверки и обработку данных.

Общая структура запроса

```sql
SELECT [DISTINCT | ALL] поля_таблиц 
[FROM список_таблиц] 
[WHERE условия_на_ограничения_строк]
[GROUP BY условия_группировки]
[HAVING условия_на_ограничения_строк_после_группировки]
[ORDER BY порядок_сортировки [ASC | DESC]]
[LIMIT ограничение_количества_записей]
```

* **DISTINCT** используется для исключения повторяющихся строк из результата
* **ALL** (по умолчанию) используется для получения всех данных, в том числе и повторений
* **FROM** перечисляет используемые в запросе таблицы из базы данных
* **WHERE** условный оператор, который используется для ограничения строк по какому-либо условию
* **GROUP BY** используется для группировки строк
* **HAVING** применяется после группировки строк для фильтрации по значениям агрегатных функций
* **ORDER BY** используется для сортировки. У него есть два параметра:
* **ASC** (по умолчанию) используется для сортировки по возрастанию
* **DESC** по убыванию
* **LIMIT** используется для ограничения количества строк для вывода

---------------------------------

### SQL-псевдонимы

Псевдонимы используются для представления столбцов или таблиц с именем отличным от оригинального. Это может быть полезно для улучшения читабельности имён и создания более короткого наименования столбца или таблицы.

Например, если в таблице есть столбец **good_type_id**, то можно переименовать его просто в **id**, для того, чтобы сделать его более коротким и удобным в использовании в будущем.

Для создания псевдонимов используется оператор AS:

```sql
SELECT 
    good_type_id AS id 
FROM 
    GoodTypes;
```

-----------------------------------------------------

### Еще примеры:

* можно выводить любые строки и числа вместо столбцов:

```sql
SELECT 
    "Hello world", 1;
```

* для того, чтобы вывести **все данные** из таблицы Company, можно использовать символ «*», который буквально означает «все столбцы»:

```sql
SELECT 
    * 
FROM 
    Company;
```

* можно вывести любой столбец, определённый в таблице, например, town_to из таблицы Trip:

```sql
SELECT 
    town_to 
FROM 
    Trip;
```

* можно вывести несколько столбцов. Для этого их нужно перечислить через запятую:

```sql
SELECT 
    member_name, status 
FROM 
    FamilyMembers;
```

* иногда возникают ситуации, в которых нужно получить только уникальные записи, для этого можно использовать DISTINCT. Например, выведем список городов без повторений, в которые летали самолеты:

```sql
SELECT 
    DISTINCT town_to 
FROM 
    Trip;
```

-----------------------------------------------------

## Условный оператор WHERE

Ситуация, когда требуется сделать выборку по определенному условию, встречается очень часто, для этого в операторе **SELECT** существует параметр **WHERE**, после которого следует условие для ограничения строк. Если запись удовлетворяет этому условию, то попадает в результат, иначе отбрасывается.

Общая структура запроса с оператором WHERE

```sql
SELECT поля_таблиц FROM список_таблиц 
WHERE условия_на_ограничения_строк
[логический_оператор другое_условия_на_ограничения_строк];
```

----------------------------------------

### Операторы сравнения

Операторы сравнения служат для сравнения 2 выражений, их результатом может являться ИСТИНА (1), ЛОЖЬ (0) и NULL.

>Результат сравнения с NULL является NULL. Исключением является оператор эквивалентности.

Оператор | Описание
:--:|---
\= | Оператор - равенство
\<\=\> | Оператор - эквивалентность<br/>Аналогичный оператору равенства, с одним лишь исключением: в отличие от него, оператор эквивалентности вернет ИСТИНУ при сравнении NULL <=> NULL
\<\><br/>или<br/>!= | Оператор - неравенство
\< | Оператор - меньше
\<\= | Оператор - меньше или равно
\> | Оператор - больше
\>\= | Оператор - больше или равно

-------------------------------------

## Добавление данных, оператор INSERT

Для добавления новых записей в таблицу предназначен оператор INSERT.

Общая структура запроса с оператором INSERT

```sql
INSERT INTO имя_таблицы [(поле_таблицы, ...)]
VALUES (значение_поля_таблицы, ...)
| SELECT поле_таблицы, ... FROM имя_таблицы ...
```

_В описанной структуре запроса необязательные параметры указаны в квадратных скобках. Вертикальной чертой обозначен альтернативный синтаксис._

Значения можно вставлять перечислением с помощью слова values, перечислив их в круглых скобках через запятую или c помощью оператора select. Таким образом, добавить новые записей можно следующими способами:

```sql
INSERT INTO Goods (good_id, good_name, type)
VALUES (5, 'Table', 2);
```

```sql
INSERT INTO Goods VALUES (5, 'Table', 2);
```

```SQL
INSERT INTO Goods 
SELECT 
    good_id, good_name, type 
FROM 
    Goods 
where 
    good_name = 2;
```

-------------------------------------------------

### Первичный ключ при добавлении новой записи

Следует помнить, что первичный ключ таблицы является уникальным значением и добавление уже существующего значения приведет к ошибке.

При добавлении новой записи с уникальными индексами выбор такого уникального значения может оказаться непростой задачей. Решением может быть дополнительный запрос, направленный на выявление максимального значения первичного ключа для генерации нового уникального значения.

```sql
INSERT INTO Goods 
SELECT 
    COUNT(*) + 1, 'Table', 2 
FROM 
    Goods;
```

В SQL введен механизм его автоматической генерации. Для этого достаточно снабдить первичный ключ good_id атрибутом AUTO_INCREMENT. Тогда при создании новой записи в качестве значения good_id достаточно передать NULL или 0 — поле автоматически получит значение, равное максимальному значению столбца good_id, плюс единица.

```sql
CREATE TABLE Goods (
	good_id INT NOT NULL AUTO_INCREMENT
	...
);
```

```sql
INSERT INTO Goods VALUES (NULL, 'Table', 2);
```
------------------------------

- [X] [ПРАКТИЧЕСКОЕ ЗАДАНИЕ №1](https://github.com/ILYA-NASA/Hello_SQL/blob/main/Creating_and_viewing_tables.sql)
    - [X] создать таблицу с мобильными телефонами, используя графический интерфейс [MySQL Workbench](https://info-comp.ru/install-mysql-on-windows-10); 
    - [X] заполнить таблицу данными;
    - [X] вывести название, производителя и цену для товаров, количество которых превышает две штуки;
    - [X] вывести весь ассортимент товаров марки “Samsung”.

------------------------------

## Функция CASE в MySQL
### Функция CASE проверяет истинность набора условий и в зависимости от результата проверки может возвращать тот или иной результат. Эта функция принимает следующую форму:

```sql
CASE
    WHEN условие_1 THEN результат_1
    WHEN условие_2 THEN результат_2
    .................................
    WHEN условие_N THEN условие_N
    [ELSE альтернативный_результат]
END
```

Возьмем для примера следующую таблицу Products:

```sql
CREATE TABLE Products
(
    Id INT AUTO_INCREMENT PRIMARY KEY,
    ProductName VARCHAR(30) NOT NULL,
    Manufacturer VARCHAR(20) NOT NULL,
    ProductCount INT DEFAULT 0,
    Price DECIMAL NOT NULL
);
```

Выполним запрос к этой таблице и используем функцию CASE:

```sql
SELECT ProductName, ProductCount, 
CASE
    WHEN ProductCount = 1 
        THEN 'Товар заканчивается'
    WHEN ProductCount = 2 
        THEN 'Мало товара'
    WHEN ProductCount = 3 
        THEN 'Есть в наличии'
    ELSE 'Много товара'
END AS Category
FROM Products;
```

------------------------------

## Функция IF в MySQL
### Функция IF в зависимости от результата условного выражения возвращает одно из двух значений. Общая форма функции выглядит следующим образом:
```sql
IF(условие, значение_1, значение_2)
```

Если условие, передаваемое в качестве первого параметра, верно, то возвращается первое значение, иначе возвращается второе значение. Например:

```sql
SELECT ProductName, Manufacturer,
    IF(ProductCount > 3, 'Много товара', 'Мало товара')
FROM Products;
```

------------------------------

## Функция IFNULL в MySQL
### Функция IFNULL проверяет значение некоторого выражения. Если оно равно NULL, то функция возвращает значение, которое передается в качестве второго параметра:

```sql
IFNULL(выражение, значение)
```

Например, возьмем следующую таблицу

```sql
CREATE TABLE Clients
(
    Id INT AUTO_INCREMENT PRIMARY KEY,
    FirstName VARCHAR(20) NOT NULL,
    LastName VARCHAR(20) NOT NULL,
    Phone VARCHAR(20) NULL,
    Email VARCHAR(20) NULL
);
  
INSERT INTO Clients (FirstName, LastName, Phone, Email)
VALUES ('Tom', 'Smith', '+36436734', NULL),
('Bob', 'Simpson', NULL, NULL);
```

И применим при получении данных функцию IFNULL:

```sql
SELECT FirstName, LastName,
        IFNULL(Phone, 'не определено') AS Phone,
        IFNULL(Email, 'неизвестно') AS Email
FROM Clients;
```

------------------------------

## Функция COALESCE в MySQL
### Функция COALESCE принимает список значений и возвращает первое из них, которое не равно NULL:

```sql
COALESCE(выражение_1, выражение_2, выражение_N)
```

Например, выберем из таблицы Clients пользователей и в контактах у них определим либо телефон, либо электронный адрес, если они не равны NULL:

```sql
SELECT FirstName, LastName,
        COALESCE(Phone, Email, 'не определено') AS Contacts
FROM Clients;
```

То есть в данном случае возвращается телефон, если он определен. Если он не определен, то возвращается электронный адрес. Если и электронный адрес не определен, то возвращается строка "не определено".

------------------------------

- [X] [ПРАКТИЧЕСКОЕ ЗАДАНИЕ №2](https://github.com/ILYA-NASA/Hello_SQL/blob/main/Using_CASE.sql)
    - [X] создать таблички "goods" и “sales”. Заполнить их данными; 
    - [X] сгруппировать значения количества в 3 сегмента — меньше 100, 100-300 и больше 300, используя CASE;
    - [X] создать таблицу “orders”, заполнить ее значениями. Показать “полный” статус заказа, используя COALESCE.

------------------------------

# Запросы
## Выборка уникальных значений. Оператор DISTINCT
### С помощью оператора DISTINCT можно выбрать уникальные данные по определенным столбцам.

К примеру, разные товары могут иметь одних и тех же производителей, и, допустим, у нас следующая таблица товаров:

```sql
CREATE TABLE Products
(
    Id INT AUTO_INCREMENT PRIMARY KEY,
    ProductName VARCHAR(30) NOT NULL,
    Manufacturer VARCHAR(20) NOT NULL,
    ProductCount INT DEFAULT 0,
    Price DECIMAL NOT NULL
);
INSERT INTO Products  (ProductName, Manufacturer, ProductCount, Price)
VALUES
('iPhone X', 'Apple', 3, 71000),
('iPhone 8', 'Apple', 3, 56000),
('Galaxy S9', 'Samsung', 6, 56000),
('Galaxy S8', 'Samsung', 2, 46000),
('Honor 10', 'Huawei', 3, 26000);
```

Выберем всех производителей:

```sql
SELECT Manufacturer FROM Products;
```
_Однако при таком запросе производители повторяются._

Теперь применим оператор **DISTINCT** для выборки уникальных значений:

```sql	
SELECT DISTINCT Manufacturer FROM Products;
```

Также мы можем задавать выборку уникальные значения по нескольким столбцам:

```sql	
SELECT DISTINCT Manufacturer, ProductCount FROM Products;
```
_В данном случае для выборки используются столбцы Manufacturer и ProductCount. Из пяти строк только для двух строк эти столбцы имеют повторяющиеся значения. Поэтому в выборке будет 4 строки._

-------------------------------------

## Операторы фильтрации
### Оператор IN

Оператор **IN** определяет набор значений, которые должны иметь столбцы:
```sql	
WHERE выражение [NOT] IN (выражение)
```

Выражение в скобках после IN определяет набор значений. Этот набор может вычисляться динамически на основании, например, еще одного запроса, либо это могут быть константные значения.
Например, выберем товары, у которых производитель либо Samsung, либо Xiaomi, либо Huawei:

```sql	
SELECT * FROM Products
WHERE Manufacturer IN ('Samsung', 'Xiaomi', 'Huawei');
```

Оператор **NOT**, наоборот, позволяет выбрать все строки, столбцы которых не имеют определенных значений:
```sql	
SELECT * FROM Products
WHERE Manufacturer NOT IN ('Samsung', 'Xiaomi', 'Huawei');
```

### Оператор BETWEEN
Оператор **BETWEEN** определяет диапазон значений с помощью начального и конечного значения, которому должно соответствовать выражение:
```sql	
WHERE выражение [NOT] BETWEEN начальное_значение AND конечное_значение
```

Например, получим все товары, у которых цена от 20 000 до 50 000 (начальное и конечное значения также включаются в диапазон):
```sql	
SELECT * FROM Products
WHERE Price BETWEEN 20000 AND 50000;
```

Если надо, наоборот, выбрать те строки, которые не попадают в данный диапазон, то добавляется оператор NOT:
```sql	
SELECT * FROM Products
WHERE Price NOT BETWEEN 20000 AND 50000;
```

Также можно использовать более сложные выражения. Например, получим товары по совокупной стоимости (цена * количество):
```sql	
SELECT * FROM Products
WHERE Price * ProductCount BETWEEN 90000 AND 150000;
```

### Операторы LIKE и REGEXP
Оператор **LIKE** принимает шаблон строки, которому должно соответствовать выражение.
```sql	
WHERE выражение [NOT] LIKE шаблон_строки
```
Для определения шаблона могут применяться ряд специальных символов подстановки:

>* **%** : соответствует любой подстроке, которая может иметь любое количество символов, при этом подстрока может и не содержать ни одного символа. Например, выражение WHERE ProductName LIKE 'Galaxy%' соответствует таким значениям как "Galaxy Ace 2" или "Galaxy S7"

>* **_** : соответствует любому одиночному символу. Например, выражение WHERE ProductName LIKE 'Galaxy S_' соответствует таким значениям как "Galaxy S7" или "Galaxy S8".

Применим оператор LIKE:
```sql	
SELECT * FROM Products
WHERE ProductName LIKE 'iPhone%';
```

**REGEXP** позволяет задать регулярное выражение, которому должно соответствовать значение столбца. В этом плане REGEXP представляет более изощренный и комплексный способ фильтрации, нежели оператор LIKE. REGEXP имеет похожий синтаксис:
```sql	
WHERE выражение [NOT] REGEXP регулярное выражение
```

Регулярное выражение может принимать следующие специальные символы:

>* **^** : указывает на начало строки

>* **$** : указывает на конец строки

>* **.** : соответствует любому одиночному символу

>* **[символы]** : соответствует любому одиночному символу из скобок

>* **[начальный_символ-конечный_символ]** : соответствует любому одиночному символу из диапазона символов

>* **|** : отделяет два шаблона строки, и значение должно соответствовать одну из этих шаблонов

Примеры **REGEXP**:

>* WHERE ProductName REGEXP 'Phone': строка должна содержать "Phone", например, iPhone X, Nokia Phone N, iPhone

>* WHERE ProductName REGEXP '^Phone': строка должна начинаться с "Phone", например, Phone 34, PhoneX

>* WHERE ProductName REGEXP 'Phone$': строка должна заканчиваться на "Phone", например, iPhone, Nokia Phone

>* WHERE ProductName REGEXP 'iPhone [78]';: строка должна содержать либо iPhone 7, либо iPhone 8

>* WHERE ProductName REGEXP 'iPhone [6-8]';: строка должна содержать либо iPhone 6, либо iPhone 7, либо iPhone 8

Например, найдем товары, названия которых содержат либо "Phone", либо "Galaxy":

```sql	
SELECT * FROM Products
WHERE ProductName REGEXP 'Phone|Galaxy';
```

### IS NULL
Оператор **IS NULL** позволяет выбрать все строки, столбцы которых имеют значение NULL:

```sql	
SELECT * FROM Products
WHERE ProductCount IS NULL;
```

С помощью добавления оператора **NOT** можно, наоброт, выбрать строки, столбцы которых не имеют значения NULL:

```sql	
SELECT * FROM Products
WHERE ProductCount IS NOT NULL;
```

------------------------------------

## Сортировка. ORDER BY
### Оператор ORDER BY сортируют значения по одному или нескольких столбцам. 
Например, упорядочим выборку из таблицы Products по столбцу Price:

```sql
SELECT * FROM Products
ORDER BY Price;
```

Также можно производить упорядочивание данных по псевдониму столбца, который определяется с помощью оператора **AS**:

```sql
SELECT ProductName, ProductCount * Price AS TotalSum
FROM Products
ORDER BY TotalSum;
```

В качестве критерия сортировки также можно использовать сложное выражение на основе столбцов:

```sql
SELECT ProductName, Price, ProductCount
FROM Products
ORDER BY ProductCount * Price;
```

### Сортировка по убыванию
По умолчанию данные сортируются по возрастанию, однако с помощью оператора **DESC** можно задать сортировку по убыванию.

```sql
SELECT ProductName, ProductCount
FROM Products
ORDER BY ProductCount DESC;
```

По умолчанию вместо DESC используется оператор **ASC**, который сортирует по возрастанию:

```sql
SELECT ProductName, ProductCount
FROM Products
ORDER BY ProductCount ASC;
```

### Сотировка по нескольким столбцам
При сортировке сразу по нескольким столбцам все эти столбцы указываются через запятую после оператора **ORDER BY**:

```sql
SELECT ProductName, Price, Manufacturer
FROM Products
ORDER BY Manufacturer, ProductName;
```

Здесь строки сначала сортируются по столбцу Manufacturer по возрастанию. Затем если есть две строки, в которых столбец Manufacturer имеет одинаковое значение, то они сортируются по столбцу ProductName также по возрастанию. Но опять же с помощью **ASC** и **DESC** можно отдельно для разных столбцов определить сортировку по возрастанию и убыванию:

```sql
SELECT ProductName, Price, Manufacturer
FROM Products
ORDER BY Manufacturer ASC, ProductName DESC;
```

---------------------------------------------

## Получение диапазона строк. Оператор LIMIT
### Оператор **LIMIT** позволяет извлечь определенное количество строк и имеет следующий синтаксис:

```sql
LIMIT [offset,] rowcount
```

Если оператору LIMIT передается один параметр, то он указывает на количество извлекаемых строк. Если передается два параметра, то первый параметр устанавливает смещение относительно начала, то есть сколько строк нужно пропустить, а второй параметр также указывает на количество извлекаемых строк.

Например, выберем первые три строки:

```sql
SELECT * FROM Products
LIMIT 3;
```

Теперь используем второй параметр и укажем смещение, с которой должна происходить выборка:

```sql
SELECT * FROM Products
LIMIT 2, 3;
```

_В данном случае пропускаются две первые строки и извлекаются следующие 3 строки._

Как правило, оператор LIMIT используетс вместе с оператором **ORDER BY**:

```sql
SELECT * FROM Products
ORDER BY ProductName
LIMIT 2, 3;
```

-----------------------------------------------------

## Агрегатные функции
### Агрегатные функции вычисляют некоторые скалярные значения в наборе строк. 
В MySQL есть следующие агрегатные функции:

* **AVG**: вычисляет среднее значение

* **SUM**: вычисляет сумму значений

* **MIN**: вычисляет наименьшее значение

* **MAX**: вычисляет наибольшее значение

* **COUNT**: вычисляет количество строк в запросе

Все агрегатные функции принимают в качестве параметра выражение, которое представляет критерий для определения значений. 
Зачастую, в качестве выражения выступает название столбца, над значениями которого надо проводить вычисления.

Выражения в функциях **AVG** и **SUM** должно представлять числовое значение (например, столбец, который хранит числовые значения). 
Выражение в функциях **MIN**, **MAX** и **COUNT** может представлять числовое или строковое значение или дату.

Все агрегатные функции за исключением **COUNT(*)** игнорируют значения NULL.

### Avg
Функция **Avg** возвращает среднее значение на диапазоне значений столбца таблицы.

Например, пусть есть следующая таблица товаров Products:
```sql
CREATE TABLE Products
(
    Id INT AUTO_INCREMENT PRIMARY KEY,
    ProductName VARCHAR(30) NOT NULL,
    Manufacturer VARCHAR(20) NOT NULL,
    ProductCount INT DEFAULT 0,
    Price DECIMAL NOT NULL
);
   
INSERT INTO Products(ProductName, Manufacturer, ProductCount, Price) 
VALUES
('iPhone X', 'Apple', 3, 76000),
('iPhone 8', 'Apple', 2, 51000),
('iPhone 7', 'Apple', 5, 32000),
('Galaxy S9', 'Samsung', 2, 56000),
('Galaxy S8', 'Samsung', 1, 46000),
('Honor 10', 'Huawei', 5, 28000),
('Nokia 8', 'HMD Global', 6, 38000)
```

Найдем среднюю цену товаров из базы данных:
```sql
SELECT AVG(Price) AS Average_Price FROM Products
```
_Для поиска среднего значения в качестве выражения в функцию передается столбец Price. Для получаемого значения устанавливается псевдоним Average_Price, хотя в принципе устанавливать псевдоним необязательно._

На этапе выборки можно применять фильтрацию. Например, найдем среднюю цену для товаров определенного производителя:
```sql
SELECT AVG(Price) FROM Products
WHERE Manufacturer='Apple'
```

Также можно находить среднее значение для более сложных выражений. Например, найдем среднюю сумму всех товаров, учитывая их количество:
```sql
SELECT AVG(Price * ProductCount) FROM Products
```

### Count
Функция **Count** вычисляет количество строк в выборке. Есть две формы этой функции. Первая форма **COUNT(*)** подсчитывает число строк в выборке:
```sql
SELECT COUNT(*) FROM Products
```

Вторая форма функции вычисляет количество строк по определенному столбцу, при этом строки со значениями NULL игнорируются:
```sql
SELECT COUNT(Manufacturer) FROM Products
```

### Min и Max
Функции **Min** и **Max** вычисляют минимальное и максимальное значение по столбцу соответственно. Например, найдем минимальную цену среди товаров:
```sql
SELECT MIN(Price), MAX(Price) FROM Products
```
_Данные функции также игнорируют значения NULL и не учитывают их при подсчете._

### Sum
Функция **Sum** вычисляет сумму значений столбца. Например, подсчитаем общее количество товаров:
```sql
SELECT SUM(ProductCount) FROM Products
```

Также вместо имени столбца может передаваться вычисляемое выражение. Например, найдем общую стоимость всех имеющихся товаров:
```sql
SELECT SUM(ProductCount * Price) FROM Products
```

### ALL и DISTINCT
По умолчанию все вышеперечисленных пять функций учитывают все строки выборки для вычисления результата. Но выборка может содержать повторяющие значения. Если необходимо выполнить вычисления только над уникальными значениями, исключив из набора значений повторяющиеся данные, то для этого применяется оператор **DISTINCT**.
```sql
SELECT COUNT(DISTINCT Manufacturer) FROM Products
```

По умолчанию вместо **DISTINCT** применяется оператор **ALL**, который выбирает все строки:
```sql
SELECT COUNT(ALL Manufacturer) FROM Products
```

В данном случае мы видим, что производители могут повторяться в таблице, так как некоторые товары могут иметь одних и тех же производителей. Поэтому чтобы подсчитать количество уникальных производителей, необходимо использовать оператор DISTINCT.

Так как **ALL** неявно подразумевается при отсутствии **DISTINCT**, то его можно не указывать.

### Комбинирование функций
Объединим применение нескольких функций:
```sql
SELECT COUNT(*) AS ProdCount,
       SUM(ProductCount) AS TotalCount,
       MIN(Price) AS MinPrice,
       MAX(Price) AS MaxPrice,
       AVG(Price) AS AvgPrice
FROM Products
```

----------------------------------------------------------

