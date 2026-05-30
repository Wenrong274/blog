---
title: "Unity Jenkins Build"
date: 2020-02-25
summary: "Unity 專案要怎麼串 Jenkins 自動建置？本文整理 Android SDK、JDK、Unity3d Plugin 的 Jenkins 環境設定步驟，以及 Editor command line arguments 的完整寫法！"
description: "Unity 專案搭配 Jenkins 持續整合（CI）的完整設定教學，包含 Jenkins 環境變數（ANDROID_HOME）設定、JDK 8 版本要求、Unity3d Plugin 安裝與 Unity Editor 路徑設定，以及 Editor command line arguments 的格式說明，支援 Android、iOS、WebGL 等多平台自動輸出。"
keywords: ["Unity", "Jenkins", "CI/CD", "DevOps", "Android SDK", "build automation", "Unity3d Plugin", "continuous integration"]
draft: false
tags: ["Unity", "DevOps"]
---

## 前言

此為使用 Jenkins 輸出 Unity 專案注意事項。

## Setting

須注意 Unity 有無安裝輸出目標平台（Android、iOS、WebGL...）。

並且要設定 Jenkins 環境（AndroidSDK、JDK、Unity Editor）。

### Jenkins Android SDK

需要新增 Jenkins 環境變數（Environment variable），來設定 Android SDK 路徑。

Jenkins 頁面路徑為 `Manage Jenkins -> Configure System -> Global properties`。

設定如下圖：

![img_1]

`Name`：`ANDROID_HOME`

`Value`：AndroidSDK 路徑。

### Jenkins JDK

JDK 版本請選 `Java SE 8`，因為 Unity 只支援 Java SE 8。

Jenkins 頁面路徑為 `Manage Jenkins -> Global Tool Configuration -> JDK`。

![img_2]

### Jenkins Unity3d Plugin

需要至 Plugin Manager 安裝 Unity3d Plugin。

Jenkins 頁面路徑為 `Manage Jenkins -> Plugin Manager -> Available`

安裝完成後，需要設定 Unity Editor 路徑。

![img_3]

`Name`：unity version

`Installation directory`：unity installed path

### Jenkins item

基本設置可參考 [使用 jenkins 建置 unity3d 專案][ref_1] 介紹。

最主要是設定 `Editor command line arguments`。

頁面路徑：`Configure -> General -> Build`

點選 Add build step -> invoke Unity3d Editor，選擇對應的 Unity 編輯器版本。

在 Editor command line arguments 輸入

```text
-projectPath "$WORKSPACE/" -executeMethod JenkinsBuild.BuildPlatforms -buildPath "$WORKSPACE\Builds" -android -batchmode -nographics -quit
```

`-buildPath "$WORKSPACE\Builds"` "$WORKSPACE\Builds 輸出放置資料夾路徑。

`-android` 為輸出平台，可改為 -windows32、-windows64、-linux64、-macos、-android、-ios、-webgl。

![img_4]

## [GitHub Repo][github]

## reference

[使用 jenkins 建置 unity3d 專案][ref_1]

[Jenkins for Unity with DigitalOcean][ref_2]

---

[img_1]: https://raw.githubusercontent.com/Wenrong274/UnityJenkinsBuild/master/doc/img/1.JPG
[img_2]: https://raw.githubusercontent.com/Wenrong274/UnityJenkinsBuild/master/doc/img/2.JPG
[img_3]: https://raw.githubusercontent.com/Wenrong274/UnityJenkinsBuild/master/doc/img/3.JPG
[img_4]: https://raw.githubusercontent.com/Wenrong274/UnityJenkinsBuild/master/doc/img/4.JPG
[github]: https://github.com/Wenrong274/UnityJenkinsBuild
[ref_1]: https://hoseex.blogspot.com/2017/12/jenkinsunity3d.html
[ref_2]: https://github.com/CarlHalstead/Jenkins-for-Unity-with-DigitalOcean/
