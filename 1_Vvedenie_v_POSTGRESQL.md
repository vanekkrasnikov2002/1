## Тема 1. ВВЕДЕНИЕ В POSTGRESQL

## Основные концепции: БД, СУБД

- **База данных (БД)** - это структурированное собрание данных, организованных для эффективного хранения, управления и доступа.

- **Система управления базами данных (СУБД)** - это программное обеспечение, которое обеспечивает управление и взаимодействие с базами данных. Оно предоставляет средства для создания, изменения и извлечения данных из БД.

## Реляционная модель и SQL

- **Реляционная модель** - это структура организации данных, где информация хранится в виде таблиц (реляций) с отношениями между ними.

- **SQL (Structured Query Language)** - это язык запросов, используемый для взаимодействия с реляционными базами данных. Он позволяет выполнять операции, такие как SELECT, INSERT, UPDATE и DELETE.

## Почему стоит выбрать PostgreSQL в качестве СУБД?

- **Открытое ПО**: PostgreSQL является open source СУБД с богатой историей разработки и активным сообществом.

- **Мощные функциональные возможности**: PostgreSQL поддерживает множество продвинутых функций, включая геопространственные данные, JSON, индексы и многое другое.

- **Высокая надежность и производительность**: PostgreSQL известен своей стабильностью и возможностью обработки больших объемов данных.


## Обзор типов данных в PostgreSQL

PostgreSQL поддерживает разнообразные типы данных, которые можно использовать для определения структуры таблиц и хранения различных видов информации.

1. **Целые числа:**
   - `INTEGER` - 4-байтовое целое число.
   - `SMALLINT` - 2-байтовое целое число (маленькое целое).
   - `BIGINT` - 8-байтовое целое число (большое целое).

2. **Вещественные числа:**
   - `REAL` - 4-байтовое вещественное число с плавающей запятой.
   - `DOUBLE PRECISION` - 8-байтовое вещественное число с плавающей запятой.

3. **Текстовые строки:**
   - `CHAR(n)` - Фиксированная длина строки до n символов.
   - `VARCHAR(n)` - Строка переменной длины с максимальной длиной n символов.
   - `TEXT` - Строка переменной длины без ограничения длины.

4. **Дата и время:**
   - `DATE` - Дата (год, месяц, день).
   - `TIME` - Время (часы, минуты, секунды).
   - `TIMESTAMP` - Дата и время вместе.
   - `INTERVAL` - Интервал времени или даты.

5. **Булев тип:**
   - `BOOLEAN` - Логический тип данных (TRUE/FALSE).




Пример создания таблицы с использованием разных типов данных:

```sql
CREATE TABLE Example (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    birthdate DATE,
    is_active BOOLEAN,
    data JSONB
);
```


## Как создать базу данных

```sql
CREATE DATABASE имя_базы_данных;
```

## Как создать и удалить таблицу в БД

```sql
CREATE TABLE publisher
(
	publisher_id integer PRIMARY KEY,
	org_name varchar(128) NOT NULL,
	address text NOT NULL
);

CREATE TABLE book
(
	book_id integer PRIMARY KEY,
	title text NOT NULL,
	isbn varchar(32) NOT NULL
);

DROP TABLE publisher;
DROP TABLE book;
```

## Отношение "один ко многим"


```sql
-- Создание таблицы Orders
CREATE TABLE publisher
(
	publisher_id integer PRIMARY KEY,
	org_name varchar(128) NOT NULL,
	address text NOT NULL
);

CREATE TABLE book
(
	book_id integer PRIMARY KEY,
	title text NOT NULL,
	isbn varchar(32) NOT NULL
);

INSERT INTO book
VALUES
(1, 'The Diary of a Young Girl', '0199535566'),
(2, 'Pride and Prejudice', '9780307594006'),
(3, 'To Kill a Mockingbird', '0446310786'),
(4, 'The Book of Gutsy Women: Favorite Stories of Courage and Resilience', '1501178415'),
(5, 'War and Peace', '1788886526');

INSERT INTO publisher
VALUES
(1, 'Everyman''s Library', 'NY'),
(2, 'Oxford University Press', 'NY'),
(3, 'Grand Central Publishing', 'Washington'),
(4, 'Simon & Schuster', 'Chicago');

SELECT * FROM publisher;

ALTER TABLE book
ADD COLUMN fk_publisher_id;

ALTER TABLE book
ADD CONSTRAINT fk_book_publisher
FOREIGN KEY(fk_publisher_id) REFERENCES publisher(publisher_id);
```


## Отношение "один к одному" в теории

В отношении "один к одному" каждая запись в одной таблице связана с одной и только одной записью в другой таблице.

## Отношение "один к одному" на практике

## На практике отношение "один к одному"
 Есть таблица `person` и таблица `passport`, где каждая запись в таблице `person` имеет связанную запись в таблице `passport`.

```sql
-- Создание таблицы person
CREATE TABLE person
(
	person_id int PRIMARY KEY,
	first_name varchar(64) NOT NULL,
	last_name varchar(64) NOT NULL
);

CREATE TABLE passport
(
	passport_id int PRIMARY KEY,
	serial_number int NOT NULL,
	fk_passport_person int UNIQUE REFERENCES person(person_id),
	registration text NOT NULL
);

INSERT INTO person VALUES (1, 'John', 'Snow');
INSERT INTO person VALUES (2, 'Ned', 'Stark');
INSERT INTO person VALUES (3, 'Rob', 'Baratheon');

INSERT INTO passport VALUES (1, 123456, 1, 'Winterfell');
INSERT INTO passport VALUES (2, 789012, 2, 'Winterfell');
INSERT INTO passport VALUES (3, 345678, 3, 'King''s Landing');
```



## Отношение "многие ко многим"

Отношение "многие ко многим" обычно связывает две другие таблицы.

```sql
-- Создание таблицы Authors
CREATE TABLE Authors (
    author_id SERIAL PRIMARY KEY,
    author_name VARCHAR(100) NOT NULL
);

-- Создание таблицы Books
CREATE TABLE Books (
    book_id SERIAL PRIMARY KEY,
    book_title VARCHAR(200) NOT NULL
);

-- Создание таблицы, связывающей книги и авторов
CREATE TABLE BookAuthors (
    book_id INT,
    author_id INT,
    PRIMARY KEY (book_id, author_id),
    FOREIGN KEY (book_id) REFERENCES Books(book_id),
    FOREIGN KEY (author_id) REFERENCES Authors(author_id)
);
```






