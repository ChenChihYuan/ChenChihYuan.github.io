---
title: Custom_Attribute_note
date: 2020-06-15 18:28:43
tags:
- CustomAttribute
- SDK
---

[clarification_about_IGH](https://www.grasshopper3d.com/forum/topics/clarification-about-gh-component-gh-param-and-gh-attributes)

> All objects that want to be part of a GH_Document must implement the `IGH_DocumentObject` **interface**.

This interface provides **core methods** needed for all objects
1. Undo
2. Autosave
3. Display
4. Menu
5. Tooltip

`IGH_ActiveObject` extends upon `IGH_DocumentObject` and it provides methods for participating in the solution.

## Parameters
Parameters(via the `IGH_Param` interface and several `GH_Param<T>` classes) maintain a data tree which is associated with a special type of `IGH_Goo` data. They have the logic build in that allows them to communicate with other parameters via their input/output wires.

## Components
Components tend to drive from GH_Component(or at the very least implemeny `IGH_Component`), as that class provides all the bookkeeping methods for managing a collection of input and output parameters.

## Attribute
Attributes are used primary for **display purposes**. Every object must be capable of creating and maintaining a class which ultimately implements `IGH_Attributes`. The attributes are in charge of 
1. rendering the object to the canvas 
2. providing basic information about the shape of an object
3. handling mouse and key events
4. responding correctly to selection actions
5. maintaining the object hierarchy (Basically, it means that component attributes must be able to **talk** to input and output parameters, and the parameters need to be able to figure out they are slaved to a component.)





---

[Custom Attribute SDK](https://developer.rhino3d.com/api/grasshopper/html/8a7974ab-7b2b-4f48-84d0-6e81b184e6b0.htm)
Objects on the Grasshopper canvas consist of **two part**.
The most important piece is the class that implements the `IGH_DocumentObject`  interface.
This interface provides the basic **plumbing** needed to make objects work within **a Grasshopper node network.**

The interface part of objects however is handled seperately.

Every `IGH_DocumentObject` carries around an instance of a class that implements the `IGH_Attriibutes` interface(indeed, every IGH_DocumentOBject knows how to create its own stand-alone attributes).

It is not possible to have an `IGH_Attributes` instance work on its own, we need an `IGH_DocoumentObject` to tie it to.