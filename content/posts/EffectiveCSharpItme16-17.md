---
title: "Effective C# 做法 16-17"
date: 2024-09-22T22:17:41+08:00
description: "Effective C# 做法 16-17 心得"
keywords: ["Effective C#", "C#"]
draft: false
showtoc: true
tags: ["C#"]
---

## 做法 16

`絕不在建構元中呼叫虛擬函式`

本章節有提到 Static Code Analyzer 工具，可以利用這些工具避免建構子中呼叫虛擬函式。
工具分別有

- Visual Studio
- JetBrains Rider
- Visual Studio + ReSharper
  針對 Unity 的 Static Code Analyzer 只能用這些工具
- JetBrains Rider
- Visual Studio + ReSharper
- Unity + Roslyn

## 做法 17

`實作標準 Dispose 模式

### Unmanaged 型別

- sbyte、byte、short、ushort、int、uint、long、ulong、nint、nuint、char、float、double、decimal 或 bool
- 任何 enum 型別

- 任何指標型別

- Tuple 其成員皆為非受控型別

- 任何只包含非受控型別欄位的使用者定義結構型別。

### Unmanaged 資源

- 是否繼承 IDisposable
- 檢查類別文件
- 分析類別用途和功能
- 看原始碼
- 利用反射，查看有沒有 Finalizer，如 sadehandle

### 需要實現 IDisposable 的類別

- 有 Unmanaged 資源
- 包含 IDisposable 成員
- 大型物件或耗資源的類別，如大型資料庫、影片、音效等
- 長生命週期物件，如連線功能、控制器等
- 自訂義資源管理類別

### 不需要實現 IDisposable 的類別

- 純數據類資料
- 無狀態工具類
- 短生命週期的簡單物件
- 不管理任何資源的類別
  不是每的類別都蓄要繼承 IDisposable 。僅當你的類別管理 Unmanaged 資源或者包含實現 IDisposable 介面的成員，才考慮繼承 IDisposable。

### 1.除非你的類別直接持有 Unmanaged 資源，否則你不應該實作 finalizer。

[實作 Dispose 方法](https://learn.microsoft.com/zh-tw/dotnet/standard/garbage-collection/implementing-dispose)

---
