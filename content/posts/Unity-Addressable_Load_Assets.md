---
title: "Unity Addressable Load Assets"
date: 2022-05-04
description: "介紹 Unity Addressable 讀取、生成、釋放方式"
keywords: [Unity, Addressable, hotfix]
draft: false
showtoc: true
tags: [Unity]
---

## 前言

介紹我使用 Addressable 的讀取、生成、釋放方式。

詳細設定還是可以先觀看[官方文件][url_1]。

## Event Viewer

![img-1]

可以利用 Event Viewer 在 Editor Runtime 時，隨時監控記憶體使用情況。就可以很明顯的發現，那些東西是忘記釋放掉的，或者不需要釋放的。

## Load Asset

```C#
IEnumerator InstantiateAsset(string asset)
{
    AsyncOperationHandle<GameObject> async = asset.LoadAssetAsync<GameObject>();
    yield return async;
    GameObject go = Instantiate(async.Result);
}

IEnumerator InstantiateAssets(string label)
{
    AsyncOperationHandle<IList<GameObject>> async = Addressables.LoadAssetsAsync<GameObject>(label, null);
    yield return async;
    for (int i = 0; i < async.Result.Count; i++)
        GameObject go = Instantiate(async.Result[i]);
}

IEnumerator InstantiateAsset(AssetReference asset)
{
    AsyncOperationHandle<GameObject> async = asset.LoadAssetAsync<GameObject>();
    yield return async;
    GameObject go = Instantiate(async.Result);
}
```

可以利用這三種方式讀取 Asset，並且把物件生成出來，當然我還是最推薦使用 AssetReference，除非有什麼特殊需求要使用字串，不然我不會換成其他方式。

## Release Asset

釋放掉 Asset 也記得要把物件`刪除`，不然場上會遺留破圖的物件。

```C#
private void ReleaseAsset(AsyncOperationHandle<T> async)
{
    Addressables.Release(async);
}

private void ReleaseAsset(T asset)
{
    Addressables.Release(asset);
}

private void ReleaseAsset(AssetReference asset)
{
    asset.ReleaseAsset();
}
```

可以利用生成的物件或者 Load asset 的 Async 釋放，假如是使用 AssetReference Load Asset，也可以使用這個方式釋放。

---

[url_1]: https://docs.unity3d.com/Packages/com.unity.addressables@1.3/manual/MemoryManagement.html
[img-1]: https://imgur.com/ohiBIeL.jpg
