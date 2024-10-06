---
title: "Effective C# 做法 20-21"
date: 2024-10-07T00:57:47+08:00
description: "Effective C# 做法 20-21 心得"
keywords: ["Effective C#", "C#"]
draft: false
showtoc: true
tags: ["C#"]
---

## 做法 20

`以 IComparable<T> 與 IComparer<T> 實作排序關係`

### 實作

```Csharp
public struct Customer : IComparable<Customer>, IComparable
{
    private readonly string name;
    private double revenue;

    public Customer(string name)
    {
        this.name = name;
    }

    public int CompareTo(Customer other)
    {
        return name.CompareTo(other.name);
    }

    int IComparable.CompareTo(object? obj)
    {
        if (obj is Customer other)
        {
            return this.CompareTo(other);
        }
        throw new ArgumentException("Argument must be a Customer", nameof(obj));
    }

    public static bool operator <(Customer left, Customer right) => left.CompareTo(right) < 0;
    public static bool operator <=(Customer left, Customer right) => left.CompareTo(right) <= 0;
    public static bool operator >(Customer left, Customer right) => left.CompareTo(right) > 0;
    public static bool operator >=(Customer left, Customer right) => left.CompareTo(right) >= 0;

    private static Lazy<RevenueComparer> revComp = new Lazy<RevenueComparer>(() => new());

    public static IComparer<Customer> RevenueCompare => revComp.Value;
    public static Comparison<Customer> CompareByRevenue => (left, right) => left.revenue.CompareTo(right.revenue);

    private class RevenueComparer : IComparer<Customer>
    {
        public int Compare(Customer left, Customer right)
        {
            return left.revenue.CompareTo(right.revenue);
        }
    }
}
```

### 使用方式

```Csharp
static void Main(string[] args)
{
    // 創建一些 Customer 實例
    var customers = new List<Customer>
    {
    new Customer("Alice",1000) ,
    new Customer("Bob",1500) ,
    new Customer("Charlie",1200),
    new Customer("David",800)
    };

    // 1. 使用默認比較（按名稱）
    customers.Sort();
    Console.WriteLine("Sorted by name:");
    // Alice: 1000
    // Bob: 1500
    // Charlie: 1200
    // David: 800
    PrintCustomers(customers);

    // 2. 使用運算符比較
    var alice = new Customer("Alice");
    var bob = new Customer("Bob");
    // Alice < Bob: True
    // Alice <= Bob: True
    // Alice > Bob: False
    // Alice >= Bob: False
    Console.WriteLine($"Alice < Bob: {alice < bob}");
    Console.WriteLine($"Alice <= Bob: {alice <= bob}");
    Console.WriteLine($"Alice > Bob: {alice > bob}");
    Console.WriteLine($"Alice >= Bob: {alice >= bob}");

    // 3. 使用 RevenueCompare
    customers.Sort(Customer.RevenueCompare);
    Console.WriteLine("\nSorted by revenue (using RevenueCompare):");
    // David: 800
    // Alice: 1000
    // Charlie: 1200
    // Bob: 1500
    PrintCustomers(customers);

    // 4. 使用 CompareByRevenue
    customers.Sort(Customer.CompareByRevenue);
    Console.WriteLine("\nSorted by revenue (using CompareByRevenue):");
    // David: 800
    // Alice: 1000
    // Charlie: 1200
    // Bob: 1500
    PrintCustomers(customers);

    // 5. 使用 LINQ 排序
    var sortedByName = customers.OrderBy(c => c);
    Console.WriteLine("\nSorted by name (using LINQ):");
    // lice: 1000
    // Bob: 1500
    // Charlie: 1200
    // David: 800

    PrintCustomers(sortedByName);

    var sortedByRevenue = customers.OrderBy(c => c, Customer.RevenueCompare);
    Console.WriteLine("\nSorted by revenue (using LINQ and RevenueCompare):");
    // David: 800
    // Alice: 1000
    // Charlie: 1200
    // Bob: 1500
    PrintCustomers(sortedByRevenue);
}

static void PrintCustomers(IEnumerable<Customer> customers)
{
    foreach (var customer in customers)
    {
        Console.WriteLine($"{customer.ToString()}");
    }
}
```

## 做法 21

`建構支援 Disposable 型別參數的泛型類別`

> 如果你建構的泛型類別的型別參數描述的任何型別實例，你必須考慮到這些型別可能有實作 IDisposable。你必須防衛性的撰寫程式並確保這些物件離開範圍後不會洩漏資源。

```Csharp
public interface IEngine
{
    void DoWork();
}

private class Engine : IEngine
{
    public void DoWork()
    {
        Console.WriteLine("Hello World!");
    }
}

public class EngineDriver1<T> where T : IEngine, new()
{
    public void GetThingsDone()
    {
        T engine = new T();
        using (engine as IDisposable)
        {
            engine.DoWork();
        }
    }
}

// using (var x = new EngineDriver2<Engine>())
// {
//     x.GetThingsDone();
// }
public class EngineDriver2<T> : IDisposable where T : IEngine, new()
{
    private Lazy<T> engine = new Lazy<T>(() => new T());

    public void GetThingsDone()
    {
        using (engine.Value as IDisposable)
        {
            engine.Value.DoWork();
        }
    }

    public void Dispose()
    {
        if (engine.IsValueCreated)
        {
            var resource = engine.Value as IDisposable;
            resource?.Dispose();
        }
    }
}

public class EngineDriver3<T> where T : IEngine
{
    private T engine;

    public EngineDriver3(T engine)
    {
        this.engine = engine;
    }

    public void GetThingsDone()
    {
        engine.DoWork();
    }
}
```

### 若 T 沒有實作 IDisposable ，則此區域變數的值為 null。

如果 T（在這個例子中是 Engine）沒有實作 IDisposable 接口，那麼 engine as IDisposable 的結果就會是 null。
這個 null 值並不會導致異常或錯誤。 using 語句被設計為可以安全地處理 null 值。當 disposable 為 null 時， using 語句簡單地不執行任何釋放資源的操作。
這就是為什麼即使 Engine 類沒有實作 IDisposable，代碼仍然可以正常運行。 using 語句在這種情況下實際上不會做任何特殊的事情，代碼會繼續執行 engine.DoWork()。

---
