---
title: "Effective C# 做法 14-15"
date: 2024-09-14T23:57:56+08:00
description: "Effective C# 做法 14-15 心得"
keywords: ["Effective C#", "C#"]
draft: false
showtoc: true
tags: ["C#"]
---

## 做法 14

`減少重複的初始化邏輯`

### 1. 建構子初始化程序讓一個建構子呼叫其他的建構子。

```Csharp
public class MyClass
{
    private List<string> coll;
    private string name;

    public MyClass() : this(0, string.Empty) { }

    public MyClass(int count, string name)
    {
        coll = new List<string>(count);
        this.name = name;
    }
}
```

### 2. 選擇預設參數與

選擇預設參數與使用多個建構子多載之間需要權衡考量，建議是不超過 3 個，超過時可以考慮使用一個類別當初始化類別使用。一般來說應該偏好預設值而非多載建構子。

### 3. 繼承類別不能修改父類別中宣告為 readonly 的欄位。

readonly 欄位只能在宣告時或在定義該欄位的類別建構子中賦值。

## 做法 15

`避免建構不必要的物件`

```Csharp
protected override void OnPaint(PaintEventArgs e)
{
    /// 劣
    using(Font myFont = new Font("Arial", 12.0f)){
        e.Graphics.DrawString("Hello, World!", myFont, Brushes.Black, 10, 10);
    }
}
```

推薦方式

```Csharp
private readonly Font myFont = new Font("Arial", 12.0f);

protected override void OnPaint(PaintEventArgs e)
{
    e.Graphics.DrawString("Hello, World!", myFont, Brushes.Black, 10, 10);
}
```

### 1. 區域變數的選擇

在區域變數是參考型別（實值類型就沒關係），且會在經常被呼叫的程序中使用時將它提升至成員變數。

### 2. string 不可變的原因

1. 安全性：敏感資訊不會因此被修改。
2. 執行緒安全：不可變性是天然的執行緒安全。
3. hash 一致：字典的 key 相同內容、相同的位址。
4. 字串池：.NET 優化
5. 性能優化：編譯器和運行可優化字串。
6. API 設計簡化

### 3.建構不可變的可變 builder 類別

可以參考設計模式中的建造者模式。

---
