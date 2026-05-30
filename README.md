# Wenrong Nexus Blog

個人技術部落格，專注於 Unity 遊戲開發、跨平台開發技術與 C# 程式設計。

- **網址**：[wenrong-nexus.com](https://wenrong-nexus.com)
- **主題**：[Hugo Stack](https://github.com/CaiJimmy/hugo-theme-stack)
- **部署**：GitHub Actions → GitHub Pages

## 開發環境

### 環境需求

- [Hugo Extended](https://gohugo.io/installation/) v0.161.1+（需要 Extended 版本以支援 OG 圖片生成）

### 初始化

複製專案後，初始化 theme submodule：

```bash
git submodule update --init --recursive
```

### 本地開發

```bash
hugo server -D
```

### 建置

```bash
hugo --cleanDestinationDir
```

> 建議加上 `--cleanDestinationDir` 避免 `public/` 殘留舊版檔案。

## 部署

Push 到 `main` 分支後，GitHub Actions 自動執行建置並部署至 GitHub Pages（`gh-pages` 分支）。

## 專案結構

```text
assets/
  fonts/          # NotoSansTC-Bold.ttf（OG 圖片中文渲染用）
  icons/          # 自訂 SVG icons（mail.svg 等）
  images/         # OG 圖片背景（og-bg.png）
  scss/           # 自訂樣式（custom.scss）
content/
  posts/          # 技術文章
  portfolios/     # 作品集
  profile/        # 個人頁面
layouts/
  partials/head/  # 自訂 head（SEO、OG 圖片、keywords）
```

---
