---
title: "Xcode Command PhaseScriptExecution Failed"
date: 2023-03-08T02:00:38+08:00
description: "write something"
keywords: ["iOS"]
draft: false
showtoc: true
tags: ["iOS"]
---

## 前言

在 Windwos 環境使用 Unity 輸出 XCode，之後使用 Mac 測試、上傳，出現了錯誤。

```text
Command PhaseScriptExecution failed with a nonzero exit code
```

![img]

## 解決方式

因為專案有使用 Cardboard，且 Unity 是使用 2022，才會出現此問題，之前使用 2021 輸出上架都沒問題。

我的解決方法是把專案改成在 MacOS 上輸出就能完美解決此問題。

## 測試過的方法

有測試過的方法，可是對我這次情況沒有效果。

* 升級或安裝 Pod。[參考][url_1]
* 修改 build phases 開啟 For install builds only。[參考][url_2]
* 修改 Workspace Setting 的 Build System，在 Xcode 14 無法修改。[參考][url_3]

______________________________________________________________________

[img]:https://i.imgur.com/AvL0uqn.png
[url_1]:https://forum.unity.com/threads/error-on-build.561706/#post-5585278
[url_2]:https://stackoverflow.com/questions/73760753/xcode-14-0-command-phasescriptexecution-failed-with-a-nonzero-exit-code
[url_3]:https://blog.csdn.net/qq_40697071/article/details/99055070
