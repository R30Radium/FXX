
## Определение и создание потоков в C#

В C# поток можно создать с помощью класса `Thread` из пространства имен `System.Threading.`Вот пример создания потока:

```C#
Class Program 
{
	static void Main() 
	{
		Thread newThread = new Thread(DoWork); // Создание потока
		newThread.Start(); // Запуск потока
	}

	static void DoWork() 
	{
		//Код, который будет выполняться на новом потоке
		Console.WriteLine("Работа в новом потоке.");
	}
}
```
