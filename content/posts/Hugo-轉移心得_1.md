---
title: "Hugo 轉移心得（1）"
date: 2022-02-14T01:14:39+08:00
description: "從 2019 開始架設自己的 Blog 的心路歷程。使用過 Wordpress、Medium、CoderBridge、Blogger，最後選擇了 Hugo 並且至找苦吃自己架設。"
keywords: ["Hugo","Wordpress","Blog"]
draft: fales
showtoc: true
tags: ["Blog"]
---

其實我的 Blog 是寫給自己看的。因為我時常忘記怎麼解決問題、怎麼製作功能、忘記是做了什麼蠢事。也因此這樣才會產生這個 Blog 用來記錄我的跌跌撞撞的過程。

## 前言

在大學寫程式時遇到問題自然會去 Google 查，有很多時候很難下關鍵字，主要是自己犯蠢導很難知道原因，也有可能是我當時太蠢。自然而然也很難對症下藥，很多時候只是在重打或參考(?)同學就能解決學生時期遇到的問題。

當開始工作時的第一年內，遇到的問題基本上還是類似的，因為我實在是太菜了，會遇到一些其實很基本又很無言的低級錯誤，一直重複的犯錯麻煩其他同事幫忙 debug，所以我一直有一個念頭想架設自己的 Blog。記錄自己的犯蠢的過程和解決方式，提醒未來的自己有錯誤可以來這邊看，假如又可以幫忙之後的人其實也不錯。

## 第一個自己 Blog - Blogger

我第一個 Blog 是使用大學某堂課程用到的 [Blogger]，其實我也沒太認真研究這個 Blog，因為覺得不夠好看用了一下沒有詳細認真的使用。

多年之後看到 [搞笑談軟工][Blogger_1] 的 Blog，才知道 Blog 的重要的部分其實不是選用什麼架構而是要`持續寫作`才是最重要的核心。

## 第一個自架 Blog - Wordpress

在 2018 時覺得自己的學習成長曲線變慢了，可能是因為都接觸類似的東西，工作上也大部分內容都能應付，也不會像以前一樣回家繼續寫程式，簡單來講就是發現自己進入了舒適圈(?)。就很坎坷不安，於是就開始買書學習、跟人學習。

在 2019 的某天 Unity 程式社群，有人發問沒有推薦的程式書籍，而 [阿祥的開發日常][tedsieblog] 的 Ted 回答了 [軟技能代碼之外的生存指南][book_tenlong]，之後我就去買了，內容整本書都沒再講程式的部分。而這本書其實大部分內容都是在講工程師除了工作以外的事情，例如運動、投資、面試、等等，很推薦沒看過的工程師可以買看看看，我個人是收穫良多，也是因為看了這本書才開始有了想認真自己架設 Blog。

在書的內容其實就有推薦自己架設屬於自己的 Blog，當時我看到就想到我當年的想法，是沒有想說可以利用 Blog 賺錢，自己畢竟技術不到家，反倒是覺得這個是可以當一個不錯的作品集，於是就開始在架設自己的 Wordpress。

當時是看著 [教學][wordpress_1] 文章邊看邊選擇，過程除了刷卡付錢沒有什麼難度，我在 [NameCheap][namecheap] 買網址，虛擬主機則是使用 [SiteGround][siteground]。

### 第一年 - 2019

``` text
SiteGround NT$1493 + NameCheap NT$285 = NT$1778
```

一年 1778 元。而且很多功能可以玩、可以使用我是可以接受的。

後來再架設目前這個 Blog，才知道 Wordpress 的好，很多事情都很簡單就完成(SEO、theme、等等)。

### 第二年 - 2020

``` text
SiteGround NT$5375 + NameCheap NT$315 = NT$5690
```

使用 Wordpress 第二年要續約了，因為我沒有認真確認第一年花費多少，就直接刷卡續約下去了，收到扣款通知才知道收費真的`很貴`。

第一年跟第二年價格差了 `NT$3912`，當時也沒有想到要退款，想說明年又要扣款時再來處理，其實我當時就已經處於寫作狀態已經很少了。

### 第三年 - 2021

``` text
NameCheap NT$368 = NT$368
```

到了第三年，覺得不能再這樣扣款下去了，所以開始把文章轉移去 [Medium][medium]，並且將 `SiteGround` 的部分停止扣款，不過 `NameCheap` 是有持續付費的，畢竟費用還是較少，而且網址現在還在使用。

將文章轉至 Medium 後，才發現有其他問題的產生，，之後又急忙的將文章轉至 [CoderBridge]。詳細原因留置 [Hugo 轉移心得（2）][nextpost] 講。

## 結論

整體而言 Wordpress 對於新手的我架設 Blog，確實是不錯的體驗，反正只要有錢基本上，都能處理，俗話說錢能解決得事情都是小事，但我沒錢，所以只能趕快跑走。

其實當時使用時還發生了許多意外的插曲。

當時使用 NameCheap 購買網址時，是使用信用卡刷卡，由於 2020 第二年續約，想說直接按一按就好了，應該不用把信用卡特地移除，想不到這就是悲劇的開頭。

由於我的帳號並沒有綁定 2FA，而不知道是 NameCheap 還是我個人問題，導致我的帳號被別人盜走，當時是早上我去上班去上個廁所回來就收到 6~7 封刷卡簡訊，大概被刷走了 2~3 萬的花費，之後請銀行處理這些帳單，之後換個信用卡跟將 NameCheap 換密碼和綁定 2FA。
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
[nextpost]:../hugo-轉移心得_2
[CoderBridge]:https://zh-tw.coderbridge.com/
[img_1]:https://imgur.com/pJBH4Q6.jpg
