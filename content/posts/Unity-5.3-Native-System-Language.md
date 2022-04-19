---
title: "Unity 5.3 Native System Language"
date: 2019-10-04T00:00:00+08:00
summary: "使用 Unity 取得 Windows、Android、iOS 原生語系。"
keywords: ["Unity"]
draft: false
showtoc: true
tags: ["Unity"]
---

## 前言

在 Unity API 中有 [Application.systemLanguage](https://docs.unity3d.com/530/Documentation/ScriptReference/Application-systemLanguage.html) 可以取得系統語言。可是 Unity 5.3 用於 iOS 上，只要是裝置為中文語系一律回傳 SystemLanguage.Chinese，無法判別簡 / 繁語系，因此才研究怎麼取得 Windows、Android、iOS 原生語系。

### Windows Platform

利用 [GetSystemDefaultLCID](https://docs.microsoft.com/en-us/windows/win32/api/winnls/nf-winnls-getsystemdefaultlcid) 取得本機端語系，再利用 [CultureInfo.GetCultureInfo](https://docs.microsoft.com/zh-tw/dotnet/api/system.globalization.cultureinfo.getcultureinfo?view=netframework-3.5) 轉化為本機端語系文化。

```C#
[DllImport("kernel32.dll")]
```

[參考文章](http://answers.unity.com/answers/1323282/view.html)

### Android Platform

直接呼叫原生系統 API 取得。

```C#
private static string CurrentAndroidLanguage()
 {
     string result = "";
     using (AndroidJavaClass cls = new AndroidJavaClass("java.util.Locale"))
     {
         if (cls != null)
         {
             using (AndroidJavaObject locale = cls.CallStatic("getDefault"))
             {
                 if (locale != null)
                 {
                     result = locale.Call("getLanguage") + "_" + locale.Call("getDefault");
                     Debug.Log("Android lang: " + result);
                 }
                 else
                 {
                     Debug.Log("locale null");
                 }
             }
         }
         else
         {
             Debug.Log("cls null");
         }
     }
     return result;
 }
 ```

[參考文章](https://forum.unity.com/threads/application-systemlanguage.211171/#post-1423369)

### iOS Platfrom

製作一個 .mm 文件，內容如下

```objc
 char* cStringCopy(const char* string)
 {
   if(string == NULL){
     return NULL;
   }
   char* newString = (char*)malloc(strlen(string) + 1);
   strcpy(newString, string);
   return newString;
 }
 extern "C"
 {
   const char* CurIOSLang ()
   {
     NSArray *languages = [NSLocale preferredLanguages];
     NSString *CurrentLanguage = [languages objectAtIndex:0];
     return cStringCopy([CurrentLanguage UTF8String]);
   }
 }
```

在 C# 寫出 CurIOSLang 的接口

```C#
[DllImport("__Internal")]
private static extern string CurIOSLang();
```

這樣就可以在 Unity 直接呼叫 CurIOSLan 取得 iOS 語系了。

[參考文章](https://blog.csdn.net/teng_ontheway/article/details/50277169)

______________________________________________________________________

### [Github repo](https://github.com/Wenrong274/NativeSystemLanguage)
