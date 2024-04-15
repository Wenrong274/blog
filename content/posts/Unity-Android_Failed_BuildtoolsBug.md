---
title: "Android Build Failed Build tools 3X.0.0 Bug"
date: 2022-03-07
description: "Fix Installed Build Tools revision 3X.0.0 is corrupted."
keywords: [Android]
draft: false
showtoc: true
tags: [Android]
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
