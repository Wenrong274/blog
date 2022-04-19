---
title: "Launch From Within a Unity App Another Unity App Android"
date: 2020-01-02T00:00:00+08:00
summary: "主要用來 Unity app A 如何傳遞資訊給 Unity app B。"
keywords: ["Unity","Android"]
draft: false
showtoc: true
tags: ["Unity","Android"]
---
## 前言

主要用來 Unity app A 如何傳遞資訊給 Unity app B。

### Demo Script

```C#
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

* * *

[Get Android intent Data for Unity](https://wenrongdev.com/get-android-intent-data-for-unity/)
