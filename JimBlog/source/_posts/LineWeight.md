---
title: LineWeight
date: 2020-07-03 10:51:14
tags:   
- Tag
- Dimension
- Attribute
- Grasshopper
---

Here I would like to note the way to customize the self-defined line weight display in Grasshopper.

## Regular Tag/ dimension
Some readers might familir with the original `tag/dimension` components provided in Grasshopper.
A very simple demo picture is like these,
### Line Dimension
![OriginalTag](/uploads/Tag1.JPG)

### Aligned Dimension
![OriginalTag](/uploads/Tag2.JPG)
Little difference might be noticed, `alignmenet dimension` can offset the display lines.

### Line Weight + Tag (Csharp customized)
![OriginalTag](/uploads/Tag3.JPG)

`Tag`
```cs
  private void RunScript(List<Point3d> Locations, List<string> Descriptions, string Font, int Size, System.Drawing.Color Color, bool Tag, ref object A)
  {
    clear_textdots();

    if(Tag){
      var attr = new Rhino.DocObjects.ObjectAttributes();
      attr.ColorSource = Rhino.DocObjects.ObjectColorSource.ColorFromObject;
      attr.ObjectColor = Color;
      for(int i = 0; i < Locations.Count;i++){
        var tdot = create_textdot(Descriptions[i], Locations[i], Size, Font);
        Rhino.RhinoDoc.ActiveDoc.Objects.AddTextDot(tdot, attr);
      }
    }
    else{
      clear_textdots();
    }
  }

  TextDot create_textdot(string text_str, Point3d point, int height, string font){
    height = -1;
    var textdot = new TextDot(text_str, point);
    if(height > 0)
      textdot.FontHeight = height;
    if(font != null)
      textdot.FontFace = font;
    return textdot;
  }

  void clear_textdots(){
    var textdots = Rhino.RhinoDoc.ActiveDoc.Objects.FindByObjectType(Rhino.DocObjects.ObjectType.TextDot);
    if(textdots.Length > 0){
      foreach(var tdot in textdots){
        Rhino.RhinoDoc.ActiveDoc.Objects.Delete(tdot, true);
      }
    }
  }

```
`Line Weight`
```cs
  private void RunScript(List<Polyline> plns, System.Drawing.Color Color, List<Point3d> pts, int width, ref object A)
  {
    pln = plns;
    IList<Point3d> ipts = pts;
    _clippingBox = new BoundingBox(ipts);
    _color = Color;
    _width = width;
    //A = _clippingBox;
  }

  // <Custom additional code> 
  List<Polyline> pln;
  Color _color;
  int _width;
  BoundingBox _clippingBox;
  public override void BeforeRunScript(){
    _clippingBox = BoundingBox.Empty;
  }
  public override BoundingBox ClippingBox{
    get{return _clippingBox;}
  }
  public override void DrawViewportMeshes(IGH_PreviewArgs args) {
    //for (int i = 0 ; i < limit; i++)
    //args.Display.DrawPoint(pts[i], Rhino.Display.PointStyle.Simple, 10, Color.FromArgb(100, 255 - i / 10, 255 - i / 10, 255 - i / 10));
    foreach(Polyline tmppln in pln)
      args.Display.DrawPolyline(tmppln, _color, _width);

  }

```



In the case of override the `DataTree(Point3d)`:
```cs
private void RunScript(DataTree<Point3d> points, List<System.Drawing.Color> colors, ref object A)
  {

    Bpoints = points;
    Bcolor = colors;
  }

  // <Custom additional code> 
  DataTree<Point3d> Bpoints = new DataTree<Point3d>();
  List<Color> Bcolor = new List<Color>();
  public override BoundingBox ClippingBox{
    get{
      return new BoundingBox(new Point3d(0, 0, 0), new Point3d(10, 10, 10));
    }
  }
  public override void DrawViewportWires(IGH_PreviewArgs args){
    int j = 0;
    for(int i = 0;i < Bpoints.BranchCount;i++){
      args.Display.DrawPoints(Bpoints.Branches[i], Rhino.Display.PointStyle.Circle, 5, Bcolor[j]);
      //args.Display.DrawPoints(,)
      j++;
    }
  }
```