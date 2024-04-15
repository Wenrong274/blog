---
title: "心得 組合模式"
date: 2024-04-01
description: "主要是針對深入淺出設計模式書籍，提到的組合模式的心得。"
keywords: ["Design Pattern", "Composite Pattern"]
draft: false
showtoc: true
tags: ["Design Pattern"]
---

## 前言

組合模式是需要 Leaf（子節點）的嗎？

這是我提出來的問題，因為我覺得 Composite，同時也繼承了 Component，這樣跟 Leaf 也有繼承 Component 差不多。

後來我覺得也許拆開是為了職責問題，Composite、Leaf 實現功能可能不一樣。

我後來也參考了別人的[定義][ref1]，其中 Composite、Leaf 的定義有明確職責。

> - Component: 是一個抽象類別，組合模式中的物件都繼承此類別。其中， inflate() 是每一個物件都要實作的功能。 add(Component) 以及 remove(Component) 都是提供給 Composite 類別組合 Leaf 使用的。
> - Leaf : 是一個具象類別 (Concrete class)，實作 Component 所定義的 inflate()方法，為一個最小單位的物件，在這裏不能包含其他 Leaf 。
> - Composite ：同樣也是一個具象類別 (Concrete class)，除了實作 Component 定義的 inflate() 方法。將多個 Leaf 紀錄在一個列表 components 裡利用 add(Component) 以及 remove(Component) 來對列表做處理。

## 結論

讀完這章節後，在我的個人是沒想到自己能使用的環境，可能使用環境是偏像資料搜尋、物件搜尋等。

---

[ref1]: https://andyludeveloper.medium.com/design-pattern-%E7%B5%84%E5%90%88%E6%A8%A1%E5%BC%8F-composite-pattern-ca0d5e0309dc
