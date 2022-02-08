---
title: "Install IPA With OTA"
date: 2019-10-10T00:00:00+08:00
draft: false
tags: ["iOS"]
---
## 前言

由於 iTunes 12.6 之後不提供 .ipa 檔安裝，導致無法提供測試 App，所以有人研究出很多安裝方式。不過這邊主要是介紹 OTA 的方式。

其他的安裝方式：[iTunes 12.7 移除了 Apps 的選項，我該如何安裝 .ipa 檔案到 iOS 裝置？](https://medium.com/@lefty./itunes-12-7-%E7%A7%BB%E9%99%A4%E4%BA%86-apps-%E7%9A%84%E9%81%B8%E9%A0%85-%E6%88%91%E8%A9%B2%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%9D-ipa-%E6%AA%94%E6%A1%88%E5%88%B0-ios-%E8%A3%9D%E7%BD%AE-2cad1d35d017)

### Using OTA

使用 OTA（Over-the-Air）需要有三個檔案 `.ipa(ad-Hoc)`、`.plist`、`index.html`

`.ipa(ad-Hoc)` 需要上傳去雲端空間，目前選擇的雲端空間是 [Dropbox](https://www.dropbox.com)。

還需要 `Host Website` 上傳 `.plist`、`index.html`，目前選擇的是 [GitHub](https://github.com/)。

所以只要使用 `Host Website` 就可以安裝了。

參考文章：[Error when distributing an IPA over the air with dropbox - iOS 7.1](https://stackoverflow.com/questions/22658987/error-when-distributing-an-ipa-over-the-air-with-dropbox-ios-7-1/25302392#25302392)

### Upload .ipa to Dropbox and Get public link

1. 先把輸出好的 `.ipa(ad-Hoc)` 上傳至 [Dropbox](https://www.dropbox.com) 並且設定分享。

1. 將分享網址裡的 `www.dropbox.com` 替換為 `dl.dropboxusercontent.com`。

1. 紀錄修改 `public link`。

#### Create manifest.plist

```plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>items</key>
    <array>
        <dict>
            <key>assets</key>
            <array>
                <dict>
                    <key>kind</key>
                    <string>software-package</string>
                    <key>url</key>
                    <string>http://YOUR_SERVER_URL/YOUR-IPA-FILE.ipa</string>
                </dict>
            </array>
            <key>metadata</key>
            <dict>
                <key>bundle-identifier</key>
                <string>com.yourCompany.productName</string>
                <key>bundle-version</key>
                <string>1.0.0</string>
                <key>kind</key>
                <string>software</string>
                <key>title</key>
                <string>YOUR APP NAME</string>
            </dict>
        </dict>
    </array>
</dict>
</plist>
```

修改好 `public link`、`app info` 就完成 `.plist`。

#### Create index.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>App install</title>
</head>
<body>
    <h2>builds</h2>
<a href="itms-services://?action=download-manifest&url=http://YOUR_SERVER_URL/manifest.plist"> App</a></body>
</body>
</html>
```

把 `url=http://YOUR_SERVER_URL/manifest.plist` 替換成 `Website manifest.plist`。

### Hosting using Github

1. Create a new repository（[教學文章](https://gitbook.tw/chapters/github/using-github-pages.html)）
1. 上傳 `manifest.plist`、`index.html`

得到 Github Website `index.html`就可以安裝 `.ipa`

### 參考文章

[iTunes 12.7 移除了 Apps 的選項，我該如何安裝 .ipa 檔案到 iOS 裝置？](https://medium.com/@lefty./itunes-12-7-%E7%A7%BB%E9%99%A4%E4%BA%86-apps-%E7%9A%84%E9%81%B8%E9%A0%85-%E6%88%91%E8%A9%B2%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%9D-ipa-%E6%AA%94%E6%A1%88%E5%88%B0-ios-%E8%A3%9D%E7%BD%AE-2cad1d35d017)

[從網路上下載ipa檔並且安裝在iPhone or iPad(Download and install an ipa from url on iOS)](https://www.dropbox.com)

[Error when distributing an IPA over the air with dropbox - iOS 7.1](https://stackoverflow.com/questions/22658987/error-when-distributing-an-ipa-over-the-air-with-dropbox-ios-7-1/25302392#25302392)

[使用 GitHub 免費製作個人網站](https://gitbook.tw/chapters/github/using-github-pages.html)

______________________________________________________________________________________________________________
