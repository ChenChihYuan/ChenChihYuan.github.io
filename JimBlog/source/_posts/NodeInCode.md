---
title: NodeInCode
date: 2020-05-03 01:22:53
tags: 
- Rhino
- Grasshopper
- Csharp
categories: 
- Grasshopper
- Csharp
mathjax: true
---

# 前言
因為想好好寫一下分享文，也整理一下自己的筆記所以弄了這個簡單的 Github Page 如果有甚麼建議還請利用下面的gitalk留言回覆，謝謝~

這篇NodeInCode主要是想跟大家分享`NodeInCode namespace`。看論壇討論串也是2017年初出來的功能，我的理解是，如果在scripting(C#/python/VB)的時候，又想用其他component的功能就很方便。
例如，想要用weaverbirds的Loop_Subd就不用要把自己的C# code因為用不到第三方的component而切成兩個或以上，對資料的整理我覺得會有更好的自由度。

這是我找到的第一次大家討論這個namespace的反應。
[Rhino.NodeInCode namespace?](https://discourse.mcneel.com/t/rhino-nodeincode-namespace/38622)
很好笑的是樓主說:
> Not sure if this is a dream...

哈哈哈我很能理解~

# 如何使用?

實際的操作我是跟著[這篇](https://discourse.mcneel.com/t/using-the-nodeincode-namespace/49735)，所以如果有不太懂的部分，可以仔細追一下這篇文。
## 1. 先確定要使用的component名稱
Component的全名我覺得不太直觀能找到，所以可以使用以下方是條列全部loaded components
```csharp
A = Rhino.NodeInCode.Components.NodeInCodeFunctions.GetDynamicMemberNames();
```
## 2. 輸入參數
```csharp
private void RunScript(Mesh M, int L, int S, ref object A)
  {
    Rhino.NodeInCode.ComponentFunctionInfo mesh = Rhino.NodeInCode.Components.FindComponent("WeaverBird_WeaverbirdsLoopSubdivision");
    if(mesh == null) return;

    string[] warnings;
    object[] result = mesh.Evaluate(new object[]{
      M,
      L,
      S
      },
      false, out warnings
      );

    if(warnings != null) foreach(var w in warnings) Print(w);

    A = result[0];
  }
```

![Node](/uploads/NodeInCode_WB.JPG)

當然有些list或DataTree的結構這邊也可以做。
透過這個方式可以重新包裝第三方的Component但因為Grasshopper基本上都是開源的，不要因為這樣就重新包裝別人的code說是自己的，要給設計方credit，這很重要。
之前有發生過pufferfish的component被包進另一個panda component的事件，很不好啦。

# 後記
之前哈佛的建築系教授[garciadelcastillo](https://www.youtube.com/channel/UCgKQS7cuioD9nfOYT4y9w-g) 做了一個project `GH#`就是想做個documentation幫助從grasshopper介面轉到C#的過程。
很多時候不一定邏輯想不通，而是不確定可以用的工具有哪些。garciadelcastillo也提過說基本上RhinoCommon的document跟grasshopper的default component應該可以找到一對一對應的(我實際上覺得C#的自由度更大)，
但有些輸入的parameters跟細微的定義會依照programmer的sense而有些微的不同。

另外最近我自己對RTree跟kangaroo的結合很感興趣，也發現也發現有這方面的研究
感覺是一個CITA的教授的project[RTree+Kangaroo](http://petrasvestartas.com/Weaving)
[Petras Vestartas](http://petrasvestartas.com/) 
雖然沒有實際看過他這個模擬的code但因為他是用kangaroo結合RTree所以應該也是用這樣`NodeInCode`的方式


{% vimeo 206718317 %}

因為之前做了Fuji Pavilion 1970 Expo用了kangaroo做這種模擬，但發現接觸點太多會讓計算變得很慢，所以有Rtree的幫忙應該會快很多。
其實目前覺得最快的應該是用FlexHopper(from Benjamin Felbrich) 因為FlexHopper用Nvidia Flex CLI作，用GPU會快很多，再加RTree那應該可以真的很快。


[RTree](https://developer.rhino3d.com/api/RhinoCommon/html/T_Rhino_Geometry_RTree.htm)



 
