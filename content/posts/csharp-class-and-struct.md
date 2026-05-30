---
title: "C# Class 和 Struct 選擇"
date: 2024-06-12T22:47:08+08:00
summary: "Class 還是 Struct？這個問題困擾很多開發者。本文整理 Microsoft 官方建議，用幾個具體條件幫你快速判斷什麼時候應該選 Struct！"
description: "深入探討 C# 中 Class 與 Struct 的選擇時機。依據 Microsoft 官方指引，說明在執行個體大小、可變性、Boxing 頻率等條件下，應優先使用 Struct 還是 Class 的決策準則。"
keywords: ["C#", "Class", "Struct", "Reference Type", "Value Type", "Boxing", ".NET"]
draft: false
tags: ["C#"]
aliases:
  - /posts/csharpclassandstruct/
---

## 前言

之前有講到 [C# Value Type、Reference Type 的差異]，現在來講一下 Class 和 Struct 選擇。

從上一篇的文章中可以知道 Class 是 Reference type，而 Struct 是 Value type。

## 如何選擇

根據[在類別和結構之間選擇]

> 作為經驗規則，架構中大部分的類型應該為類別。 不過，在某些情況下，實值型別的特性會使它更適合使用結構。

可以知道其實大部分時都是使用 Class，但在某些情況下使用 Struct 會比較適合。

### 什麼時候使用 Struct

**如果類型的執行個體很小，且通常短期或通常內嵌在其他物件中，請考慮定義結構，而不是類別。**

除非類型具有下列所有特性，否則請「避免」定義結構：

- 其以邏輯方式表示單一值，類似於基本類型 (int、double 等)。

- 其執行個體大小低於 16 個位元組。

- 類型為不可變，

- 而且不需要經常進行 Box。

**在所有其他情況下，您應該將類型定義為類別。**

## 參考連結

[一起學 Class and Struct (C#)]

---

[C# Value Type、Reference Type 的差異]: ../csharpvaluetypereferencetype
[一起學 Class and Struct (C#)]: https://hackmd.io/@SuFrank/H1coLlCaq
[在類別和結構之間選擇]: https://learn.microsoft.com/zh-tw/dotnet/standard/design-guidelines/choosing-between-class-and-struct
