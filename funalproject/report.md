## Draw.io
![base](https://github.com/Tufuru/SQL/assets/144116593/609c2e13-e3b5-48c7-9827-15f6f76b1220)
### Описание
База данных для сети ломбардов
## Tables
### Materials

```sql
create table materials
( id bigint primary key ,
  title varchar not null
  );
```

### body_types

```sql
create table body_types
( id bigint primary key ,
  title varchar not null
  );
```

### item

```sql
create table item
( id bigint primary key ,
  title varchar not null,
  material_id bigint not null,
  img varchar,
  body_types_id bigint not null,
  weight numeric not null,
  FOREIGN KEY (material_id) REFERENCES materials(id),
  FOREIGN KEY (body_types_id) REFERENCES body_types(id)
  );
```

### lombards

```sql
create table lombards
( id bigint primary key ,
  address varchar not null
  );
```

### deal_type

```sql
create table deal_type
( id bigint primary key ,
  title varchar not null
  );
```

### conf_data

```sql
create table conf_data
( id bigint primary key ,
  pasport_seria varchar not null,
  pasport_number varchar not null,
  issued varchar not null,
  vhen date not null,
  client_id bigint not null
  --FOREIGN KEY (client_id) REFERENCES client(id)
  );
 
```

### client

```sql
create table client
 ( id bigint primary key ,
  lastname varchar not null,
  firstname varchar not null,
  patrinimyc varchar not null,
  client_conf_data_id bigint not null
  --FOREIGN KEY (client_conf_data_id) REFERENCES conf_data(id)
  );
```

### employee

```sql
create table employee
 ( id bigint primary key ,
  firstname varchar not null,
  lastname varchar not null,
  patrinimyc varchar not null,
  employee_conf_data_id bigint not null
  --FOREIGN KEY (employee_conf_data_id) REFERENCES employee_data(id)
  );
```

### employee_data

```sql
create table employee_data
( id bigint primary key ,
  pasport_seria varchar not null,
  pasport_number varchar not null,
  issued varchar not null,
  vhen varchar not null,
  employee_id bigint not null
  --FOREIGN KEY (employee_id) REFERENCES employee(id)
  );
  
```

### employee_working_graphic

```sql
create table employee_working_graphic
( id bigint primary key ,
  data varchar not null,
  employee_id bigint not null,
  FOREIGN KEY (employee_id) REFERENCES employee(id)
  );
```

### deals

```sql
create table deals
( id bigint primary key ,
  client_id bigint not null,
  item_id bigint not null,
  price numeric not null default 0,
  data date not null,
  lombards_id bigint not null,
  deal_type_id bigint not null,
  employee_id bigint not null,
  FOREIGN KEY (client_id) REFERENCES client(id),
  FOREIGN KEY (item_id) REFERENCES item(id),
  FOREIGN KEY (lombards_id) REFERENCES lombards(id),
  FOREIGN KEY (deal_type_id) REFERENCES deal_type(id),
  FOREIGN KEY (employee_id) REFERENCES employee(id)
  );
```
## View
### view_full_deals
```sql
create VIEW view_full_deals AS
	SELECT client.id AS "client-ID", 
    client.firstname AS "client-name", 
    item.title AS "item-name", 
    body_types.title AS "body-type-name",
    deals.price,deal_type.title,employee.id AS "employee-id",
    employee.firstname AS "employee-name",deals.data AS "deal-date"
	FROM deals
	JOIN client ON deals.client_id = client.id
	JOIN item ON deals.item_id = item.id
	JOIN body_types ON item.body_types_id = body_types.id
	JOIN employee ON deals.employee_id = employee.id
	JOIN deal_type ON deals.deal_type_id = deal_type.id;
```
### view_bying_item
```sql
create VIEW view_bying_item AS 
	SELECT client.firstname AS "client_name", item.title AS "item_name"
	FROM deals
	JOIN client ON deals.client_id = client.id
	JOIN item ON deals.item_id = item.id
	WHERE deals.deal_type_id = 1;
```

### view_fk_item
```sql
create VIEW view_fk_item AS 
	SELECT item.*
	FROM item
	JOIN body_types ON item.body_types_id = body_types.id
	JOIN materials ON item.material_id = materials.id;

```
### view_working_graph
```sql
create VIEW view_working_graph AS 
	SELECT employee_working_graphic.data AS "weekday", 
  employee.firstname AS "name-employee"
	FROM employee_working_graphic
	JOIN employee ON employee_working_graphic.employee_id = employee.id;
```

## Triggers and function


### working_graphic_examination
```sql
CREATE OR REPLACE FUNCTION working_graphic_examination() RETURNS TRIGGER AS $error$
BEGIN
	IF NEW.date == "date" THEN
		RAISE EXCEPTION 'ERROR!';
	END IF;
	IF NEW.employee_id == employee_id THEN
		RAISE EXCEPTION 'ERROR!';
	END IF;

END;
$error$ LANGUAGE 'plpgsql';
CREATE TRIGGER error BEFORE INSERT OR UPDATE ON employee_working_graphic
FOR EACH ROW EXECUTE FUNCTION working_graphic_examination();
```

### lombards_examination
```sql
CREATE OR REPLACE FUNCTION lombards_examination() RETURNS TRIGGER AS 
$lombards_examination$
BEGIN
	IF NEW.address == "address" THEN
		RAISE EXCEPTION 'ERROR!';
	END IF;
END;
$lombards_examination$ LANGUAGE 'plpgsql';
CREATE TRIGGER mytrig BEFORE INSERT OR UPDATE ON lombards
FOR EACH ROW EXECUTE FUNCTION lombards_examination();
```

### average_of_days_sold
```sql
CREATE FUNCTION average_of_days_sold(i char) RETURNS integer AS $$
	DECLARE
	res integer;
	BEGIN
		WITH bought_dates AS (
			SELECT item_id, deals.data AS bought_date FROM deals
			JOIN item ON deals.item_id = item.id
			JOIN body_types ON item.body_types_id = body_types.id
			WHERE deal_type_id = 1 AND body_types.title = i
		), sales_dates AS (
			SELECT item_id, deals.data AS sale_date  FROM deals
			JOIN item ON deals.item_id = item.id 
			JOIN body_types ON item.body_types_id = body_types.id
			WHERE deal_type_id = 2 AND body_types.title = i
		), between_dates AS (
			SELECT * FROM bought_dates
			JOIN sales_dates ON bought_dates.item_id = sales_dates.item_id
		)
		SELECT AVG(sale_date - bought_date) FROM between_dates into res;
		RETURN res;
	END;
$$ LANGUAGE 'plpgsql';
```

### insert
```sql
insert into materials values (1,'gold 585');
insert into materials values (2,'gold 750');
insert into materials values (3,'gold 958');
insert into materials values (4,'gold 999');

insert into body_types values (1,'Bracelets');
insert into body_types values (2,'Suspensions');
insert into body_types values (3,'Chains');
insert into body_types values (4,'Earring');
insert into body_types values (5,'Ring');

insert into item values (1,'q',1,'img',5,12);
insert into item values (2,'w',1,'img',5,13);
insert into item values (3,'e',2,'img',5,15);
insert into item values (4,'r',3,'img',4,20);
insert into item values (5,'t',1,'img',5,18);
insert into item values (6,'y',4,'img',3,12);
insert into item values (7,'u',1,'img',3,16);
insert into item values (8,'i',2,'img',4,8);
insert into item values (9,'o',2,'img',5,22);
insert into item values (10,'p',3,'img',1,19);
insert into item values (11,'a',3,'img',2,13);
insert into item values (12,'s',4,'img',5,15);
insert into item values (13,'d',3,'img',5,16);
insert into item values (14,'f',1,'img',4,12);
insert into item values (15,'g',1,'img',4,14);
insert into item values (16,'h',2,'img',4,22);
insert into item values (17,'j',2,'img',5,12);
insert into item values (18,'k',4,'img',3,11);
insert into item values (19,'l',3,'img',5,14);
insert into item values (20,'z',1,'img',5,15);
insert into item values (21,'x',4,'img',4,12);
insert into item values (22,'c',2,'img',2,14);
insert into item values (23,'v',1,'img',4,19);
insert into item values (24,'b',1,'img',5,18);
insert into item values (25,'n',2,'img',5,17);
insert into item values (26,'m',2,'img',2,24);
insert into item values (27,'a',3,'img',2,25);
insert into item values (28,'s',2,'img',3,17);
insert into item values (29,'d',2,'img',5,14);
insert into item values (30,'f',1,'img',4,10);

insert into lombards values (1,'Oktyabrskaya Ul., bld. 188/2, appt. 10');
insert into lombards values (2,'Ryabikova B-R, bld. 22/А, appt. 123');
insert into lombards values (3,'Engelsa, bld. 42/А, appt. 38');
insert into lombards values (4,'Tereshkovoy Ul., bld. 29/А, appt. 50');
insert into lombards values (5,'Minskaya, bld. 49, appt. 101');

insert into deal_type values (1,'buying');
insert into deal_type values (2,'sell');
insert into deal_type values (3,'repair');
insert into deal_type values (4,'recovery');
insert into deal_type values (5,'refund');

insert into conf_data values (1,'5419','457243','MVD','25.05.1989',1);
insert into conf_data values (2,'5419','591684','MVD','30.06.1996',2);
insert into conf_data values (3,'5419','952543','MVD','08.12.1994',3);
insert into conf_data values (4,'5420','118639','MVD','19.10.1997',4);
insert into conf_data values (5,'5420','580661','MVD','10.02.1956',5);
insert into conf_data values (6,'5419','454484','MVD','15.09.1953',6);
insert into conf_data values (7,'5420','712222','MVD','18.04.1987',7);
insert into conf_data values (8,'5419','825911','MVD','24.03.1985',8);
insert into conf_data values (9,'5420','843083','MVD','28.02.1962',9);
insert into conf_data values (10,'5419','458571','MVD','26.02.1958',10);

insert into client values (1,'Timofeev','Pavel','Adamovich',1);
insert into client values (2,'Stepanov','Matvey','Alexandrovich',2);
insert into client values (3,'Sokolova','Nika','Alexandrovna',3);
insert into client values (4,'Denisova','Vladislava','Sergeevna',4);
insert into client values (5,'Alekseeva','Diana','Artemyevna',5);
insert into client values (6,'Ageeva','Anastasia','Matveevna',6);
insert into client values (7,'Orlov','Lev','Lukich',7);
insert into client values (8,'Konstantinov','Makar','Danilovich',8);
insert into client values (9,'Mikhailov','Artemiy','Konstantinovich',9);
insert into client values (10,'Petrova','Milana','Demidova',10);

insert into employee_data values (1,'5419','457243','MVD','25.05.1989',1);
insert into employee_data values (2,'5419','591684','MVD','30.06.1996',2);
insert into employee_data values (3,'5419','952543','MVD','08.12.1994',3);
insert into employee_data values (4,'5420','118639','MVD','19.10.1997',4);
insert into employee_data values (5,'5420','580661','MVD','10.02.1956',5);
insert into employee_data values (6,'5419','454484','MVD','15.09.1953',6);
insert into employee_data values (7,'5420','712222','MVD','18.04.1987',7);
insert into employee_data values (8,'5419','825911','MVD','24.03.1985',8);
insert into employee_data values (9,'5420','843083','MVD','28.02.1962',9);
insert into employee_data values (10,'5419','458571','MVD','26.02.1958',10);

insert into employee values (1,'Timofeev','Pavel','Adamovich',1);
insert into employee values (2,'Stepanov','Matvey','Alexandrovich',2);
insert into employee values (3,'Sokolova','Nika','Alexandrovna',3);
insert into employee values (4,'Denisova','Vladislava','Sergeevna',4);
insert into employee values (5,'Alekseeva','Diana','Artemyevna',5);
insert into employee values (6,'Ageeva','Anastasia','Matveevna',6);
insert into employee values (7,'Orlov','Lev','Lukich',7);
insert into employee values (8,'Konstantinov','Makar','Danilovich',8);
insert into employee values (9,'Mikhailov','Artemiy','Konstantinovich',9);
insert into employee values (10,'Petrova','Milana','Demidova',10);

insert into employee_working_graphic values (1,'Monday',1);
insert into employee_working_graphic values (2,'Tuesday',2);
insert into employee_working_graphic values (3,'Wednesday',3);
insert into employee_working_graphic values (4,'Thursday',4);
insert into employee_working_graphic values (5,'Friday',5);
insert into employee_working_graphic values (6,'Saturday',6);
insert into employee_working_graphic values (7,'Sunday',7);
insert into employee_working_graphic values (8,'Monday',8);
insert into employee_working_graphic values (9,'Tuesday',9);
insert into employee_working_graphic values (10,'Wednesday',10);

insert into deals values (1,1,1,20000,'2018-01-01',1,1,1);
insert into deals values (2,2,2,1500,'2018-01-18',2,1,2);
insert into deals values (3,3,3,2000,'2018-02-27',3,1,3);
insert into deals values (4,4,4,4000,'2018-03-07',4,1,3);
insert into deals values (5,5,5,6000,'2018-04-24',5,1,4);
insert into deals values (6,6,1,25000,'2018-06-13',1,2,5);
insert into deals values (7,7,2,3500,'2018-06-21',2,2,6);
insert into deals values (8,8,3,4000,'2018-08-29',3,2,7);
insert into deals values (9,9,4,6000,'2018-11-19',4,2,8);
insert into deals values (10,10,5,9000,'2018-12-05',5,2,9);
insert into deals values (11,1,6,2000,'2018-12-12',1,1,10);
insert into deals values (12,8,7,2000,'2019-01-09',2,1,1);
insert into deals values (13,2,8,4000,'2019-01-11',3,1,2);
insert into deals values (14,3,9,6000,'2019-01-14',4,1,3);
insert into deals values (15,4,10,9000,'2019-01-25',5,1,4);
insert into deals values (16,5,6,4000,'2019-01-28',1,2,5);
insert into deals values (17,6,7,4000,'2019-02-05',2,2,6);
insert into deals values (18,7,8,6000,'2019-06-28',3,2,7);
insert into deals values (19,9,9,9000,'2019-08-19',4,2,8);
insert into deals values (20,10,10,12000,'2019-11-04',5,2,9);
insert into deals values (21,1,11,1500,'2020-04-16',1,1,10);
insert into deals values (22,2,12,2500,'2020-04-22',2,1,1);
insert into deals values (23,3,13,3500,'2020-05-15',3,1,2);
insert into deals values (24,4,14,4500,'2020-05-26',4,1,3);
insert into deals values (25,5,15,5500,'2020-07-03',5,1,4);
insert into deals values (26,6,11,3000,'2020-07-07',1,2,5);
insert into deals values (27,7,12,4000,'2020-08-24',2,2,6);
insert into deals values (28,8,13,5000,'2020-10-02',3,2,7);
insert into deals values (29,9,14,6000,'2020-10-14',4,2,8);
insert into deals values (30,10,15,4000,'2020-10-29',5,2,9);
insert into deals values (31,1,16,5000,'2020-11-20',1,1,10);
insert into deals values (32,2,17,6000,'2020-12-02',2,1,1);
insert into deals values (33,3,18,7000,'2020-12-15',3,1,2);
insert into deals values (34,4,19,8000,'2020-12-16',4,1,3);
insert into deals values (35,5,20,9000,'2021-01-07',5,1,4);
insert into deals values (36,6,16,7000,'2021-01-20',1,2,5);
insert into deals values (37,7,17,9000,'2021-02-22',2,2,6);
insert into deals values (38,8,18,10000,'2021-04-06',3,2,7);
insert into deals values (39,9,19,13000,'2021-04-12',4,2,8);
insert into deals values (40,10,20,15000,'2021-05-18',5,2,9);
insert into deals values (41,1,21,15000,'2021-05-25',1,1,10);
insert into deals values (42,2,22,17000,'2021-05-26',2,1,1);
insert into deals values (43,3,23,19000,'2021-07-21',3,1,2);
insert into deals values (44,4,24,21000,'2021-08-10',4,1,3);
insert into deals values (45,5,25,22000,'2021-08-11',5,1,4);
insert into deals values (46,6,21,20000,'2021-09-14',1,2,5);
insert into deals values (47,7,22,23000,'2021-09-15',2,2,6);
insert into deals values (48,8,23,26000,'2021-10-01',3,2,7);
insert into deals values (49,9,24,28000,'2021-10-04',4,2,8);
insert into deals values (50,10,25,30000,'2021-11-29',5,2,9);
insert into deals values (51,1,26,15000,'2022-02-15',1,1,10);
insert into deals values (52,2,27,20000,'2022-03-01',2,1,1);
insert into deals values (53,3,28,25000,'2022-04-13',3,1,2);
insert into deals values (54,4,29,27000,'2022-06-14',4,1,3);
insert into deals values (55,5,30,29000,'2022-06-17',5,1,4);
insert into deals values (56,6,26,20000,'2022-09-26',1,2,5);
insert into deals values (57,7,27,23000,'2022-10-04',2,2,6);
insert into deals values (58,8,28,29000,'2022-11-18',3,2,7);
insert into deals values (59,9,29,34000,'2022-11-22',4,2,8);
insert into deals values (60,10,30,36000,'2022-12-20',5,2,9);
```
