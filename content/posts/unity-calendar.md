---
title: "Unity Calendar"
date: 2021-06-17
summary: "Unity 遊戲裡要怎麼讓玩家選擇日期？本文提供完整的 Unity 日曆元件實作，預設顯示當天日期，用 GetDate() 就能取得使用者選擇的日期！"
description: "Unity 日曆 UI 功能實作介紹，提供可互動的日期選擇元件，預設顯示當天日期，透過 UnityCalendar.GetDate() 方法取得使用者選擇的 DateTime，並支援 Init() 重置功能，附 GIF 示範與 GitHub 專案連結。"
keywords: ["Unity", "Calendar", "DateTime", "UI", "date picker", "uGUI", "C#"]
draft: false
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

---

[gif]: https://i.imgur.com/Pe4nXry.gif
[Github]: https://github.com/Wenrong274/Unity-Calendar
