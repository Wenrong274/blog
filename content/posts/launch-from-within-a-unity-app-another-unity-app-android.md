---
title: "Launch From Within a Unity App Another Unity App Android"
date: 2020-01-02
summary: "Unity App A 要啟動另一個 Unity App B 並且傳資料過去？本文提供完整的 AndroidJavaObject + putExtra 範例，還包含 App 未安裝時轉跳 Store 的邏輯！"
description: "說明如何在 Android 平台上，從一個 Unity 應用透過 AndroidJavaObject 呼叫另一個 Unity 應用並傳遞資料，使用 getLaunchIntentForPackage 取得目標 App 的 Intent、透過 putExtra 附加資料，以及 App 未安裝時自動導向 App Store 的處理邏輯。"
keywords: ["Unity", "Android", "AndroidJavaObject", "Intent", "putExtra", "inter-app launch", "package manager"]
draft: false
tags: ["Unity", "Android"]
---

## 前言

主要用來 Unity app A 如何傳遞資訊給 Unity app B。

### Demo Script

```CSharp
private class PropertyInfo
{
    public string elementA = string.Empty;
    public string elementB = string.Empty;
    public string elementC = string.Empty;
}


public void Launch(string bundleId, string storelink)
{
    bool fail = false;
    AndroidJavaClass up = new AndroidJavaClass("com.unity3d.player.UnityPlayer");
    AndroidJavaObject ca = up.GetStatic<AndroidJavaObject>("currentActivity");
    AndroidJavaObject packageManager = ca.Call<AndroidJavaObject>("getPackageManager");

    AndroidJavaObject launchIntent = null;
    try
    {
        launchIntent = packageManager.Call<AndroidJavaObject>("getLaunchIntentForPackage", bundleId);
    }
    catch (Exception e)
    {
        fail = true;
    }

    if (fail || launchIntent == null)
        Application.OpenURL(storelink);
    else
    {
        launchIntent.Call<AndroidJavaObject>("putExtra", "elementA", LaunchData.elementA);
        launchIntent.Call<AndroidJavaObject>("putExtra", "elementB", LaunchData.elementB);
        launchIntent.Call<AndroidJavaObject>("putExtra", "elementC", LaunchData.elementC);
        ca.Call("startActivity", launchIntent);
    }
    up.Dispose();
    ca.Dispose();
    packageManager.Dispose();
    launchIntent.Dispose();
}
```

## Property

PropertyInfo 是用來接受資訊的 class，這邊可以自行修改。

---

[Get Android intent Data for Unity](https://wenrongdev.com/get-android-intent-data-for-unity/)
