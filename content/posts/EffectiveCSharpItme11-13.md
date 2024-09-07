---
title: "Effective C# 做法 11-13"
date: 2024-09-07T13:54:06+08:00
description: "Effective C# 做法 11-13 心得"
keywords: ["Effective C#", "C#"]
draft: false
showtoc: true
tags: ["C#"]
---

## 做法 11

`認識 .NET 資源管理`

### 1. 簡易圖示記憶體區塊的關聯圖

![img_1](https://i.imgur.com/dfX2Obh.png)

### 2. 記憶體排列物件的放置位址

![img_2](https://i.imgur.com/FOmH0z0.png)

呼叫 GC 後，記憶體位置改變了，這就是記憶體回收的概念

![img_3](https://i.imgur.com/JNXcMs0.png)

### 3. 記憶體層代

- 層代 0

  最新的層代而且包含存留較短的物件。大部分物件都會在層代 0 的記憶體回收，而且不會存留至下一個層代。記憶體回收從第 0 個層代開始。

- 層代 1

  這個層代包含存留較短的物件，且當做較短與較長物件的緩衝區。層代 0 回收後，將壓縮可取得的物件，升階至層代 1。

  層代 0 已滿時，記憶體會執行回收。如果層代 0 回收沒有足夠的記憶體，才會執行層代 1 回收，再執行層代 2 回收，層代 1 回收之後有存留的物件，會提升至層代 2。

- 層代 2

  存留較長的物件。層代 2 回收存留的物件還是回留在層代 2。

  最後結論就是，假如不在一開始就把物件釋放掉，最後存留至層代 1、2 時，要回收物件的時間就會越來越久，造成的記憶體效率不佳，所以要手動釋放掉物件。

[記憶體回收的基本概念](https://learn.microsoft.com/zh-tw/dotnet/standard/garbage-collection/fundamentals)

### 4. 使用 Dispose 釋放資源，而不是 Finalizer

## 做法 12

`偏好成員初始化程序而非指派陳述`

### 為什麼 initobj 會導致 box、unbox？

initobj 指令用於將值類型的變量初始化為其默認值，因此 initobj 不會導致 boxing 或 unboxing。
使用 initobj 時，是直接記憶體操作，不涉及對象的創建或類型的轉換。

推薦用法

```Csharp
public class MyClass
{
    private List<string> labels = new List<string>();
}
```

## 做法 13

`對靜態類別成員進行適當的初始化`

Singleton 初始化具有複雜的邏輯，可以在 `static` 建構子中寫。

```Csharp
public class MySingleton
{
    private static readonly MySingleton instance = new MySingleton();

    public static MySingleton Instance => instance;

    private MySingleton() { }
}

public class MySingleton2
{
    private static readonly MySingleton2 instance;

    static MySingleton2()
    {
        instance = new MySingleton2();
    }

    public static MySingleton2 Instance => instance;

    private MySingleton2() { }
}
```

---
