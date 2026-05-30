---
title: "Unity ParticleBezierPath"
date: 2023-01-18
summary: "10 萬顆粒子跑貝茲路徑只有 8 FPS？加上 Job System 後提升到 18-20 FPS，PC 更可達 100 FPS！本文介紹如何用 Job System 大幅優化 Unity 粒子路徑效能。"
description: "Unity 粒子貝茲曲線路徑系統（ParticleBezierPath）的效能優化實作，使用 Unity Job System 與 Burst Compiler 改善大量粒子的路徑計算效能。以 10 萬顆粒子為測試基準，Samsung Galaxy S7 從 8-9 FPS 提升至 18-20 FPS，PC 版本更可達 100 FPS。"
keywords: ["Unity", "Particle System", "Job System", "Burst Compiler", "Bezier", "performance optimization", "ECS"]
draft: false
tags: ["Unity"]
aliases:
  - /posts/particlebezierpath/
---

## 前言

此篇是優化原本的 [Unity ParticlePath][Unity ParticlePath]

## 簡介

有使用 Job System 優化功能。

100000 顆粒子使用路徑功能時，SAMSUNG GALAXY S7 在不使用 Job System FPS 約 8-9 FPS，使用後變成 18-20 FPS，PC 版多使用了 Burst 會從 30 FPS 提升至 100 FPS。

可能因為測試的硬體裝置數據優化有所不同，建議還是實際測試後才決定。

## 使用方式

可以先使用 demo 場景測試，必須要打開 `IsJob`，才會啟動 Job System。

![demogif]

## [GitHub][github]

---

[Unity ParticlePath]: https://wenrongdev.com/posts/unity-particlepath/
[demogif]: https://imgur.com/lfos4S0.gif
[github]: https://github.com/Wenrong274/ParticleBezierPath
