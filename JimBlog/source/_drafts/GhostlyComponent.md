---
title: GhostlyComponent
date: 2020-05-17 14:06:51
tags: 
- Grasshopper
- Component
- Csharp
---

This article is for storing some notes I took while reading The `ghostlyComponent` made by professor Long Nguyen. 

- [GH_DocumentObject.OnPingDocument](https://developer.rhino3d.com/api/grasshopper/html/M_Grasshopper_Kernel_GH_DocumentObject_OnPingDocument.htm)
Raise the `PingDocument` Event on the toplevel object anf try to find the document which owns this object.

- [Instance.ActiveCanvas](https://developer.rhino3d.com/api/grasshopper/html/P_Grasshopper_Instances_ActiveCanvas.htm)
Gets the currently active Grasshopper canvas(if any)

- [GH_Canvas.CanvasPaintBackground](https://developer.rhino3d.com/api/grasshopper/html/E_Grasshopper_GUI_Canvas_GH_Canvas_CanvasPaintBackground.htm)
Raised after the background has been drawn.

- canvasPaintHandler
```csharp
private void canvasPaintHandler(GH_Canvas canvas)
{
    if (Instances.ActiveCanvas.Document != OnPingDocument()) return;

    float t = (float)stopwatch.Elapsed.TotalSeconds;

    //=================================================
    // Background flash
    //=================================================

    if (!float.IsNaN(backgroundFlashStart))
    {
        float deltaT = t - backgroundFlashStart;
        if (deltaT > 2.0) backgroundFlashStart = float.NaN;
        else if (deltaT > 1.0)
        {
            GH_Viewport viewport = Instances.ActiveCanvas.Viewport;

            canvas.Graphics.FillRectangle(
                new SolidBrush(
                    Color.FromArgb((int)(100 - 99 * Math.Cos((deltaT - 1) * 10.0 * 3.1412)), 0, 0, 0)),
                -viewport.ZoomInverse * viewport.Tx,
                -viewport.ZoomInverse * viewport.Ty,
                viewport.ZoomInverse * viewport.Width,
                viewport.ZoomInverse * viewport.Height);
        }
    }

    //=================================================
    // Little ghosts flying out from the recently added component
    //=================================================

    List<Ghost> ghostsToBeRemoved = new List<Ghost>();

    foreach (Ghost ghost in ghosts)
    {
        ghost.Draw(canvas);
        if (ghost.Inactive) ghostsToBeRemoved.Add(ghost);
    }

    foreach (Ghost ghost in ghostsToBeRemoved) ghosts.Remove(ghost);

    //=================================================
    // Main ghost that follow the mouse cursor
    //=================================================

    ghostImage1.SelectActiveFrame(frameDim1, (int)(t * 15) % frameCount1);

    float k = 0.01f;
    PointF cursor = Instances.ActiveCanvas.CursorCanvasPosition;
    ghostyPosition.X = (1f - k) * ghostyPosition.X + k * cursor.X;
    ghostyPosition.Y = (1f - k) * ghostyPosition.Y + k * cursor.Y;

    canvas.Graphics.DrawImage(
        ghostImage1,
        new Rectangle(
            (int)(ghostyPosition.X - 42),
            (int)(ghostyPosition.Y - 89),
            (int)(95),
            (int)(180)));

    Instances.ActiveCanvas.Invalidate();
}
```

so the `canvasPaintHandler` method is a bit huge, I will note some new things I learned.


```csharp
if(Instances.ActiveCanvas.Document != OnPingDocument()) return; //  To make sure the doc opened is the current/correct one.
```
------
## System.Diagnostics.Stopwatch()
```csharp
private static readonly Stopwatch stopwatch = Stopwatch.StartNew();
```

Here Long introduced one class `Stopwatch` from `System.Diagnostics` namespace.
### [Stopwatch](https://docs.microsoft.com/en-us/dotnet/api/system.diagnostics.stopwatch?view=netcore-3.1)
  Provides a set of methods and properties that you can use to accurately measure elapsed time.
- [Stopwatch.StartNew()](https://docs.microsoft.com/en-us/dotnet/api/system.diagnostics.stopwatch.startnew?view=netcore-3.1)
  Initializes a new <u>Stopwatch</u> instance, sets the elapsed time property to zero, and starts measuring elapsed time.
  **Return Value**
  `Stopwatch`
- [Stopwatch.Elapsed](https://docs.microsoft.com/en-us/dotnet/api/system.diagnostics.stopwatch.elapsed?view=netcore-3.1)
  Gets the total elapsed time measured by the current instance.
  **Return Value**
  `TimeSpan`
  A read-only TimeSpac representing the total elapsed time measured  by the current instance.
- [TimeSpan.TotalSeconds()](https://docs.microsoft.com/en-us/dotnet/api/system.timespan.totalseconds?view=netcore-3.1)
  Gets the value of the current `TimeSpan` structure expressed in whole and fractinal seconds.

------
### Background flash
```csharp
if (!float.IsNaN(backgroundFlashStart))
{
    float deltaT = t - backgroundFlashStart;
    if (deltaT > 2.0) backgroundFlashStart = float.NaN;
    else if (deltaT > 1.0)
    {
        GH_Viewport viewport = Instances.ActiveCanvas.Viewport;

        canvas.Graphics.FillRectangle(
            new SolidBrush(
                Color.FromArgb((int)(100 - 99 * Math.Cos((deltaT - 1) * 10.0 * 3.1412)), 0, 0, 0)),
            -viewport.ZoomInverse * viewport.Tx,
            -viewport.ZoomInverse * viewport.Ty,
            viewport.ZoomInverse * viewport.Width,
            viewport.ZoomInverse * viewport.Height);
    }
}
```

------
## class Ghost
- [System.Drawing.PointF](https://docs.microsoft.com/en-us/dotnet/api/system.drawing.pointf?view=netcore-3.1)
  Represents an ordered pair of floating-point x- and y-coordinates that defines a point in a two dimensional plane.
  
