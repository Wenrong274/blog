---
title: "Effective C# 做法 07-10"
date: 2024-08-28T00:19:53+08:00
description: "Effective C# 做法  07-10 心得"
keywords: ["Effective C#", "C#"]
draft: false
showtoc: true
tags: ["C#"]
---

## 做法 07

`以 delegate 表示 callback`

可以利用 delegate 回傳 bool 當判斷或者是回傳類別等功能降低城市耦合。

```Csharp
List<int> numbers = Enumerable.Range(1, 10).ToList();
var oddNumbers = numbers.Find(x => x % 2 == 1);
var test = numbers.TrueForAll(x => x < 50);
numbers.RemoveAll(x => x % 2 == 0);
numbers.ForEach(x => Console.WriteLine(x));

public void LengthyOperation2(Func<bool> pred)
{
    bool bContinue = true;
    foreach (var cl in container)
    {
        cl.DoLengthyOperation();
        foreach (Func<bool> pr in pred.GetInvocationList())
                bContinue &= pr();

        if (!bContinue) return;
    }
}
```

## 做法 08

`對事件叫用使用空條件運算子`

推薦這樣寫

```Csharp
public void RaiseUpdate()
{
    counter++;
    Updated?.Invoke(this, counter);
}
```

## 做法 09

`減少 boxing 與 unboxing`

```Csharp
int i = 25;
object o = i; /// box
Console.WriteLine(o.ToString()); /// 25

object fp = 5;
object fo = fp;
int ip = (int)fo; /// unbox
Console.WriteLine(ip.ToString()); /// 5
```

> 實值類型可變轉換成 System.Object 或任何界面參考。這些轉換隱含的發生，使得尋找它們變得更為複雜。
> boxing 與 unboxing 操作會在你預料之外的複製拷貝，這導致 bug。以多型方式處理實值類型也有效能成本。

參考類型轉換為 System.Object 或介面時的行為確實不同：

1. 參考類型轉換:
   - 參考類型本來就是從 System.Object 繼承的，所以轉換為 System.Object 或實現的介面時不需要 boxing。
   - 這種轉換只是改變了引用的類型，不會創建新的對象或複製數據。
2. 效能影響:
   - 參考類型轉換為 System.Object 或介面基本上沒有額外的效能成本。
   - 不會發生像實值類型那樣的額外記憶體分配或數據複製。
3. 多態性:
   - 參考類型本來就支持多態，不需要額外的轉換步驟。
4. 潛在問題:
   - 雖然參考類型不會遇到 boxing/unboxing 的問題，但可能會遇到其他問題，如意外的類型轉換或空引用。

## 做法 10

`只對基底類別更新使用 new 修飾詞`

```Csharp
public class MyClass
{
    public new void MagicMethod() { }
}
```

> new 修飾詞必須小心使用。如果你不假思索的套用它，你會在你的物件中產生模糊的方法呼叫。這是發生在升級你的基底類別導致與你的類別衝突的特殊情況。就算在這種情況下，使用前還是要仔細思考。更重要的是，其他情況不要使用它。

---
