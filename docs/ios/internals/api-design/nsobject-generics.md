---
title: Xamarin.iOS での NSObject のジェネリック サブクラス
description: このドキュメントを作成する方法を説明します NSObject の汎用サブクラスを作成します。 ことができますとは実行できません、静的のレジストラーについて説明しますおよびパフォーマンス観察対象何を調査します。
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 4d2ab7a546925e92bb59f626279dcbf9c3c4ab4f
ms.sourcegitcommit: 93b1e2255d59c8ca6674485938f26bd425740dd1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/17/2019
ms.locfileid: "67157704"
---
# <a name="generic-subclasses-of-nsobject-in-xamarinios"></a>Xamarin.iOS での NSObject のジェネリック サブクラス

## <a name="using-generics-with-nsobjects"></a>NSObjects でジェネリックを使用します。

サブクラスでジェネリックを使用することは`NSObject`、たとえば[UIView](xref:UIKit.UIView):

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

オブジェクトのサブクラスであるため`NSObject`が登録されている、OBJECTIVE-C ランタイムでは、いくつかの制限の汎用サブクラスでできることについて`NSObject`型。
    
## <a name="considerations-for-generic-subclasses-of-nsobject"></a>NSObject の汎用サブクラスの考慮事項

このドキュメントの汎用サブクラスの制限付きサポートの制限事項の詳細を`NSObjects`します。
    
### <a name="generic-type-arguments-in-member-signatures"></a>メンバーのシグネチャのジェネリック型引数

Objective C に公開されるメンバー シグネチャ内のすべてのジェネリック型引数が必要、`NSObject`制約。

**適切な**:

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**理由**:ジェネリック型パラメーターは、`NSObject`ため、セレクターの署名`myMethod:`OBJECTIVE-C に安全に公開することができます (常に`NSObject`またはそのサブクラス)。

**不適切な**:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**理由**: 署名は、ジェネリック型の正確な型によって異なりますので、Objective C コードを呼び出すことができるエクスポートされたメンバーの Objective C の署名を作成することはできません`T`します。

**適切な**:

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

**理由**: エクスポートされたメンバーのシグネチャの一部を取らない限り、ジェネリック型引数を無制限がすることができます。

**適切な**:

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

**理由**: `T` Objective C でのパラメーターはエクスポート`MyMethod`に制約は、 `NSObject`、制約のない型`U`シグネチャの一部ではありません。
    
### <a name="instantiations-of-generic-types-from-objective-c"></a>Objective C からのジェネリック型のインスタンス化

Objective C からのジェネリック型のインスタンス化することはできません。 これは通常、マネージ型を xib またはストーリー ボードで使用する場合に発生します。

このクラスの定義を受け取るコンス トラクターを公開するを検討してください、 `IntPtr` (構築の Xamarin.iOS 方法、C#ネイティブ OBJECTIVE-C インスタンスからオブジェクト)。
    
```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

上記の構造は実行時に、問題ありませんが、Objective C のインスタンスを作成しようとする場合に、例外がスローされます。

これは Objective C には、ジェネリック型の概念がなく、作成する正確なジェネリック型は指定できないために発生します。

ジェネリック型の特殊なサブクラスを作成してこの問題を回避することができます。 例:
    
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

あいまいさになったクラスはありませんので`GenericUIView`xib またはストーリー ボードで使用できます。

## <a name="no-support-for-generic-methods"></a>ジェネリック メソッドはサポートされていません

### <a name="generic-methods-are-not-allowed"></a>ジェネリック メソッドを指定することはできません。

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

**理由**:Xamarin.iOS が型引数を使用する種類を把握していないため、これは`T`Objective C からのメソッドの呼び出し時にします。

別の方法は、特殊なメソッドを作成し、代わりにエクスポートするには。

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

### <a name="no-exported-static-members-allowed"></a>許可されているエクスポートされた静的メンバーを持たない

汎用サブクラス内でホストされている場合に、objective-c の静的メンバーを公開できます`NSObject`します。

サポートされないシナリオの例:

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

**理由:** ジェネリック型引数を使用する種類を把握できる必要があります、Xamarin.iOS ランタイム、ジェネリック メソッドと同様`T`します。

インスタンス自体が使用されるメンバーのインスタンス (インスタンスされませんので`Generic<T>`、常に`Generic<SomeSpecificClass>`) が静的メンバーにこの情報が存在しません。

対象のメンバーが型引数を使用しない場合でも適用されるこのことに注意してください。`T`何らかの方法でします。

特殊なサブクラスを作成する方法はここでは。

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

静的レジストラーは、通常とに、ビルド時のジェネリック型で、エクスポートされたメンバーを解決できません、実行時に検索する必要があります。 つまり、OBJECTIVE-C から、このようなメソッドを呼び出すことは非ジェネリック クラスからメンバーの呼び出しよりもわずかに遅くなります。

