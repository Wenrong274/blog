---
title: "FCM Notifications Not Received on Android"
date: 2020-01-16
summary: "FCM 推播在 App 背景時收不到通知？元凶是 Android 的省電模式（Doze mode）！本文說明問題根因與官方解決方向。"
description: "Android 裝置在 App 不活躍狀態下無法接收 FCM（Firebase Cloud Messaging）推播通知的問題解析。根本原因為 Android 省電模式（Doze mode）阻擋後台網路活動，本文提供官方文件參考與解決方向。"
keywords: ["Android", "FCM", "Firebase Cloud Messaging", "push notification", "Doze mode", "background", "Unity"]
draft: false
tags: ["Android"]
---

## 前言

前陣子因為專案需要後台推播功能，所以開始測試 FCM(Firebase Cloud Messaging）功能。

測試的過程一直沒辦法在 APP 不活躍或不喚醒（not active）狀態推送推播訊息。

後來發現主因是`省電模式（Doze mode）`導致 APP 無法接受任何推波內容。

## 參考連結

[https://developer.android.com/training/monitoring-device-state/doze-standby?hl=zh_cn](https://developer.android.com/training/monitoring-device-state/doze-standby?hl=zh_cn)

[https://blog.csdn.net/pkorochi/article/details/87186659](https://blog.csdn.net/pkorochi/article/details/87186659)

---
