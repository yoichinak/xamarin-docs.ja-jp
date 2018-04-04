---
title: NSObject の汎用サブクラス
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 89df751d74b9b54ae8138d2e1b24c61d82c3cac8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="generic-subclasses-of-nsobject"></a>NSObject の汎用サブクラス

## <a name="using-generics-with-nsobjects"></a>NSObjects にジェネリックを使用します。

サブクラスでジェネリックを使用できる Xamarin.iOS 7.2.1 で始まる`NSObject`(たとえば[UIView](https://developer.xamarin.com/api/type/UIKit.UIView/))。

次のようなジェネリック クラスを作成できます。

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

オブジェクトをそのサブクラス`NSObject`が登録されている Objective C ランタイムでは、いくつかの制限についての一般的なサブクラスで何ができる`NSObject`型です。
    
## <a name="considerations-for-generic-subclasses-of-nsobject"></a>NSObject の汎用サブクラスの考慮事項

このドキュメントの汎用のサブクラスの制限付きサポートの制限事項の詳細を`NSObjects`Xamarin.iOS 7.2.1 で導入されました。
    
### <a name="generic-type-arguments-in-member-signatures"></a>メンバー シグニチャ内のジェネリック型引数

Objective C に公開されるメンバーのシグネチャのジェネリック型引数がすべて必要があります、`NSObject`制約。

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

**理由**: ジェネリック型パラメーターは、`NSObject`ため、セレクターの署名`myMethod:`Objective C に安全に公開することができます (は常に`NSObject`またはそのサブクラス)。

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

**理由**: 署名は、ジェネリック型の正確な型によって異なりますので、Objective C コードを呼び出すことができる、エクスポートされたメンバーを Objective C の署名を作成することはできません`T`です。

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

**理由**: エクスポートされたメンバーのシグネチャの一部を受け取らない限り、ジェネリック型引数を制約が行うことができます。

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

**理由**: `T` Objective C のパラメーターがエクスポート`MyMethod`に強制的に、 `NSObject`、制約のない型`U`シグネチャの一部ではありません。
    
### <a name="instantiations-of-generic-types-from-objective-c"></a>Objective C からのジェネリック型のインスタンス化

Objective C からのジェネリック型のインスタンス化することはできません。 これは通常、マネージ型を xib で使用する場合に発生します。

受け取るコンス トラクターを公開する次のクラス定義を検討してください、 `IntPtr` (ネイティブ OBJECTIVE-C インスタンスから c# オブジェクトを構築するは、Xamarin.iOS 方向)。
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

上記の構造は実行時に、問題ありませんが、OBJECTIVE-C、そのインスタンスを作成しようとする場合に例外がスローされます。

これは Objective C には、ジェネリック型の概念がないと、作成する場合は、正確なジェネリック型を指定できないために発生します。

この問題は、ジェネリック型の特殊なサブクラスを作成することで回避できます。   例えば:
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}

class GenericUIView : Generic<UIView>
{
}
```

これで、あいまいさ今後、このクラスはありません`GenericUIView`xibs で使用できます。

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

**理由**: これは許可されません Xamarin.iOS が型引数を使用する型を把握していないため`T`Objective C からのメソッドが呼び出される場合。

代わりには、特殊なメソッドを作成し、代わりにエクスポートするには。

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

### <a name="no-exported-static-members-allowed"></a>許可されているエクスポートされた静的メンバーはありません。

汎用サブクラス内でホストされている場合、されません Objective C に静的メンバーを公開することができます`NSObject`です。

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

**理由:**ジェネリック メソッドの場合は t ですジェネリック型引数を使用する型を把握できる Xamarin.iOS ランタイム ニーズと同じように。

インスタンス自体が使用されるメンバーのインスタンス (インスタンス ジェネリックされませんので<T>、ジェネリックは常に<SomeSpecificClass>) が静的メンバーにこの情報が存在しません。

任意の方法で問題のメンバーが T 型の引数を使用しない場合でも該当することに注意してください。

代替手段は、ここでは特殊なサブクラスを作成するのには。

```csharp
class GenericUIView : Generic<UIView>
{
    [Export ("myUIViewMethod")]
    public static void MyUIViewMethod ()
    {
        MyMethod ();
    }

    [Export ("myProperty")]
    public static UIView MyUIVIewProperty {
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

### <a name="requires-new-static-registrar"></a>新しい静的レジストラーが必要です。

ジェネリックのサポートが必要です、新しい[登録システム](~/ios/internals/registrar.md)です。

古いを使用しようとする場合は、ジェネリック型 (内の未定義の動作の正しいコードを生成せずに加算) に到達したときに、レガシ登録システムの警告を表示します。
    
## <a name="performance"></a>パフォーマンス

静的レジストラーは、通常に、ビルド時のジェネリック型のエクスポート時のメンバーを解決できない、実行時に検索する必要があります。 つまり、OBJECTIVE-C からこのようなメソッドを呼び出すことは非ジェネリック クラスからのメンバーを呼び出すよりもわずかに遅くなります。

