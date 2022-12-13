# Syntax



Inheritance

```csharp
class Parent {}
class Children : Parent {}
```

Inheritance

```csharp
class Animal
{
    public virtual void Sound()
    {
        Console.WriteLine("Making a sound");
    }
}

class Pig : Animal
{
    public override void Sound()
    {
        Console.WriteLine("Wee wee");
    }
}

class Dog : Animal
{
    public override void Sound()
    {
        Console.WriteLine("Woof woof");
    }
}
```

Abstraction

```csharp
abstract class Animal
{
    public abstract void Sound();
    public void Sleep()
    {
        Console.WriteLine("Zzz");
    }
}

class Pig : Animal
{
    public override void Sound()
    {
        Console.WriteLine("Wee wee");
    }
}
```

### Interface

```csharp
interface IAnimal
{
    void Sound();
    void Run();
}

class Pig : IAnimal
{
    public void Sound()
    {
        Console.WriteLine("Wee wee");
    }
}
```

Interface members are `abstract` and `public` by default.

Multiple interfaces

```csharp
interface IFirstInterface
{
    void FirstMethod();
}

interface ISecondInterface
{
    void SecondInterface();
}

class SomeClass : IFirstInterface, ISecondInterface
{
    public void FirstMethod()
    {
        Console.WriteLine("Some text...");
    }
    
    public void SecondMethod()
    {
        Console.WriteLine("Other text...");
    }
    public string SomeProperty { set; get; }
}
```
