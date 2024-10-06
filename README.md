# Observer Example

Цей репозиторій містить приклад патерну проектування "Спостерігач".

## Патерн Спостерігач (Observer)

Патерн "Спостерігач" дозволяє об'єкту (суб'єкту) повідомляти інші об'єкти (спостерігачі) про зміни свого стану. Це забезпечує слабку зв'язність між об'єктами, оскільки спостерігачі не знають про реалізацію суб'єкта.

### Приклад коду:

```csharp
using System;
using System.Collections.Generic;

// Інтерфейс спостерігача
public interface IObserver
{
    void Update(string message);
}

// Інтерфейс суб'єкта
public interface ISubject
{
    void Attach(IObserver observer);
    void Detach(IObserver observer);
    void Notify();
}

// Суб'єкт
public class Subject : ISubject
{
    private readonly List<IObserver> _observers = new List<IObserver>();
    private string _state;

    public string State
    {
        get => _state;
        set
        {
            _state = value;
            Notify();
        }
    }

    public void Attach(IObserver observer)
    {
        _observers.Add(observer);
    }

    public void Detach(IObserver observer)
    {
        _observers.Remove(observer);
    }

    public void Notify()
    {
        foreach (var observer in _observers)
        {
            observer.Update(_state);
        }
    }
}

// Конкретний спостерігач
public class ConcreteObserver : IObserver
{
    private readonly string _name;

    public ConcreteObserver(string name)
    {
        _name = name;
    }

    public void Update(string message)
    {
        Console.WriteLine($"{_name} отримав оновлення: {message}");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Subject subject = new Subject();

        ConcreteObserver observer1 = new ConcreteObserver("Спостерігач 1");
        ConcreteObserver observer2 = new ConcreteObserver("Спостерігач 2");

        subject.Attach(observer1);
        subject.Attach(observer2);

        subject.State = "Перше оновлення";
        subject.State = "Друге оновлення";

        subject.Detach(observer1);
        subject.State = "Третє оновлення";
    }
}
