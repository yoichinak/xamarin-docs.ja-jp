---
title: Xamarin.iOS での NSObject のジェネリック サブクラス
description: このドキュメントでは、NSObject のジェネリック サブクラスを作成する方法について説明します。 できることと、できないことを調べ、静的レジストラーについて説明し、パフォーマンスを確認します。
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 279fcac1611038613bf442e1b766fda45dd5a429
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73022361"
---
# <a name="generic-subclasses-of-nsobject-in-xamarinios"></a>Xamarin.iOS での NSObject のジェネリック サブクラス

## <a name="using-generics-with-nsobjects"></a>NSObject でのジェネリックの使用

`NSObject` のサブクラスでジェネリックを使用できます ([UIView](xref:UIKit.UIView) など)。

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

`NSObject` をサブクラス化するオブジェクトは Objective-C のランタイムに登録されるため、`NSObject` 型のジェネリック サブクラスでできることにはいくつかの制限があります。

## <a name="considerations-for-generic-subclasses-of-nsobject"></a>NSObject のジェネリック サブクラスに関する考慮事項

このドキュメントでは、`NSObjects` のジェネリック サブクラスの制限付きサポートの制限事項について詳しく説明します。

### <a name="generic-type-arguments-in-member-signatures"></a>メンバー シグネチャでのジェネリック型引数

Objective-C に公開されているメンバー シグネチャ内のすべてのジェネリック型引数には、`NSObject` 制約が必要です。

**良い例**:

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**[Reason]\(理由\)** :ジェネリック型パラメーターは `NSObject` であるため、`myMethod:` のセレクター シグネチャを Objective-C に安全に公開できます (常に `NSObject` またはそのサブクラスになります)。

**悪い例**:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**理由**: ジェネリック型 `T` の正確な型によってシグネチャが異なるため、Objective-C のコードで呼び出すことができるエクスポートされたメンバーに対して、Objective-C のシグネチャを作成できません。

**良い例**:

```csharp
class Generic<T> : NSObject
{
    T storage;

    [Export ("myMethod:")]
    public void MyMethod (NSObject value)
    {
    }
}
```

**理由**: エクスポートされるメンバー シグネチャの一部でない限り、制約のないジェネリック型引数を使用できます。

**良い例**:

```csharp
class Generic<T, U> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
        Console.WriteLine (typeof (U));
    }
}
```

**理由**: Objective-C のエクスポートされた `MyMethod` における `T` パラメーターは `NSObject` になるように制約されているため、制約のない型 `U` はシグネチャの一部ではありません。

### <a name="instantiations-of-generic-types-from-objective-c"></a>Objective-C からのジェネリック型のインスタンス化

Objective-C からのジェネリック型のインスタンス化は許可されていません。 これは通常、xib またはストーリーボードでマネージド型が使用されている場合に発生します。

次のようなクラス定義を考えてみます。このクラス定義では、`IntPtr` を受け取るコンストラクターが公開されています (Xamarin.iOS において、ネイティブ Objective-C インスタンスから C# オブジェクトを構築する方法)。

```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

上記のコンストラクトは問題ありませんが、実行時には、Objective-C がそのインスタンスを作成しようとすると、例外がスローされます。

これは、Objective-C にはジェネリック型の概念がなく、作成する厳密なジェネリック型を指定できないために発生します。

この問題は、ジェネリック型の特殊なサブクラスを作成することによって回避できます。 次に例を示します。

```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}

class GenericUIView : Generic<UIView>
{
}
```

これで、あいまいさがなくなったため、xib またはストーリーボードで `GenericUIView` クラスを使用できます。

## <a name="no-support-for-generic-methods"></a>ジェネリック メソッドはサポートされない

### <a name="generic-methods-are-not-allowed"></a>ジェネリック メソッドは許可されません。

次のコードはコンパイルされません。

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyMethod<T> (T argument)
    {
    }
}
```

**[Reason]\(理由\)** :メソッドが Objective-C から呼び出されるとき、Xamarin.iOS では型引数 `T` に対して使用する型がわからないため、これは許可されません。

代わりに、特殊なメソッドを作成してエクスポートする必要があります。

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyUIViewMethod (UIView argument)
    {
        MyMethod<UIView> (argument);
    }
    public void MyMethod<T> (T argument)
    {
    }
}
```

### <a name="no-exported-static-members-allowed"></a>エクスポートされた静的メンバーは許可されない

静的メンバーが `NSObject` のジェネリック サブクラスの内部でホストされている場合、そのメンバーを Objective-C に公開することはできません。

サポートされていないシナリオの例:

```csharp
class Generic<T> : NSObject where T : NSObject
{
    [Export ("myMethod:")]
    public static void MyMethod ()
    {
    }

    [Export ("myProperty")]
    public static T MyProperty { get; set; }
}
```

**理由:** ジェネリック メソッドと同様に、Xamarin.iOS のランタイムでは、ジェネリック型引数 `T` に対して使用する型を認識できる必要があります。

インスタンス メンバーの場合はインスタンス自体が使用されますが (インスタンス `Generic<T>` はないため、常に `Generic<SomeSpecificClass>` になります)、静的メンバーの場合はこの情報は存在しません。

これは、問題のメンバーで型引数 `T` が使用されていない場合でも適用されることに注意してください。

この場合の代替手段は、特殊なサブクラスを作成することです。

```csharp
class GenericUIView : Generic<UIView>
{
    [Export ("myUIViewMethod")]
    public static void MyUIViewMethod ()
    {
        MyMethod ();
    }

    [Export ("myProperty")]
    public static UIView MyUIViewProperty {
        get { return MyProperty; }
        set { MyProperty = value; }
    }
}

class Generic<T> : NSObject where T : NSObject
{
    public static void MyMethod () {}
    public static T MyProperty { get; set; }
}
```

## <a name="performance"></a>パフォーマンス

静的レジストラーでは、通常のようにビルド時にジェネリック型のエクスポートされたメンバーを解決することはできません。実行時に検索する必要があります。 これは、そのようなメソッドを Objective-C から呼び出すと、非ジェネリック クラスのメンバーを呼び出すより少し遅くなることを意味します。
