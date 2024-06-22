---
title: "C# Value Type、Reference Type 的差異"
date: 2024-06-11T22:38:53+08:00
description: "討論 C# Value Type、Reference Type 的差異"
keywords: ["C#", "Reference Type", "Value Type"]
draft: false
showtoc: true
tags: ["C#"]
---

## 前言

在公司讀書會與同事討論淺複製與深複製的差異和使用時機時，聊到 Reference Type、Value Type 的不同，但是要講淺複製與深複製之前要先講 Reference Type、Value Type 的差異。

## Value Type 有哪些

1. 整數的數字型別：sbyte、byte、short、ushort、int、uint、long、ulong、nint、nuint

1. 浮點數值型別：float、double、decimal

1. [內建數值轉換]

1. bool

1. char

1. enum

1. struct

1. ref struct

1. tuples

1. [可為 Null 的值類型 (C# 參考)]

參考：[實值類型 (C# 參考)]

## Reference Type 有哪些

1. 宣告參考型別：class、interface、delegate、record

1. 內建參考類型：dynamic、object、string

1. [可為 Null 的參考型別]

1. 集合和陣列：集合（List、Dictionary）、陣列（int[]、string[]）

參考：[參考型別 (C# 參考)]

## Value Type、Reference Type 的差異

根據[在類別和結構之間選擇]裡的一段話，可以分出 5 點差異。

### 1. 存放記憶體

> 參考型別會配置在回收的堆積和記憶體上，而實值型別則會配置在堆疊上，或內嵌在包含型別上，並在堆疊回溯時或其包含類型解除配置時解除配置。
>
> 因此，實值型別的配置和解除配置的成本通常比參考型別的配置和解除配置的成本更低。

可以得知主要是在存放的記憶體差異，因此 Value Type 配置和解除配置的成本比 Reference Type 成本更低。

實值型別的配置和解除配置的成本通常比參考型別的配置和解除配置的成本更低。

### 2. 陣列

> 參考型別的陣列會以換行方式配置，這表示陣列元素只是位於堆積上之參考型別執行個體的參考。實值型別陣列會內嵌配置，這表示陣列元素是實值型別實際的執行個體。
>
> 因此，實值型別陣列的配置和解除配置的成本會遠比參考型別陣列的配置和解除配置的成本來得低。此外，在大部分情況下，實值型別陣列會呈現較佳的參考位置。

Value Type 陣列的配置和解除配置成本較低，且通常在記憶體存取表現上優於 Reference Type 陣列。然而，Reference Type 陣列在處理複雜的物件或需要物件參考的情境中仍有其必要性。

### 3. 記憶體使用量

> 強制轉型為參考型別或其實作的其中一個介面時，實值型別會進行 Boxed。 當強制轉型回實值型別時，它們會進行 Unboxed。
>
> 因為 Box 是配置在堆積上且被回收記憶體的物件，所以過多 Boxing 和 unboxing 可能會對堆積、記憶體回收行程，以及最終是應用程式的效能造成負面影響。

強制轉型為 Reference Type 時，Value Type 會進行 Boxed。 當強制轉型回實值型別時會進行 Unboxed，過多的 Boxed 和 Unboxed 會增加垃圾回收（GC）的負擔，從而影響應用程式效能。

### 4. 複製參考

> 參考型別指派會複製參考，而實值型別指派則會複製整個值。 因此，大型參考型別的指派成本比大型實值型別的指派成本更低。

### 5. 傳遞方式

> 參考型別會以傳址方式傳遞，而實值型別則是以傳值方式傳遞。對參考型別的執行個體所做的變更會影響所有指向執行個體的參考。實值型別執行個體會在以傳值方式傳遞時複製。變更實值型別的執行個體時，它必然不會影響其任何複本。
>
> 由於不會由使用者明確建立複本，而是會在傳回引數或傳回值時隱含建立，因此可以變更的實值型別可能會對許多使用者造成混淆。 因此，實值型別應該為不可變。

由於 Value Type 在傳遞過程中會複製，變更複本時不會影響原始值，這有時會讓使用者感到困惑，因為預期的變更並未反映在原始變數上。

為了避免這種混淆，Value Type 通常應設計為不可變（immutable）。不可變的 Value Type 一旦創建，其狀態就無法改變，這樣可以確保每個複本都是一致且獨立的。

- Reference Type

傳遞物件的記憶體地址。因此，對該物件所做的變更會影響所有持有該物件地址的參考。

當 Reference Type 的物件進行變更時，因為所有指向該物件的參考都是同一個實例，所有持有該參考的變數都會受到變更影響。

- Value Type

會複製該值並將複本傳遞給目標位置。因此，對複本的變更不會影響原始值。

當傳遞 Value Type 並對其進行變更時，因為是對複本進行操作，原始實例不會受到任何影響。這意味著每個複本都是獨立的。

## 參考連結

[一起學 Class and Struct]

---

[在類別和結構之間選擇]: https://learn.microsoft.com/zh-tw/dotnet/standard/design-guidelines/choosing-between-class-and-struct
[內建數值轉換]: https://learn.microsoft.com/zh-tw/dotnet/csharp/language-reference/builtin-types/numeric-conversions
[可為 Null 的值類型 (C# 參考)]: https://learn.microsoft.com/zh-tw/dotnet/csharp/language-reference/builtin-types/nullable-value-types
[參考型別 (C# 參考)]: https://learn.microsoft.com/zh-tw/dotnet/csharp/language-reference/keywords/reference-types
[實值類型 (C# 參考)]: https://learn.microsoft.com/zh-tw/dotnet/csharp/language-reference/builtin-types/value-types
[可為 Null 的參考型別]: https://learn.microsoft.com/zh-tw/dotnet/csharp/language-reference/builtin-types/nullable-reference-types
[一起學 Class and Struct]: https://hackmd.io/@SuFrank/H1coLlCaq
