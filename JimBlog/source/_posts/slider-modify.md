---
title: slider_modify
date: 2020-05-08 23:01:23
tags: 
- Grasshopper
- Csharp
- Rhino
---

# 前言
因為在grasshopper中slider的上下界是在創建的時後就被確定的，像是直接在canvas中輸入`0..10.0..20.0`這樣就能產生slider滿足
- 上界是20.0，下界是`0.0`
- 精準度是小數點下一位
- slider產生時的起始值就是`10.0`

不過這樣做不能runtime的時候更改slider的上下界，雖然必要性沒有很大，而且想要在runtime有新的值不一定要改到slider本身，但因為相對起來slider還是滿直觀的，所以找了一下在runtime更改slider的文章。
主要內容參考來自[James-Ramsden這篇](http://james-ramsden.com/remotely-change-value-of-number-slider-grasshopper/)

# 內文
有時候會想要很明確掌握slider的範圍。像下面我自己嘗試的一小例子。

![Node](/uploads/SliderModifier0.png)

![Node](/uploads/SliderModifier1.gif)

這個範例的邏輯很簡單，就是有個大的矩形然後我用大矩形的四個頂點做出四個空間，但因為我想要讓大矩形長寬是可以調整的，如果調整的話其他四個房間的slider如果能在runtie的時候就決定上界到多少就不會有超過跟重疊的狀況了。
所以用一下以下的C# code可以幫助決定上下界。
```csharp
private void RunScript(object x, double y, ref object A)
  {
    var input = Component.Params.Input[0].Sources[0]; //get the first thing connected to the first input of this component
    var slider = (Grasshopper.Kernel.Special.GH_NumberSlider) input; //try to cast that thing as a slider

    if(slider != null) //if the component was successfully cast as a slider
    {
      slider.Slider.Value = (decimal) y;
    }
  }
```
這是讓`y`的值去寫進`x`裡面
![Node](/uploads/SliderModifier2.gif)
但你也可以用`slider.Slider.Minimum` 和 `slider.Slider.Maximum`去決定他的上下界。
比如我x的上界會等於y值的一半。就可以這樣寫
```csharp
private void RunScript(object x, double y, ref object A)
  {
    var input = Component.Params.Input[0].Sources[0]; //get the first thing connected to the first input of this component
    var slider = (Grasshopper.Kernel.Special.GH_NumberSlider) input; //try to cast that thing as a slider

    if(slider != null) //if the component was successfully cast as a slider
    {
      if(slider.Slider.Value > (decimal) y / 2){
        slider.Slider.Value = (decimal) y / 2;
      }
      slider.Slider.Maximum = (decimal) y/2;
    }
  }
```
我有寫一個防護的判斷如果原本`x`的值就超過`y/2`的話我就讓值變成`x = y/2` 就不會有奇怪的UI出現了XD
![Node](/uploads/SliderModifier3.gif)

那前面房間長寬slider上下界可以這樣修改
```csharp
private void RunScript(object x, object y, double z, ref object A)
  {
    var input1 = Component.Params.Input[0].Sources[0];
    var slider1 = (Grasshopper.Kernel.Special.GH_NumberSlider) input1;

    var input2 = Component.Params.Input[1].Sources[0];
    var slider2 = (Grasshopper.Kernel.Special.GH_NumberSlider) input2;

    decimal val = slider2.Slider.Value;
    if(slider1 != null && slider2 != null){

      if(slider2.Slider.Value > (decimal) z - slider1.Slider.Value){
        slider2.Slider.Value = (decimal) z - slider1.Slider.Value;
      }
      slider2.Slider.Minimum = 0;
      slider2.Slider.Maximum = (decimal) z - slider1.Slider.Value;

      if(slider1.Slider.Value > (decimal) z - slider2.Slider.Value){
        slider1.Slider.Value = (decimal) z - slider2.Slider.Value;
      }

      slider1.Slider.Minimum = 0;
      slider1.Slider.Maximum = (decimal) z - slider2.Slider.Value;

    }
    //A = val;
  }
```

可以觀察一下`Dining Room` 和 `Living Room` 的水平方向 slider 調整一個另一個的上下界也會改，主要的邏輯是 `大矩形的寬 = DiningRoom_width + LivingRoom_Width`。
這是邊界條件啦 
![Node](/uploads/SliderModifier4.gif)

碰到了XD 但沒有超過~

最後阿，有個可以換slider顏色的方法滿酷的，雖然不知道有甚麼用
![Node](/uploads/SliderModifier5.gif)


```csharp
private void RunScript(object x, double y, System.Drawing.Color z, ref object A)
  {
    var input = Component.Params.Input[0].Sources[0]; //get the first thing connected to the first input of this component
    var slider = (Grasshopper.Kernel.Special.GH_NumberSlider) input; //try to cast that thing as a slider

    if(slider != null) //if the component was successfully cast as a slider
    {
      slider.Slider.DrawControlBackground = true;
      slider.Slider.DrawControlBorder = true;
      slider.Slider.ControlEdgeColour = Color.Blue;
      slider.Slider.ControlBackColour = z;
    }
  }
```

要記得先`using System.Drawing;` 這行喔


# 參考
[James-Ramsden這篇](http://james-ramsden.com/remotely-change-value-of-number-slider-grasshopper/)
James感覺也是grasshopper/Revit相關的部落客/工程師，有很多很好的資源，推薦。