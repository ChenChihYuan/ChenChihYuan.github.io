---
title: note_ComputaionalGeometric
date: 2020-06-27 12:36:07
tags:
- Computational
- Geometric
---

## Jordan Curve Theorem
> Every simple closed plane cuvre divides the plane into two components

The two parts of plane
- interior (bounded)
- exterior (unbounded)

Note that we define a polygon P as **a closed region** of the plane.

We will use notation $\partial P$ to **mean the boundary of P**; this notation borrowed from topology. By our definition $\partial P \subseteq P$

## Diagonals and Triangulation
A **diagonal** of a polygon P is a line segment between two of its veritecs **a** and **b** that are clearly visible to one another.


## Lemma
1. Every polygon must have at least one strictly convex vertex.
2. Every polygon of $n \geq 4$ has a **diagonal** (Meisrers)
3. Every polygon P of n vertices may be partitioned into triangles by the addition  of (zero or more) diagonals
4. Every triangluation of a polygon P of n vertices uses **n-3** diagonals and consist of **n-2** triangles
5. The sum of the internal angles of a polygon of n vertices is $(n-2)\pi$


## Properties of Triangulations
Although in general there can be a large number of different ways to triangulate a given polygon, **they all have the same number of diagonals and triangles.**

## Triangulation Dual
An important concept in graph theory is the "`dual`" of a graph.
> The dual T of a triangulation og a polygon is a `graph` with a node associated with each triangle and an arc between two nodes iff their triangles share a diagonal.

![dual](/uploads/Dual.JPG)
6. The `dual T` of a triangulation is a **tree**, with each `node of degree` at most **three**.

Proof: The each node has degree at most of 3 is immediate from the fact that a triangle has at most 3 sided to share.
> A tree is a connected graph with on cycles.


7. (Meister's Two Ear Theorem) Every polygon of $n \geq 4$ vertices has at least two nonoverlapping ears
8. (3-coloring) The triangulation graph of a polygon P may be 3-colored.

Because the value of cross product of two vectors will equal to the area of the parallelgram.

1. Twice the area of a triangle T = (a, b, c) is given by
$$ 2A(T) = 
\begin{vmatrix}
a_0 & a_1& 1\\
b_0 & b_1& 1\\
c_0 & c_1& 1
\end{vmatrix}
= (b_0-a_0)(c_1-a_1)-(c_0-a_0)(b_1-a_1)
$$

2. If T = $\delta abc$ is a triangle, with vertices oriented counterclockwise, and p is any point in the plane, then
$$A(T) = A(p,a,b) + A(p,b,c) +A(p,c,a)$$

Since to be a diagonal should satisfy two conditions
1. nonintersectino with polygon edges
2. being interior

If we ingore the distinction between internal and external diagonals, finding diagonals is a straightforward repeated application of `Intersect`: 
