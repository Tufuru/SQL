## Задание 2
![image](https://github.com/Tufuru/SQL/assets/144116593/fe239006-2db6-4a42-a4cc-e36476ae3e2d)
```sql
SELECT name, age
FROM person
WHERE address LIKE 'Moscow';
```
## Задание 3
![image](https://github.com/Tufuru/SQL/assets/144116593/c089e243-8c8d-4b47-b374-3a594ead94fd)
```sql
SELECT name, age
FROM person
WHERE gender LIKE 'female' AND address LIKE 'Moscow'
ORDER BY name;
```
## Задание 4.1
![image](https://github.com/Tufuru/SQL/assets/144116593/41676a86-ca18-421c-8dc5-8559c7446e14)

```sql
SELECT name, rating
FROM pizzeria
WHERE rating >= 3.5
ORDER BY rating ASC;
```
## Задание 4.2
![image](https://github.com/Tufuru/SQL/assets/144116593/de867445-5445-493b-844b-6cd2c6266fdc)

```sql
SELECT name, rating
FROM pizzeria
WHERE NOT rating < 3.5
ORDER BY rating DESC;
```
## Задание 5
![image](https://github.com/Tufuru/SQL/assets/144116593/c1e61b14-a1fd-47ce-9faf-ee9b40ad11e9)

```sql
SELECT id
FROM person_visits
WHERE (visit_date BETWEEN '2022-01-08' AND '2022-01-10') OR pizzeria_id = 2
ORDER BY id DESC;
```
## Задание 6
![image](https://github.com/Tufuru/SQL/assets/144116593/3217765e-def6-482d-a553-d455409f364c)
```sql
SELECT name
FROM person
WHERE id = (SELECT person_id FROM person_order WHERE (order_date BETWEEN '2022-01-04' AND '2022-01-04'))
```
## Задание 7
![image](https://github.com/Tufuru/SQL/assets/144116593/c4d9a9f2-c2df-4f01-9947-65d1d0e21dce)
```sql
SELECT CASE WHEN name = 'Kate' THEN 'True' ELSE 'False' END
FROM person;
```
