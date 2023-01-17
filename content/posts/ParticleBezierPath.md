---
title: "Unity ParticleBezierPath"
date: 2023-01-18T01:06:00+08:00
description: "Unity 實作粒子路徑系統。並且使用 Job System 優化效能。"
keywords: ["Unity","Particle"]
draft: false
showtoc: true
tags: ["Unity"]
---

## 前言

有使用 Job System 優化功能。

100000 顆粒子使用路徑功能時，SAMSUNG GALAXY S7 在不使用 Job System FPS 約 8-9 FPS，使用後變成 18-20 FPS，PC 版多使用了 Burst 會從 30 FPS 提升至 100 FPS。

可能因為測試的硬體裝置數據優化有所不同，建議還是實際測試後才決定。

## 使用方式

可以先使用 demo 場景測試，必須要打開 `IsJob`，才會啟動 Job System。

## [GitHub][github]

______________________________________________________________________

[demogif]:https://imgur.com/lfos4S0.gif
[github]:https://github.com/Wenrong274/ParticleBezierPath
