---
title: "Effective C# 做法 01-03"
date: 2024-08-17T23:09:29+08:00
summary: "var 要怎麼用才正確？const 和 readonly 差在哪？型別轉換要用 is 還是 as？本文整理 Effective C# 前三個做法的重點筆記！"
description: "Effective C# 做法 01-03 讀書心得，涵蓋：偏好使用 var 隱含型別宣告的時機、const 與 readonly 的選擇準則（編譯期 vs 執行期常數），以及使用 is/as 運算子進行安全型別轉換的最佳實踐。"
keywords: ["Effective C#", "C#", "var", "const", "readonly", "type casting", "is operator", "as operator", ".NET"]
draft: false
tags: ["C#"]
aliases:
  - /posts/effectivecsharpitme01-03/
---

## 做法 01

`偏好隱含型別的區域變數`

參考書籍提到的最後一段

> 簡單說，除非開者（包括以後的你）必須看到型別宣告才能理解程式，否則就使用 var 宣告區域變數。這個做法的標題是＂偏好＂而不是＂總是＂。我建議明確的宣告所有數值型別（int、float、double 與其他）而不要使用 var 宣告。其他東西就是用 var。多打幾個字 - 明確的宣告型別 - 不會提升行別安全或改善可讀性。如果挑錯宣告型別，你可能會造成編譯器本來能夠避免的低效率。

## 做法 02

`偏好 readonly 而非 const`

> 必須在編譯期確定的值必須使用 const：屬性參數、switch case 標籤與 enum 定義，以及少數部會在版本間變化的數值。其餘狀況則傾向以 readonly 常數提升彈性。

### 1. const 使用建議

- 對於在編譯時就能確定且永不改變的值
- 適用於基本數據類型（int、float、bool 等）和字符串
- 效能略優於 readonly，因為它是編譯時常量

### 2. readonly 使用建議

- 對於運行時才能確定值的情況
- 可用於任何數據類型，包括引用類型和複雜類型
- 允許在構造函數中賦值

### 選擇指南

- 如果值在編譯時就能確定，且是基本類型或字符串，優先使用 const。
- 如果是引用類型或需要在運行時計算的值，使用 readonly。
- 如果需要在不同的構造函數中賦予不同的值，使用 readonly。
- 對於靜態成員，如果符合 const 的條件，優先使用 const；否則使用 static readonly。

## 做法 03

`偏好 is 或 as 運算子而非型別轉換`

推薦這樣寫

```Csharp
object o = new MyType();

/// 不推薦
var t = (MyType)o;

/// 推薦
if (o is MyType myType)
{
    myType.Do();
    /// do samething ...
}
```

---
