## Introduction
The Observer design pattern is a fundamental pattern in software design, particularly useful in scenarios where an object (known as the subject) needs to automatically notify a list of other objects (known as observers) about state changes or events occurring in the subject. This pattern is widely used in event handling systems.

## Purpose
The Observer pattern is used to:
- Establish a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
- Encourage loose coupling between the subject and the observers, as the subject doesn't need to know anything about the observers.

## Example of Usage
Consider a simple weather monitoring application. The `WeatherStation` (the subject) monitors weather data and updates various displays (`WeatherDisplay` objects, the observers) which show current weather conditions.

When the `WeatherStation` gets new data, it notifies all the registered `WeatherDisplay` objects. Each display then updates its content accordingly.

## Sample Code in C#

Here's a basic implementation in C#:

```csharp
using System;
using System.Collections.Generic;

// Observer interface
public interface IObserver
{
    void Update(float temperature, float humidity, float pressure);
}

// Subject interface
public interface ISubject
{
    void RegisterObserver(IObserver observer);
    void RemoveObserver(IObserver observer);
    void NotifyObservers();
}

// Concrete Subject
public class WeatherStation : ISubject
{
    private List<IObserver> observers;
    private float temperature;
    private float humidity;
    private float pressure;

    public WeatherStation()
    {
        observers = new List<IObserver>();
    }

    public void RegisterObserver(IObserver observer)
    {
        observers.Add(observer);
    }

    public void RemoveObserver(IObserver observer)
    {
        observers.Remove(observer);
    }

    public void NotifyObservers()
    {
        foreach (var observer in observers)
        {
            observer.Update(temperature, humidity, pressure);
        }
    }

    public void MeasurementsChanged()
    {
        NotifyObservers();
    }

    public void SetMeasurements(float temperature, float humidity, float pressure)
    {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        MeasurementsChanged();
    }
}

// Concrete Observer
public class WeatherDisplay : IObserver
{
    private float temperature;
    private float humidity;
    private float pressure;
    private ISubject weatherStation;

    public WeatherDisplay(ISubject weatherStation)
    {
        this.weatherStation = weatherStation;
        weatherStation.RegisterObserver(this);
    }

    public void Update(float temperature, float humidity, float pressure)
    {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        Display();
    }

    public void Display()
    {
        Console.WriteLine("Current conditions: " + temperature + "F degrees and " + humidity + "% humidity");
    }
}
```
