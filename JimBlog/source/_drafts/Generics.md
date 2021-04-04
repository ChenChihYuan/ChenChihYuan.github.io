---
title: Generics
date: 2020-06-18 20:29:25
tags:
- Csharp
- Generics
---

[Tutorial Video](https://www.youtube.com/watch?v=gyal6TbgmSU)

# What problem Generic tend to solve?
Problem: Every type you have to create a seperate kind of function for that `type`

## Bad solution
- Use object list
Since `object` is the root(**parent class**) of any C# types, using `object` list seems to be an intuitive solition

`Another Problem`
But, this lead the problem that `poor performance`


## Better Solution - use `Generic`

For example, assume we have **List** class in Generics namespace for **int** list.
```cs
using System;

namespace Generics{
    public class List{
        public void Add(int number){
            throw new NotImplementedException();
        }
        public int this[int index]{

        }
    }
}
```

How about in the case we need a list contain self-defined class **Book**?
Does this mean we have to make the same List structure again and the only difference will be the input type?
```cs
namespace Generics{
    public class BookList{
        public void Add(Book book){
            throw new NotImplementedException();
        }
        public Book this[int index]{
            get {throw new NotImplementedException();}
        }
    }
}
```
### Solution1 not recommended List<object>
```cs
public class ObjectList{
    public void Add(object value){

    }
    public object this[int index]{
        get {throw new NotImplementedException();}
    }
}
```
But by doing so, it performed **poorly**.

### Instead, we can use `Generics`
Similarly, we can do things like this.
```cs
public class GenericsList<T>{
    public void Add(T value){

    }
    public T this[int index]{
        get {throw new NotImplementedException();}
    }
}
```

Assume we our execution code is placed in **Program.cs**
```cs
namespace Generics{
    class Program{
        static void Main(string[] args){
            var book = new Book{Isbn="1111", Title = "C# Advanced"};

            var numbers = new List();
            number.Add(10);

            var books = new BookList();
            books.Add(book);

            var numbers = new GenericList<int>();
            numbers.Add(10);

            var books = new GenericList<Book>();
            books.Add(new Book());
        }
    }
}
```
We do not need to code another type of List for fitting Book type, and also it won't have performace penalty.
> Because in runtime the type in GenericList won't change, there's no casting.

From Mosh's experience, the most case C# developer will choose to use Generic List in `.NET`, instead of self-define one.
In .NET, Generics collection can be found under,
```cs
    System.Collections.Generics.List
    System.Collections.Generics.IList
    System.Collections.Generics.Stack
    //...
```
## Dictionary Implemation
A dictionary is a data structure used **hash table** to store **Key-value**.

```cs
public class GenericDictionary<TKey, TValue>{
    public void Add(TKey key, TValue value){

    }
}
```

In Program.cs

```cs
namespace Generics{
    class Program{
        static void Main(string[] args){
            var book = new Book{Isbn="1111", Title = "C# Advanced"};

            var numbers = new List();
            number.Add(10);

            var books = new BookList();
            books.Add(book);

            var dictionary = new GenericDictionary<string, Book>();
            dictionary.Add("1234", new Book());
        }
    }
}
```

> Since T can hold any type of data, sometimes you might want to constrain what **T** should be about.

Normally if we want to compare two instances in csharp
```cs
namespace Generics{
    public class Utilities{
        public int Max(int a, int b){
            return a > b? a : b;
        }

        public T Max<T>(T a, T b){  
            return a > b ? a : b;   // Here is a compile Error, because the compiler cannot know the type of T
                                    // a and b are both objects
        }
    }
}
```

To tackle the problem that compiler cannot figure it out the type in compile time, we can set that both a and b implemented the `IComparable` interface.
This is one way to provide the contrain.

```cs
public T Max<T>(T a, T b) where T : IComparable
{  
    return a.CompareTo(b) > 0 ? a : b; 
}
```
notice that here we defined a Generics method inside a non-Generic class, and it is fine.

Also, we can put the contrain into the class level, and do things like,
```cs
namespace Generics
{
    //where T : IComparable
    //where T : Product
    //where T : struct
    //where T : class
    //where T : new()
    public class Utilities<T> where T : IComparable
    {
        public int Max(int a, int b)
        {
            return a > b? a : b;
        }

        public T Max(T a, T b)
        {
            return a.CompareTo(b) > 0 ? a : b;
        }
    }

}
```
