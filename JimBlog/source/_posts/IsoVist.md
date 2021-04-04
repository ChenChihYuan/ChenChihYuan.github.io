---
title: IsoVist
date: 2020-06-24 02:55:00
tags:
- Csharp
- Grasshopper
---

# 前言
可能大家有接觸過用Grasshopper作環境分析，那可能會知道`Isovist`這個方便的component。從下面看到這Component可以幫助決定**視野範圍**。
![Orientation](/uploads/IsoVistDefault.gif)
> 原理很簡單就是2D的圓看根最近的建築物交界到哪裡。

建築物的線我是透過`Elk`放入OpenStreetMap達成的。可以參考一下
[OSM_Through_Elk](https://www.youtube.com/watch?v=EJ5_0dQiKPc&t=411s)
或是我的方式:
![Orientation](/uploads/IsoVist.jpg)

# Csharp code
我意外看到這篇討論，想說自己也來實作一下用C#做一個IsoVist之後討論一下怎麼加速，跟後續可能可以玩的方向
[IsoVist_Optimization](https://discourse.mcneel.com/t/isovist-optimization/103024/4)
而我自己的Code
```cs
private void RunScript(Point3d PlanePt, int Count, int Radius, List<Polyline> ObsCrv, ref object Points, ref object C, ref object A)
  {
    List<Point3d> pts = new List<Point3d>();
    Circle cir = new Circle(PlanePt, Radius);
    Curve CirCrv = cir.ToNurbsCurve();
    Point3d[] CirPts;
    double[] CirPts_double = CirCrv.DivideByCount(Count, true, out CirPts);
    List<Point3d> result_net = new List<Point3d>();
    for(int k = 0; k < Count; k++){
      Line ln = new Line(PlanePt, CirPts[k]);
      List<Point3d> ln_Pts = new List<Point3d>();
      for(int i = 0; i < ObsCrv.Count; i++){
        Line[] segs = ObsCrv[i].GetSegments();
        for(int j = 0; j < segs.Length; j++){
          double a;
          double b;
          Intersection.LineLine(ln, segs[j], out a, out b, 0.00000001, true);
          if(Intersection.LineLine(ln, segs[j], out a, out b, 0.00000001, true)){
            ln_Pts.Add(segs[j].PointAt(b));
          }
        }
      }
      Point3d closest_pt;
      if(ln_Pts.Count > 0)
        closest_pt = Point3dList.ClosestPointInList(ln_Pts, PlanePt);
      else{
        closest_pt = CirPts[k];
      }
      result_net.Add(closest_pt);
    }
    A = result_net;
    C = CirPts;
  }
```
## 演算法的想法
- 平分個一個以人為圓點的圓，分成Count分
- 用圓點跟在圓周上的點連線`ln` 和 每個建築物的牆壁`segs[j]`找相交點
- 有兩個以上交點的話找離圓點最近的，沒有跟建築物相交的話 回傳圓周上的點

## 關於LineLine的相交
在部落格我分別寫了兩篇(2D/3D)的線與線相交的判定。
三維的線與線相交其實是看`兩條線的最短距離<容許值`

ParametricCamp有比較完整的介紹
[Computational Design Live Stream #14](https://www.youtube.com/watch?v=iZdAJ2CwmCk)

論壇po文想達成的是盡量加速這個運算，之後我也能往那方向想，目前是跟Component效能差不多
![Orientation](/uploads/IsoVist_Performance.JPG)

- 目前的想到的想法是可以先對那些牆壁的line以角度sort過一遍，應該可以加速一個`for-loop`
- 
## Demo
![Orientation](/uploads/IsoVistJim.gif)


# 可能的發展

### 加速
加速的理由有很多，
1. 為了一次分析多一點人
2. 讓計算時間變短-Real time result
  
### 導入Magnetic field
在Grasshopper裡面有Fields對人流動線的分析應該會很有幫助
有興趣可以看看這個
https://www.youtube.com/watch?v=zSI1StT07LM

### 扇形視野
最後可能可以分析的是更接近真實的視野，不是360度。轉成170之類的。
![Orientation](/uploads/GettingLost.gif)
