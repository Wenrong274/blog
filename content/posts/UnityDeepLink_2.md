---
title: "Unity Deep Link -2"
date: 2022-12-13T00:20:11+08:00
description: "介紹 Unity Deep Link 呼叫方式"
keywords: ["Unity"]
draft: false
showtoc: true
tags: ["Unity"]
---

## 前言

deep link 可以用網址來當 link id，類似像手機點開 Youtube 網址時，假如裝置內有 Youtube App 就會自動開啟 App，並且切換至該影片內容。而 deep link 也可以用網址來當 link id 達到這樣的效果。

也可以用來呼叫 app 時，假如該裝置沒有安裝可以直接轉移到 app store上面讓使用者直接下載該 app。

## Android

需要在 `AndroidManifest` 上寫上 link id，可以根據 [Create Deep Links to App Content][dl_android] 參考詳細的設置方式。

* url 呼叫方式

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data android:scheme="http" />
    <data android:scheme="https" />
    <data android:host="wenrongdev.com" />
</intent-filter>
```

可以利用這種方式，app 超連結開啟或者網頁輸入 `https://wenrongdev.com/`、`http://wenrongdev.com/` 時就會自動對應到 App。

* 自訂 id

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data android:scheme="app" />
    <data android:host="wenrongdev" />
</intent-filter>
```

可以利用這種方式，app 超連結開啟或者網頁輸入 `app://wenrongdev` 時就會自動對應到 App。

## iOS

Unity 設定 link id 的方式在 [Deep linking on iOS][unitydl_ios] 有介紹如何設定，而 Xcode 也能設定，不過建議是在 Unity 中設定非必要不建議額外自己在手動設置，主要是怕輸出時忘記導致功能失效。

Xcode 詳細設定可以參考這邊 [IOS Deep linking: URL Scheme vs Universal Links][dl_ios]

![img_1]

這樣設定後就會與 Android 一樣的功能，在瀏覽器輸出該 link id 就會自動對應到 app。

## 其他

* [Github][repo]
* [Unity Deep Link -1][blog-1]

______________________________________________________________________

[unitydl_ios]:https://docs.unity3d.com/Manual/deep-linking-ios.html
[dl_android]:https://developer.android.com/training/app-links/deep-linking
[dl_ios]:https://medium.com/wolox/ios-deep-linking-url-scheme-vs-universal-links-50abd3802f97
[img_1]:https://imgur.com/WIvC4gC.png
[blog-1]:https://wenrongdev.com/posts/unitydeeplink_1/
[repo]:https://github.com/WenRongDev/Unity-DeepLink
