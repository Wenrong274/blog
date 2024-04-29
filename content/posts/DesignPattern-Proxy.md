---
title: "心得 代理人模式"
date: 2024-04-15
description: "主要是針對深入淺出設計模式書籍，提到的代理人模式的心得。"
keywords: ["Design Pattern", "Proxy Pattern"]
draft: false
showtoc: true
tags: ["Design Pattern"]
---

## 前言

主要是用來解決大部與後端同步資料的方法，在實作方面代理人模式是最常見的解決方案。

## 討論

### Q1 代理人模式實作方向問題

`多人連線後與後端資料同步`

不過確實在實作主題選擇有限，尤其遠端代理需要與後端同步資料，在目前公司遊戲的架構是不需要這樣做的。

### Q2 需要與後端同步記憶體資料嗎？

不一定，[Java RMI][RMI]、[C# WCF][WCF]、[Android AIDL][AIDL] 可以做到同步記憶體資料。

---

[WCF]: https://learn.microsoft.com/zh-tw/dotnet/framework/wcf/?redirectedfrom=MSDN
[RMI]: https://docs.oracle.com/javase/tutorial/rmi/
[AIDL]: https://developer.android.com/develop/background-work/services/aidl?hl=zh-tw
