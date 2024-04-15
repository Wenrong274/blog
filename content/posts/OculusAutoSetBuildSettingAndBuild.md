---
title: "Oculus Auto Set Build Setting && Build"
date: 2022-12-01
description: "自動設定 Oculus XR 等細項設定，方便用於多平台多操作方式功能。"
keywords: ["Unity", "Oculus"]
draft: false
showtoc: true
tags: ["Unity"]
---

## 前言

之前因為遇到多平台功能，要輸出時各個平台 Player Setting 細項設定皆為不同，會因為某些沒有設定導致輸出時出包，所以才寫了一個自動輸出個平台功能。

## XR Setting

可以利用這段來新增或移除 XR 裡面的 Oculus 勾選。

```C#
private static void SetOculusXRLoader(BuildTargetGroup buildTarget, bool active)
{
    XRGeneralSettingsPerBuildTarget buildTargetSettings = null;
    EditorBuildSettings.TryGetConfigObject(XRGeneralSettings.k_SettingsKey, out buildTargetSettings);
    XRGeneralSettings settings = buildTargetSettings.SettingsForBuildTarget(buildTarget);

    if (active)
        XRPackageMetadataStore.AssignLoader(settings.Manager, "Unity.XR.Oculus.OculusLoader", buildTarget);
    else
        XRPackageMetadataStore.RemoveLoader(settings.Manager, "Unity.XR.Oculus.OculusLoader", buildTarget);
}
```

![img_1]

## Build

利用這段自動輸出，options 可以設定 `BuildOptions.None`、`BuildOptions.AutoRunPlayer`，一般的 Build 和 Build and Run。

```C#
private static void BuildRelease(string Path, BuildTarget Target, BuildOptions options)
{
    Console.Clear();
    BuildPlayerOptions playerOptions = GetBuildPlayer(Path, Target, options);
    BuildReport Report = BuildPipeline.BuildPlayer(playerOptions);
    EditorUtility.RevealInFinder(Path);
    Debug.Log(string.Format("{0} Build completed with a result of '{1}' ", Application.platform, Report.summary.result.ToString()));
}

private static BuildPlayerOptions GetBuildPlayer(string path, BuildTarget Target, BuildOptions options)
{
    return new BuildPlayerOptions()
    {
        scenes = EnabledScenePaths,
        locationPathName = path,
        target = Target,
        options = options
    };
}
```

## 完整 Script

```C#
public class SampleBuildRelease
{
    private static string AppName => PlayerSettings.productName;

    private static string Version => Application.version.Replace(".", "");

    private static string BuildFolder
    {
        get { return Directory.GetParent(Application.dataPath).FullName.Replace('\\', '/') + "/Build"; }
    }

    private static string[] EnabledScenePaths => EditorBuildSettings.scenes
        .Where((scene) => scene.enabled)
        .Select((scene) => scene.path)
        .ToArray();

    [MenuItem("Builds/Build/Oculus")]
    public static void Build_Oculus()
    {
        string path = Path.Combine(BuildFolder, "Oculus", $"Oculus_{AppName}_Ver{Version}.apk");
        BuildReleaseOculus(path, BuildOptions.None, true);
    }

    [MenuItem("Builds/Build And Run/Oculus")]
    public static void BuildAndRun_Oculus()
    {
        string path = Path.Combine(BuildFolder, "Oculus", $"Oculus_{AppName}_Ver{Version}.apk");
        BuildReleaseOculus(path, BuildOptions.AutoRunPlayer, true);
    }

    private static void BuildReleaseOculus(string path, BuildOptions buildOptions, bool AddXR)
    {
        SetOculusXRLoader(BuildTargetGroup.Android, AddXR);
        BuildRelease(path, BuildTarget.Android, buildOptions);
    }

    [MenuItem("Builds/Build/Windows")]
    public static void Build_Windows()
    {
        BuildReleaseWindows(BuildOptions.None);
    }

    [MenuItem("Builds/Build And Run/Windows")]
    public static void BuildAndRun_Windows()
    {
        BuildReleaseWindows(BuildOptions.AutoRunPlayer);
    }

    private static void BuildReleaseWindows(BuildOptions buildOptions)
    {
        string folder = Path.Combine(BuildFolder, $"{BuildTarget.StandaloneWindows}", $"Windows_{AppName}_Ver{Version}");
        string path = Path.Combine(folder, $"{AppName}.exe");
        SetOculusXRLoader(BuildTargetGroup.Standalone, false);
        BuildRelease(path, BuildTarget.StandaloneWindows, buildOptions);
    }

    private static void SetOculusXRLoader(BuildTargetGroup buildTarget, bool active)
    {
        XRGeneralSettingsPerBuildTarget buildTargetSettings = null;
        EditorBuildSettings.TryGetConfigObject(XRGeneralSettings.k_SettingsKey, out buildTargetSettings);
        XRGeneralSettings settings = buildTargetSettings.SettingsForBuildTarget(buildTarget);

        if (active)
            XRPackageMetadataStore.AssignLoader(settings.Manager, "Unity.XR.Oculus.OculusLoader", buildTarget);
        else
            XRPackageMetadataStore.RemoveLoader(settings.Manager, "Unity.XR.Oculus.OculusLoader", buildTarget);
    }

    private static void BuildRelease(string Path, BuildTarget Target, BuildOptions options)
    {
        Console.Clear();
        BuildPlayerOptions playerOptions = GetBuildPlayer(Path, Target, options);
        BuildReport Report = BuildPipeline.BuildPlayer(playerOptions);
        EditorUtility.RevealInFinder(Path);
        Debug.Log(string.Format("{0} Build completed with a result of '{1}' ", Application.platform, Report.summary.result.ToString()));
    }

    private static BuildPlayerOptions GetBuildPlayer(string path, BuildTarget Target, BuildOptions options)
    {
        return new BuildPlayerOptions()
        {
            scenes = EnabledScenePaths,
            locationPathName = path,
            target = Target,
            options = options
        };
    }
}
```

---

[img_1]: https://i.imgur.com/l0ovOpl.png
