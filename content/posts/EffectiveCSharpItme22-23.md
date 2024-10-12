---
title: "Effective C# 做法 22-23"
date: 2024-10-13T03:11:00+08:00
description: "Effective C# 做法 22-23 心得"
keywords: ["Effective C#", "C#"]
draft: false
showtoc: true
tags: ["C#"]
---

## 做法 22

`支援泛型的共變數與反變數`

### 1. 這個 out 也是 共變數?

```Csharp
public static bool TryParse (string? s, IFormatProvider? provider, out int result);
```

方法參數中的 out 關鍵字：
在 TryParse 方法中的 out int result 是一個輸出參數。這個 out 關鍵字的作用是：

- 表示這個參數是一個輸出參數。
- 方法必須在返回前為這個參數賦值。
- 呼叫方法時，不需要初始化這個參數，但必須在呼叫後使用它。

在泛型接口或委託定義中使用的 out 關鍵字表示協變。例如：

```Csharp
interface IEnumerable<out T> { ... }
```

這裡的 out 表示：

- T 只能用作輸出（返回值），不能用作輸入（參數）。
- 允許將更具體（派生）類型的集合賦值給更一般（基類）類型的集合變量。

區別總結：

1. 用途不同：
   - 方法參數的 out：用於指定輸出參數。
   - 泛型的 out：用於指定類型參數的協變性。
2. 位置不同：
   - 方法參數的 out：用在方法參數列表中。
   - 泛型的 out：用在泛型類型參數定義中。
3. 功能不同：
   - 方法參數的 out：確保方法將值賦給參數。
   - 泛型的 out：允許更靈活的類型轉換和賦值。
4. 編譯時行為： - 方法參數的 out：編譯器確保方法內部對參數賦值。 - 泛型的 out：編譯器允許特定的類型轉換。
   所以， TryParse 方法中的 out 不是用於協變的，而是一個輸出參數標記。這兩種 out 雖然使用相同的關鍵字，但在 C# 中代表完全不同的概念。

### 簡單解釋共變數與反變數

#### 共變數（Covariance）

定義：允許使用比原本指定的類別更具體（更繼承）的類型。
關鍵字：out
應用：通常用於返回類別或只讀泛型介面。

```CSharp
IEnumerable<string> strings = new List<string>();
IEnumerable<object> objects = strings; // 合法，因為 IEnumerable<T> 是共變數
```

#### 反變數（Contravariance）

定義：允許使用比原本指定的類別更一般（更基礎）的類別。
關鍵字：in
應用：通常用於參數類別或只寫泛型介面。

```CSharp
Action<object> objectAction = (object obj) => Console.WriteLine(obj);
Action<string> stringAction = objectAction; // 合法，因為 Action<T> 是反變數
```

#### 詳細解釋和更多例子

共變數例子：

```CSharp
public class Animal { }
public class Dog : Animal { }

IEnumerable<Dog> dogs = new List<Dog>();
IEnumerable<Animal> animals = dogs; // 共變數允許這種賦值

// 使用場景
void ProcessAnimals(IEnumerable<Animal> animals) { ... }
ProcessAnimals(dogs); // 可以傳入 Dog 的集合
```

反變數例子：

```CSharp
public interface IComparer<in T>
{
    int Compare(T x, T y);
}

IComparer<Animal> animalComparer = new AnimalComparer();
IComparer<Dog> dogComparer = animalComparer; // 反變數允許這種賦值

// 使用場景
class AnimalComparer : IComparer<Animal>
{
    public int Compare(Animal x, Animal y) { ... }
}

List<Dog> dogs = new List<Dog>();
dogs.Sort(dogComparer); // 可以使用 Animal 的比較器來比較 Dog
```

共變數和反變數的組合

```CSharp
public interface IConverter<in TInput, out TOutput>
{
    TOutput Convert(TInput input);
}

IConverter<Animal, Dog> animalToDog = new AnimalToDogConverter();
IConverter<Dog, Animal> dogToAnimal = animalToDog; // 合法
```

#### 限制和注意事項

- 共變數和反變數只適用於引用類型，不適用於值類型。
- 只能用於介面和委託的定義，不能用於類別。
- 一個類別參數不能同時標記為共變數和反變數。
- 共變數類別參數不能用作方法參數，反變數類別參數不能用作方法返回類型。

1. 實際應用

- LINQ: LINQ 大量使用共變數和反變數來提高其靈活性。
- 事件處理：允許更靈活地處理不同類別的事件。
- 依賴注入：在依賴注入框架中常用，提高容器的靈活性。

1. 性能考慮
   共變數和反變數主要是編譯時特性，不會對運行時性能產生顯著影響。
   總結：
   共變數和反變數強大的特性，能夠提高程式碼的靈活性和可重用性。
   共變數（out）允許使用更具體的類型，而反變數（in）允許使用更一般的類型。正確理解和使用這些概念可以幫助設計更靈活、更強大的 API 和框架。

```CSharp
static void Main(string[] args)
{
    Dog dog = new Dog("doggy");
    IOutDog<Dog> dogProvider = new DogProvider(dog);
    IOutDog<Animal> animalProvider = dogProvider;
    Console.WriteLine($"{animalProvider.Value.GetName()}, {dogProvider.Value.GetName()}");

    // 使用基本的 AnimalSetter
    IInDog<Animal> animalSetter = new AnimalSetter();
    animalSetter.SetValue(new Animal());
    animalSetter.SetValue(new Dog()); // 這也是有效的，因為 Dog 是 Animal 的子類

    // 使用 DogSetter，展示反變數
    // IInDog<Animal> dogSetterAsAnimal = (IInDog<Animal>)new DogSetter();
    // dogSetterAsAnimal.SetValue(new Animal()); // 注意：這在運行時可能會拋出異常，因為實際的實現期望一個 Dog

    // 使用泛型實現
    IInDog<Dog> dogSetter = new GenericAnimalSetter<Dog>();
    dogSetter.SetValue(new Dog());

    IConverter<Animal, Dog> animalToDog = new AnimalToDogConverter();
    IConverter<Dog, Animal> dogToAnimal = animalToDog; // 合法
}

public class Animal
{
    private readonly string name;
    public Animal(string name = "") => this.name = name;
    public virtual string GetName() => name;
}

public class Dog : Animal
{
    public Dog(string name = "") : base(name) { }
    public override string GetName() => $"Dog {base.GetName()}";
}

public interface IOutDog<out T> where T : Animal
{
    T Value { get; }
}

public class DogProvider : IOutDog<Dog>
{
    public Dog Value { get; set; }
    public DogProvider(Dog value) => Value = value;
}

public interface IInDog<in T> where T : Animal
{
    void SetValue(T value);
}

public class AnimalSetter : IInDog<Animal>
{
    private Animal value;
    public void SetValue(Animal value) => this.value = value;
}

public class DogSetter : IInDog<Dog>
{
    private Dog value;
    public void SetValue(Dog value) => this.value = value;
}

public class GenericAnimalSetter<T> : IInDog<T> where T : Animal
{
    private T value;
    public void SetValue(T value) => this.value = value;
}

public interface IConverter<in TInput, out TOutput>
{
    TOutput Convert(TInput input);
}

public class AnimalToDogConverter : IConverter<Animal, Dog>
{
    public Dog Convert(Animal input) => new Dog(input.GetName());
}
```

### 心得

如果編譯器告訴你錯了 就要小心是不是有什麼地方搞錯了
做 cast 時要了解自己在做什麼 不然能編譯成功也會在 runtime 時炸掉

## 做法 23

`使用 delegate 定義型別參數的方法約束`

```Csharp
// double[] xValues = { 0,1,2,3,4,5,6,7,8,9,
//                 0,1,2,3,4,5,6,7,8,9,};
// double[] yValues = { 0,1,2,3,4,5,6,7,8,9,
//                 0,1,2,3,4,5,6,7,8,9,};
// List<Point> values = new List<Point>(Zip(xValues, yValues, (x, y) => new Point(x, y)));
private static IEnumerable<TOutput> Zip<T1, T2, TOutput>(IEnumerable<T1> left, IEnumerable<T2> right, Func<T1, T2, TOutput> generator)
{
    IEnumerator<T1> leftEnumerator = left.GetEnumerator();
    IEnumerator<T2> rightEnumerator = right.GetEnumerator();
    while (leftEnumerator.MoveNext() && rightEnumerator.MoveNext())
    {
        yield return generator(leftEnumerator.Current, rightEnumerator.Current);
    }

    leftEnumerator.Dispose();
    rightEnumerator.Dispose();
}

private class Point
{
    public double X { get; set; }
    public double Y { get; set; }

    public Point(double x, double y)
    {
        X = x;
        Y = y;
    }

    public Point(TextReader textReader) { }
}

/// InputCollection<Point> points = new InputCollection<Point>((InputStream) => new Point(InputStream));
public delegate T CreateFromStream<T>(System.IO.TextReader textReader);
public class InputCollection<T>
{
    private List<T> thingsRead = new List<T>();

    private readonly CreateFromStream<T> readFunc;
    public InputCollection(CreateFromStream<T> readFunc)
    {
        this.readFunc = readFunc;
    }

    public void ReadFromStream(System.IO.TextReader textReader)
    {
        thingsRead.Add(readFunc(textReader));
    }
    public IEnumerable<T> Values => thingsRead;
}
```

### 做法 23 心得

可以利用 這種方式，讓不同類別擁有相同介面，使用相同的方法，讓程式彈性變高

```Csharp
// ForeachIn(items,(item, i) =>
// {
//      item.SetIcon(icon[i]);
// });
// ForeachIn(items,(item, i) =>
// {
//      item.SetPosition(positions[i]);
// });
public void ForeachIn<T>(T[] arr, Action<T, int> action)
{
    for (int i = 0; i < arr.Length; i++)
    {
        action(arr[i], i);
    }
}
```

---
