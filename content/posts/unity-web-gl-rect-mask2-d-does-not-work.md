---
title: "Unity WebGL RectMask2D Does Not Work"
date: 2019-08-29
summary: "Unity 輸出 WebGL 後 RectMask2D 失效？只要掛上這個修正腳本就好！本文提供在 Canvas 上啟用 UNITY_UI_CLIP_RECT 的一鍵解法。"
description: "解決 Unity WebGL 平台 RectMask2D 遮罩失效的問題，透過在 Canvas 物件加入 FixRectMask2dWebGL 元件，手動啟用 MaskableGraphic 的 UNITY_UI_CLIP_RECT Shader Keyword，使 RectMask2D 在 WebGL 平台正常運作，附完整 C# 程式碼。"
keywords: ["Unity", "WebGL", "RectMask2D", "uGUI", "UI mask", "UNITY_UI_CLIP_RECT", "shader keyword", "bug fix"]
draft: false
tags: ["Unity"]
---

## 前言

此 Script 用於 WebGL RectMask2D 失去作用的簡易修正。

`建議`還是先輸出測試確定 RectMask2D 失效再使用此 Script。

### 使用方式

直接在 Canvas 物件底下 Add Component FixRectMask2dWebGL 即可。

### Script

```CSharp
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
