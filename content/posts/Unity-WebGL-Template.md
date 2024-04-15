---
title: "Unity WebGL Template"
date: 2019-09-05
summary: "執行 WebGL 時都會有 Unity Logo & Loading。目前此專案修改 Unity Logo 的部分。"
keywords: ["Unity", "WebGL"]
draft: false
showtoc: true
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
