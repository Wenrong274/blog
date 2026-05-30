---
title: "心得 代理人模式"
date: 2024-04-15
summary: "需要與後端同步資料？代理人模式是最常見的解法！本文分享讀書會中對 Remote Proxy 的討論，以及 Java RMI、C# WCF、Android AIDL 都是怎麼做到記憶體資料同步的。"
description: "《深入淺出設計模式》代理人模式（Proxy Pattern）讀書心得。探討遠端代理（Remote Proxy）在多人連線後端資料同步的應用，以及 Java RMI、C# WCF、Android AIDL 等技術如何實現跨程序記憶體同步。"
keywords: ["Design Pattern", "Proxy Pattern", "Remote Proxy", "WCF", "RMI", "AIDL", "GoF", "C#"]
draft: false
tags: ["Design Pattern"]
aliases:
  - /posts/designpattern-proxy/
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
