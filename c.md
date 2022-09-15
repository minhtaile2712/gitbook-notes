# C\#

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

### Implicit `using` directives <a href="#implicit-using-directives" id="implicit-using-directives"></a>

The term _implicit `using` directives_ means the compiler automatically adds a set of [`using` directives](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/using-directive) based on the project type. For console applications, the following directives are implicitly included in the application:

* `using System;`
* `using System.IO;`
* `using System.Collections.Generic;`
* `using System.Linq;`
* `using System.Net.Http;`
* `using System.Threading;`
* `using System.Threading.Tasks;`

#### Disable implicit `using` directives <a href="#disable-implicit-using-directives" id="disable-implicit-using-directives"></a>

```csharp
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    ...
    <ImplicitUsings>disable</ImplicitUsings>
  </PropertyGroup>

</Project>
```





### Global `using` directives <a href="#global-using-directives" id="global-using-directives"></a>

A _global `using` directive_ imports a namespace for your whole application instead of a single file. These global directives can be added either by adding a [`<Using>`](https://docs.microsoft.com/en-us/dotnet/core/project-sdk/msbuild-props#using) item to the project file, or by adding the `global using` directive to a code file.

```csharp
<ItemGroup>
  <Using Remove="System.Net.Http" />
</ItemGroup>
```
