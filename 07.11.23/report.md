## Draw.io
![Снимок экрана 2023-11-07 161408](https://github.com/Tufuru/SQL/assets/144116593/b000885e-5f3d-411e-9d11-8a5b09632c45)
### Описание
Общая база данных для ломбардов с таблицами покупателей, продавцов, ломбардов, товаров и владельцев ломбардов
## Buyer
```sql
create table buyer
( id bigint primary key ,
  name varchar not null,
  surname varchar not null,
  age integer not null default 0,
  phone varchar not null
  );
  
insert into buyer values (1, 'Denis', 'Chernov', 17, '+7 (993) 453-47-01');
insert into buyer values (2, 'Vladimir', 'Kononova', 25, '+7 (905) 786-87-98');
insert into buyer values (3, 'Anatoliy', 'Shulgin', 38, '+7 (914) 897-74-15');
insert into buyer values (4, 'Katya', 'Kulagina', 16, '+7 (933) 269-11-70');
insert into buyer values (5, 'Anna', 'Alice', 18, '+7 (995) 904-23-40');
insert into buyer values (6, 'Natalya', 'Sorokina', 43, '+7 (929) 048-72-41');
insert into buyer values (7, 'Ekaterina', 'Frolova', 55, '+7 (987) 457-26-99');
insert into buyer values (8, 'Kirill', 'Ilyin', 21, '+7 (902) 845-47-32');
insert into buyer values (9, 'Kamil', 'Petrov', 32, '+7 (978) 005-05-57');
insert into buyer values (10, 'Viktor', 'Andrianov', 15, '+7 (980) 150-12-22');
```
