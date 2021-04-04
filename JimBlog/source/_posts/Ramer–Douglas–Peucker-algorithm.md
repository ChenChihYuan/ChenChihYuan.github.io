---
title: Ramer–Douglas–Peucker_algorithm
date: 2020-05-21 23:16:36
tags:
- algorithm
- Grasshopper
- Csharp
- Jose 
- Divide-And-Conquor

categories: 
- Grasshopper
- Csharp
- Algorithm
---


![Node](/uploads/DAC1.gif)

# Content
I just learned this algorithm to simplified polyline, with [Professor. Jose's amazing tutorial](https://www.youtube.com/watch?v=y8uO-0JB1V4&list=PLx3k0RGeXZ_zEzxbWCmTjGoxnphn1bwiZ&index=1). People who want to learn a bit more, should definitely follow this channel.

The idea is relatively simple, but the implementation could be thousands and millions way.
The goal of this algorithm is to reduce the points in one polyline(points + line), but still want to remain to soul(properity, or shape to be more specifically). There are tons of benefits by doing this, obvious one is that this can clearly releave your memory while speed up the computation time.

# Implement
> The method here is to **recursively** find the `farthest point` to the two segment endpoints. That's it.

## Dive into the Csharp code
Something should be took care
- epilson 
- left points
- right points
- two lists combinition
- recursive function
- Point-Line distance

Because this blog is for recording something I have learned, it is definitely not detail-clear to everyone. Please refer to the Professor Jose's tutorial video, if you want to have a clear idea of this.

## code
### crossProduct
Though in RhinoCommon, there are one predefined crossProduct method that we could use directly, it is convenient that we made by ourselves in the cases of bring this method to another Csharp-based platform.


```charp
Vector3d crossProduct(Vector3d u, Vector3d v){
    double x = u.Y * v.Z - u.Z * v.Y;
    double y = u.Z * v.X - u.X * v.Z;
    double z = u.X * v.Y - u.Y * v.X;

    return new Vector3d(x, y, z);
  }
```
------

### Point-Line distance
$$ \frac{\vec{AB} \times \vec{AP}}{\| \vec{AB}\|}$$
```csharp
double distToLine(Point3d P, Point3d A, Point3d B){
    Vector3d AB = B - A;
    Vector3d AP = P - A;

    Vector3d cross = crossProduct(AB, AP);
    double lengthCross = cross.Length;
    double lengthAB = AB.Length;

    double h = lengthCross / lengthAB;
    return h;
  }
```

### simplifyPolyline

```csharp
List<Point3d> simplifyPolyline(List<Point3d> points, double epsilon){
    
    double maxDist = -1.0;
    int maxIndex = -1;
    
    for( int i = 1; i < points.Count - 1 ;i++){
      double d = distToLine(points[i], points[0], points[points.Count - 1]);
      if( d > maxDist){
        maxDist = d;
        maxIndex = i;
      }
    }
    
    Print("max index: " + maxIndex + "\t maxdist:" + maxDist);
    
    List<Point3d> cleanPoints;
    
    if(maxDist > epsilon){
      // Subdivide poly in two parts
      Print("should do subdivision here");
      List<Point3d> leftPoly = points.GetRange(0, maxIndex +1);
      List<Point3d> rightPoly = points.GetRange(maxIndex, points.Count - maxIndex);
      
      // Apply recursion to each one
      List<Point3d> leftSimplified = simplifyPolyline(leftPoly, epsilon);
      List<Point3d> rightSimplified = simplifyPolyline(rightPoly, epsilon);
      
      // Avoid duplicating split points
      rightSimplified.RemoveAt(0);
      //rightSimplified = rightSimplified.GetRange(1,rightSimplified.Count-1);
      
      cleanPoints = leftSimplified;
      cleanPoints.AddRange(rightSimplified);
      
    }
    else{
      
      // remove all points between endpoints and return
      cleanPoints = new List<Point3d>(){points[0], points[points.Count - 1]};
    }
    
    return cleanPoints;
  }
```

# Note from reading Wiki
## Application
The algorithm is used for the processing of **vector graphics** and **cateographic generalization**.
> It does not always preserve the property of `non-self intersection` for curves which has led to the development of variant algorithms.

> 

Like Jose mentioned in his video, this method is widely used in robotics to perform simplification and denoiseing of range data acuquired by a rotating `range scanner`.

## Complexity


# Reference
- [Professor. Jose's amazing tutorial](https://www.youtube.com/watch?v=y8uO-0JB1V4&list=PLx3k0RGeXZ_zEzxbWCmTjGoxnphn1bwiZ&index=1)
- [wiki_Ramer–Douglas–Peucker_algorithm](https://en.wikipedia.org/wiki/Ramer%E2%80%93Douglas%E2%80%93Peucker_algorithm)