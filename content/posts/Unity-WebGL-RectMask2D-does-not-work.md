---
title: "Unity WebGL RectMask2D Does Not Work"
date: 2019-08-29
summary: "用於 WebGL RectMask2D 失去作用的簡易修正。"
keywords: ["Unity", "WebGL"]
draft: false
showtoc: true
tags: ["Unity"]
---

## 前言

此 Script 用於 WebGL RectMask2D 失去作用的簡易修正。

`建議`還是先輸出測試確定 RectMask2D 失效再使用此 Script。

### 使用方式

直接在 Canvas 物件底下 Add Component FixRectMask2dWebGL 即可。

### Script

```C#
public class FixRectMask2dWebGL : MonoBehaviour
{
#if PlatformWebGL
    private void Awake()
    {
        var items = GetComponentsInChildren<MaskableGraphic>(true);
        for (int i = 0; i < items.Length; i++)
        {
            Material m = items[i].materialForRendering;
            if (m != null)
                m.EnableKeyword("UNITY_UI_CLIP_RECT");
        }
    }
#endif
}
```

---

[參考文章](https://forum.unity.com/threads/rectmask2d-does-not-work-when-canvas-render-mode-is-sceen-space-camera-or-world-space-2017-2-0f3.499966/#post-4484971)
