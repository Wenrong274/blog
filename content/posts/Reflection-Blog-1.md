---
title: "Blog 心得（1）"
date: 2022-02-14T01:14:39+08:00
description: "從 2019 開始架設自己的 Blog 的心路歷程。使用過 Wordpress、Medium、CoderBridge、Blogger，最後選擇了 Hugo 並且自討苦吃。"
keywords: ["Hugo", "Medium", "Wordpress", "Blog"]
draft: fales
showtoc: true
tags: ["Blog"]
---

其實我的 Blog 是寫給自己看的。因為我時常忘記怎麼解決問題、怎麼製作功能、忘記是做了什麼蠢事。因此產生這個 Blog 用來記錄我的跌跌撞撞的過程。

## 前言

在大學寫程式時遇到問題去 Google 查，很多時候很難下關鍵字，主要是自己犯蠢導致很難知道原因。自然而然也很難對症下藥，很多時候只是重打或參考(?)同學就能解決學生時期遇到的問題。

當開始工作時的第一年內，遇到的問題基本上還是類似的，因為我實在是太菜了，會遇到一些其實很基本又很無言的低級錯誤，一直重複的犯錯麻煩其他同事幫忙 debug，所以我一直有一個念頭想架設自己的 Blog。記錄解決過程和解決方式，提醒未來自己有錯誤可以來這邊看，假如又可以幫助別人其實也不錯。

## Blogger

第一個 Blog 是使用大學某堂課程用到的 [Blogger]，其實也沒太認真研究這個 Blog，因為覺得不夠好看用了一下沒有認真的使用。

多年之後看到 [搞笑談軟工][Blogger_1] 的 Blog，才知道 Blog 的重要的部分其實不是選用什麼架構，**持續寫作** 才是最重要的核心。

## Wordpress

2018 時覺得自己的學習成長曲線變慢了，可能是因為都接觸類似的東西，工作上也大部分內容都能應付，也不會像以前一樣回家繼續寫程式，簡單來講就是發現自己進入了舒適圈(?)。就很坎坷不安，於是就開始買書學習、跟人學習。

2019 的某天 Unity 程式社群，有人發問推薦的程式書籍，而 [阿祥的開發日常][tedsieblog] 的 Ted 回答了 [軟技能代碼之外的生存指南][book_tenlong]。整本書內容都沒在講程式的部分，大部分內容都是在講工程師除了工作以外的事情，例如運動、投資、面試、等等，推薦沒看過的工程師可以買，對於我個人是獲益良多，也是因為看了才有想認真的自己架設 Blog。

書的內容其實就有推薦架設屬於自己的 Blog，當時看到就想到我當年的想法，是沒有想要跟作者一樣利用 Blog 賺錢，畢竟技術不到家。倒是覺得可以當一個不錯的作品集，於是就開始在架設自己的 Wordpress。

當時是看著 [教學][wordpress_1] 文章邊看邊選擇，過程除了刷卡付錢沒有什麼難度，我在 [NameCheap][namecheap] 買網址，虛擬主機則是使用 [SiteGround][siteground]。

### 使用三年心得 (2019~2020)

|  年分  | SiteGround | NameCheap | 花費(NTD) |
| :----: | ---------: | --------: | --------: |
| 第一年 |       1493 |       285 |      1778 |
| 第二年 |       5375 |       315 |      5690 |
| 第三年 |          0 |       368 |       368 |
| 總花費 |            |           |      7836 |

第一年 1778 元。而且很多功能可以玩、可以使用，整體使用是很滿意的。而到了第二年，沒有認真確認第一年花費多少就直接續約了，收到扣款通知才知道收費真的**很貴**。

第一年跟第二年價格差了 **3912** 元，當時也沒有想到要退款，想說明年扣款時再來處理。

到了第三年，就打算不續約 SiteGround，所以開始把文章轉移去 [Medium][medium]，不過 NameCheap 是有持續付費的，畢竟費用還是較少，而且網址現在還在使用。

後來用 Hugo 架設這個 Blog，才知道 Wordpress 的好，很多事情都很簡單就完成（SEO、theme、等等）。

## 結論

整體而言 Wordpress 對於新手的我架設 Blog，確實是不錯的體驗，只要有錢基本都能處理。不過續約的花費太貴，所以才放棄使用 SiteGround，尋找一些免費的方式。

當時使用 NameCheap 時也有被盜刷過，記得要綁定 2FA、密碼不要重複。
![img_1]

______________________________________________________________________

[book_tenlong]:https://www.tenlong.com.tw/products/9787115429476
[Blogger]:https://www.blogger.com/
[Blogger_1]:http://teddy-chen-tw.blogspot.com/
[tedsieblog]:https://tedsieblog.wordpress.com/
[wordpress_1]:https://jessielab.com/wordpress%E6%95%99%E5%AD%B8-setting-up-a-website/
[siteground]:https://www.siteground.com/
[namecheap]:https://www.namecheap.com/
[medium]:https://medium.com/
[nextpost]:../Reflection-Blog-2
[CoderBridge]:https://zh-tw.coderbridge.com/
[img_1]:https://imgur.com/pJBH4Q6.jpg
