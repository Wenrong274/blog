---
title: "C# Reference Type String"
date: 2024-06-22T17:40:52+08:00
description: "淺談 C# Reference Type String"
keywords: ["C#", "Reference Type", "Value Type", "String"]
draft: false
showtoc: true
tags: ["CSharp"]
---

## 前言

之前有提到 [C# Value Type、Reference Type 的差異]，提到 String 是 Reference Type，可是在使用時卻很像 Value Type。

### 比較值

Value Type 以 `int` 為舉例

```CSharp
int a = 1;
int b = a;
a = 2;
Console.WriteLine(a == b); // 輸出: false
```

`b` 並沒有隨著 `a` 改變，看得出 int 是 Value Type。

```CSharp
string str1 = "abc";
string str2 = str1;
str1 = "def";
Console.WriteLine(str1 == str2); // 輸出:  false
```

修改 str1 卻沒有影響到 str2，所以會覺得 string 也是 Value Type(?)。

### 比較地址

使用 `object.ReferenceEquals` 來比較，假如是 Value Type，則會輸出 `false`。

```CSharp
int a = 1;
int b = 1;
Console.WriteLine(object.ReferenceEquals(a, b)); // 輸出: false
```

`int` 確實是 Value Type，所以會輸出 false。

```CSharp
string str1 = "abc";
string str2 =  "abc";
Console.WriteLine(object.ReferenceEquals(str1, str2)); // 輸出: true
```

比較結果為 true，假如 String 是 Value Type，應該會與 int 的結果一樣會是 false。

也就是 String 其實是 Reference Type。

## String 為什麼是 Reference Type

[.NET 框架-string 是 value or reference type?][ref1]

這裡面提到兩點

- String 對象，若值相同，則其引用地址相同。

- String 對象，若值不等，則其引用地址不等。

```CSharp
string str1 = "abc"; //str1指向記憶體位置 addressA 為 abc
string str2 = str1; //str2指向記憶體位置 addressA
str1 = "def"; //str1新指向記憶體位置 addressB 為 def
Console.WriteLine(str1 == str2); // 輸出:  false
```

## String 特點

String 特點就是具有不可變性（immutable），一旦 new String 在記憶體(managed heap)上為它分配一塊連續記憶體空間，我們將不能以任何方式對這個 String 進行修改。所有對這個 String 進行各項操作而返回的 String，實際上是另一個重新 new 的 String，其本身並不會產生任何變化。

### String 效能如何？

從上面就可以知道 String 有不可變性，一旦創建了就不能修改值，每次修改 String 都會產生一個新的 String，所以 String 效能比較低。

所以需要經常性操作 String 可以考慮使用 [StringBuilder]。

## 結論

來自 ChatGPT 的解釋

C# String 有以下特性：

- String 有不可變性：一旦創建 string，它的內容就不能被改變。

- 賦值操作的實際行為：當執行 `str = "abc"` 時，實際上是創建了一個新的 string 對象，而不是修改原有的對象。然後，變量 `str` 被重新指向這個新對象。

- 為什麼 str2 不變：當執行 `string str2 = str1` 時，str1 和 str2 確實指向了同一個對象。但是當 str1 被賦予新值時，它指向了一個新對象，而 str2 仍然指向原來的對象。

- 值類型的行為：這種行為看起來很像值類型，但 string 仍然是引用類型。這是因為 string 的不可變性和特殊的內存管理方式。

- 性能和內存管理：這種設計有助於提高性能和簡化內存管理，特別是在字符串被廣泛使用的情況下。

- 字符串池（String Interning）：C#使用字符串池來優化內存使用。相同的字符串字面量會指向內存中的同一位置。

這種行為是 C# 語言設計的一個特點，旨在結合引用類型的靈活性和值類型的一些優勢。它可能看起來有點反直覺，但在實際使用中通常是有益的。

---

[C# Value Type、Reference Type 的差異]: .../csharpvaluetypereferencetype
[ref1]: https://blog.csdn.net/daigualu/article/details/59096659
[StringBuilder]: https://learn.microsoft.com/zh-tw/dotnet/api/system.text.stringbuilder?view=net-8.0
