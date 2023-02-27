---
layout: post
title: Getting Started With C#
---
## Introduction
C# is a high level object-oriented language designed by Microsoft in 2000 released in 2002 along with the .Net frame work and visual studio. It became open source in 2014, Microsoft release Visual Studio Code shortly after. I am learning this because I want to expand my knowledge and skills in OOP so that I can do that kind of work in industry once I've graduated. To learn this language I have been mostly utilizing material from a free learning course called "C#: From Zero to Hero". It includes many lessons from the basics to more advanced material and can be found [here](https://github.com/Almantask/CSharp-From-Zero-To-Hero) if you wish to take a look.

Much like Java C# does not get compiled into machine code to be run directly on hardware. Instead it is translated in CLR (Common Language Runtime) to be run on machines that support and have installed the .NET runtime. In Java this translated code is known as "bytecode" while in C# it is IL (Intermediate Language).

## Basics of C#
The basics of C# is very similar to Java in syntax; the declaration of variables and constants, class and function signatures are the same, only small difference is in C# the name of a function starts with a capital letter. Logical operations such as if-else and switch statements are also the same as Java.

C# has more primitive data types available. While Java has boolean, byte, short, int, long, float and double. C# has all those and unsigned versions of byte (sbyte), short (ushort), int (uint) and long (ulong); also in C# "boolean" is simply named "bool". When signed numbers are stored in memory the first bit inidcates whether the number is positive (0) or negative (1), unsigned number types can store larger values in the same space as signed number types but they cannot be negative. Unlike Java however C# supports the use of implicit varaiables using the "var" keyword, this allows developers to delcare variables without predefined data types, typically used when the developer does not know what type of variable needs to be stored. This shouldn't be used very often though.

## Loops
Loops are also mostly the same as it is in Java. For loops, while loops and do-while have the same function and syntax as Java. Though if you want to loop through an array or a list using shorthand the syntax is slightly different.

Java :
```
for (int i : intArray){
  //do something
}
```
C# :
```
foreach (var i in intArray){
  //do something
}
```

The "break" and "continue" keywords also work the same, allowing you to exit the loop or skip an itertion respectively.

## Strings
In some languages such as C++ Strings are simply char arrays that you can modify as any other array. This is not the case in either C# or Java but in C# you may access characters of a string using the index.
ex:
```
char firstLetter = name[0];
```
C# supports a feature known as string interpolation which allows us to assign and format strings in a convient syntax.

Normal:
```
string s = "This person is named " + name + ", they live in " + city;
```
Interpolation:
```
string s = $"This person is named {name}, they live in {city}";
```
String interpolation can also be used to format strings. There are two components to this, alignment component and format string component. To align a string you specify a value that defines the minimum number of characters in the expression result. If the number is positive the string will be right-aligned, if negative the string will be left-aligned. Such as:

Right alignment:
```
double sum = 9.99;
Console.WriteLine($"Total amount owed:{sum,10}");
```
Result:
```
Total amount owed:      9.99
```
Left aligned:
```
string name = "Eric";
Console.WriteLine($"The author of this blog post is {name, -10}, this is his first blog.");
```
Result:
```
The author of this blog post is Eric      , this is his first blog.
```
This is very useful for formatting strings especially if for fomatting large amounts of data in a table with aligned columns or even a receipt.

The other component, format string component can be used to display a string with for exmaple a currency symbol or as a floating point number with a specific number of decimals.

Currency:
```
double sum = 9.99;
Console.WriteLine($"Total amount owed:{sum,10:C}");
```
Result:
```
Total amount owed:      $9.99
```
Floating point decimial:
```
Console.WriteLine($"Pi to two decimal points: {Math.PI:F2}");
```
Result:
```
Pi to two decimal points: 3.14
```
There are more possibilies than I can reasonably list here but you can check out this quick reference for more:

http://www.independent-software.com/net-string-formatting-in-csharp-cheat-sheet.html

## Files
A string prefixed with an "@" symbol will cancel out special characters so it will be stored exactly as written, this is known as a verbatim string. It is useful for defining file paths becuase the "\" symbol is a special character.

Non-verbatim string:
```
string path = "\\Project\\file.txt";
```
Verbatim string:
```
string path = @"\Project\file.txt";
```
There are a two ways to read and write from a file. For simple operations the File class can be used through using the following functions.
```
string path = @"C:\Documents\file.txt"
string text = "Hello World!";
File.WriteAllText(path, text);
File.ReadAllText(path);
```
The former will write the given string to the file located by the path, overwriting any existing data. The latter will return the contents of the file as a string. Both of these functions will open and close the file for you.

For more advanced applications streams can be used. C# provides many different streams for I/O including ones for certain Oracle database files, compressed files, pipes between different threads and child-parent processes, sockets, printing, security and file streams. All streams can also be buffered through the BufferedStream class. A stream is simply a sequence of bytes and we can use the StreamReader and StreamWriter classes on any stream to easily read and write to them. Streams are unmanaged resources, meaning they persist outside of their scope unless closed. To do this we can call the Dispose() function of any stream to close it. Also we can use the "using" keyword when we are writing to a file to automatically handle the closing of files.

"Dispose()" Example:
```
const string path = @"C:\directory\file.txt";
//opens or creates a new file if it does not exist
FileStream fs = new FileStream(path, OpenOrCreate);
StreamWriter sw = new StreamWriter(fs));
sw.WriteLine("Hello world!");
sw.Dispose();
```

"using" Example:
```
const string path = @"C:\directory\file.txt";
//opens or creates a new file if it does not exist
FileStream fs = new FileStream(path, OpenOrCreate);

using (StreamWriter sw = new StreamWriter(fs))
{
  sw.WriteLine("Hello world!");
}
```

## Exceptions
Exceptions in C# are handled the same way that Java exceptions are. Using the try, catch and finally keywords.
```
try {
  //some code that may throw an exception
} catch (Exception ex){
  //handle exception
} finally {
  //code that will run regardless of whether there is an exception or not
}
```

## Random
Random numbers may be required for many different reasons, to generate random numbers we can use the Random class.

Random integer:
```
Random random = new Random();
int random = random.Next();
```
Or for a random number within a given range:
```
Random random = new Random();
int random = random.Next(*max-value*);
int random2 = random.Next(*min-value*, *max-value*);
```
For doubles the Random class can only generate a value between 0 and 1, so we must multiply it to effectively increase the range.
```
Random random = new Random();
//will generate a random number between 0 and 50
double random = random.NextDouble() * 50;
```
The numbers generated by the Random class are not entirely random and can be predicted, therefore it is not secure. If a Random object is initialized with the default contructor it will use the system clock time to seed it. If you try to generate two random numbers without much time in between you can end up with the same number, we can avoid this using two different Random objects with different seeds. Passing a seed can make a Random object a little less predictable.

### Conclusion
This concludes my first blog on C#. In the next blog I will be dicussing OOP principles in the language, lists, arrays and much more.