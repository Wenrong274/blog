---
title: "IOS Build Note for Unity 2019 Vuforia"
date: 2019-10-17
summary: "Vuforia iOS Build and Run Error。"
keywords: ["Unity", "Vuforia", "iOS"]
draft: false
showtoc: true
tags: ["Unity", "Vuforia"]
---

## 前言

- Unity 2019.2.11f1
- Vuforia 8.5.9

### iOS Build and Run Error

```text
ld: library not found for -liPhone-lib

clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

![img_1](https://i.imgur.com/VfEWoVv.jpg)

#### Solution - iOS Build and Run Error

Build Setting -> Search Paths -> Library Search Paths

移除 `"$(SECROOT)"` 參數

![img_2](https://i.imgur.com/mHa5XkT.jpg)

## iOS Archive Error

```text
ld: warning: ignoring file ...  building for iOS-armv7 but attempting to link with file built for iOS-arm64.
```

![img_3](https://i.imgur.com/7YCtki6.jpg)

#### Solution - iOS Archive Error

根據 [Vuforia Engine Release Notes](https://library.vuforia.com/content/vuforia-library/en/articles/Release_Notes/Vuforia-SDK-Release-Notes.html) 在 `v8.1.7`之後不支援 `32-bit`，並且最低支援 `iOS 11`，因此需要把專案版本最低版本設定為 iOS。

- 設定 iOS architectures

  Build Setting -> Architectures -> Architectures

  Architectures 改為 `Standard architectures`

  ![img_5](https://i.imgur.com/1fcwuH9.jpg)

- 設定 iOS 版本

  Build Setting -> Deployment -> iOS Deployment Target

  iOS Deployment Target 改為 `iOS 11.0`

  ![img_4](https://i.imgur.com/rPGArpU.jpg)

## iOS Distribution Error

```text
ERROR ITMS-90534
```

![img_6](https://i.imgur.com/0KoHlnL.jpg)

### Solution - iOS Distribution Error

請使用 [**Xcode 11.2.1**](https://developer.apple.com/download/) 輸出，即可修正。

## 參考文章

[Unity Xcode Error "library not found for -" の解決方法](https://qiita.com/Narinii/items/d571ac9a4b2193f19bef)

[Can't submit apps to AppStore: ERROR ITMS-90534: "Invalid Toolchain](https://stackoverflow.com/a/58747930)

---
