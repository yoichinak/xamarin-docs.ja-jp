---
title: Xamarin. iOS の NSObject の汎用サブクラス
description: このドキュメントでは、NSObject の汎用サブクラスを作成する方法について説明します。 どのようなことができるか、および実行できないかを調べ、静的レジストラーについて説明し、パフォーマンスを確認します。
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 279fcac1611038613bf442e1b766fda45dd5a429
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022361"
---
# <a name="generic-subclasses-of-nsobject-in-xamarinios"></a>Xamarin. iOS の NSObject の汎用サブクラス

## <a name="using-generics-with-nsobjects"></a>NSObjects でのジェネリックの使用

`NSObject`のサブクラスでジェネリックを使用することもできます (例: [Uiview](xref:UIKit.UIView))。

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

`NSObject` サブクラス化されたオブジェクトは目的の C ランタイムに登録されるため、`NSObject` 型のジェネリックサブクラスでできることにはいくつかの制限があります。

## <a name="considerations-for-generic-subclasses-of-nsobject"></a>NSObject の汎用サブクラスに関する考慮事項

このドキュメントでは、`NSObjects`の汎用サブクラスの制限付きサポートの制限事項について詳しく説明します。

### <a name="generic-type-arguments-in-member-signatures"></a>メンバーシグネチャのジェネリック型引数

目的の C に公開されているメンバーシグネチャ内のすべてのジェネリック型引数には、`NSObject` 制約が必要です。

**いいですね**。

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**理由**: ジェネリック型パラメーターは `NSObject`であるため、`myMethod:` のセレクターシグネチャを目的の C に安全に公開できます (常に `NSObject` またはそのサブクラスになります)。

**無効**:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**理由**: シグネチャは `T`ジェネリック型の正確な型によって異なるため、目的の c コードで呼び出すことができるエクスポートされるメンバーに対して、c の署名を作成することはできません。

**いいですね**。

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

**理由**: エクスポートされたメンバーシグネチャの一部を受け取らない限り、制約のないジェネリック型引数を含めることができます。

**いいですね**。

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

**理由**: 目的 C でエクスポートされた `MyMethod` の `T` パラメーターは `NSObject`に制限されています。制約のない型 `U` はシグネチャの一部ではありません。

### <a name="instantiations-of-generic-types-from-objective-c"></a>目標からのジェネリック型のインスタンス化-C

目的の C からのジェネリック型のインスタンス化は許可されていません。 これは通常、xib またはストーリーボードでマネージ型が使用されている場合に発生します。

このクラス定義を考えてみます。このクラス定義は、`IntPtr` (ネイティブの目的 C のインスタンスC#からオブジェクトを構築する Xamarin) を受け取るコンストラクターを公開しています。

```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

上記のコンストラクトは問題ありませんが、実行時には、前の例で例外がスローされます。

これは、目標 C にはジェネリック型の概念がなく、作成する厳密なジェネリック型を指定できないために発生します。

この問題は、ジェネリック型の特殊なサブクラスを作成することによって回避できます。 (例:

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

これで、あいまいさはなくなりました。 xib または storyboard でクラス `GenericUIView` を使用できます。

## <a name="no-support-for-generic-methods"></a>ジェネリックメソッドはサポートしていません

### <a name="generic-methods-are-not-allowed"></a>ジェネリックメソッドは使用できません。

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

**理由**: これは許可されていません。これは、メソッドが目的の C から呼び出された場合に、`T` 型引数に使用する種類が Xamarin にわからないためです。

代わりに、特殊なメソッドを作成してエクスポートする方法もあります。

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

### <a name="no-exported-static-members-allowed"></a>エクスポートされた静的メンバーは使用できません

静的メンバーが `NSObject`のジェネリックサブクラスの内部でホストされている場合、そのメンバーを目的の C に公開することはできません。

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

**理由:** ジェネリックメソッドと同様に、Xamarin の iOS ランタイムは、ジェネリック型引数 `T`に使用する型を認識できる必要があります。

インスタンスメンバーの場合は、インスタンス自体が使用されます (`Generic<T>`インスタンスになることはないため、常に `Generic<SomeSpecificClass>`)。ただし、静的メンバーの場合、この情報は存在しません。

これは、問題のメンバーが型引数 `T` 使用しない場合でも適用されることに注意してください。

この場合の代替手段は、特化されたサブクラスを作成することです。

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

静的レジストラーは、通常のように、ビルド時にジェネリック型のエクスポートされたメンバーを解決できません。実行時に検索する必要があります。 これは、このようなメソッドを目的の C から呼び出すことは、非ジェネリッククラスからのメンバーの呼び出しよりも少し遅くなることを意味します。
