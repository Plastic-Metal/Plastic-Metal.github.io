---
title: Get Started - PlasticMetal.MobileSuit
date: 2020-04-11 12:45:11
---


## Create your project

So firstly, you need to [create a new Console Application in Visual Studio](https://docs.microsoft.com/en-us/visualstudio/get-started/csharp/tutorial-console?view=vs-2019). The language (Visual Basic or C#) doesn't matter, but the following examples will be written in C#.

Then, you should add [PlasticMetal.MobileSuit](https://www.nuget.org/packages/PlasticMetal.MobileSuit/) to your project([How?](https://docs.microsoft.com/en-us/nuget/quickstart/install-and-use-a-package-in-visual-studio)).

## Write the MobileSuit Client class

### Create the class

Add a Class to your project, named ***Client***. It inherits class ***PlasticMetal.MobileSuit.ObjectModel.SuitClient***.

### Add the first command

Add a method called ***Hello*** to class ***Client***. It has no parameters and return value.

The content of method can be anything you like. You can use *IO.WriteLine* and *IO.ReadLine* instead of *Console.WriteLine* and *Console.ReadLine*.

### Add information and Alias for the first command

Add custom attributes to the method:

1. *PlasticMetal.MobileSuit.ObjectModel.Attributes.SuitInfo* with argument "hello command."
2. *PlasticMetal.MobileSuit.ObjectModel.Attributes.SuitAlias* with argument "H"

### Add another command

Add a method called ***Bye*** to class ***Client***. It has a string parameter, named *name*. It returns a *string*.

The content of method can be anything you like. You can use *Io.WriteLine* and *Io.ReadLine* instead of *Console.WriteLine* and *Console.ReadLine*.

### Check your code for Client.cs

It may looks like:

``` csharp
using PlasticMetal.MobileSuit.ObjectModel;
using PlasticMetal.MobileSuit.ObjectModel.Attributes;

namespace MobileSuitDemo
{
    public class Client : SuitClient
    {
        [SuitAlias("H")]
        [SuitInfo("hello command.")]
        public void Hello()
        {
            IO.WriteLine("Hello! MobileSuit!");
        }

        public string Bye(string name)
        {
            IO.WriteLine($"Bye! {name}");
            return "bye";
        }
    }
}
```

## Update ***Program*** class

Add the following code to the ***Main()*** method:

``` csharp
new PlasticMetal.MobileSuit.SuitHost(new Client()).Run();
```

## Run and test your Application

Build and run your application.

In the console, you may input:

1. **Help** to see help for MobileSuit
2. **List** or **ls** to see all available commands for current client.
3. **Hello** or **h** to run *Client.Hello()*
4. **Bye** ***name*** to run *Client.Bye(* ***name*** *)*
5. **Exit** to exit the progress

The process of build such a app is so easy that you just need one class to write.

