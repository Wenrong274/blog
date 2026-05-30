---
title: "Android P HTTP Error"
date: 2020-01-09
summary: "Android 9.0 升級後 HTTP 請求全部失敗？錯誤訊息 Cleartext HTTP traffic not permitted 的快速解法，一行設定搞定！"
description: "Android 9.0（Android P）預設封鎖明文 HTTP 流量，導致 WebRequest 回傳錯誤。本文說明如何在 AndroidManifest.xml 加入 usesCleartextTraffic 設定來解決此問題。"
keywords: ["Android", "Android P", "HTTP", "AndroidManifest", "Cleartext", "WebRequest", "Unity"]
draft: false
tags: ["Android"]
---

## 前言

在 Android 9.0 中使用 WebRequest 時，URL 是需要用 Https 才能正常使用，不然 Response 都是 Error。（[Google Doc](https://developer.android.com/about/versions/pie/android-9.0-changes-28?hl=zh-cn#apache-p)）

Error Log：`Cleartext HTTP traffic to 45.xx.xxx.xx not permitted`

## Solution

在 `AndroidManifest.xml` 的 `application` 加入 `android:usesCleartextTraffic="true"`。

```xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest ...>
        <uses-permission android:name="android.permission.INTERNET" />
        <application
            ...
            android:usesCleartextTraffic="true"
            ...>
            ...
        </application>
    </manifest>
```

## 參考連結

[Android 中 HTTP 网络请求相关问题](https://michaelyb.top/2018/08/Android-HTTP/)

---
