---
title: "Data Access WebGL"
date: 2020-02-27
summary: "Unity 在 WebGL 時存/讀檔案的方式，檔案是放在 IndexedDB。是有容量大小限制，所以需要注意存儲的檔案大小。"
keywords: ["Unity", "WebGL"]
draft: false
showtoc: true
tags: ["Unity", "WebGL"]
---

## 前言

Unity 在 WebGL 時存/讀檔案的方式，檔案是放在 IndexedDB。

`IndexedDB` 是有容量大小限制，所以需要注意存儲的檔案大小。

## About IndexedDB

[Working with quota on mobile browsers]

![img_1]

## [GitHub](https://github.com/Wenrong274/DataAccessWebGL)

## Introduction

- File Path

  `string.Format("{0}/{1}.dat", Application.persistentDataPath, FileName);`

- Save Method

  DataAccess.Save(fileName, bytes);

- Load Method

  byte[] bytes = DataAccess.Load(fileName);

- Example Scene

  `root\Assets\WebGL\Example\Scenes\Example`

---

[img_1]: https://raw.githubusercontent.com/Wenrong274/DataAccessWebGL/master/doc/img/1.png
[Working with quota on mobile browsers]: https://www.html5rocks.com/en/tutorials/offline/quota-research/
