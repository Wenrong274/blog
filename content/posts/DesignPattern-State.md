---
title: "心得 狀態模式"
date: 2024-04-10
description: "主要是針對深入淺出設計模式書籍，提到的狀態模式的心得。"
keywords: ["Design Pattern", "Composite Pattern"]
draft: false
showtoc: true
tags: ["Design Pattern"]
---

## 前言

狀態模式主要用來解決多個 `if else` 判斷，並且不會因為多了一個 `if`，導致很多方法都要重新寫判斷。

## 討論

我提出的問題是狀態模式是由狀態(State) 來控制前往哪個 State，為什麼不是狀態機(Context)來去控制 State 流程。

- State 控制 State 切換

優點：控制流程可以簡化 if else 的判斷，因為在 State 會少很多判斷。

缺點：也因為在 State 裡面判斷，在 Context 是無法知道什麼時候切換 State。

- Context 控制 State 切換

優點：可以明確的知道切換時機，並且也知道切換的 State。

缺點：在 Context 切換 State 會有較多的 `if else` 判斷。

## 結論

結論策略模式(Strategy Pattern)與狀態模式的差異，有同事提出是 `State 控制 State 切換`。假如由 Context 控制 State 切換，就跟策略模式類似。

我後來想想也有點道理，Context 控制 State 切換並沒有解決多個 `if else` 判斷，且與策略模式類似。

---
