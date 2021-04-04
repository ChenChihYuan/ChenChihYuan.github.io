---
title: SweepLine
date: 2020-07-01 01:58:52
tags:
- Algorithm
- SweepLine
---

[SweepLineSeries](https://www.youtube.com/watch?v=nNtiZM-j3Pk&list=PLubYOWSl9mItBLmB2WiFU0A_WINUSLtGH&index=1)


# Line-Segment Intersection
## Problem
Given a set **S** of *n closed* non-overlapping line segments in the plane, compute...
- all points where at least two segments intersect and
- for each such point report all segments that contain it.
  
1. Brute Force, takes $O(n^2)$
2. Process segments `top-to-bottom` using a `sweep line`.
   
Consider when we should do something while using sweepLine algorithm
1. SweepLine touch the endpoint
2. SweepLine touch(identify) the intersection point

## Event Points
- Endpoints on the segments
- intersection points

Once the SweepLine run over the intersection point, should reconsider the possible intersect pair(*neighbor*) situation.
## DataStructure For Event Points `queue Q`
When we think about situable data structure to tackle this problem should consider,
The data structure should fit 
1. can easily find the top most point
2. can insert new event point while finding the intersection points, in run time.

Store event points in **balanced binary search tree** 


## SweepLine `status T`
When segments intersect to the SweepLine, we want them be ordered from left to right.
1. easily add/reomove a segment
2. swap the segment order, happen right after the intersection point found


# Pseudo-code
findIntersections(S)
## Input: 
set S of n non-overlapping closed line segments
## Output:
- set *I* of intersection points
- for each $p \in I$ every $s \in S$ with $p \in s$