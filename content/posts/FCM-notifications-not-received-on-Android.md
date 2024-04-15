---
title: "FCM Notifications Not Received on Android"
date: 2020-01-16
summary: "沒辦法在 APP 不活躍或不喚醒（not active）狀態推送推播訊息解決方式。"
keywords: ["Android", "notification"]
draft: false
showtoc: true
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
