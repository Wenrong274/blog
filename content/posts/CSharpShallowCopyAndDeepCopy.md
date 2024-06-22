---
title: "C# 淺複製與深複製"
date: 2024-06-22T19:02:04+08:00
description: "C# 淺複製與深複製"
keywords:
  [
    "C#",
    "Class",
    "Struct",
    "Reference Type",
    "Value Type",
    "Shallow Copy",
    "Deep Copy",
  ]
draft: false
showtoc: true
tags: ["C#"]
---

## 前言

之前有講到 [C# Value Type、Reference Type 的差異]，現在來講一下淺複製（Shallow Copy）與深複製（Deep Copy）。

## 淺複製

將原有物件的欄位依照其型別來複製，Value Type 欄位複製其數值到另一個空間，Reference Type 欄位則是複製其參考到另一個空間，因此此兩物件的 Refetence Type 都是參考到同一 instance。

### 淺複製方式

```C#
public class Address
{
    public string Street = string.Empty;
    public string City = string.Empty;
}

public class Person
{
    public string Name = string.Empty;
    public int Age = 0;
    public Address Address = new Address();

    public Person ShallowCopy()
    {
        return (Person)this.MemberwiseClone();
    }
}
```

```C#
Person origin = new Person()
{
    Name = "Sam",
    Age = 20,
    Address = new Address()
    {
        Street = "123 Main St",
        City = "Anytown"
    }
};
Person copy = origin.ShallowCopy();

copy.Name = "John";
copy.Age = 30;
copy.Address.Street = "456 Main St";
copy.Address.City = "Taiwan";

Console.WriteLine("Shallow Copy");
// Origin: Sam, 20, 456 Main St, Taiwan
Console.WriteLine($"Origin: {origin.Name}, {origin.Age}, {origin.Address.Street}, {origin.Address.City}");
// Copy: John, 30, 456 Main St, Taiwan
Console.WriteLine($"Copy: {copy.Name}, {copy.Age}, {copy.Address.Street}, {copy.Address.City}");
```

輸出結果

`Origin: Sam, 20, 456 Main St, Taiwan`

`Copy: John, 30, 456 Main St, Taiwan`

`Name` 是 String，可是沒有跟著改變，可以先看[C# Reference Type String]這篇。

`Age` 是 int，也是 Value Type，所以會改變。

`Address` 是 class 是 Reference Type，因為淺複製的關係導致他們（Origin、Copy）有一起改變

## 深複製

ValueType 與`淺複製`相同，主要差異是`深複製`的 Reference Type 欄位會產生新的 instance，所以其倆欄位是參考到不同 instance 的。

### 深複製方式

```C#
public class Address
{
    public string Street = string.Empty;
    public string City = string.Empty;
}

public class Person
{
    public string Name = string.Empty;
    public int Age = 0;
    public Address Address = new Address();

    public Person DeepCopy()
    {
        Person clone = (Person)this.MemberwiseClone();
        clone.Address = new Address
        {
            Street = this.Address.Street,
            City = this.Address.City
        };
        return clone;
    }
}
```

```C#
Person origin = new Person()
{
    Name = "Sam",
    Age = 20,
    Address = new Address()
    {
        Street = "123 Main St",
        City = "Anytown"
    }
};
Person copy = origin.DeepCopy();

copy.Name = "John";
copy.Age = 30;
copy.Address.Street = "456 Main St";
copy.Address.City = "Taiwan";

Console.WriteLine("Shallow Copy");
// Origin: Sam, 20, 123 Main St, Anytown
Console.WriteLine($"Origin: {origin.Name}, {origin.Age}, {origin.Address.Street}, {origin.Address.City}");
// Copy: John, 30, 456 Main St, Taiwan
Console.WriteLine($"Copy: {copy.Name}, {copy.Age}, {copy.Address.Street}, {copy.Address.City}");
```

`Origin: Sam, 20, 123 Main St, Anytown`

`Copy: John, 30, 456 Main St, Taiwan`

與淺複製的差異，深複製的 `Address` 欄位也會產生新的 instance，在修改 `copy.Address` 時，不會影響到 `origin.Address`。

## 參考連結

[C# 複製物件的方式比較]

---

[C# Value Type、Reference Type 的差異]: ../csharpvaluetypereferencetype
[C# Reference Type String]: ../csharpreferencetypestring
[C# 複製物件的方式比較]: https://dotblogs.com.tw/lazycodestyle/2016/06/18/183737
