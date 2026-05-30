---
title: "Unity WebGL Template"
date: 2019-09-05
summary: "不想在 WebGL 遊戲中顯示 Unity Logo？本文提供自訂 WebGL Template 的步驟，替換 Loading 頁面的 Logo，讓你的 WebGL 更有品牌感！"
description: "Unity WebGL 自訂 Loading 頁面模板教學，說明如何替換預設 Unity Logo，透過 Player Settings > Resolution and Presentation 選擇自訂 Template，修改 logo.png 路徑，以及使用 Responsive WebGL Template 套件省去手動調整的步驟。"
keywords: ["Unity", "WebGL", "WebGL template", "custom loading", "Unity Logo", "Player Settings", "HTML template"]
draft: false
tags: ["Unity"]
---

## 前言

執行 WebGL 時都會有 Unity Logo & Loading。目前此專案修改 Unity Logo 的部分。

需要更詳細的內容可以參考官方文件（[Unity Document](https://docs.unity3d.com/Manual/webgl-templates.html)）。

會比較建議使用 [Responsive WebGL Template](https://assetstore.unity.com/packages/tools/gui/responsive-webgl-template-117308)，省去自己測試修改的麻煩，不過還是需要改 Logo、Icon 的部分。

### Setting Up Your Template

1. Import [Unitypackage](https://github.com/hybrid274/UnityWebGLTemplate/blob/master/build/release.unitypackage)

1. Set up Unity Player Setting
   Edit -> Project Settings -> Player, On the WebGL tab -> Resolution and Presentation -> **Selcet LogoTemplates**

   ![image_1](https://raw.githubusercontent.com/hybrid274/UnityWebGLTemplate/master/images/logotemplate.jpg)

1. Change Your Logo

   Logo 規格建議不要太大張。

   Path: root/Assets/WebGLTemplates/LogoTemplate/**logo.png**

   ![image_2](https://raw.githubusercontent.com/hybrid274/UnityWebGLTemplate/master/images/setinglogo.JPG)

### [GitHub repo](https://github.com/Wenrong274/UnityWebGLTemplate)

[參考文章](https://ocias.com/blog/how-to-set-up-a-unity-webgl-template/)

---
