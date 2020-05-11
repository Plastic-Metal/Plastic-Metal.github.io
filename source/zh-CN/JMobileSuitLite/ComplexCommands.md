---
title: 创建更复杂的 PlasticMetal.JMobileSuitLite 命令
date: 2020-04-11 12:45:11
---

**由于JVM和CLR的区别, 这些特性在 0.1.1.4 或更晚版本才被完整移植**

代码样例在本节末尾。

## 使用数组参数的命令

### 创建只使用一个数组参数的命令

添加一个方法叫做 ***GoodEvening*** 到类 ***Client*** 中. 它有一个 ***String[]*** 类型的参数. 你可以为这个方法增加别名 'GE' . 方法的内容是任意的.

编译并运行你的应用.

在控制台中, 你可以输入:

**GoodEvening** 接着 0, 1, 2 ... 个参数, 它们会被解析为一个数组. 

### 创建使用一个数组和一些字符串参数的命令

添加一个方法叫做 ***GoodEvening2*** 到类 ***Client*** 中. 它有一个 ***String[]*** 类型的参数, 和一个 ***String*** 类型的参数. 类型为 ***String[]*** 的参数 **必须是** 这个方法的最后一个参数.  你可以为这个方法增加别名 'GE2'. 方法的内容是任意的.

编译并运行你的应用.

在控制台中, 你可以输入:

**GoodEvening2** 接着 1, 2 ... 个参数, 第一个参数会被映射到 ***String*** 类型的参数, 其它的(可能是0个)会被视为数组，映射到 ***String[]*** 类型的参数. 

编写这种命令最重要的是把使用 ***String[]*** 类型的参数 **必须放在** 方法参数列表的最后一位，否则JMobileSuitLite将无法正确解析它。

## 创建使用动态参数的命令

### 创建一个实现DynamicParameter的类

添加一个类到类 ***Client*** 中, 叫做 ***GoodMorningParameter***, 它实现 ***PlasticMetal.JMobileSuitLite.ObjectModel.Interfaces.DynamicParameter*** 接口. 这个类应该是 `public static`的:

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

向 ***GoodMorningParameter*** 添加内容, 填充 ***Parse(String[] options)***. 这个函数返回true **当且仅当** 解析成功。

例如:

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

### 创建一个只使用动态参数的命令

添加一个方法叫做 ***GoodMorning*** 到类 ***Client*** . 它有一个 ***GoodMorningParameter*** 类型的参数. 你可以为这个方法增加别名 'GM'. 方法的内容是任意的.

编译并运行你的应用.

在控制台中, 你可以输入:

**GoodMorning** 跟着 0, 1, 2 ... 个参数, 他们会被视为一个数组, 然后被 ***GoodMorningParameter::Parse*** 解析. 

### 创建一个使用动态参数和一些字符串参数的命令

添加一个方法叫做 ***GoodMorning2*** 到类 ***Client*** . 它有一个 ***GoodMorningParameter*** 类型的参数, 和一个 ***String*** 类型的参数. 类型为 ***GoodMorningParameter*** 的参数 **必须放在** 方法参数列表的最后一位.  你可以为这个方法增加别名 'GE2'. 方法的内容是任意的.

编译并运行你的应用.

在控制台中, 你可以输入:

**GoodMorning2** 跟着 1, 2 ... 个参数, 第一个参数会被映射到 ***String*** 类型的参数, 其它的会被视为数组并被解析为 ***GoodMorningParameter*** 类型的参数. 

编写这种命令最重要的是 ***? extends DynamicParameter*** 类型的参数 **必须放在** 方法参数列表的最后一位，否则JMobileSuitLite将无法正确解析它。

## 代码示例

在本节之后，你 **Client.java** 中的代码大概像:

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
