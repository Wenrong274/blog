---
title: "心得 迭代器模式"
date: 2024-03-26
summary: "IEnumerable 只能用 foreach、IEnumerator 只能用 for？Unity 的 foreach 效能問題是真的嗎？本文整理迭代器模式的讀書會 Q&A，解答各種讓人搞混的細節！"
description: "《深入淺出設計模式》迭代器模式（Iterator Pattern）讀書心得。涵蓋 IEnumerator 與 IEnumerable 的差異、yield return 的延遲執行特性、LINQ 的內外部迭代器、Unity 中 foreach 效能問題的真相，以及 Coroutine 與迭代器的關係。"
keywords: ["Design Pattern", "Iterator Pattern", "IEnumerator", "IEnumerable", "yield return", "LINQ", "C#", "Unity"]
draft: false
tags: ["Design Pattern"]
aliases:
  - /posts/designpattern-iterator/
---

## IEnumerator、IEnumerable 使用 for、foreach 的差別

Q：實作發現 IEnumerable 能使用 foreach、不能使用 for，IEnumerator 能使用 for、不能使用 foreach，這是為什麼。

A：IEnumerator 是無法使用 for，foreach 就是跑迭代器。需要有 index 的概念建議使用 for。

Aforeach 裡面的 List 陣列有改變的話，會出現錯誤。

## C# 迭代器延遲執行

A：使用 LinQ 時需要注意到延遲執行的部分。

## 是否能夠使用迭代器製作 C# List

A：可以，可是想自己寫 `yield return` 是無法的這是 C# 內建的。

## C# 底層都有繼承 IEnumerable 那是否不需要迭代器模式了？

A：實作不太可能，因為會為了做而做，除非是這個類別很特別才需要。

## Unity 使用 for、foreach 的效能，使用 foreach 會比較耗效能嗎？

A：這是 Unity 舊版 bug。一般用法還好除非是在 Update 使用 foreach 才會有明顯的差異。

A：因為 Unity C# 與 MS C# 不太一樣，在使用 Unity 物件時不要使用 `?.` 的方式。

[Null 條件運算子 ?. 和 ?[]][null-conditional]

```CSharp
A?.B();
```

## 更理解迭代器運作方式

可以先理解 LinQ 運作方式，就可以知道內部迭代跟外部的差異。

再來可以理解 C# IEnumerator、IEnumerable，要搭配 for、foreach，並且搭配 yield return，就可以理解 yield return 在迭代器的概念。

最後實作 Unity 協成(Coroutine)，寫一個方法 to IEnumerator 並且使用 StarCoroutine，並且不使用 UnityEngine。

## IEnumerable 在方法上使用是什麼情況會使用到？

A：延遲執行，可以在需要使用的時候再呼叫就可。

Q：假如不懂的話是不是會是一個坑？

A：非同步執行或感受非同步執行時使用。希望執行這行程式的時候，不要卡死在這邊，延遲執行可以等資料匯入後再執行。不懂的人使用確實會是一個坑。

---

[null-conditional]: https://learn.microsoft.com/zh-tw/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-
