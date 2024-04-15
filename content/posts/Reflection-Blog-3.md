---
title: "Blog 心得（3）"
date: 2022-02-28
description: "使用 Hugo 心得，並且使用 PaperMod 當主題時遇到的問題。"
keywords: ["Hugo", "PaperMod", "Blog"]
draft: false
showtoc: true
tags: ["Blog"]
---

## 前言

此篇在講如何使用 Hugo 在 Github 上自架 Blog。

### 安裝 Hugo

關於安裝的部分可以參考這些文章

- [Quick Start][hugo_0]
- [GitHub 部署 Hugo 靜態網站][hugo_1]
- [使用 Hugo 建立靜態網站，並部署在 Github Page][hugo_2]
- [Hugo 貼身打造個人部落格 系列][hugo_3]

與文章不同的地方主題我是選擇 [PaperMod][theme]，由於需要設定 **config.yml**，建議先參考 [PaperMod-Installation][theme-instal]。

想要完整的設定或理解 PaperMod，最好是完整的看完 [PaperMod][theme] 的 repo，有不知道的問題可以在 [Issues][theme-issues] 或者 [FAQs][theme-faqs] 搜尋看看，會比 google 搜尋來得快。

### PaperMod

由於 PaperMod 推薦使用 `config.yml`，因此推薦把原本的 `config.toml` 刪除。並且去複製官方提供的 [config.yml][theme-config]。

### config.yml

#### Search Post

先在 config menu main 新增一個 Search 頁面

```text
menu:
    main:
        - identifier: archives
          name: Archives
          url: /archives/
          weight: 5
        - identifier: tags
          name: Tags
          url: /tags/
          weight: 10
        - identifier: search
          name: Search
          url: /search/
          weight: 20
```

且在最底下此內容。（[參考文件][theme-searchpage]）

```text
outputs:
    home:
        - HTML
        - RSS
        - JSON # is necessary
```

最後在專案的 `content` 底下新增 `search.md`，即可完成功能。（[參考文件][theme-searchpage]）

```markdown
---
title: "Search" # in any language you want
layout: "search" # is necessary
# url: "/archive"
# description: "Description for Search"
summary: "search"
placeholder: "placeholder text in search input box"
---
```

#### Comments

此功能是參考 [Day 20. Hugo Comments System][theme-comments] 文章製作出來的。

## Github Action

有使用 Custom domain 的話，且 workflows 沒有設定 domain 的話，會造成每次更新文章時，都會清掉 Custom domain，變回原本的 github.io。

### GitHub Pages workflow.yml

```yml
name: GitHub Pages

on:
  push:
    branches:
      - main # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0 # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "0.91.2"
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          github_token: ${{ secrets.HUGO_DEPLOY_TOKEN  }}
          PUBLISH_BRANCH: gh-pages # 推送到 gh-pages 分支
          commit_message: ${{ github.event.head_commit.message }}
          publish_dir: ./public
          cname: wenrongdev.com
```

只需要新增或替換掉 `cname` 後面為 domain 即可。

## 結論

Hugo 或 PaperMod 我都處於摸索階段。

這次架起來的感覺各方面都不錯，不管是讀取 Blog 速度、支援 Markdown、架設 Github 上且保留原始檔，到目前為止沒有明顯的缺點。

目前最大問題就是 `SEO`，這是我完全沒有接觸過的。問題在於 Google 搜尋不到我的 Blog，所以這是之後要研究的部分。

---

<!-- Hugo 連結 -->

[hugo_0]: https://gohugo.io/getting-started/quick-start/
[hugo_1]: https://medium.com/@chswei/%E5%9C%A8-github-%E9%83%A8%E7%BD%B2-hugo-%E9%9D%9C%E6%85%8B%E7%B6%B2%E7%AB%99-9c40682dfe40
[hugo_2]: https://jimmylin212.github.io/post/0001_create_hugo_and_deploy_on-github_page/
[hugo_3]: https://ithelp.ithome.com.tw/users/20106430/ironman/3613

<!-- theme 連結 -->

[theme]: https://github.com/adityatelange/hugo-PaperMod
[theme-instal]: https://github.com/adityatelange/hugo-PaperMod/wiki/Installation
[theme-issues]: https://github.com/adityatelange/hugo-PaperMod/issues
[theme-faqs]: https://github.com/adityatelange/hugo-PaperMod/wiki/FAQs
[theme-config]: https://github.com/adityatelange/hugo-PaperMod/wiki/Installation#sample-configyml
[theme-searchpage]: https://github.com/adityatelange/hugo-PaperMod/wiki/Features#search-page
[theme-comments]: https://ithelp.ithome.com.tw/articles/10248312
