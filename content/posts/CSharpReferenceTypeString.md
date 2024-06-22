---
title: "C# Reference Type String"
date: 2024-06-22T17:40:52+08:00
description: "淺談 C# Reference Type String"
keywords: ["C#", "Reference Type", "Value Type", "String"]
draft: false
showtoc: true
tags: ["C#"]
---

## 前言

之前有提到 [C# Value Type、Reference Type 的差異]，提到 String 是 Reference Type，可是在使用時卻很像 Value Type。

### 比較值

Value Type 以 `int` 為舉例

```C#
int a = 1;
int b = a;
a = 2;
Console.WriteLine(a == b); // 輸出: false
```

`b` 並沒有隨著 `a` 改變，看得出 int 是 Value Type。

```C#
string str1 = "abc";
string str2 = str1;
str1 = "def";
Console.WriteLine(str1 == str2); // 輸出:  false
```

修改 str1 卻沒有影響到 str2，所以會覺得 string 也是 Value Type(?)。

### 比較地址

使用 `object.ReferenceEquals` 來比較，假如是 Value Type，則會輸出 `false`。

```C#
int a = 1;
int b = 1;
Console.WriteLine(object.ReferenceEquals(a, b)); // 輸出: false
```

`int` 確實是 Value Type，所以會輸出 false。

```C#
string str1 = "abc";
string str2 =  "abc";
Console.WriteLine(object.ReferenceEquals(str1, str2)); // 輸出: true
```

比較結果為 true，假如 String 是應該會與 int 的結果一樣會是 false。

也就是 String 其實是 Reference Type。

## String 為什麼是 Reference Type

[.NET 框架-string 是 value or reference type?][ref1]

這裡面提到兩點

- String 對象，若值相同，則其引用地址相同。

- String 對象，若值不等，則其引用地址不等。

```C#
string str1 = "abc"; //str1指向記憶體位置 addressA 為 abc
string str2 = str1; //str2指向記憶體位置 addressA
str1 = "def"; //str1新指向記憶體位置 addressB 為 def
Console.WriteLine(str1 == str2); // 輸出:  false
```

## 結論

String 特點就是具有不可變性（immutable），一旦 new String 在記憶體(managed heap)上為它分配一塊連續記憶體空間，我們將不能以任何方式對這個 String 進行修改。所有對這個 String 進行各項操作而返回的 String，實際上是另一個重新 new 的 String，其本身並不會產生任何變化。

### String 效能如何？

從上面就可以知道 String 有不可變性，一旦創建了就不能修改值，每次修改 String 都會產生一個新的 String，所以 String 效能比較低。

所以需要經常性操作 String 可以考慮使用 [StringBuilder]。

---

[C# Value Type、Reference Type 的差異]: .../csharpvaluetypereferencetype
[ref1]: https://blog.csdn.net/daigualu/article/details/59096659
[StringBuilder]: https://learn.microsoft.com/zh-tw/dotnet/api/system.text.stringbuilder?view=net-8.0
