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
## Owner
```sql
create table owner
( id bigint primary key ,
  name varchar not null,
  surname varchar not null,
  phone varchar not null
  );
  
insert into owner values (1,'Varvara','Ustinova','8(495)878-07-02');
insert into owner values (2,'Mikhail','Korchagin','8(495)377-96-92');
insert into owner values (3,'Andrey','Gusev','8(495)858-62-57');
insert into owner values (4,'Miron','Kozyrev','8(495)119-10-86');
insert into owner values (5,'Vera','Bykova','8(495)550-36-22');
insert into owner values (6,'Tatiana','Alekseeva','8(495)909-59-18');
insert into owner values (7,'Yaroslav','Kapustin','8(495)775-93-39');
insert into owner values (8,'Karina','Volkova','8(495)486-57-24');
insert into owner values (9,'Alexey','Mayorov','8(495)856-47-94');
insert into owner values (10,'Varvara','Martynova','8(495)033-95-27');
```
## Shop
```sql
create table shop
( id bigint primary key ,
  name varchar not null,
  address varchar,
  owner_id bigint,
  phone varchar not null ,
  FOREIGN KEY (owner_id) REFERENCES owner(id)
);
  
insert into shop values (1,'MarketMaster','Oktyabrskaya Ul., bld. 188/2, appt. 10',1,'8(488)184-15-71');
insert into shop values (2,'CreativeMarketHub','Ryabikova B-R, bld. 22/А, appt. 123',2,'8(488)116-37-38');
insert into shop values (3,'InnovativeBrand','Engelsa, bld. 42/А, appt. 38',3,'8(488)827-59-04');
insert into shop values (4,'BrandGenius','Tereshkovoy Ul., bld. 29/А, appt. 50',4,'8(488)496-91-19');
insert into shop values (5,'DreamSellers','Minskaya, bld. 49, appt. 101',5,'8(488)044-61-62');
insert into shop values (6,'SalesSuccess','Industrialnaya, bld. 18, appt. 54',6,'8(488)896-52-48');
insert into shop values (7,'Glitter','Korobova, bld. 16/1, appt. 24',7,'8(488)656-65-16');
insert into shop values (8,'Pawn King','Turgeneva, bld. 219, appt. 56',8,'8(488)139-25-95');
insert into shop values (9,'Pocket Pawnshop','Selezneva, bld. 146, appt. 91',9,'8(488)302-57-47');
insert into shop values (10,'Golden Spring','Komsomolskiy Per., bld. 5, appt. 114',10,'8(488)431-68-97');
```
## Seller
```sql
create table seller
( id bigint primary key ,
  name varchar not null,
  surname varchar not null,
  phone varchar not null
  );
  
insert into seller values (1,'Miron','Ulyanov','8(954)075-68-55');
insert into seller values (2,'Anastasia','Petukhova','8(954)246-56-66');
insert into seller values (3,'Victoria','Maslennikova','8(954)405-58-98');
insert into seller values (4,'Ilya','Popov','8(954)588-89-93');
insert into seller values (5,'Stepan','Korneev','8(954)796-65-25');
insert into seller values (6,'Leonid','Balashov','8(954)796-65-25');
insert into seller values (7,'Anton','Larionov','8(954)796-65-25');
insert into seller values (8,'Alexander','Solovyov','8(954)565-46-94');
insert into seller values (9,'Arina','Grigorieva','8(954)909-43-51');
insert into seller values (10,'Semyon','Mironov','8(954)743-54-98');
```
## Products
```sql
create table products
( id bigint primary key ,
  name varchar not null,
  price numeric not null default 0,
  shop_id bigint,
  buyer_id bigint,
  seller_id bigint,
  FOREIGN KEY (shop_id) REFERENCES shop(id),
  FOREIGN KEY (buyer_id) REFERENCES buyer(id),
  FOREIGN KEY (seller_id) REFERENCES seller(id)
  );
  
insert into products values (1,'Computer',1200,1,1,1);
insert into products values (2,'photo album',1900,2,2,2);
insert into products values (3,'outlet',12876,3,3,3);
insert into products values (4,'sketch pad',2301,4,4,4);
insert into products values (5,'greeting card',5064,5,5,5);
insert into products values (6,'hair brush',4500,6,6,6);
insert into products values (7,'Wifi Router',3200,7,7,7);
insert into products values (8,'beanie',2230,8,8,8);
insert into products values (9,'hoodie',1000,9,9,9);
insert into products values (10,'speakers',500,10,10,10);
```
## View products_shop
```sql
DROP VIEW IF EXISTS products_shop;
CREATE VIEW products_shop AS 
SELECT products.name AS products_name, shop.name
FROM products
JOIN shop ON products.shop_id = shop.id;
```
![image](https://github.com/Tufuru/SQL/assets/144116593/fcf7f14c-4834-4107-9962-9b1493159897)

## View buyer_seller
```sql
DROP VIEW IF EXISTS buyer_seller;
CREATE VIEW buyer_seller AS 
SELECT buyer.name AS buyer_name, seller.name AS seller_name
FROM products
JOIN buyer ON products.buyer_id = buyer.id
JOIN seller ON products.seller_id = seller.id;
```
![image](https://github.com/Tufuru/SQL/assets/144116593/b8e57cad-6c7d-4e4f-a9cf-d0745ee0026b)

## View owner_shop
```sql
DROP VIEW IF EXISTS owner_shop;
CREATE VIEW owner_shop AS
SELECT owner.name AS owner_name, shop.name AS shop_name
FROM shop
JOIN owner ON shop.owner_id =owner.id;
```
![image](https://github.com/Tufuru/SQL/assets/144116593/84fd8613-46f6-465d-93e4-1ee8340b62e6)
