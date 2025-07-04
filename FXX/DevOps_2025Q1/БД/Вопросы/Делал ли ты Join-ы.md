
В PostgreSQL (как и в других SQL-системах) существует несколько типов JOIN, которые позволяют объединять данные из двух или более таблиц. Вот краткие примеры каждого типа JOIN:

---

### 1. **INNER JOIN**
Возвращает только те строки, где есть совпадение в обеих таблицах.

```sql
SELECT *
FROM table1
INNER JOIN table2 ON table1.id = table2.table1_id;
```

---

### 2. **LEFT JOIN (или LEFT OUTER JOIN)**
Возвращает все строки из левой таблицы (`table1`) и соответствующие строки из правой таблицы (`table2`). Если совпадений нет, то справа будут `NULL`.

```sql
SELECT *
FROM table1
LEFT JOIN table2 ON table1.id = table2.table1_id;
```

---

### 3. **RIGHT JOIN (или RIGHT OUTER JOIN)**
Возвращает все строки из правой таблицы (`table2`) и соответствующие строки из левой таблицы (`table1`). Если совпадений нет, то слева будут `NULL`.

```sql
SELECT *
FROM table1
RIGHT JOIN table2 ON table1.id = table2.table1_id;
```

---

### 4. **FULL JOIN (или FULL OUTER JOIN)**
Возвращает все строки из обеих таблиц. Если совпадений нет, то недостающие значения заполняются `NULL`.

```sql
SELECT *
FROM table1
FULL JOIN table2 ON table1.id = table2.table1_id;
```

---

### 5. **CROSS JOIN**
Возвращает декартово произведение двух таблиц (все возможные комбинации строк).

```sql
SELECT *
FROM table1
CROSS JOIN table2;
```

---

### 6. **SELF JOIN**
Это JOIN таблицы с самой собой. Используется, когда нужно сравнить строки внутри одной таблицы.

```sql
SELECT t1.*, t2.*
FROM table1 t1
INNER JOIN table1 t2 ON t1.id = t2.related_id;
```

---

### 7. **NATURAL JOIN**
Автоматически объединяет таблицы по столбцам с одинаковыми именами. **Используйте с осторожностью**, так как это может привести к неожиданным результатам.

```sql
SELECT *
FROM table1
NATURAL JOIN table2;
```

---

### 8. **LATERAL JOIN**
Позволяет использовать столбцы из предыдущей таблицы в подзапросе. Это полезно для сложных запросов.

```sql
SELECT *
FROM table1 t1,
LATERAL (SELECT * FROM table2 t2 WHERE t1.id = t2.table1_id) t2;
```

---

### Итог:
- **INNER JOIN** — только совпадения.
- **LEFT JOIN** — все строки слева + совпадения справа.
- **RIGHT JOIN** — все строки справа + совпадения слева.
- **FULL JOIN** — все строки из обеих таблиц.
- **CROSS JOIN** — декартово произведение.
- **SELF JOIN** — JOIN таблицы с самой собой.
- **NATURAL JOIN** — автоматический JOIN по одинаковым столбцам.
- **LATERAL JOIN** — подзапрос с использованием данных из предыдущей таблицы.

Эти примеры помогут вам быстро разобраться с JOIN в PostgreSQL!