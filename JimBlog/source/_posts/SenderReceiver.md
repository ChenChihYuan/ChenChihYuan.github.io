---
title: SenderReceiver
date: 2020-05-03 22:58:52
tags: 
- Grasshopper
- Csharp
- Rhino
- Unity
  
categories: 
- Grasshopper
- Csharp
- RhinoInside
- Rhino
---


# 前言
因為最近RIR(Rhino Inside Revit)的整合技術陸續Mcneel官方慢慢釋出相關的document跟陸續有點資料大家在討論，想整理一下目前的資料，以及剛聽完[hiron](https://hrntsm.github.io/)對RhinoInside的簡介，想寫一下部分的筆記，也不確定可以寫多少，讀者加減把這篇看成入門的練習吧，如果有甚麼不詳盡的部分還請利用下方留言板跟我討論一下，謝謝~

# [What is Rhino.Inside](https://www.rhino3d.com/inside/revit/beta/getting-started)
Rhino.Inside is a new technology developed by Robert McNeel & Associates that allows embedding Rhino WIP into other applications. Rhino.Inside is being embedded into many applications from a wide variety of disciplines.

## Rhino.Inside.Revit
Rhino.Inside.Revit is an addon for Autodesk Revit that allows Rhino WIP to be loaded into the memory of Revit just like other Revit addons.

# 2020年5月的進度
如果大家點開[RiR_github](https://github.com/mcneel/rhino.inside)然後可以發現McNeel官方在`.NET` framework下實踐了把Rhino塞到各個平台上。
大概的操作狀況可以參考[hironのblog](https://qiita.com/hiron_rgkr/items/ba00b7ae75068a54ff20), 我自己嘗試了在autocad軟體上成功執行`TestRhinoInside`了。

## Rhino.Inside
- Rhino.Inside illastrator
- Rhino.Inside AutoCAD
- Rhino.Inside Revit
- Rhino.Inside BricsCAD
- Rhino.Inside ConsoleApp
- Rhino.Inside DotNet
- Rhino.Inside JavaScript
- Rhino.Inside UE
- Rhino.Inside Excel
- Rhino.Inside Unity

稍微嘗試了一下Rhino.Inside DotNet，拿了以前的模型丟進WinForms的小視窗。如果有用過WPF或是WinForms的話，大概可以知道做出一個pop up windows的感覺，Revit API也常常會遇到要設計給使用者輸入資訊的情況。
大概會像這樣
![Node](/uploads/RiWin.gif)

不過也有發現現在功能相對不穩定，不一定一開就可以成功，可能開發團隊還在努力吧。

# Sender&Receiver
這部份透過hiron的講解，大概讓我們知道Rhino.Inside的實作方式。因為還沒很深入去看底層的東西(一部分應該目前看也看不懂)就紀錄一下我的猜測，因為Rhino.Inside是把整個Rhino當作子程式掛件掛在其他程式上，如果用Rhino.Inside的時候開檔案管理員，應該會發現程式占用的記憶體會飆高。 但因為是在同一個主程式下的記憶體中運算，所以如果能做成一個通道把Rhino運算的東西傳給其他程式(傳回主程式)，或讀取主程式的資料的話。Rhino.Inside 任何程式都可以看成是資料寫出跟寫入的過程。其實大方向很簡單的來說就是省去了存檔再讀取的步驟，但因為寫出往往是寫到硬碟，又到硬碟讀會讓流程整個變慢，更別說是想要Real Time修改東西了。

## Implement in Grasshopper
hiron介紹了一段code如下:
![Node](/uploads/Sender.gif)

Sender
```csharp
private void RunScript(string Send_Str)
{
    using(var args = new Rhino.Runtime.NamedParametersEventArgs()){
      args.Set("str", Send_Str);
      Rhino.Runtime.HostUtils.ExecuteNamedCallback("ToRiR", args);
    }
}

```

Receiver
```csharp
 private void RunScript(ref object Receive_str)
  {
    Receive(Component);
    Receive_str = str;
  }

  // <Custom additional code> 
  string str;
  bool registered = false;
  IGH_Component comp = null;

  void Receive(IGH_Component component){
    if(!registered){
      Rhino.Runtime.HostUtils.RegisterNamedCallback("ToRiR", RiRhino);
      comp = component;
      registered = true;
    }
  }
  void RiRhino(object sender, Rhino.Runtime.NamedParametersEventArgs args){
    string values;
    if(args.TryGetString("str", out values)){
      str = values;
    }
    comp.ExpireSolution(true);
  }
```
因為其實C# Component中 `Member`縮排那邊有先定義過一個`private readonly IGH_Component Component;` ，所以在上面的`RunScript`可以直接用。
### Runtime.HostUtils.ExecuteNamedCallback
> Execute a named callback
- [Runtime.HostUtils.ExecuteNamedCallback](https://developer.rhino3d.com/wip/api/RhinoCommon/html/M_Rhino_Runtime_HostUtils_ExecuteNamedCallback.htm)
  
### NamedParametersEventArgs
> Dictionary style class used for named callback from `c++` -> `.NET`
- [NamedParametersEventArgs](https://developer.rhino3d.com/api/RhinoCommon/html/T_Rhino_Runtime_NamedParametersEventArgs.htm)

### Runtime.HostUtils.RegisterNamedCallback()
> Register a named callback from `C++` -> `.NET`
- [Runtime.HostUtils.RegisterNamedCallback](https://developer.rhino3d.com/api/RhinoCommon/html/M_Rhino_Runtime_HostUtils_RegisterNamedCallback.htm)

# 參考資料
- [RhinoInside_OfficialPage](https://www.rhino3d.com/inside)
- [RiR_github](https://github.com/mcneel/rhino.inside)
- [hironのblog](https://qiita.com/hiron_rgkr/items/ba00b7ae75068a54ff20)
- [AMDlab](https://amdlaboratory.com/amdblog/rhino-inside%E3%82%92%E4%BD%BF%E7%94%A8%E3%81%97%E3%81%A6revit%E3%81%A8%E9%80%A3%E6%90%BA%E3%81%97%E3%81%A6%E3%81%BF%E3%82%8B/)
- [Introduce_To_Rhino.Inside_Revit](https://www.youtube.com/watch?v=x_MU3vO1_II)