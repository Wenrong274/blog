---
title: "Unity Addressable Download Assets"
date: 2022-03-14T16:38:48+08:00
description: "介紹 Unity Addressable AssetReference 下載方式"
keywords: [Unity, Addressable, hotfix]
draft: false
showtoc: true
tags: [Unity]
---
## 前言

在前一篇 [Unity Addressable][blog-1] 介紹了簡易使用的方式，此篇是介紹下載/更新 Asset 的方法。

目前我個人使用過的方式有 單一 Asset、Asset Tag、 Array Asset 的方式。

## Addressables.LoadAssetAsync

官方範例提供的下載方式 [Addressables.LoadAsset(s)Async][ref_LoadingAddressableAssets]，雖然這個是要把 Asset 讀取出來，其實他也有下載功能

```C#
    Addressables.LoadAssetAsync<GameObject>(asset);
```

我是很少使用這種方式下載或更新 Asset，主因是我個人認為 Addressables.LoadAssetAsync 是讀取物件而不是更新物件的功能，所以我在需要更新時不會使用它。

## Update Addressable Name/ Addressable Label

```C#
IEnumerator UpdateAsset(string asset)
{
    var downloadAsync = Addressables.DownloadDependenciesAsync(asset, false);
    yield return downloadAsync;
    Addressables.Release(downloadAsync);
}
```

這個是下載 `Addressable Name` 或 `Label`的方法，可以利用 Label 下載多個不同 Group 的 Asset。

![img-1]

`Label` 是無法利用 Hotfix 的方式產生，所以是要先創好需要的。若假如需要新的 Label 是必須要重新輸出 App 才會有新的 Label。

![img-2]

## Update Asset Reference

```C#
IEnumerator UpdateAsset(AssetReference asset)
{
    var downloadAsync = Addressables.DownloadDependenciesAsync(asset, false);
    yield return downloadAsync;
    Addressables.Release(downloadAsync);
}
```

[AssetReference][ref_AssetReference] 的用法是我最推薦的下載方式，可以很明顯的知道更新物件，不會因為打錯文字導致更新失敗，而且也可以利用這個方式組合物件，讓更新的內容比較簡單方式處理。

例如用一個物件或場景夾帶了多個不同的需要更新的物件，缺點就是這包 pack 輸出會過大，可能需要把每個物件獨立分成多個 AssetReference，利用系統特性夾帶物件變小。不過要是不喜歡這種方式可以看 [Update Multiple Asset References](#Update-Multiple-Asset-References)。

不過 AssetReference 最方便還是使用它來生成、釋放、等等，才是最好用的方式。

## Update Multiple Asset References

```C#
IEnumerator DonwloadMultipleAssets(AssetReference[] assets)
{
    var assetKeys = assets.Cast<AssetReference>();
    var downloadAsync = Addressables.DownloadDependenciesAsync(assetKeys, Addressables.MergeMode.Union);
    yield return downloadAsync;
    Addressables.Release(downloadAsync);
}
```

多個 AssetReference 更新的方式，然後可以一起更新。可以完成一個簡單的多個物件更新，不用利用 Label、整合包、等方式更新。

## 取得下載容量大小方法

```C#
private long size;

public IEnumerator CheckSizeAsync(string asset)
{
    var async = Addressables.GetDownloadSizeAsync(asset);
    yield return async;
    if (async.Status == AsyncOperationStatus.Succeeded)
        size = async.Result;
    Addressables.Release(async);
}
```

## 更新進度條寫法

```C#
IEnumerator UpdateAsset(AssetReference asset)
{
    var downloadAsync = Addressables.DownloadDependenciesAsync(asset, false);
    while (!downloadAsync.IsDone)
    {
        float percent = downloadAsync.PercentComplete;
        Debug.Log($"{asset}: {downloadAsync.PercentComplete * 100} %");
        yield return new WaitForEndOfFrame();
    }
    Addressables.Release(downloadAsync);
}
```
______________________________________________________________________

[blog-1]:../posts/Unity-Addressable.md

[ref_1]:https://docs.unity3d.com/Packages/com.unity.addressables@1.15/manual/DownloadDependenciesAsync.html
[ref_LoadingAddressableAssets]:https://docs.unity3d.com/Packages/com.unity.addressables@1.15/manual/LoadingAddressableAssets.html
[ref_AssetReference]:https://docs.unity3d.com/Packages/com.unity.addressables@0.4/api/UnityEngine.AddressableAssets.AssetReference.html

[img-1]:https://imgur.com/aPKLTt3.jpg
[img-2]:https://imgur.com/2j7oGN0.jpg
