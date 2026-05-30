---
title: "Xcode Command PhaseScriptExecution Failed"
date: 2023-03-08
summary: "Unity 在 Windows 輸出 XCode 專案後，到 Mac 建置出現 Command PhaseScriptExecution failed？本文分享使用 Google Cardboard + Unity 2022 遇到此問題的解決方式！"
description: "解決 Unity 在 Windows 環境輸出 XCode 專案後，在 Mac 進行建置時出現「Command PhaseScriptExecution failed with a nonzero exit code」錯誤的問題。本文記錄使用 Unity 2022 搭配 Google Cardboard 時的發生條件，以及改在 macOS 上輸出 XCode 專案的根本解法，並整理升級 Pod、修改 Build Phases 等測試過但無效的方法。"
keywords: ["Unity", "Xcode", "iOS", "PhaseScriptExecution", "build error", "Cardboard", "macOS", "CocoaPods"]
draft: false
tags: ["iOS"]
aliases:
  - /posts/xcode_command_phasescriptexecution_failed/
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

- 升級或安裝 Pod。[參考][url_1]
- 修改 build phases 開啟 For install builds only。[參考][url_2]
- 修改 Workspace Setting 的 Build System，在 Xcode 14 無法修改。[參考][url_3]

---

[img]: https://i.imgur.com/AvL0uqn.png
[url_1]: https://forum.unity.com/threads/error-on-build.561706/#post-5585278
[url_2]: https://stackoverflow.com/questions/73760753/xcode-14-0-command-phasescriptexecution-failed-with-a-nonzero-exit-code
[url_3]: https://blog.csdn.net/qq_40697071/article/details/99055070
