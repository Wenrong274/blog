---
title: "用重度 AI Workflow 做了一個 Solo 遊戲，然後砍掉——我學到什麼"
date: 2026-07-21T00:00:00+08:00
slug: "ai-heavy-solo-game-postmortem"
summary: "我用 Claude Code、Unity MCP、Subagent、Hooks 與 CI，在 29 天內完成一個 Unity Solo 遊戲 Demo，繳交後卻決定結案。這不是『AI 一天做完遊戲』的成功故事，而是一次關於工具鏈成本、驗證邊界與知道何時停手的誠實覆盤。"
description: "Unity Solo 遊戲 Bulwark 的 AI-heavy 開發覆盤：29 天、541 個 commits、378 個測試，從 Claude Code、Unity MCP、DOER/CHECKER 到主動拆除 Skill 堆疊，整理 AI Coding 真正有效與失效的地方。"
keywords: ["Unity", "AI Coding", "Claude Code", "Unity MCP", "Solo Game Development", "Game Development", "Postmortem", "AI Workflow"]
draft: false
tags: ["Unity", "AI", "Game Development"]
---

先說結果：我用重度 AI workflow 做了一個 Unity 遊戲，在 29 天內完成並繳交，然後決定不再繼續開發。

不是因為專案完全不能跑。它有完整戰鬥流程、八層波次、Boss、卡牌抽選、隨機賠付、存檔、音效、CI 與正式 Release。它甚至有 378 個測試。

但「做得出來」和「值得繼續做」是兩個不同的問題。

這篇不是「我用 AI 一天做完一款遊戲」的故事。比較接近：我把 AI Coding 的油門踩到底，最後學會最重要的操作不是怎麼加速，而是什麼時候該放開油門。

## 這個專案到底做了多少

專案叫《獸潮：四靈》，codename 是 Bulwark，是一個 TD × Roguelike 的 Unity 6.3、URP 2D 遊戲，也是 AI Coding 比賽作品。

從 2026 年 6 月 19 日開工，到 7 月 17 日繳交 v0.3.3，總共 29 個日曆日、24 個活躍開發日。

| 指標 | 結果 |
| --- | ---: |
| Commit | 541 |
| C# 程式碼 | 157 個檔案、17,135 行 |
| 測試 | 73 個檔案、378 個測試方法 |
| Release | 8 個 tag |
| TODO / FIXME | 0 |

這些數字看起來很厲害，也很容易被拿來包裝成 AI 生產力案例。

但 541 個 commits 不代表方向正確，378 個測試不代表遊戲好玩，TODO 是零也不代表產品值得繼續投資。它們只能證明我在很短的時間內做了很多工程工作。

這兩件事差很多。

## 我的 AI Workflow 到底有多重

開工時使用的工具包括：

- Claude Code CLI：主要程式實作與 repo 操作；
- Unity MCP：操作 Unity Editor、填資料、接 Prefab、跑測試；
- Claude Design：做畫面探索與美術溝通素材；
- Superpowers：從 spec、plan 到 subagent 執行；
- Loop Engineering：DOER／CHECKER 雙代理迴圈；
- 多套 Skills：caveman、karpathy、mattpocock、ponytail；
- 自建 Hooks：格式檢查、繁體中文檢查；
- Self-hosted CI：EditMode 測試、warning gate、Win64 build、tag release。

有一段時間，我不像在做 Solo Project，比較像在管理一間不存在的小公司。只是「同事」全是 AI，而且每位同事都會非常有自信地回報已完成。

第一天就能看出這套 workflow 的速度：兩個巨型 commits 一口氣建立 165 個檔案，隔天就有 v0.1.0 可以玩。

速度是真的。問題也是真的。

## AI 最有價值的地方：規格明確的工程工作

Bulwark 最穩定的部分，是純 C# 的 Core 層。

我把遊戲邏輯與 UnityEngine、MonoBehaviour 生命週期隔離，Core 使用 POCO 與 asmdef 分層。157 個 C# 檔案裡，有 51 個集中在這個可直接 `new()`、可獨立測試的區域，而且多數寫完後幾乎沒有再改。

AI 很適合處理這些事情：

- 輸入輸出明確的計算；
- 狀態轉移與純函式；
- ScriptableObject 資料回填；
- 測試生成；
- 文件與程式碼對帳；
- 離線模擬腳本。

遊戲數值全部外置到 ScriptableObject，再用 9 個 Python simulation 掃參數，Runtime 則輸出 JSONL 遙測資料校正模型。平衡迭代可以在不改程式碼的情況下快速驗證。

這裡真正值得帶走的不是某個 class，也不只是「使用 ScriptableObject」。而是「單一數值來源 → 離線模擬 → Runtime 遙測校正」的閉環。

當問題可以被清楚描述、結果可以機械驗證時，AI 的 CP 值非常高。

## AI 不會替我知道遊戲哪裡不對

相反地，AI 不擅長的地方也很一致：

- 遊戲好不好玩；
- 畫面應該長什麼樣子；
- UI 排版是否舒服；
- VFX 的位置與節奏對不對；
- 數值問題是否已經影響玩家感受；
- 我到底想做什麼產品。

平衡問題都是我先 Play 出「感覺不對」，再把問題交給 AI 分析。AI 可以幫忙算，但它不會主動知道玩家為什麼無聊。

Unity MCP 很適合跑測試、查場景、批次填值；拿來排 UI 或調 VFX，最後仍然要自己手動處理。Claude Design 的產物也不是最終畫面，而是我和美術討論方向時使用的參考。

AI 能承接「怎麼實作」，但「想要什麼」仍然是人的工作。

更麻煩的是，AI 很擅長把錯的方向實作得很完整。

## 最大的錯覺：更多 Workflow 等於更高品質

我原本使用 DOER／CHECKER pattern：一個 agent 實作，另一個 agent 檢查，希望用第二層 AI 擋住第一層 AI 的錯誤。

回頭看完整專案，我想不起來 CHECKER 曾經攔下哪一個真正重要的語意錯誤。

實際漏掉的問題包括：

- AI 根據搜尋結果虛構檔名，讓第一版符合度分析判斷錯誤；
- Unity MCP 斷線後，agent 仍然提交沒有實際編譯過的 code；
- Authoring tool 覆蓋我手動調整的 VFX 位置；
- 規格理解錯誤與遊戲手感問題，最後仍靠人工讀 code 與 Play 發現。

第二個 agent 和第一個 agent 共享相似的盲點。當規格理解本身錯誤時，多一輪語言模型審查不一定會得到不同答案。

更糟的是，每個 Skill、每層 agent、每份冗長 plan 都會消耗 context 與注意力。專案中後期，這些流程開始讓實作變慢。

所以在繳交前四天，我主動移除了 superpowers、loop-me、caveman、karpathy、mattpocock 等 Skill，只留下最簡單的開發規範與 ponytail。

結果不是專案失控，反而更順。

我的結論是：**AI 鷹架的價值前重後輕。**

開工期可以用重流程建立慣例；當慣例已經沉澱進 codebase、測試與 AGENTS.md，鷹架就應該拆掉。鷹架不是地基，留太久只會變成 context 稅。

## 寫在 AGENTS.md，不代表專案真的有做

Bulwark 最荒謬也最有價值的案例是 R3。

我安裝了 R3，在 asmdef 加入 reference，也在 AGENTS.md 寫了 `ReactiveProperty` 的規範與範例。我一直以為專案有在使用 Reactive。

直到覆盤才發現：R3 的實際使用次數是零。

View 綁定全部走 `event Action`，總共有 41 處。因為規範寫的是「event Action **或** ReactiveProperty」，第一天的骨架選擇 event，後續 AI 自然沿著既有程式碼繼續複製。

AI 模仿 codebase 的重力，通常比閱讀文件中的理想更強。

所以我對 Context Engineering 的看法也改了：

1. 在乎的規則不要寫成沒有條件的選擇題。
2. 重要架構規則必須配一個機械 gate，哪怕只是 CI 裡的一行搜尋。
3. 定期檢查「文件說正在做什麼」與「repo 實際在做什麼」。
4. 未使用的套件要和它的規範一起刪掉。

AGENTS.md 不是架構。程式碼、測試與 gate 才是。

## 378 個測試，還是漏掉一個沒聲音的塔

Bulwark 的繳交版有一個很諷刺的 bug：四座塔的 `ShootSfx` 資產 reference 已經斷鏈，所以射擊時沒有音效。

這不是複雜的演算法錯誤。某次整理資產後 GUID 失效，Unity 靜默地留下 missing reference。378 個測試全部通過，遊戲也能跑，只是少了聲音。

這件事提醒我，測試數量不是 coverage 的完整答案。Unity 專案還有一整層資產與場景完整性：

- missing script；
- missing reference；
- Prefab 接線；
- Scene serialization；
- 真實 Play 時才會發現的呈現問題。

下一個專案與其再加一個 CHECKER agent，我更願意把預算投在 GUID 斷鏈掃描、MCP 失效時 fail-loud，以及一份真的會執行的 Play 驗收清單。

## 工具鏈最後留下什麼

如果重開一個 Unity Solo Project，我不會照搬整套 Bulwark workflow。

| 工具或做法 | 決定 | 原因 |
| --- | --- | --- |
| Claude Code CLI | 保留 | 規格明確的 Core、測試、文件與重複工作 CP 值高 |
| Unity MCP | 縮小範圍後保留 | 用於測試、查詢、資料填值；不期待它完成最終排版 |
| Claude Design | 有條件保留 | 適合前期探索與美術溝通，不是假裝成正式產出 |
| AGENTS.md | 保留 | 只留下已選定且能驗證的單一路徑規則 |
| Hooks / CI | 保留 | 把重要規則變成機械 gate，但要持續處理誤報 |
| Python sim + JSONL 遙測 | 保留 | 數值調整有客觀閉環，不只靠感覺猜 |
| DOER／CHECKER subagent loop | 移除 | 沒攔下值得記住的語意問題，卻增加 context 成本 |
| 大量通用 Skills | 移除 | 專案中期後成為鷹架稅 |
| R3、Cinemachine 等預裝套件 | 不再預裝 | 先在一個真實 use case 落地，再升格成專案慣例 |

最後真正帶走的不是 Bulwark 的敵人、塔或共鳴系統，而是幾個比較樸素的東西：

- Core POCO 與 Unity glue 分離；
- 數值外置、離線模擬、Runtime 遙測的閉環；
- 一個 commit 對應一句可驗收敘述；
- Scene／Prefab 禁止 agent 直接修改 YAML；
- AI 說完成不算完成，編譯、測試與實際 Play 才算。

## 為什麼最後還是砍掉

v0.3.3 繳交後，我選擇把專案凍結，沒有再開下一輪 backlog。

這個決定不是在否定 29 天的工作。Bulwark 已完成比賽作品與 AI workflow 實驗的任務，而且留下足夠多可以帶進下個專案的東西。

但把 Demo 變成值得長期營運的產品，接下來需要大量 Play、內容、美術、手感與市場判斷。這些工作不能因為程式碼已經很多，就假裝只差最後 10%。

「已經投入很多」不是繼續投入的理由。

我甚至在覆盤後做了一個 `project-postmortem` Skill，接著馬上問自己：這個 Skill 真的有必要嗎？答案是，只有當第二、第三個專案真的重複使用，它才有價值。否則一份 `POSTMORTEM.md` 已經夠了。

這個問題本身就是很好的停手訊號。

## 最後學到的不是怎麼用更多 AI

這次專案證明 AI 可以讓一個人更快地建立系統、補測試、同步文件與處理大量重複工作。

它也證明 AI 不會替我決定方向，不會替玩家感到無聊，不會因為多了一個 CHECKER 就自動理解規格，更不會因為 repo 很乾淨，就讓一個 Demo 自動變成值得繼續的產品。

我原本想練的是如何指揮更多 AI。最後真正練到的是：

- 哪些工作值得交給 AI；
- 哪些規則必須交給機器 gate，而不是模型自覺；
- 哪些判斷一定要自己 Play、自己看；
- 什麼時候該拆掉工具；
- 什麼時候該結束專案。

AI 能讓我更快抵達目的地，也能讓我更快走錯路。

下一個專案，我仍然會重度使用 AI。只是工具會更少、驗證會更實際，而且我會更早問一句：**現在缺的是更多 code，還是一個人應該做的決定？**
