---
title: "Whitelist for Windows UAC"
date: 2019-11-04
summary: "每次開程式都要輸入管理員密碼很煩？不能關 UAC 又不能改使用者權限，解法是寫入 Registry 白名單！本文提供 C# 自動寫入 Regedit 的完整程式碼。"
description: "解決 Windows 程式每次啟動都跳出 UAC 管理員權限請求的問題，在不關閉 Windows UAC 的前提下，透過寫入 Registry（HKEY_CURRENT_USER\\Software\\Microsoft\\Windows NT\\CurrentVersion\\AppCompatFlags\\Layers）加入 RunAsInvoker 白名單，附完整 C# RegEditWhiteList 類別實作。"
keywords: ["C#", "Windows UAC", "Registry", "RunAsInvoker", "Regedit", "administrator", "Windows", ".NET"]
draft: false
tags: ["C#"]
---

## 前言

因為某些程式開啟時，會跳出需要系統管理員（Administrator）權限執行程式，也導致了只要是ㄧ般使用者每次開啟時都需要輸入系統管理員密碼來執行。為了ㄧ般使用者的權限問題也不能關閉 Windows UAC。 也不可能修改一般使用者的權限，所以需要讓 Windows UAC 加入此程式為白名單，這樣就不會每次都會跳出權限要求。

## 已知限制條件

1. 程式必須以 Administrator 執行
1. 一般使用者可以執行
1. 不可完全關閉 Windows UAC

## 解決方式

根據[不變更 UAC 安全性，但執行程式時又不擾民的設定方式][taodeUrl]，可以在 Windows Regedit 新增白名單。

1. Win+R 輸入 regedit 執行
1. 根據此路徑尋找 `HKEY_CURRENT_USERS\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers`
1. 右鍵新增字串值，名稱為程式（exe）路徑、資料為`~ RunAsInvoker`

## C# 解決方式

```CSharp
public class RegEditWhiteList
{
    public string keyName { get; set; }
    private readonly string root = @"Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers";
    private readonly string keyValue = "~ RunAsInvoker";

    public RegEditWhiteList(string keyName)
    {
        this.keyName = keyName;
    }

    public void SendRegedit()
    {
        RegistryKey key = Registry.CurrentUser.OpenSubKey(root, true);
        key.SetValue(keyName, keyValue,RegistryValueKind.String);
        key.Close();
    }
}
```

## [GitHub][repo]

### 使用方式

因為修改註冊碼是修正當前使用者的註冊碼，因此只要換使用者就需要再新增一次白名單。

## 參考資料

[不變更 UAC 安全性，但執行程式時又不擾民的設定方式][taodeUrl]

---

[taodeUrl]: https://www.taode.idv.tw/wordpress/?p=639
[repo]: https://github.com/Wenrong274/UACWhitelist
