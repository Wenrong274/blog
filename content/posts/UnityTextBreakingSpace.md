---
title: "Unity Text Breaking Space"
date: 2022-12-03
description: "Unity Text 中英混雜字串，導致 Space 字串跳行問題"
keywords: ["Unity"]
draft: false
showtoc: true
tags: ["Unity"]
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
