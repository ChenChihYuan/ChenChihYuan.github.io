---
title: RhinoMesh
tags:
- Rhino
- Python
- Mesh
- Grasshopper
---

I would like to take some notes while reading [Rhino_Python_Primer101](https://developer.rhino3d.com/guides/rhinopython/primer-101/).

# Meshes are defined locally.
This means that a single mesh surface can have any topology it wants.
> A mesh surface can even be a disjoint(not connected) compound of floating surfaces, something which is absoulately impossible with Rhino nurbs surfaces.

Because meshes are stored **locally**, they can also store more information directly inside the mesh format, such as **colors**, **texture-coordinates** and **normals**.

# Mesh vs. Nurbs
> Most differences between meshes and Nurbs are self-evident and flow from the way in which are defined.

For example, you can delete any number of polygons from the mesh and still have a valid object, whereas you cannot delete **knot spans** without breaking apart the nurbs geometry.

1. Coordinates of mesh vertices are stored as **single precision** number in Rhino in order to save memory consumption. Meshes are therefore kess accurrate entities than nurbs objects.
   > This is especially notable with objects that are very small, extremely large or very far away from the world origin. Mesh objects go hay-wire sooner than nurbs objects because single precision numbers have larger gaps between them than double precision numbers.              **hay wire: out of control**
2. **Nurbs cannot be shaded**, only the isocurves and edges of nurbs geometry can be drawn directly in the viewport. If a nurbs surface has to be shaded, then it has to fall back on meshes. This means that inserting nurbs surfaces into a shaded viewport will result in a significant (sometimes very significant) time lag while a mesh representation is calculated.
3. Meshed in Rhino can be non-manifold, meaning that more than two faces share a single edge. ALthough it is not technically impossible for nurbs to behave in this way, **Rhino does not allow it.** Non-manifold shapes are topologically much harder to deal with. If an edge belongs to only a single face it is an exterior edge(**naked**), if it belongs to two faces it is considered interior.

# Geometry vs. Topology
> Only the vertices and faces are essential components of the mesh definition.

- The vertices represent the geometric part of the mesh definition.
- The faces represent the topological part.
  
> Topology is the mathematical study of the properties that are preserved through deformations, twistings, and stretching of objects. In other words, topology does not care about size, shape or smell, it only deal with the plationic properties of objects, such as "how many holes does it have? ", " how many naked edges are there? " and "how do I get from Paris to Lyon without passing ant tollbooths?".