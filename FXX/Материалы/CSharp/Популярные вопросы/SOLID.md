SOLID — это акроним, представляющий пять принципов объектно-ориентированного программирования, которые помогают создавать более понятный, гибкий и поддерживаемый код. Вот краткое описание каждого принципа с примерами:

1. Single Responsibility Principle (SRP)
2. Open/Closed Principle (OCP)
3. Liskov Substitution Principle (LSP)
4. Interface Segregation Principle (ISP)
5. Dependency Inversion Principle (DIP)

### 1. Single Responsibility Principle (SRP)
**Принцип единственной ответственности**: Каждый класс должен иметь только одну причину для изменения, то есть выполнять только одну задачу.

**Пример**:
```csharp
public class ReportGenerator {
    public void GenerateReport() {
        // Генерация отчета }
}

public class ReportPrinter {
    public void PrintReport(Report report) {
        // Печать отчета }
}
```

### 2. Open/Closed Principle (OCP)
**Принцип открытости/закрытости**: Классы должны быть открыты для расширения, но закрыты для модификации.

**Пример**:
```csharp
public abstract class Shape {
    public abstract double Area();
}

public class Circle : Shape {
    public double Radius { get; set; }
    public override double Area() => Math.PI * Radius * Radius;
}

public class Rectangle : Shape {
    public double Width { get; set; }
    public double Height { get; set; }
    public override double Area() => Width * Height;
}
```

### 3. Liskov Substitution Principle (LSP)
**Принцип подстановки Барбары Лисков**: Объекты производных классов должны быть заменяемы объектами базового класса без изменения правильности программы.

**Пример**:
```csharp
public class Bird {
    public virtual void Fly() { /* Летаем */ }
}

public class Sparrow : Bird {
    public override void Fly() { /* Летаем */ }
}

public class Ostrich : Bird {
    public override void Fly() { /* Ошибка, страусы не летают */ }
}
```

### 4. Interface Segregation Principle (ISP)
**Принцип разделения интерфейсов**: Клиенты не должны зависеть от интерфейсов, которые они не используют. Лучше иметь несколько специализированных интерфейсов, чем один общий.

**Пример**:
```csharp
public interface IFlyable {
    void Fly();
}

public interface ISwimmable {
    void Swim();
}

public class Duck : IFlyable, ISwimmable {
    public void Fly() { /* Летаем */ }
    public void Swim() { /* Плаваем */ }
}

public class Fish : ISwimmable {
    public void Swim() { /* Плаваем */ }
}
```

### 5. Dependency Inversion Principle (DIP)
**Принцип инверсии зависимостей**: Высокоуровневые модули не должны зависеть от низкоуровневых. Оба должны зависеть от абстракций.

**Пример**:
```csharp
public interface IMessageService {
    void SendMessage(string message);
}

public class EmailService : IMessageService {
    public void SendMessage(string message) { /* Отправка email */ }
}

public class Notification {
    private readonly IMessageService _messageService;

    public Notification(IMessageService messageService) {
        _messageService = messageService;
    }

    public void Notify(string message) {
        _messageService.SendMessage(message);
    }
}
```

### Заключение
Принципы SOLID помогают создавать гибкие и поддерживаемые системы, улучшая структуру кода и облегчая его модификацию и тестирование.