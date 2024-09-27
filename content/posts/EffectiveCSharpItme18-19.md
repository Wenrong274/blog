---
title: "Effective C# 做法 18-19"
date: 2024-09-28T01:48:09+08:00
description: "Effective C# 做法 18-19 心得"
keywords: ["Effective C#", "C#"]
draft: false
showtoc: true
tags: ["C#"]
---

## 做法 18

`使用泛型 & 定義最少與足夠的約束

### 使用泛型

- 為什麼這三組執行期都共用相同的程式

  ```Csharp
  List<string> strings = new List<string>();
  List<Stream> streams=newList<Stream>();
  List<MyClassType> myClassTypes=newList<MyClassType>();
  ```

JIT 編譯器識別出這些類型都是引用類型，因此可以使用相同的機器代碼。

- 為什麼這三組有不同的機械碼?

  ```Csharp
  List<double> doubles = new List<double>();
  List<int> ints = new List<int>();
  List<MyStruct> myStructs = new List<MyStruct>();
  ```

  CLR 為每種值類型參數生成特定的代碼，以優化性能和類型安全性。

#### 1. 泛型類別使用 default()

在講到泛型類別使用 default() 的方法寫時，需要把 T 改成 T?，因為 Value Type 的 Default = 0，而 Reference Type 是沒有 Default，回傳的結果會是 null。
需要告訴使用者可能會回傳 null 建議還是要回傳 T?，也提高程式碼閱讀性。

```Csharp
public static T? FirstOrDefault<T>(this IEnumerable<T> source, Predicate<T> predicate)
{
    foreach (var item in source)
    {
        if (predicate(item))
            return item;
    }
    return default;
}
```

#### 2. 少了 null 確認

原本課本的判斷，少了 null 確認

```Csharp
public static bool AreEqual(T left, T right) =>	left.Equals(right);
```

可以改這種寫法，多了做法 03 的寫法，用來判斷 null。

```Csharp
public static bool AreEqual<T>(T left, T right)
{
    if (left is T qu)
    {
        return qu.Equals(right);
    }
    return right == null;
}
```

#### 3. 使用 T?

使用 T? 明確表示 factory 方法可能返回 null。這增加了代碼的可讀性和意圖的清晰度。

```Csharp
MyClass obj1 = Factory(() => new MyClass { Name = "Object 1" });
Console.WriteLine(obj1.Name); // 輸出: Object 1

// 使用返回 null 的工廠方法
MyClass obj2 = Factory<MyClass>(() => null);
Console.WriteLine(obj2.Name); // 輸出: Default Name

public delegate T FactoryFunc<T>();

public static T Factory<T>(FactoryFunc<T?> factory) where T : new()
{
    T? resultVal = factory();
    if (resultVal == null)
    {
        resultVal = new T();
    }
    return resultVal;
}
```

#### 4. 為什麼要盡量避免約束 new()、struct、class

#### new() 約束

優點：

- 保證類型有無參數構造函數。
  缺點：
- 限制了類型的靈活性，排除了沒有公共無參數構造函數的類型。
- 可能導致不必要的對象創建，影響性能。
- 難以進行單元測試，因為無法輕易模擬或替換對象。

#### struct 約束

優點：

- 確保類型是值類型。

缺點：

- 過度限制了類型，排除了可能同樣適用的引用類型。
- 可能導致不必要的裝箱和拆箱操作。
- 限制了代碼的重用性和靈活性。

### class 約束

優點：

- 確保類型是引用類型。

缺點：

- 排除了可能同樣適用的值類型。
- 限制了代碼的通用性。
- 可能導致不必要的堆分配。

一般建議：

1. 優先使用接口約束：接口約束更靈活，能同時適用於值類型和引用類型。例如：

   ```Csharp
   public class GenericClass<T> where T : IComparable<T>
   ```

2. 基於行為而非類型種類進行約束：關注類型能做什麼，而不是類型是什麼。
3. 考慮使用泛型約束的組合：在必要時，可以組合多個接口約束來精確定義所需的功能。
4. 使用 default 關鍵字：代替 new()，使用 default(T) 來獲取類型的默認值。
5. 設計靈活的 API：避免過度約束，以增加代碼的重用性和適應性。
6. 考慮性能影響：某些約束（如 struct）可能導致意外的性能問題。

### 總結

避免使用 new()、 struct 和 class 約束主要是為了提高代碼的靈活性、可重用性和可測試性。通過使用更通用的約束（如接口），我們可以編寫出更加靈活和強大的泛型代碼，適用於更廣泛的類型。

## 做法 19

使用執行期型別檢查特化泛型演算法
實作

```Csharp
public sealed class ReverseEnumerable<T> : IEnumerable<T>
{
    private class ReverseEnumerator : IEnumerator<T>
    {
        private int currentIndex = 0;
        private IList<T> collection;

        public T Current => collection[currentIndex];

        object IEnumerator.Current => Current;
        public ReverseEnumerator(List<T> collection) : this(collection.ToArray())
        {
        }
        public ReverseEnumerator(IList<T> collection)
        {
            currentIndex = collection.Count;
            this.collection = collection;
        }

        public void Dispose()
        {

        }

        public bool MoveNext()
        {
            return --currentIndex >= 0;
        }

        public void Reset()
        {
            currentIndex = collection.Count;
        }
    }

    private sealed class ReverseStringEnumerator : IEnumerator<char>
    {
        private string sourceSequence;
        private int currentIndex;

        public ReverseStringEnumerator(string source)
        {
            sourceSequence = source;
            currentIndex = source.Length;
        }

        public char Current => sourceSequence[currentIndex];

        object IEnumerator.Current => sourceSequence[currentIndex];

        public void Dispose() { }

        public bool MoveNext()
        {
            return --currentIndex >= 0;
        }

        public void Reset()
        {
            currentIndex = sourceSequence.Length;
        }
    }

    private IEnumerable<T>? sourceSequence;
    private IList<T>? originalSequence;

    public ReverseEnumerable(IEnumerable<T> source)
    {
        if (source is ReverseEnumerable<T> reverseEnumerable)
            sourceSequence = reverseEnumerable;

        if (source is string str)
        {
            originalSequence = str.ToArray() as IList<T>;
        }
        else
        {
            originalSequence = source as IList<T>;
        }
    }

    public ReverseEnumerable(IList<T> source) : this((IEnumerable<T>)source)
    { }

    public IEnumerator<T> GetEnumerator()
    {
        if (sourceSequence is string str)
        {
            return new ReverseStringEnumerator(str) as IEnumerator<T>;
        }

        if (originalSequence == null)
        {
            if (sourceSequence is ICollection<T> source)
            {
                originalSequence = new List<T>(source.Count);
            }
            else
            {
                originalSequence = new List<T>(sourceSequence);
            }
        }

        return new ReverseEnumerator(originalSequence);
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }
}
```

使用方式

```Csharp
private static void StringEnumerable()
{
    string str = "Hello, World!";

    var reverseEnumerable = new ReverseEnumerable<char>(str);
    foreach (char c in reverseEnumerable)
    {
        Console.Write(c);
    }
    /// !dlroW ,olleH
    Console.WriteLine();
}

private static void ListEnumerable()
{
    List<int> list = new List<int>() { 1, 2, 3, 4, 5, 6, 7 };
    var reverseEnumerable = new ReverseEnumerable<int>(list);
    foreach (int i in reverseEnumerable)
    {
        Console.Write(i);
    }
    /// 76543210
    Console.WriteLine();
}
```

---
