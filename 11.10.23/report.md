## Exercise 00
```sql
SET enable_seqscan = OFF;

CREATE INDEX idx_menu_pizzeria_id ON menu(pizzeria_id);
CREATE INDEX idx_menu_pizza_name ON menu(pizza_name);
CREATE INDEX idx_person_name ON person(name);
CREATE INDEX idx_person_age ON person(age);
CREATE INDEX idx_person_address ON person(address);
```
## Exercise 01
```sql
EXPLAIN ANALYZE

SELECT menu.pizza_name, pizzeria.name
FROM menu
INNER JOIN pizzeria ON menu.pizzeria_id = pizzeria.id;
```

## Exercise 02
```sql

```
