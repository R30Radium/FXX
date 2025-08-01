
В SQL, включая PostgreSQL, **INNER JOIN** и **OUTER JOIN** — 
это две основные категории JOIN, которые используются для объединения данных из разных таблиц. Давайте разберем их кратко и с примерами.

INNER JOIN, когда вам нужны только совпадающие данные, и OUTER JOIN, когда нужно сохранить все строки из одной или обеих таблиц.

---

### 1. **INNER JOIN**
- **Что делает**: Возвращает только те строки, где есть совпадение в обеих таблицах.
- **Когда использовать**: Когда вам нужны только те данные, которые присутствуют в обеих таблицах.

#### Пример:
```sql
SELECT *
FROM table1
INNER JOIN table2 ON table1.id = table2.table1_id;
```
- Результат: Только строки, где `table1.id` совпадает с `table2.table1_id`.

---

### 2. **OUTER JOIN**
OUTER JOIN делится на три типа: **LEFT**, **RIGHT** и **FULL**. Они возвращают не только совпадающие строки, но и строки, которые не имеют совпадений в другой таблице.

#### a) **LEFT OUTER JOIN (или просто LEFT JOIN)**
- **Что делает**: Возвращает все строки из левой таблицы (`table1`) и соответствующие строки из правой таблицы (`table2`). Если совпадений нет, то справа будут `NULL`.
- **Когда использовать**: Когда вам нужны все данные из левой таблицы, даже если нет совпадений в правой.

#### Пример:
```sql
SELECT *
FROM table1
LEFT JOIN table2 ON table1.id = table2.table1_id;
```
- Результат: Все строки из `table1` и совпадающие строки из `table2`. Если совпадений нет, то столбцы из `table2` будут `NULL`.

---

#### b) **RIGHT OUTER JOIN (или просто RIGHT JOIN)**
- **Что делает**: Возвращает все строки из правой таблицы (`table2`) и соответствующие строки из левой таблицы (`table1`). Если совпадений нет, то слева будут `NULL`.
- **Когда использовать**: Когда вам нужны все данные из правой таблицы, даже если нет совпадений в левой.

#### Пример:
```sql
SELECT *
FROM table1
RIGHT JOIN table2 ON table1.id = table2.table1_id;
```
- Результат: Все строки из `table2` и совпадающие строки из `table1`. Если совпадений нет, то столбцы из `table1` будут `NULL`.

---

#### c) **FULL OUTER JOIN (или просто FULL JOIN)**
- **Что делает**: Возвращает все строки из обеих таблиц. Если совпадений нет, то недостающие значения заполняются `NULL`.
- **Когда использовать**: Когда вам нужны все данные из обеих таблиц, включая те, которые не имеют совпадений.

#### Пример:
```sql
SELECT *
FROM table1
FULL JOIN table2 ON table1.id = table2.table1_id;
```
- Результат: Все строки из `table1` и `table2`. Если совпадений нет, то недостающие значения будут `NULL`.

---

### Сравнение INNER JOIN и OUTER JOIN:
| Тип JOIN         | Возвращаемые строки                                                                 |
|------------------|-------------------------------------------------------------------------------------|
| **INNER JOIN**   | Только строки с совпадениями в обеих таблицах.                                      |
| **LEFT JOIN**    | Все строки из левой таблицы + совпадения из правой. Если нет совпадений — `NULL`.   |
| **RIGHT JOIN**   | Все строки из правой таблицы + совпадения из левой. Если нет совпадений — `NULL`.   |
| **FULL JOIN**    | Все строки из обеих таблиц. Если нет совпадений — `NULL`.                           |

---

### Итог:
- **INNER JOIN** — только совпадения.
- **OUTER JOIN** — все строки из одной или обеих таблиц, включая `NULL` для отсутствующих совпадений.

Используйте INNER JOIN, когда вам нужны только совпадающие данные, и OUTER JOIN, когда нужно сохранить все строки из одной или обеих таблиц.