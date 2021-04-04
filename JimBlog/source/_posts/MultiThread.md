---
title: MultiThread
date: 2020-06-10 01:06:16
tags:
- MultiTheard
- Grasshopper
- Rhino
- C++
- Csharp
- Cherno
---

# How Cherno explain about Thread

> How we could do multi-thread(parallel computating)

```cpp
#include <iostream>
#include <thread>

static bool s_Finished = false;

void DoWork(){
    while(!s_Finished){
        std::cout<<"Working...."<<std::endl();
    }
}

int main(){
    std::thread worker(DoWork); //pass as Function pointer

    std::cin.get();
    s_Finished = true;

    worker.join();

    std::cin.get(); // this line will wait until the threads(worker) joined.
}

```

Here we want program to do two things at the same time.
1. keep printing `Working....`
2. receive the `cin.get()` from user, once get something from the user `s_Finished = true`

[thread::join](http://www.cplusplus.com/reference/thread/thread/join/)

The behavior will be like this 
![idea](/uploads/Thread2.gif)

However we might not want to let one single thread occupy whole CPU resource, we can use `std::this_thread::sleep_for(1s)`.
```cpp
#include <iostream>
#include <thread>

static bool s_Finished = false;
void DoWork() {
	using namespace std::literals::chrono_literals;
	std::cout << "Started thread id=" << std::this_thread::get_id() << std::endl;
	while (!s_Finished) {
		std::cout << "Working...\n";
		std::this_thread::sleep_for(1s); //sleep for a second
	}
}

int main(){
	std::thread worker(DoWork);
	std::cin.get();
	s_Finished = true;
	worker.join(); //await in C#

	std::cout << "Finished\n";
	std::cout << "Started thread id=" << std::this_thread::get_id() << std::endl;

	std::cin.get();
}
```
The demo of code above.
![idea](/uploads/Thread3.gif)
Can notice that two `std::this_thread::get_id()` are different, cuz they are on the different threads.

# How we could implement multi-thread in Grasshopper

[Grasshopper Tutorial](https://www.youtube.com/watch?v=J6CwUuto9QA&list=PLIfg3sUCgQXDBRK478d6PwxX0IBdgkLE4&index=7&t=13s)

Like this tutorial shown, we can use
```csharp
System.Threading.Tasks.Parallel.For(0, loopCount, i =>{ {
    //for loop body
}});
```
instead of using **regular for-loop** to achieve multi-thread.

## unsafe problem and how to tackle it
This is the part I might not fully understand, and you are welcome to correct me. Thanks.
Seems simply use multi_thread with `List`, like the example in tutorial, is not safe.
Means multi-thread cannot merge to the List in the order correctly. As a result, this cause some data lost.

### How we can tackle this problem
1. We can use `lock` syntax to ensure the action of **List**
2. We use regular `array` instead of **List**

Here are the codes.
#### Lock
```csharp
private void RunScript(List<Point3d> startPts, object y, ref object A)
  {
    List<Point3d> ptList = new List<Point3d>();
    //for(int i = 0;i < startPts.Count; i++)
    System.Threading.Tasks.Parallel.For(0, startPts.Count, i => {
      {
        //Declare out agent location
        Point3d agentLoc = startPts[i];
        //Add agents to ptLists
        lock(ptList){
          ptList.Add(agentLoc);
        }
      }
      });
    A = ptList;
  }
```
#### array
```csharp
private void RunScript(List<Point3d> startPts, object y, ref object A)
  {
    //List<Point3d> ptList = new List<Point3d>();
    Point3d[] ptList = new Point3d[startPts.Count];
    //for(int i = 0;i < startPts.Count; i++)
    System.Threading.Tasks.Parallel.For(0, startPts.Count, i => {
      {
        //Declare out agent location
        Point3d agentLoc = startPts[i];
        //Add agents to ptLists
        ptList[i] = agentLoc;
      }
      });
    A = ptList;
  }
```

# Rhino Official Documents_General Overview
[multi-thread](https://developer.rhino3d.com/guides/grasshopper/multi-treaded-components/)

# Rhino Official Documents_Developer
[Task Capable Component](https://developer.rhino3d.com/guides/grasshopper/programming-task-capable-component/)
## Overview 
Grasshopper for Rhino6 allows you to develop mult-threaded components by way of the new `IGH_TaskCapableComponent` interface. Benchmarks have shown that Grasshopper can run **significantly faster** when using multi-threaded components. Results may vary, **as not all solutions can be compute in parallel**

## The Interface
When a component implement the `IGH_TaskCapableComponent` interface, Grasshopper will notice and potentially call a **full enumeration** of `SolveInstance` for the component twice in consecutive passes:
1. The first pass is for collecting data and starting tasks to compute result
2. The second pass is for using the results from the tasks to sets outputs.



https://discourse.mcneel.com/t/v6-feature-multi-threaded-gh-components/47049

I found that there are some Rhino official posts talk about `mult-threaded` in grasshopper.
From User interface, if you see the component has two grey dots on its upper-left corner, means that component support mult-threaded computation(`parallel computing`).

Still working on.... `To be continue`

(James_Ramsden_Multithreading)[http://james-ramsden.com/multithreading-a-foreach-loop-in-a-grasshopper-components-in-c/]