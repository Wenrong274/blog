---
title: "Unity Calendar"
date: 2021-06-17T02:32:14+08:00
summary: "Unity 實做日曆功能。"
keywords: ["Unity","Calendar"]
draft: false
showtoc: true
tags: ["Unity"]
---

## 前言

![gif]

## 使用方式

預設日期為`當天日期`。

可以直接使用 `UnityCalendar.GetDate()` 取得使用者設定日期，假如有錯誤會回報錯誤。

`testGetDate.cs`

```csharp
public void OnClick_GetDate()
{
    DateTime dt = unityCalendar.GetDate();
    text.text = dt.ToString("yyyy-MM-dd");
}

public void OnClick_Clear()
{
    text.text = string.Empty;
    unityCalendar.Init();
}
```

## [Github]

______________________________________________________________________
[gif]:https://i.imgur.com/Pe4nXry.gif
[Github]:https://github.com/Wenrong274/Unity-Calendar
