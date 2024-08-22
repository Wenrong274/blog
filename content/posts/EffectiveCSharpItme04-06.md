---
title: "Effective C# 做法 04-06"
date: 2024-08-22T21:28:59+08:00
description: "Effective C# 做法 04-06 心得"
keywords: ["Effective C#", "C#"]
draft: false
showtoc: true
tags: ["C#"]
---

## 做法 04

`以內插字串取代 string.Format()`

推薦這樣寫

```Csharp
string name = "test";
float height = 180;
int age = 20;
string str = $"userName: {name}, age: {age}, height: {height}";
```

### 1. 為什麼 Math.PI 不寫 ToString 會造成 box 呢?

當需要調用 ToString() 方法時，如果我們沒有顯式調用，編譯器會嘗試使用 Object.ToString()。但是，為了使用 Object 上的方法，值類型必須先轉換為 Object，這就導致了裝箱。

## 做法 05

`對文化特定字串偏好 FormattableString`

推薦使用 `FormattableString`，不會因為當地時間顯示或者數字的顯示有所差異。double 的小數點會是＂.＂；如果再歐洲會是顯示＂,＂。

## 做法 06

`避免字串型別 API`

使用 `nameof` 運算子時，任何對於屬性名稱的改變都會正確的反應在用於事件參數的字串中。

```Csharp
public string Name
{
    get { return name; }
    set
    {
        if (value != name)
        {
            name = value;
            PropertyChanged?.Invoke(this,
            new PropertyChangedEventArgs(nameof(Name)));
        }
    }
}
```

---
