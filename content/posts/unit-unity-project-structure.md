---
title: "Unity Project Structure"
date: 2022-10-05
summary: "每次開新 Unity 專案都要手動建立一堆資料夾？本工具一鍵自動建立標準化的專案目錄結構，讓 Art 和 Program 資源各就各位！"
description: "Unity 編輯器工具，透過選單 Tools > Generate Project Structure 自動建立標準化的專案資料夾結構，包含 Art（Prefabs、Shaders、Models、UI、Audio、Video）和 Program（Scenes、Scripts、Tests、Prefabs）分類目錄，提升團隊協作一致性。"
keywords: ["Unity", "project structure", "Editor tool", "folder structure", "Unity Editor extension"]
draft: true
tags: ["Unity"]
aliases:
  - /posts/unit-unity-projectstructure/
---

## Unity Project Structure

主要是建立一個自動建立簡易專案資料夾結構。

此參考 [UnityProjectTreeGenerator][ref] 方法建立資料夾，

### 使用方式

`Tools > Generate Project Structure`

必須要設定 Root Name 才能點擊 `Create Structure`

![img_1]

### 資料夾結構

```text
|- Assets
    |- Project Name /// 自己設定
        |- 00_Art
        |   |- 00_Profabs
        |   |   |- Models
        |   |   |- UI
        |   |- 01_Shaders
        |   |   |- UI_Shaders
        |   |- 02_Timeline
        |   |- 03_Models
        |   |   |- Example_Model
        |   |   |   |- 3D
        |   |   |   |- Animation
        |   |   |   |- Textures
        |   |   |- Example_Effect
        |   |       |- Textures
        |   |- 04_Scenes
        |   |- 05_UI
        |   |   |- Textures
        |   |   |- Effect
        |   |       |- Textures
        |   |       |- Animation
        |   |       |- Material
        |   |- 07_Audio
        |   |- 08_Video
        |- 01_Program
           |- 00_Scenes
           |- 01_Scripts
           |- 02_Tests
           |- 03_Prefabs
           |- 05_UI
```

## [Github][github]

---

[img_1]: https://imgur.com/iBAEGNO.png
[ref]: https://github.com/dkoprowski/UnityProjectTreeGenerator
[github]: https://github.com/Wenrong274/Unity-ProjectStructure
