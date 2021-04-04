---
title: FunctionPointerC++
date: 2020-06-09 22:55:25
tags:
- C++
- FunctionPointer
---

[Recource Video](https://www.youtube.com/watch?v=p4sDgQ-jao4)
Thanks for the youtube channel, `The Cherno`. This post is basically a note I take to remind me what `Function Pointer` is about.

`Function Pointer` is a way to assign pointer to a variable.

> Pass a function as a parameter. 


# The usual way about function
The implementation of a regular function should look like.
```cpp
#include <iostream>

void HelloWorld(){
    std::cout<< "Hello World!"<<std::endl;
}

int main(){
    HelloWorld();
    std::cin.get();
}
```

The result should be like
![idea](/uploads/HelloWorld.JPG)

## What if we try to use auto key for this function?
![idea](/uploads/auto1.JPG)
then you will see an error at the `auto`.
That is because what `HelloWorld()` return is a `void`.

But what if we do like this. `auto function = HelloWorld`
![idea](/uploads/auto2.JPG)
then the error disapper.
This is because `HelloWorld` now return a `Function Pointer` instead of `void`.
so if we try to print the variable function in the next line, we will get the pointer of the `HelloWorld` function.
![idea](/uploads/auto3.JPG)
![idea](/uploads/functionPointer_result1.JPG)
Due to the implicitly conversion,
```cpp
auto function = HelloWorld;
//much the same as 
auto function = &HelloWorld;
```
## What Function Pointer is about 
> Functions are of course just CPU instructions, and they are stored in somewhere in our binary/executable file.

Find the `Hello World` function inside the CPU instruction and give it a `pointer`, to remember the location of this function code.

![idea](/uploads/CallFunctionTwice.JPG)
![idea](/uploads/functionPointer_result2.JPG)

so here we can assign a function to a varible.
> But what type is this `auto` actually?

Here the auto hold the type write like
```cpp
void(*function)()
```

`void(*)()` is the type eventually, but since we have to get it a name so it became `void(*function)()`.
It works with any other name.(Like I named it jim here)
![idea](/uploads/functionPointer_result3.JPG)

Becauce this syntax is kinda confusing, programmer tend to either 
- use `auto`, like what we did above 
- use `typedef`/`using` it

![idea](/uploads/functionPointer_result4.JPG)
Here we make HelloWorld function require one `int` parameter
![idea](/uploads/functionPointer_result5.JPG)

# Back to programmer want to use function pointer in the first place
> Programmers can pass one function to another function as a variable.

![idea](/uploads/functionPointer_result6.JPG)

Here this code also show how to use `lambda function` instead of writing out the full function body.

Reference: [Cherno's video](https://www.youtube.com/watch?v=p4sDgQ-jao4)