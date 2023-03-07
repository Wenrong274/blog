---
title: "Unity2021 Video Player Crush"
date: 2023-02-01T00:14:21+08:00
description: "在 Android 11 以上的版本使用 VideoPlayer 呼叫 Stop 時會造成 App Crush。"
keywords: ["Unity"]
draft: false
showtoc: true
tags: ["Unity"]
---

## 前言

在 Android 11 以上的版本使用 VideoPlayer 呼叫 `Stop();` 時會造成 App Crush。

官方論壇討論此問題[文章][post]。

## 錯誤 log

```text
Stack trace:
Error AndroidRuntime signal 11 (SIGSEGV), code 1 (SEGV_MAPERR), fault addr 0x0
Error AndroidRuntime Cause: null pointer dereference
Error AndroidRuntime r0 00000000 r1 00003f06 r2 71303e68 r3 00000002
Error AndroidRuntime r4 0848fed2 r5 e5f9b138 r6 ea0e93e0 r7 00000000
Error AndroidRuntime r8 b5c5bfa8 r9 00000000 r10 b5c5bfe8 r11 00000002
Error AndroidRuntime ip e9e63e58 sp b5c5bf00 lr e9df8263 pc e9d823fa
Error AndroidRuntime
Error AndroidRuntime backtrace:
Error AndroidRuntime #00 pc 000773fa /system/lib/libandroid_runtime.so (BuildId: cb59abe29c72e464af331ce6551ec035)
Error AndroidRuntime #01 pc 00000136 [anon:.bss]
Error AndroidRuntime
Error AndroidRuntime at libandroid_runtime.0x773fa(Native Method)
Error AndroidRuntime at [anon:.0x136(Native Method)
```

## 解決方式

官方在論壇回覆是建議回去 2020 版，之後會修復。因為我使用的專案不方便降版。

我解決的方式 VideoPlayer.Pause()，然後`生成`一個新的 VideoPlayer 物件，原本舊的 VideoPlayer 物件不要關閉物件、不要刪除物件，不然都會造成 App Crush。

不過最終解決方式還是需要等官方處理結束，可以看這個 bug 什麼時候解決 [Issue Tracker][issuetracker]。

______________________________________________________________________

[post]:https://forum.unity.com/threads/android-crash-when-videoplayer-stop-is-executed.1361863/
[issuetracker]:https://issuetracker.unity3d.com/issues/android-application-crashes-when-changing-the-source-url-of-a-video-player-in-android
