---
title: LinesIntersection_in_3D
date: 2020-06-01 22:30:52
tags:
- grasshopper
- Intersection
- 3D
- Algorithm
---


# Intersection in 3D
According to [this post](https://stackoverflow.com/questions/2316490/the-algorithm-to-find-the-point-of-intersection-of-two-3d-line-segment)

>Most 3D lines do not intersect. A reliable method is to find the `shortest line` between two 3D lines. If the shortest line has a lenght of zero(or less than whatever tolerance you specify) then you know that the two original lines intersect.

Therefore, the question become 
>How can we find the `shortest line` between two 3D lines.

[Check this article by Paul](http://paulbourke.net/geometry/pointlineplane/)
Two examples I read
- [C++](http://paulbourke.net/geometry/pointlineplane/lineline.c)
- [C#](http://paulbourke.net/geometry/pointlineplane/calclineline.cs)

In short, this article introduced two thoughts/ implementation to get the minimal distance between two 3D lines.
1. Write down the lenght of the line segment joining the two lines and then find the minimum
1. dot product

If the `minimal distance == 0`, then we can say these two lines intersected

# Intersection.LineLine in RhinoCommon

## [Intersection.LineLine](https://developer.rhino3d.com/api/RhinoCommon/html/M_Rhino_Geometry_Intersect_Intersection_LineLine_1.htm)
Example code I made:

```csharp
using Rhino.Geometry.Intersect;
//
private void RunScript(Line lineA, Line lineB, double tolerance, bool finiteSegments, ref object A, ref object a, ref object b)
  {
    double lineA_a;
    double lineB_b;
    A = Intersection.LineLine(lineA, lineB, out lineA_a, out lineB_b, tolerance, finiteSegments);

    a = lineA_a;
    b = lineB_b;
  }
```
![IntersectinoLineLine_RhinoCommon](/uploads/IntersectionLineLine.JPG)

Also, there's a componeny called `Line|Line` in grasshopper, we can directly use.
![Line|Line](/uploads/IntersectionLineLineGH.JPG)

