---
title: "Get Android Intent Data for Unity"
date: 2019-12-26
summary: "主要用來 A App 呼叫 B App 時，B App 該如何接受資料。而 B App 是使用 `Unity` 接收。"
keywords: ["Unity", "Android"]
draft: false
showtoc: true
tags: ["Unity"]
---

## 前言

主要用來 A App 呼叫 B App 時，B App 該如何接受資料。

而 B App 是使用 `Unity` 接收。

### Demo Script

```CSharp
private class PropertyInfo
{
    public string elementA = string.Empty;
    public string elementB = string.Empty;
    public string elementC = string.Empty;
}

public class ExternalCall : MonoBehaviour
{
    PropertyInfo info = new PropertyInfo();

    private void Awake()
    {
#if (!UNITY_EDITOR && UNITY_ANDROID)
        CreatePushClass(new AndroidJavaClass("com.unity3d.player.UnityPlayer"));
#endif
    }

    public void CreatePushClass(AndroidJavaClass UnityPlayer)
    {
#if UNITY_ANDROID
        AndroidJavaObject currentActivity = UnityPlayer.GetStatic<AndroidJavaObject>("currentActivity");
        AndroidJavaObject intent = currentActivity.Call<AndroidJavaObject>("getIntent");
        bool elementA_hasExtra = IsBool(intent, "elementA");
        bool elementB_hasExtra = IsBool(intent, "elementB");
        bool elementC_hasExtra = IsBool(intent, "elementC");
        AndroidJavaObject extras = GetExtras(intent);

        if (extras != null)
        {
            if (elementA_hasExtra)
                info.elementA = GetProperty(extras, "elementA");
            if (elementB_hasExtra)
                info.elementB = GetProperty(extras, "elementB");
            if (elementC_hasExtra)
                info.elementC = GetProperty(extras, "elementC");
        }
#endif
    }

    private bool IsBool(AndroidJavaObject intent, string method)
    {
        bool b = false;

        try
        {
            b = intent.Call<bool>("hasExtra", method);
        }
        catch (Exception e)
        {
            Debug.Log(e.Message);
        }

        return b;
    }

    private AndroidJavaObject GetExtras(AndroidJavaObject intent)
    {
        AndroidJavaObject extras = null;

        try
        {
            extras = intent.Call<AndroidJavaObject>("getExtras");
        }
        catch (Exception e)
        {
            Debug.Log(e.Message);
        }

        return extras;
    }

    private string GetProperty(AndroidJavaObject extras, string name)
    {
        string s = string.Empty;

        try
        {
            s = extras.Call<string>("getString", name);
        }
        catch (Exception e)
        {
            Debug.Log(e.Message);
        }

        return s;
    }
}
```

### Property

`PropertyInfo` 是用來接受資訊的 `class`，這邊可以自行修改。

---

[Launch from within a Unity app another Unity app(Android)](https://wenrongdev.com/launch-from-within-a-unity-app-another-unity-appandroid/)
