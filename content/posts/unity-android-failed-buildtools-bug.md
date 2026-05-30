---
title: "Android Build Failed Build tools 3X.0.0 Bug"
date: 2022-03-07
summary: "輸出 APK 遇到 Installed Build Tools revision 3X.0.0 is corrupted？本文提供快速修復方法：改兩個檔名就搞定！"
description: "解決 Android Build Tools 31.0.0（及以上版本）出現「Installed Build Tools revision 3X.0.0 is corrupted」錯誤的方法，透過將 Android SDK build-tools 目錄下的 d8.bat 重新命名為 dx.bat，以及 d8.jar 重新命名為 dx.jar 來修復此問題。"
keywords: ["Android", "Android SDK", "Build Tools", "APK", "Unity", "build error", "dx.bat", "d8.bat"]
draft: false
tags: ["Android"]
aliases:
  - /posts/unity-android_failed_buildtoolsbug/
---

## 前言

輸出 Apk 遇到的錯誤

```text
Fix Installed Build Tools revision 3X.0.0 is corrupted.
```

![img_1]

## 修改方式

### 修改 d8.bat

檔案路徑 `<Android SDK root>\build-tools\3X.0.0`

![simg_1]

將 `d8.bat` 改為 `dx.bat`。

### 修改 d8.jar

檔案路徑 `<Android SDK root>\build-tools\3X.0.0\lib`

![simg_2]

將 `d8.jar` 改為 `dx.jar`。

就完成修改。

## 參考連結

[Android Studio error "Installed Build Tools revision 31.0.0 is corrupted"][url_1]

---

[img_1]: https://imgur.com/CziTHbS.jpg
[simg_1]: https://imgur.com/YwqG9ek.jpg
[simg_2]: https://imgur.com/sLwOhQx.jpg
[url_1]: https://stackoverflow.com/a/68430992
