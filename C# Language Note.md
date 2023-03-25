# 1.注释

## 1.单行注释

```c#
//这是一个单行注释
```

## 2.多行注释

```c#
/*
 * 这是多行注释1
 * 这是多行注释2
 * 这是多行注释3
 */
```

## 3.文档注释

```c#
/// <summary>
/// 这是一个文档注释
/// </summary>
```

## 4.代码折叠

```c#
//宏中的代码会被折叠
#region name
public static void func1()
{
    
}
public static void func1()
{
    
}
public static void func1()
{
    
}
#endregion
```

# 2.变量

## 1.变量类型

- 整数 ： int
- 浮点 : double / float
- 字符 ： char
- 字符串 : string

## 2.声明

```c#
int    variable = 1;
float  variable_1 = 1.0;
double variable_2 = 1.9;
char   variable_3 = 'A';
string str1, str2, str3, str4;
```

# 3.占位符

```C#
int a=3, b=4;

Console.WriteLine("{0} + {1} = {2}", a, b, a+b);
Console.WriteLine($"{a} + {b} = {a+b}");
```

# 4.命名规范

- Pascal(帕斯卡命名法) : 当有多个单词组成变量或者方法时，每个单词首字母大写，例如：`TwoNumberAdd()`
  - 类名
  - 属性
  - 方法名
- Camel(驼峰命名法) : 多个单词出现， 首单词字母小写，其余单词首字母大写, 例如：`twoNumberAdd()`
  - 变量
