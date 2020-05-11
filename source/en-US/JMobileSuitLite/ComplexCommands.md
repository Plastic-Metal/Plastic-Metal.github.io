---
title: Create Complex Commands - PlasticMetal.JMobileSuitLite
date: 2020-04-11 12:45:11
---

**These features only works on JMobileSuitLite 0.1.1.4 or later version**

Code example is at the end of this part.

## Array parameter

### Create a command with only a array parameter

Add a method called ***GoodEvening*** to class ***Client*** . It has a ***String[]*** parameter. You may add a alias 'GE' for this method. The content of method can be anything you like.

Build and run your application.

In the console, you may input:

**GoodEvening** with 0, 1, 2 ... arguments, which will be seen as an array. 

### Create a command with a array parameter and some String parameters

Add a method called ***GoodEvening2*** to class ***Client*** . It has a ***String[]*** parameter, and a ***String*** parameter. The parameter with ***String[]*** type **MUST BE** the last parameter of the method.  You may add a alias 'GE2' for this method. The content of method can be anything you like.

Build and run your application.

In the console, you may input:

**GoodEvening2** with 1, 2 ... arguments, first will be mapped to the ***String*** parameter, else will be seen as an array and mapped to the ***String[]*** one. 

The most important thing when you're using this type of command is that The parameter with ***String[]*** type **MUST BE** the last parameter of the method. If not, JMobileSuit will not parse you command correctly.

## Dynamic Parameter

### Create a class implements DynamicParameter

Add a class to class ***Client***, called ***GoodMorningParameter***, it implements ***PlasticMetal.JMobileSuitLite.ObjectModel.Interfaces.DynamicParameter*** interface. The class should be `public static`:

``` java
    public static class GoodMorningParameter implements DynamicParameter{

        /**
         * Parse this Parameter from String[].
         *
         * @param options String[] to parse from.
         * @return Whether the parsing is successful
         */
        @Override
        public Boolean Parse(String[] options)
        {


        }
    }
```

Add some Contents to ***GoodMorningParameter*** , fill the ***Parse(String[] options)***. The method should return true, if parsing is successful, otherwise, it should return false.

For example:

``` java
    public static class GoodMorningParameter implements DynamicParameter{
        public String name="foo";

        /**
         * Parse this Parameter from String[].
         *
         * @param options String[] to parse from.
         * @return Whether the parsing is successful
         */
        @Override
        public Boolean Parse(String[] options)
        {
            if(options.length==1){
                name=options[0];
                return true;
            }else return options.length==0;

        }
    }
```

### Create a command with only a DynamicParameter

Add a method called ***GoodMorning*** to class ***Client*** . It has a ***GoodMorningParameter*** parameter. You may add a alias 'GM' for this method. The content of method can be anything you like.

Build and run your application.

In the console, you may input:

**GoodMorning** with 0, 1, 2 ... arguments, which will be seen as an array, then parsed by ***GoodMorningParameter::Parse***. 

### Create a command with a DynamicParameter and some String parameters

Add a method called ***GoodMorning2*** to class ***Client*** . It has a ***GoodMorningParameter*** parameter, and a ***String*** parameter. The parameter with ***GoodMorningParameter*** type **MUST BE** the last parameter of the method.  You may add a alias 'GE2' for this method. The content of method can be anything you like.

Build and run your application.

In the console, you may input:

**GoodMorning2** with 1, 2 ... arguments, first will be mapped to the ***String*** parameter, else will be seen as an array and parsed to the ***GoodMorningParameter*** one. 

The most important thing when you're using this type of command is that The parameter with ***? extends DynamicParameter*** type **MUST BE** the last parameter of the method. If not, JMobileSuit will not parse you command correctly.

### Code example

After this part, your **Client.java** may looks like:

``` java
import PlasticMetal.JMobileSuitLite.ObjectModel.Annotions.SuitAlias;
import PlasticMetal.JMobileSuitLite.ObjectModel.Annotions.SuitInfo;
import PlasticMetal.JMobileSuitLite.ObjectModel.Interfaces.DynamicParameter;
import PlasticMetal.JMobileSuitLite.ObjectModel.SuitClient;
import PlasticMetal.JMobileSuitLite.SuitHost;

@SuitInfo("Demo")
public class Client extends SuitClient
{
    @SuitAlias("H")
    @SuitInfo("hello command")
    public void Hello()
    {
        IO().WriteLine("Hello! MobileSuit!");
    }

    public String Bye(String name)
    {
        IO().WriteLine("Bye!" + name);
        return "bye";
    }

    public static void main(String[] args) throws Exception
    {
        new SuitHost(Client.class).Run();
    }
    @SuitAlias("GM")
    public void GoodMorning(GoodMorningParameter arg){
        IO().WriteLine("Good morning,"+arg.name);
    }

    @SuitAlias("GM2")
    public void GoodMorning2(String arg, GoodMorningParameter arg1){
        IO().WriteLine("Good moring, "+arg+" and "+ arg1.name);
    }

    @SuitAlias("GE")
    public void GoodEvening(String[] arg){

        IO().WriteLine("Good Evening, "+(arg.length>=1?arg[0]:""));
    }

    @SuitAlias("GE2")
    public void GoodEvening2(String arg0,String[] arg){

        IO().WriteLine("Good Evening, "+arg0+(arg.length>=1?" and "+arg[0]:""));
    }

    public static class GoodMorningParameter implements DynamicParameter{
        public String name="foo";

        /**
         * Parse this Parameter from String[].
         *
         * @param options String[] to parse from.
         * @return Whether the parsing is successful
         */
        @Override
        public Boolean Parse(String[] options)
        {
            if(options.length==1){
                name=options[0];
                return true;
            }else return options.length==0;

        }
    }
}

```