---
title: Chaikins_Algorithm
date: 2020-07-05 23:52:16
tags:
- Algorthm
- Geometrics
- Chaikins
- Grasshopper 
- Csharp
---


![OriginalTag](/uploads/Chaikin.gif)

From this week's(2020/07/03) Parametric Camp, Professor Jose Luis García del Castillo y López  challenged us to do Chaikin's Algorithm by ourselves.
I think it will be great if I made some notes about how I thought and my work here.

The code is pretty straight forward, some little thing might notice
- while iterating thought points, the index // should careful segmentation fault
- Points in RhinoCommon can work like vector, this concept inherit from linear algebra, anyway this means you can multiple them `scale the vector`

```cs
private void RunScript(List<Point3d> polyLinePts, int iterNum, ref object A)
  {
    vertex = polyLinePts;
    List<Point3d> smoothPts = new List<Point3d>();
    for(int i = 0; i < iterNum ;i++){
      smooth(vertex, ref vertex);
    }
    A = vertex;
  }
  // <Custom additional code> 
  List<Point3d> vertex;
  //Deal with one iteration
  void smooth(List<Point3d> inputPts, ref List<Point3d> resultPts){
    List<Point3d> nextGen = new List<Point3d>();
    // point 0 1 2 ... n
    // point 1 2 3 ... n-1
    // line  0 1 2 ... n
    for(int i = 0; i < inputPts.Count - 1; i++){
      Point3d oneFourth = inputPts[i] * 0.25 + inputPts[i + 1] * 0.75;
      Point3d threeFourth = inputPts[i] * 0.75 + inputPts[i + 1] * 0.25;

      nextGen.Add(threeFourth);
      nextGen.Add(oneFourth);
    }
    resultPts = nextGen;
  }
```


Thing that I have not achieved,
- the endpoints are not included.
- Have not tackled the `closed` polyline situation

Some thought about Closed one, should deal with the iteration, especially the end point. LOL

We can also change the ratio(0.25, 0.75) to something else
![OriginalTag](/uploads/Chaikin1.gif)

One thing can be noticed that there is always one point will converge to the original segment.
While iteration goes larger the point will be the middle point of the original segment.
