## Exercise 00
```sql
SELECT person_id, COUNT(*) AS count_of_visits
FROM person_visits
GROUP BY person_id
ORDER BY person_id DESC, count_of_visits ASC;
```
![image](https://github.com/Tufuru/SQL/assets/144116593/56497f28-5fac-4f83-9edb-4710b204a28a)


## Exercise 01
```sql
SELECT name, COUNT(*) AS count_of_visits
FROM person_visits
INNER JOIN person ON person_visits.person_id = person.id
GROUP BY name
ORDER BY count_of_visits DESC, name ASC;
```
![image](https://github.com/Tufuru/SQL/assets/144116593/dc53a8e4-eede-4943-9108-bcad4c054fd5)

## Exercise 04
```sql
SELECT name, COUNT(*) AS count
FROM person_visits
INNER JOIN person ON person_visits.person_id = person.id
GROUP BY name
HAVING COUNT(*) > 3;
```
![image](https://github.com/Tufuru/SQL/assets/144116593/fb472bcf-a15e-40b0-8e46-df7f9cb1ef22)

## Exercise 05
```sql
SELECT name
FROM person_order
INNER JOIN person ON person_order.person_id = person.id
GROUP BY name
ORDER BY name ASC;
```
![image](https://github.com/Tufuru/SQL/assets/144116593/f5db44d5-d36f-4d42-85ac-747eafa99396)
