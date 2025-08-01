
**PRIMARY KEY НЕ МОЖЕТ БЫТЬ NULL

### Объяснение:
1. **Primary Key (Первичный ключ)** — это уникальный идентификатор записи в таблице базы данных. Он должен удовлетворять следующим условиям:
   - Уникальность: каждая запись должна иметь уникальное значение первичного ключа.
   - Непустое значение: первичный ключ не может быть `NULL`.
   - Неизменяемость: значение первичного ключа не должно изменяться после создания записи.

2. **Почему остальные утверждения могут быть верны или неверны в зависимости от контекста**:
   - **"Праймари кей может быть много"**:
     - В одной таблице может быть только **один** первичный ключ, но он может состоять из нескольких столбцов (составной ключ). В этом смысле "много" может относиться к количеству столбцов, но не к количеству первичных ключей.
   - **"Праймари кей может быть 0"**:
     - Значение первичного ключа может быть `0`, если это допустимо в контексте данных (например, числовой идентификатор).
   - **"Праймари кей может быть пустым"**:
     - Пустое значение (например, пустая строка) может быть допустимо в некоторых случаях, но это зависит от реализации базы данных. Однако это крайне не рекомендуется, так как нарушает принципы уникальности и идентификации.

3. **"Праймари кей может быть null"**:
   - Это **неверно**. Первичный ключ не может быть `NULL`, так как его основная задача — однозначно идентифицировать запись. Если значение может быть `NULL`, это нарушает уникальность и целостность данных.

### Итог:
Неправильное утверждение — **"Праймари кей может быть null"**.