---
title: "心得 虛擬代理模式"
date: 2024-04-30T00:39:46+08:00
summary: "等待 API 回應期間要怎麼顯示「資料更新中」？虛擬代理模式就是解法！本文示範用中央氣象署 API 實作 WeatherProxy，讓資料載入過程更優雅。"
description: "《深入淺出設計模式》虛擬代理模式（Virtual Proxy Pattern）實作心得。以呼叫中央氣象署 Open API 取得天氣資料為例，示範 WeatherProxy 如何在資料載入期間回傳暫時訊息，防止重複請求，附完整 C# 程式碼與 UML。"
keywords: ["Design Pattern", "Proxy Pattern", "Virtual Proxy", "async", "Task", "HttpClient", "C#", "GoF"]
draft: false
tags: ["Design Pattern"]
aliases:
  - /posts/designpattern-virtualproxy/
---

## 前言

這篇主要是在講虛擬代理人的實作部分。

實作的主題的方向是讀取[中央氣象署][cwa]的氣象資訊。利用中央氣象署的 [API][cwa_api] 呼叫後，取得氣象資訊，並且在等待回應時實作虛擬代理設計。

## UML & Build

![uml]

![build]

## WeatherProxy

使用 weather is null 判斷是否成功取得資料，並且 `weatherDataTask == null || weatherDataTask.Status != TaskStatus.Running` 來防止重複取得資料。

```CSharp
    public string GetWeather(string area)
    {
        if (weather != null)
        {
            return weather.GetWeather(area);
        }
        else
        {
            if (weatherDataTask == null|| weatherDataTask.Status != TaskStatus.Running)
            {
                weatherDataTask = Task.Run(FetchWeatherData);
            }
            return "天氣資料更新中...\n";
        }
    }
```

完整程式碼

```CSharp
public class WeatherProxy : IWeather
{
    /// <summary>
    /// https://opendata.cwa.gov.tw
    /// https://opendata.cwa.gov.tw/dist/opendata-swagger.html
    /// </summary>
    private const string Url = "https://opendata.cwa.gov.tw/api/v1/rest/datastore/F-D0047-073";
    private const string AuthorizationKey = "AuthorizationKey";
    private const string Format = "JSON";

    private IWeather weather;
    private Task weatherDataTask;

    public string GetWeather(string area)
    {
        if (weather != null)
        {
            return weather.GetWeather(area);
        }
        else
        {
            if (weatherDataTask == null|| weatherDataTask.Status != TaskStatus.Running)
            {
                weatherDataTask = Task.Run(FetchWeatherData);
            }
            return "天氣資料更新中...\n";
        }
    }

    private async Task FetchWeatherData()
    {
        weather = new Weather(await SendRequest());
    }

    private async Task<Root> SendRequest()
    {
        using var client = new HttpClient();
        var query = HttpUtility.ParseQueryString(string.Empty);
        query["Authorization"] = AuthorizationKey;
        query["format"] = Format;
        var builder = new UriBuilder(Url);
        builder.Query = query.ToString();
        string requestUri = builder.ToString();
        string responseJson = await client.GetStringAsync(requestUri);
        return string.IsNullOrEmpty(responseJson) ? null : JsonSerializer.Deserialize<Root>(responseJson);
    }
}
```

## [GitHub][repo]

---

[uml]: https://i.imgur.com/FmnyNhG.png
[build]: https://i.imgur.com/r8KdOgM.png
[cwa]: https://opendata.cwa.gov.tw
[cwa_api]: https://opendata.cwa.gov.tw/dist/opendata-swagger.html
[repo]: https://github.com/Wenrong274/WeatherForecast
