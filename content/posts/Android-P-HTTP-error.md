---
title: "Android P HTTP Error"
date: 2020-01-09
summary: "在 Android 9.0 中使用 WebReqesut 時，URL 是需要用 Https 才能正常使用，不然 Response 都是 Error。"
keywords: ["Android"]
draft: false
showtoc: true
tags: ["Android"]
---

## 前言

在 Android 9.0 中使用 WebReqesut 時，URL 是需要用 Https 才能正常使用，不然 Response 都是 Error。（[Google Doc](https://developer.android.com/about/versions/pie/android-9.0-changes-28?hl=zh-cn#apache-p)）

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
