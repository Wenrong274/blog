---
title: "Unity Text Breaking Space"
date: 2022-12-03
summary: "Unity Text 顯示中英混合文字時，英文總是跑到下一行？問題出在 Space 換行，用 \\u00A0 取代空格就能解決，一行 code 搞定！"
description: "解決 Unity uGUI Text 在中英混雜字串中因空格（Space）導致英文換行的問題，使用 Unicode 非斷行空格（\\u00A0，No-Break Space）取代一般空格，附 C# 字串替換方法與修正前後的截圖對比。"
keywords: ["Unity", "uGUI", "Text", "no-break space", "Unicode", "line break", "Chinese English mixed text", "UI bug"]
draft: false
tags: ["Unity"]
aliases:
  - /posts/unitytextbreakingspace/
---

## 前言

Unity Text 中英混雜導致英文字跳行問題，主要是 Space 字串的問題，可以用 `\u00A0` 替代 Space。

## Code

```C#
private static readonly string no_breaking_space = "\u00A0";

public static string ReplaceSpace(string context)
{
    return context.Replace(" ", no_breaking_space);
}
```

可以這樣替代全部的 Space。

## 範例

- 使用前

![img_1]

- 使用後

![img_2]

---

[img_1]: https://imgur.com/4dSyVJL.jpg
[img_2]: https://imgur.com/GhMA0mV.jpg
