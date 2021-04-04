title: BoundingBox for group of objects
author: JimYuan
date: 2020-07-13 22:30:02
tags:
- Csharp
- Goo
- BoundingBox
---
## BoundingBox with Goo

[Goo_Intro](https://www.grasshopper3d.com/forum/topics/goo-in-scripting)
`IGH_Goo` is an interface which is supposed to **wrap** data type in such a way that Grasshopper understand them. 
You rarely have to deal with goo directly because Grasshopper tries to abstract it away as much as possible.

Using **ObjectWrapper** you can for example easily pass arrays, dictionaries or other collections between script components.

[Pass_Group_BBox](https://discourse.mcneel.com/t/group-object-in-gh-pass-to-c-component/50309/5)
```cs
private void RunScript(object G, ref object E)
{
  List<object> gp = G as List<object>;
  BoundingBox box = BoundingBox.Empty;

  foreach (object element in gp)
    if (element != null)
    {
      IGH_GeometricGoo goo = GH_Convert.ToGeometricGoo(element);
      box = BoundingBox.Union(box, goo.Boundingbox);
    }
  E = box;
}
```

## Example
![OriginalTag](/uploads/GroupBBox.JPG)