---
title: "Folder Manager"
date: 2020-03-29
summary: "在 Unity 中管理各種路徑字串很麻煩？FolderManager 提供視覺化介面管理路徑，存成 ScriptableObject 讓程式直接呼叫，不用再硬刻路徑字串！"
description: "Unity 編輯器擴充工具 FolderManager 介紹，透過視覺化介面建立與管理路徑設定，將路徑儲存為 ScriptableObject（FolderManager.asset），讓程式碼直接宣告使用，避免路徑字串散落各處的維護問題。"
keywords: ["Unity", "Editor", "ScriptableObject", "path management", "Unity Editor extension", "StreamingAssets"]
draft: false
tags: ["Unity"]
---

## 前言

視覺化管理使用路徑，不過目前功能還是很粗糙。

## Feature

視覺化管理

![img_1]

## Usage

Create path 之後會在
`Assets\FolderManager\StreamingAssets\FolderManager.asset`
出現 asset。

`Asset` 即是`FolderManager.Folders`，因此可以直接宣告此 class 使用。

## [Github]

---

[img_1]: https://raw.githubusercontent.com/Wenrong274/FolderManager/master/doc/img/img_1.jpg
[Github]: https://github.com/Wenrong274/FolderManager
