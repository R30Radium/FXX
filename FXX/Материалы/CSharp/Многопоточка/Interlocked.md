Класс `Interlocked` в C# и .NET предоставляет методы для безопасного выполнения атомарных операций на разделяемых переменных. Атомарные операции - это операции, которые выполняются за одну неделимую единицу, не могут быть прерваны другими потоками и гарантируют согласованность данных при работе с многопоточностью.

Разделяемые переменные могут быть доступны из нескольких потоков одновременно, поэтому защита их значения от гонок данных является крайне важной задачей. Класс `Interlocked` помогает обеспечить безопасное выполнение операций на таких переменных путём гарантированного сохранения целостности данных.

Рассмотрим подробнее некоторые методы класса `Interlocked` и приведём примеры их использования:

1. Метод `Increment`: Увеличивает значение указанной переменной типа `int` или `long` на 1 и возвращает новое значение. Метод `Increment` гарантирует, что операция будет выполнена неделимой единицей и никакой другой поток не будет иметь доступ к значению переменной в процессе выполнения операции.
    

```
int counter = 0;
Interlocked.Increment(ref counter);
Console.WriteLine(counter); // Выведет 1
```

2. Метод `Decrement`: Уменьшает значение указанной переменной типа `int` или `long` на 1 и возвращает новое значение. Работает аналогично методу `Increment`.
    

```
int counter = 5;
Interlocked.Decrement(ref counter);
Console.WriteLine(counter); // Выведет 4
```

3. Метод `Add`: Добавляет указанное значение к переменной типа `int` или `long` и возвращает новое значение. Операция выполняется атомарно, не допуская интерференции других потоков.
    

```
int total = 10;
int increment = 5;
Interlocked.Add(ref total, increment);
Console.WriteLine(total); // Выведет 15
```

4. Метод `Exchange`: Заменяет значение указанного поля или переменной на новое значение и возвращает старое значение. Этот метод также является неделимой операцией.
    

```
int value = 10;
int newValue = 20;
int oldValue = Interlocked.Exchange(ref value, newValue);
Console.WriteLine(oldValue); // Выведет 10
Console.WriteLine(value);    // Выведет 20
```

5. Метод `CompareExchange`: Сравнивает значение указанной переменной с ожидаемым значением. Если значения равны, заменяет его новым значением и возвращает предыдущее значение. Если значения не равны, то не выполняет замену и возвращает текущее значение переменной.
    

```
int value = 10;
int newValue = 20;
int expectedValue = 15;
int oldValue = Interlocked.CompareExchange(ref value, newValue, expectedValue);
Console.WriteLine(oldValue); // Выведет 10, так как ожидаемое значение (15) не совпало со значением переменной
Console.WriteLine(value);    // Выведет 10, потому что замены не произошло из-за несовпадения ожидаемого значения
```

Класс `Interlocked` также предоставляет другие методы для выполнения различных атомарных операций над переменными разных типов, таких как `And`, `Or`, `Xor` и др. Они обеспечивают безопасность данных при работе с многопоточностью и помогают избежать гонок данных(которые мы разберем ниже).

Важно отметить, что класс `Interlocked` может использоваться только с переменными, поддерживающими операции чтения и записи в одну единицу времени (atomic operations). Это обычно применяется к типам, размер которых не превышает размера указателя в целевой системе (32 бита или 64 бита).

Использование методов класса `Interlocked` помогает создавать безопасный и отзывчивый многопоточный код, который правильно обрабатывает разделяемые переменные в контексте параллельного программирования.