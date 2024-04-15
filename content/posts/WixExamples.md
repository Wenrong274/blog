---
title: "Wix Examples"
date: 2023-10-20
description: "主要是用來打包程式輸出成 .msi 檔"
keywords: [".msi", "wix"]
draft: false
showtoc: true
tags: ["tool"]
---

主要是用來打包程式輸出成 `.msi` 檔（[Windows Installer][wiki]）。

建議先看完 「[30 天 | C# WixToolset + WPF 帥到不行的安裝包 系列][30d]」，能夠把 80% 以上的問題解決。

### 輸出 Wix 文件方法

先到 [WiX Toolset Release][wix3] 安裝需要的工具。

需要開啟 [CMD][cmd] 且必須要用`工作管理員權限`打開，之後 cd 到 WiX Toolset 的安裝資料夾。

```text
C:\Program Files (x86)\WiX Toolset v3.11\bin
```

![img_1]

#### heat.exe

可以掃描目錄中的所有文件和子目錄，並生成 WiX 文件中所需的 Component、Directory、File 和其他元素的定義。

使用下面的參數可以拿到 `myKeyIn.wxs`。需要把 [MyDemo] 替換成正確的路徑，可以參考我的 `Product.wxs`。

```text
heat.exe dir "[PUT_YOUR_PATH]" -dr INSTALLFOLDER -cg ProductComponents -gg -gl -sf -srd -var "[MyDemo]" -out "[PUT_YOUR_OUTPUT_PATH]\myKeyIn.wxs"
```

| 參數名稱               | 解釋                   |
| :--------------------- | :--------------------- |
| [PUT_YOUR_PATH]        | 需要打包的路徑資料夾   |
| [MyDemo]               | WiX 文件中要使用的變數 |
| [PUT_YOUR_OUTPUT_PATH] | WiX 文件的路徑         |

![img_2]

## Wix Product.wxs

[Product.wxs][Productwxs]

### 桌面捷徑

需要注意一些細項設定，如 Guid、執行檔名稱、路徑等。

```wxc
<Directory Id="DesktopFolder" Name="Desktop" >
    <Component Id="DesktopFolderShortcut" Guid="{94D38478-869E-4CA8-BEA6-7905A7135DB8}">
        <Shortcut Id="DesktopShortcut" Directory="DesktopFolder" Name="Wix-Hello Unity" Target="[INSTALLFOLDER]Wix-Hello Unity.exe" WorkingDirectory="INSTALLFOLDER" Icon="WixToolsetIcon">
        </Shortcut>
        <RegistryValue Root="HKCU" Key="Software\WenRongStudio\Wix-Hello Unity" Name="installed" Type="integer" Value="1" KeyPath="yes"/>
    </Component>
</Directory>
```

#### 資料夾權限

避免安裝在系統槽時，無法變動檔案，所以必須把資料夾權限打開。

需要注意一些細項設定，如 Guid 等。

```wxc
<Component Id="InstallationDirectory" Guid='{0D92D4B6-CACE-4556-8CBD-C8C385B22D28}' >
    <CreateFolder Directory="INSTALLFOLDER">
        <Permission User="SYSTEM" GenericAll="yes"/>
        <Permission User="EveryOne" GenericAll="yes" GenericRead="yes" Read="yes" ReadAttributes="yes"  GenericExecute="yes" TakeOwnership ="yes"  GenericWrite ="yes"    WriteAttributes="yes" ReadPermission ="yes"   ChangePermission="yes" />
        <Permission User="Users" Domain="[LOCAL_MACHINE_NAME]" GenericAll="yes" GenericRead="yes" Read="yes" ReadAttributes="yes"  GenericExecute="yes" TakeOwnership ="yes"  GenericWrite ="yes"    WriteAttributes="yes" ReadPermission ="yes"   ChangePermission="yes"/>
    </CreateFolder>
</Component>
```

#### 建立 Windows Menu 資料

可以 Windows Menu 啟動檔案跟卸載執行檔。

需要注意一些細項設定，如 Guid、執行檔名稱、路徑等。

```wxc
<Directory Id="ProgramMenuFolder">
    <Directory Id="ApplicationProgramsFolder" Name="Wix-Hello Unity">
        <Component Id="ApplicationShortcut" Guid="{9724BE7E-7132-4978-B806-4B0D1B3DE350}">
            <Shortcut Id="ApplicationStartMenuShortcut"
                    Name="Wix-Hello Unity"
                    Description="Wix-Hello Unity"
                    Target="[INSTALLFOLDER]\Wix-Hello Unity.exe"
                    WorkingDirectory="APPLICATIONROOTDIRECTORY"
                    Icon="WixToolsetIcon"/>
            <Shortcut Id="UninstallProduct"
                    Name="Uninstall"
                    Description="Uninstalls Wix-Hello Unity"
                    Target="[System64Folder]msiexec.exe"
                    Arguments="/x [ProductCode]" />
            <RemoveFolder Id="ProgramMenuSubfolder" On="uninstall"/>
            <RemoveFolder Id="ApplicationProgramsFolder" On="uninstall"/>

            <RegistryValue Root="HKCU" Key="Software\WenRongStudio\Wix-Hello Unity" Name="installed" Type="integer" Value="1" KeyPath="yes"/>
        </Component>
    </Directory>
</Directory>
```

### [GitHub][github]

### 參考文章

[用 WiX 制作安装包：创建一个简单的 msi 安装包][wixblog]

[30 天 | C# WixToolset + WPF 帥到不行的安裝包 系列][30d]

[Create an Uninstall Shortcut][wix1]

[Create a Shortcut on the Start Menu][wix2]

[Removing files when uninstalling WiX][Cleanup]

---

[30d]: https://ithelp.ithome.com.tw/users/20139206/ironman/3901
[wix1]: https://wixtoolset.org/docs/v3/howtos/files_and_registry/create_uninstall_shortcut/
[wix2]: https://wixtoolset.org/docs/v3/howtos/files_and_registry/create_start_menu_shortcut/
[Cleanup]: https://stackoverflow.com/a/17513551
[wix3]: https://github.com/wixtoolset/wix3/releases
[wixblog]: https://blog.walterlv.com/post/getting-started-with-wix-toolset-msi-hello-world
[cmd]: https://zh.wikipedia.org/zh-tw/cmd.exe
[wiki]: https://zh.wikipedia.org/zh-tw/Windows_Installer
[img_1]: https://i.imgur.com/ovuJCka.png
[img_2]: https://i.imgur.com/SbjBwmv.png
[Productwxs]: https://raw.githubusercontent.com/Wenrong274/WixExamples/master/WixInstaller/WixToolset/Product.wxs
[github]: https://github.com/Wenrong274/WixExamples
