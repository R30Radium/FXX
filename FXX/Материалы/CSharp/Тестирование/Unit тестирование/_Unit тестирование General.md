Unit-тесты в C# обычно пишутся с использованием фреймворков для тестирования, таких как NUnit, MSTest или xUnit. В этом примере я покажу, как писать unit-тесты с использованием xUnit, так как он является одним из самых популярных фреймворков.

### Установка xUnit

1. Откройте ваш проект в Visual Studio.
2. Перейдите в `Tools` -> `NuGet Package Manager` -> `Manage NuGet Packages for Solution`.
3. Найдите и установите пакеты `xunit` и `xunit.runner.visualstudio`.

### Пример кода

Предположим, у вас есть класс, который вы хотите протестировать:

```csharp
public class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }

    public int Subtract(int a, int b)
    {
        return a - b;
    }
}
```

Теперь создадим проект для тестов:

1. Создайте новый проект типа "Class Library" в вашем решении (например, `MyProject.Tests`).
2. Установите в нем пакеты `xunit` и `xunit.runner.visualstudio`, как описано выше.

### Написание тестов

Теперь создадим тесты для нашего класса `Calculator`:

```csharp
using Xunit;

namespace MyProject.Tests
{
    public class CalculatorTests {
        [Fact]
        public void Add_ShouldReturnSum_WhenTwoIntegersAreProvided()
        {
            // Arrange var calculator = new Calculator();
            int a = 5;
            int b = 3;

            // Act var result = calculator.Add(a, b);

            // Assert Assert.Equal(8, result);
        }

        [Fact]
        public void Subtract_ShouldReturnDifference_WhenTwoIntegersAreProvided()
        {
            // Arrange var calculator = new Calculator();
            int a = 5;
            int b = 3;

            // Act 
            var result = calculator.Subtract(a, b);

            // Assert
             Assert.Equal(2, result);
        }
    }
}
```

### Запуск тестов

1. Откройте окно "Test Explorer" в Visual Studio (`Test` -> `Windows` -> `Test Explorer`).
2. Выберите тесты и нажмите "Run All" для их выполнения.

### Объяснение кода

- `[Fact]` — это атрибут, который указывает, что метод является тестом. Каждый тест должен быть помечен этим атрибутом.
- В каждом тесте используется три этапа: 
  - **Arrange** — подготовка данных и объектов, необходимых для теста.
  - **Act** — выполнение тестируемого метода.
  - **Assert** — проверка результата с ожидаемым значением с помощью методов, таких как `Assert.Equal`.

### Заключение

Это базовый пример написания unit-тестов на C# с использованием xUnit. Вы можете расширять свои тесты, добавляя больше методов и проверок, а также использовать другие атрибуты, такие как `[Theory]`, для параметризованных тестов.