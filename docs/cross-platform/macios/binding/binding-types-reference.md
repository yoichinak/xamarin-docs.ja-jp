---
title: バインディングの種類のリファレンスガイド
description: このリファレンスガイドでは、C# バインドを目的の C ライブラリに作成するときに理解しておく必要があるさまざまな属性と概念について説明します。
ms.prod: xamarin
ms.assetid: C6618E9D-07FA-4C84-D014-10DAC989E48D
author: davidortinau
ms.author: daortin
ms.date: 03/06/2018
ms.openlocfilehash: 764c6303956199982da779d87b0a29977575e767
ms.sourcegitcommit: 4f0223cf13e14d35c52fa72a026b1c7696bf8929
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2020
ms.locfileid: "93278352"
---
# <a name="binding-types-reference-guide"></a>バインディングの種類のリファレンスガイド

このドキュメントでは、API コントラクトファイルに注釈を付けて、バインディングと生成されたコードを駆動するために使用できる属性の一覧について説明します。

Xamarin と Xamarin の API コントラクトは、ほとんどの場合、c# では、目的の C コードを C# に提示する方法を定義するインターフェイス定義として記述されています。 このプロセスには、インターフェイス宣言と、API コントラクトが必要とする基本的な型定義の組み合わせが含まれます。 バインディングの種類の概要については、「関連ガイド」バインドの「 [目標 C ライブラリ](~/cross-platform/macios/binding/objective-c-libraries.md)」を参照してください。

## <a name="type-definitions"></a>型定義

構文:

```csharp
[BaseType (typeof (BTYPE))
interface MyType : [Protocol1, Protocol2] {
     IntPtr Constructor (string foo);
}
```

属性を持つコントラクト定義内のすべてのインターフェイスは、 [`[BaseType]`](#BaseTypeAttribute) 生成されたオブジェクトの基本型を宣言します。 上記の宣言では、 `MyType` という名前の目的の C 型にバインドするクラス C# 型が生成され `MyType` ます。

インターフェイスの継承構文を使用して、(上のサンプルでは) typename の後に型を指定した場合 `Protocol1` `Protocol2` 、それらのインターフェイスの内容は、のコントラクトの一部としてインライン化され `MyType` ます。
Xamarin の iOS では、型がプロトコルを採用する方法として、プロトコルで宣言されたすべてのメソッドとプロパティを型自体にインライン展開します。

次に、の目的の C の宣言を `UITextField` Xamarin. iOS コントラクトで定義する方法を示します。

```objc
@interface UITextField : UIControl <UITextInput> {

}
```

は、次のように C# API コントラクトとして記述されます。

```csharp
[BaseType (typeof (UIControl))]
interface UITextField : UITextInput {
}
```

他の属性をインターフェイスに適用したり、属性を構成したりすることによって、コード生成の他の多くの側面を制御でき [`[BaseType]`](#BaseTypeAttribute) ます。

### <a name="generating-events"></a>生成 (イベントを)

Xamarin および Xamarin API の設計の1つの機能として、目的の C デリゲートクラスを C# のイベントやコールバックとしてマップすることが挙げられます。 ユーザーは、目的の c プログラミングパターンを採用するかどうかをインスタンス単位で選択できます。これを行うには、目的の `Delegate` c ランタイムが呼び出すさまざまなメソッドを実装するクラスのインスタンスのようなプロパティを割り当てるか、C# スタイルのイベントとプロパティを選択します。

目的の C モデルを使用する方法の一例を見てみましょう。

```csharp
bool MakeDecision ()
{
    return true;
}

void Setup ()
{
     var scrollView = new UIScrollView (myRect);
     scrollView.Delegate = new MyScrollViewDelegate ();
     ...
}

class MyScrollViewDelegate : UIScrollViewDelegate {
    public override void Scrolled (UIScrollView scrollView)
    {
        Console.WriteLine ("Scrolled");
    }

    public override bool ShouldScrollToTop (UIScrollView scrollView)
    {
        return MakeDecision ();
    }
}
```

上の例では、2つのメソッドを上書きすることを選択したことがわかります。これは、スクロールイベントが発生したことを示す通知です。2番目のメソッドは、 `scrollView` トップにスクロールするかどうかを指示するブール値を返すコールバックです。

C# モデルでは、ライブラリのユーザーは C# のイベント構文またはプロパティ構文を使用して通知をリッスンし、値を返すことが想定されているコールバックをフックできます。

これは、同じ機能の C# コードがラムダを使用した場合の動作です。

```csharp
void Setup ()
{
    var scrollview = new UIScrollView (myRect);
    // Event connection, use += and multiple events can be connected
    scrollView.Scrolled += (sender, eventArgs) { Console.WriteLine ("Scrolled"); }

    // Property connection, use = only a single callback can be used
    scrollView.ShouldScrollToTop = (sv) => MakeDecision ();
}
```

イベントは値を返さないため (戻り値の型が void であるため)、複数のコピーを接続できます。 はイベントではなく、次の `ShouldScrollToTop` シグネチャを持つ型を持つプロパティです `UIScrollViewCondition` 。

```csharp
public delegate bool UIScrollViewCondition (UIScrollView scrollView);
```

`bool`この例では、値を返します。この場合、ラムダ構文を使用すると、関数から値を返すことができ `MakeDecision` ます。

バインディングジェネレーターは、のようなクラスをと関連付けて (これらのモデルクラスを呼び出すことができる) イベントとプロパティの生成をサポートし `UIScrollView` `UIScrollViewDelegate` ます。これは、 [`[BaseType]`](#BaseTypeAttribute) パラメーターと `Events` `Delegates` パラメーター (以下で説明) を使用して定義に注釈を付けることによって行われます。 これらのパラメーターを使用してに注釈を付けるだけで [`[BaseType]`](#BaseTypeAttribute) なく、いくつかのコンポーネントをジェネレーターに通知する必要があります。

複数のパラメーターを受け取るイベントの場合 (目的 C では、デリゲートクラスの最初のパラメーターが sender オブジェクトのインスタンスであること)、生成されたクラスに対して使用する名前を指定する必要があり `EventArgs` ます。 これは、 [`[EventArgs]`](#EventArgsAttribute) モデルクラスのメソッド宣言の属性を使用して行います。 次に例を示します。

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

上記の宣言は、 `UIImagePickerImagePickedEventArgs` から派生するクラスを生成 `EventArgs` し、との両方のパラメーターをパックし `UIImage` `NSDictionary` ます。 ジェネレーターは次のものを生成します。

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

その後、クラスで次のものを公開し `UIImagePickerController` ます。

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

値を返すモデルメソッドは、異なる方法でバインドされます。 これには、生成された C# デリゲートの名前 (メソッドのシグネチャ) と、ユーザーが実装を提供しない場合に返す既定値が必要です。 たとえば、 `ShouldScrollToTop` 定義は次のようになります。

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIScrollViewDelegate {
    [Export ("scrollViewShouldScrollToTop:"), DelegateName ("UIScrollViewCondition"), DefaultValue ("true")]
    bool ShouldScrollToTop (UIScrollView scrollView);
}
```

上の例では、 `UIScrollViewCondition` 上に示したシグネチャを持つデリゲートを作成します。ユーザーが実装を提供していない場合、戻り値は true になります。

属性に加え [`[DefaultValue]`](#DefaultValueAttribute) [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute) て、呼び出しで指定されたパラメーターの値を返すようにジェネレーターに指示する属性、または [`[NoDefaultValue]`](#NoDefaultValueAttribute) 既定値がないことをジェネレーターに指示するパラメーターを使用することもできます。

<a name="BaseTypeAttribute"></a>

### <a name="basetypeattribute"></a>BaseTypeAttribute

構文:

```csharp
public class BaseTypeAttribute : Attribute {
        public BaseTypeAttribute (Type t);

        // Properties
        public Type BaseType { get; set; }
        public string Name { get; set; }
        public Type [] Events { get; set; }
        public string [] Delegates { get; set; }
        public string KeepRefUntil { get; set; }
}
```

#### <a name="basetypename"></a>BaseType.Name

`Name`この型が目的の C 言語でバインドされる名前を制御するには、プロパティを使用します。 これは通常、.NET Framework 設計ガイドラインに準拠している名前を C# 型に付与するために使用されますが、この規則に従わない目的で C の名前にマップされます。

たとえば、次の例では、 `NSURLConnection` `NSUrlConnection` .NET Framework 設計ガイドラインでは "url" ではなく "url" を使用しているので、目的の C 型をにマップします。

```csharp
[BaseType (typeof (NSObject), Name="NSURLConnection")]
interface NSUrlConnection {
}
```

指定された名前は、バインディングで生成される属性の値として使用され `[Register]` ます。 `Name`が指定されていない場合、型の短い名前が、生成される出力の属性の値として使用され `[Register]` ます。

#### <a name="basetypeevents-and-basetypedelegates"></a>BaseType. イベントと BaseType

これらのプロパティは、生成されたクラスの C# スタイルのイベントの生成を促進するために使用されます。 これらは、特定のクラスとその目的の C デリゲートクラスをリンクするために使用されます。 クラスがデリゲートクラスを使用して通知とイベントを送信する多くのケースが発生します。 たとえば、には `BarcodeScanner` コンパニオンクラスがあり `BardodeScannerDelegate` ます。 クラスには、 `BarcodeScanner` 通常、 `Delegate` のインスタンスを割り当てるプロパティがあります。 `BarcodeScannerDelegate` これは、C# に似たスタイルのイベントインターフェイスをユーザーに公開し、その場合は `Events` `Delegates` 属性のプロパティとプロパティを使用 [`[BaseType]`](#BaseTypeAttribute) することになります。

これらのプロパティは常に一緒に設定され、同じ数の要素を持ち、同期されたままにする必要があります。配列には `Delegates` 、ラップする弱い型指定のデリゲートごとに1つの文字列が含まれ `Events` ます。配列には、関連付ける型ごとに1つの型が含まれています。

```csharp
[BaseType (typeof (NSObject),
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIAccelerometerDelegate)})]
public interface UIAccelerometer {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIAccelerometerDelegate {
}
```

#### <a name="basetypekeeprefuntil"></a>KeepRefUntil

このクラスの新しいインスタンスが作成されたときにこの属性を適用すると、そのオブジェクトのインスタンスは、によって参照されるメソッドが呼び出されるまで保持され `KeepRefUntil` ます。 これは、ユーザーがコードを使用するためにオブジェクトへの参照を保持する必要がない場合に、Api の使いやすさを向上させるために役立ちます。 このプロパティの値は、クラス内のメソッドの名前である `Delegate` ため、プロパティとプロパティの組み合わせでも使用する必要があり `Events` `Delegates` ます。

次の例では、Xamarin のでを使用する方法を示します `UIActionSheet` 。

```csharp
[BaseType (typeof (NSObject), KeepRefUntil="Dismissed")]
[BaseType (typeof (UIView),
           KeepRefUntil="Dismissed",
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIActionSheetDelegate)})]
public interface UIActionSheet {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIActionSheetDelegate {
    [Export ("actionSheet:didDismissWithButtonIndex:"), EventArgs ("UIButton")]
    void Dismissed (UIActionSheet actionSheet, nint buttonIndex);
}
```

<a name="DesignatedDefaultCtorAttribute"></a>

### <a name="designateddefaultctorattribute"></a>DesignatedDefaultCtorAttribute

この属性がインターフェイス定義に適用されると、既定の `[DesignatedInitializer]` (生成される) コンストラクターに属性が生成され、セレクターにマップされ `init` ます。

<a name="DisableDefaultCtorAttribute"></a>

### <a name="disabledefaultctorattribute"></a>DisableDefaultCtorAttribute

この属性がインターフェイス定義に適用されると、ジェネレーターが既定のコンストラクターを生成できなくなります。

クラスの他のコンストラクターの1つを使用してオブジェクトを初期化する必要がある場合は、この属性を使用します。

<a name="PrivateDefaultCtorAttribute"></a>

### <a name="privatedefaultctorattribute"></a>PrivateDefaultCtorAttribute

この属性がインターフェイス定義に適用されると、既定のコンストラクターにプライベートとしてフラグが設定されます。 これは、このクラスのオブジェクトを拡張ファイルから内部的にインスタンス化できることを意味しますが、クラスのユーザーがアクセスすることはできません。

<a name="CategoryAttribute"></a>

### <a name="categoryattribute"></a>カテゴリの属性

この属性を型定義で使用して、目的の C カテゴリをバインドし、それらを C# の拡張メソッドとして公開し、目的の C が機能を公開する方法を反映させることができます。

カテゴリは、クラスで使用可能なメソッドとプロパティのセットを拡張するために使用される、目的の C の機構です。   実際には、特定のフレームワークがリンクされている場合 (たとえば)、 `NSObject` 新しいフレームワークがにリンクされている場合にのみ、基本クラスの機能を拡張する (たとえば) ために使用され `UIKit` ます。   場合によっては、機能によってクラスの特徴を整理するために使用されます。   これらは、精神と C# の拡張メソッドに似ています。

このカテゴリは、次のようになります。

```objc
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

上の例は、メソッドを使用してのインスタンスを拡張するライブラリにあり `UIView` `makeBackgroundRed` ます。

これらをバインドするには、 [`[Category]`](#CategoryAttribute) インターフェイス定義で属性を使用します。   属性を使用すると [`[Category]`](#CategoryAttribute) 、属性の意味が、拡張 [`[BaseType]`](#BaseTypeAttribute) する基底クラスを指定するために使用されるように変更されます。

`UIView`拡張機能がどのようにバインドされ、C# 拡張メソッドに変換されるかを次に示します。

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

上の例では、 `MyUIViewExtension` 拡張メソッドを含むクラスを作成し `MakeBackgroundRed` ます。   これは、任意のサブクラスでを呼び出すことができるようになったことを意味します。これにより、 `MakeBackgroundRed` `UIView` 目的の C でも同じ機能が得られます。

場合によっては、次の例のように、 **静的** メンバーがカテゴリ内に存在することがあります。

```objc
@interface FooObject (MyFooObjectExtension)
+ (BOOL)boolMethod:(NSRange *)range;
@end
```

これにより、 **正しくない** カテゴリの C# インターフェイス定義が発生します。

```csharp
[Category]
[BaseType (typeof (FooObject))]
interface FooObject_Extensions {

    // Incorrect Interface definition
    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

これは正しくありません。のインスタンスが必要な拡張機能を使用していて `BoolMethod` `FooObject` も、objc **静的** 拡張をバインドしているためです。これは、C# の拡張メソッドの実装方法の事実による副作用になります。

上記の定義を使用する唯一の方法は、次のような厄介なコードです。

```csharp
(null as FooObject).BoolMethod (range);
```

これを回避するための推奨事項は、インターフェイス定義の内部で定義をインライン化することです `BoolMethod` `FooObject` 。これにより、意図したようにこの拡張機能を呼び出すことができ `FooObject.BoolMethod (range)` ます。

```csharp
[BaseType (typeof (NSObject))]
interface FooObject {

    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

定義内でメンバーが見つかった場合は常に、警告 (BI1117) を発行 [`[Static]`](#StaticAttribute) [`[Category]`](#CategoryAttribute) します。 実際に定義内にメンバーを作成する場合は、 [`[Static]`](#StaticAttribute) [`[Category]`](#CategoryAttribute) またはを使用して警告を無音にし、 `[Category (allowStaticMembers: true)]` メンバーまたはインターフェイスの定義をで修飾することができ [`[Category]`](#CategoryAttribute) [`[Internal]`](#InternalAttribute) ます。

<a name="StaticAttribute_Class"></a>

### <a name="staticattribute"></a>StaticAttribute

この属性がクラスに適用されると、静的クラス (から派生したものではない) が生成され `NSObject` ます。そのため、 [`[BaseType]`](#BaseTypeAttribute) 属性は無視されます。 静的クラスは、公開する C パブリック変数をホストするために使用されます。

次に例を示します。

```csharp
[Static]
interface CBAdvertisement {
    [Field ("CBAdvertisementDataServiceUUIDsKey")]
    NSString DataServiceUUIDsKey { get; }
```

では、次の API を使用して C# クラスが生成されます。

```csharp
public partial class CBAdvertisement  {
    public static NSString DataServiceUUIDsKey { get; }
}
```

## <a name="protocolmodel-definitions"></a>プロトコル/モデルの定義

モデルは、通常、プロトコルの実装によって使用されます。
これは、ランタイムが、実際に上書きされたメソッドのみを目的の C に登録する点が異なります。
それ以外の場合、メソッドは登録されません。

これは一般に、でフラグが設定されたクラスをサブクラス化する場合 `ModelAttribute` 、基本メソッドを呼び出さないことを意味します。   このメソッドを呼び出すと、次の例外がスローされます: Foundation.You_Should_Not_Call_base_In_This_Method。 オーバーライドするすべてのメソッドに対して、サブクラスに動作全体を実装することになります。

<a name="AbstractAttribute"></a>

### <a name="abstractattribute"></a>AbstractAttribute

既定では、プロトコルの一部であるメンバーは必須ではありません。 これにより、C# のクラスから派生させ、注意するメソッドだけをオーバーライドするだけで、オブジェクトのサブクラスを作成でき `Model` ます。 場合によっては、ユーザーがこのメソッドの実装を提供することが必要になることがあります (これには、ターゲット-C のディレクティブでフラグが付けられ `@required` ます)。 そのような場合は、属性を使用してこれらのメソッドにフラグを付ける必要があり `[Abstract]` ます。

属性は、 `[Abstract]` メソッドまたはプロパティのいずれかに適用でき、生成されたメンバーに abstract としてフラグを設定し、クラスを抽象クラスにします。

Xamarin からは次のものが作成されます。

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UITableViewDataSource {
    [Export ("tableView:numberOfRowsInSection:")]
    [Abstract]
    nint RowsInSection (UITableView tableView, nint section);
}
```

<a name="DefaultValueAttribute"></a>

### <a name="defaultvalueattribute"></a>DefaultValueAttribute

モデルオブジェクトでこの特定のメソッドに対してユーザーがメソッドを提供しない場合に、モデルメソッドによって返される既定値を指定します。

構文:

```csharp
public class DefaultValueAttribute : Attribute {
        public DefaultValueAttribute (object o);
        public object Default { get; set; }
}
```

たとえば、クラスの次の虚数デリゲートクラスで `Camera` は、 `ShouldUploadToServer` クラスのプロパティとして公開されるを提供し `Camera` ます。 クラスのユーザーが、 `Camera` true または false に応答できるラムダに値を明示的に設定していない場合、この場合の既定値は false になり、属性に指定した値が返され `DefaultValue` ます。

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("camera:shouldPromptForAction:"), DefaultValue (false)]
    bool ShouldUploadToServer (Camera camera, CameraAction action);
}
```

ユーザーが虚数クラスにハンドラーを設定した場合、この値は無視されます。

```csharp
var camera = new Camera ();
camera.ShouldUploadToServer = (camera, action) => return SomeDecision ();
```

関連項目: 「」、「」 [`[NoDefaultValue]`](#NoDefaultValueAttribute) [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)

<a name="DefaultValueFromArgumentAttribute"></a>

### <a name="defaultvaluefromargumentattribute"></a>DefaultValueFromArgumentAttribute

構文:

```csharp
public class DefaultValueFromArgumentAttribute : Attribute {
    public DefaultValueFromArgumentAttribute (string argument);
    public string Argument { get; }
}
```

この属性は、モデルクラスの値を返すメソッドに指定すると、ユーザーが独自のメソッドまたはラムダを指定しなかった場合に、指定したパラメーターの値を返すようにジェネレーターに指示します。

例:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, nfloat progress);
}
```

上記の例では、クラスのユーザーが `NSAnimation` C# のイベント/プロパティを使用することを選択し、 `NSAnimation.ComputeAnimationCurve` メソッドまたはラムダに設定しなかった場合、戻り値は progress パラメーターに渡された値になります。

関連項目: 「」、「」 [`[NoDefaultValue]`](#NoDefaultValueAttribute)[`[DefaultValue]`](#DefaultValueAttribute)

### <a name="ignoredindelegateattribute"></a>IgnoredInDelegateAttribute

場合によっては、イベントまたはデリゲートプロパティをモデルクラスからホストクラスに公開しないことが適切な場合があります。この属性を追加すると、そのメソッドで修飾されたメソッドの生成を回避するようジェネレーターに指示されます。

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);

    [Export ("imagePickerController:didFinishPickingImage:"), IgnoredInDelegate)] // No event generated for this method
    void FinishedPickingImage (UIImagePickerController picker, UIImage image);
}
```

### <a name="delegatenameattribute"></a>DelegateNameAttribute

この属性は、使用するデリゲートシグネチャの名前を設定する値を返すモデルメソッドで使用されます。

例:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, float progress);
}
```

上記の定義では、ジェネレーターによって次のパブリック宣言が生成されます。

```csharp
public delegate float NSAnimationProgress (MonoMac.AppKit.NSAnimation animation, float progress);
```

### <a name="delegateapinameattribute"></a>DelegateApiNameAttribute

この属性は、ホストクラスで生成されたプロパティの名前をジェネレーターが変更できるようにするために使用されます。 場合によっては、FooDelegate クラスのメソッドの名前が Delegate クラスにとって理にかなっていても、ホストクラスではプロパティとして奇妙に見えることがあります。

また、これは、2つ以上のオーバーロードメソッドがあり、それらを FooDelegate クラスのままにしておき、より適切な名前でホストクラスに公開する必要がある場合にも、非常に便利です (および必要です)。

例:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateApiName ("ComputeAnimationCurve"), DelegateName ("Func<NSAnimation, float, float>"), DefaultValueFromArgument ("progress")]
    float GetValueForProgress (NSAnimation animation, float progress);
}
```

上記の定義では、ジェネレーターは host クラスで次のパブリック宣言を生成します。

```csharp
public Func<NSAnimation, float, float> ComputeAnimationCurve { get; set; }
```

<a name="EventArgsAttribute"></a>

### <a name="eventargsattribute"></a>EventArgsAttribute

複数のパラメーターを受け取るイベントの場合 (目的 C では、デリゲートクラスの最初のパラメーターが sender オブジェクトのインスタンスであること)、生成された EventArgs クラスの名前をに指定する必要があります。 これは、クラスのメソッド宣言の属性を使用して行い `[EventArgs]` `Model` ます。

次に例を示します。

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

上記の宣言は、 `UIImagePickerImagePickedEventArgs` EventArgs から派生したクラスを生成し、との両方のパラメーターをパックし `UIImage` `NSDictionary` ます。 ジェネレーターは次のものを生成します。

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

その後、クラスで次のものを公開し `UIImagePickerController` ます。

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

### <a name="eventnameattribute"></a>EventNameAttribute

この属性は、クラスで生成されたイベントまたはプロパティの名前をジェネレーターが変更できるようにするために使用されます。 モデルクラスのメソッドの名前がモデルクラスにとって意味があるものの、元のクラスではイベントまたはプロパティとして奇妙に見える場合に役立つことがあります。

たとえば、は、 `UIWebView` の次のビットを使用し `UIWebViewDelegate` ます。

```csharp
[Export ("webViewDidFinishLoad:"), EventArgs ("UIWebView"), EventName ("LoadFinished")]
void LoadingFinished (UIWebView webView);
```

上のは、 `LoadingFinished` のメソッドとしてを公開し `UIWebViewDelegate` ますが、に `LoadFinished` フックするイベントとしてを公開し `UIWebView` ます。

```csharp
var webView = new UIWebView (...);
webView.LoadFinished += delegate { Console.WriteLine ("done!"); }
```

<a name="ModelAttribute"></a>

### <a name="modelattribute"></a>ModelAttribute

`[Model]`コントラクト API の型定義に属性を適用すると、ユーザーがクラスのメソッドを上書きした場合に、クラスのメソッドへの呼び出しのみを実行する特殊なコードがランタイムによって生成されます。 この属性は、通常、目的の C デリゲートクラスをラップするすべての Api に適用されます。

<a name="NoDefaultValueAttribute"></a>

### <a name="nodefaultvalueattribute"></a>NoDefaultValueAttribute

モデルのメソッドが既定の戻り値を提供しないことを指定します。

これは、目的の c ランタイム要求に応答して、 `false` 指定されたセレクターがこのクラスに実装されているかどうかを判断することで、目的の c ランタイムで動作します。

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("shouldDisplayPopup"), NoDefaultValue]
    bool ShouldUploadToServer ();
}
```

関連項目: 「」、「」 [`[DefaultValue]`](#DefaultValueAttribute)[`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)  

<a name="ProtocolAttribute"></a>

## <a name="protocols"></a>プロトコル

C# では、C プロトコルの概念は実際には存在しません。 プロトコルは C# インターフェイスに似ていますが、プロトコルで宣言されているすべてのメソッドとプロパティが、それを採用するクラスによって実装されている必要はありません。 代わりに、一部のメソッドとプロパティは省略可能です。

一部のプロトコルは通常、モデルクラスとして使用されます。これらは属性を使用してバインドする必要があり [`[Model]`](#ModelAttribute) ます。

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}
```

Xamarin 以降では、新たに強化されたプロトコルバインド機能が組み込まれてい7.0 ます。  属性を含む定義で `[Protocol]` は、次の3つのサポートクラスが生成され、プロトコルの使用方法が大幅に向上します。

```csharp
// Full method implementation, contains all methods
class MyProtocol : IMyProtocol {
    public void Say (string msg);
    public void Listen (string msg);
}

// Interface that contains only the required methods
interface IMyProtocol: INativeObject, IDisposable {
    [Export ("say:")]
    void Say (string msg);
}

// Extension methods
static class IMyProtocol_Extensions {
    public static void Optional (this IMyProtocol this, string msg);
    }
}
```

**クラス実装** は、の個々のメソッドをオーバーライドし、完全なタイプセーフを取得できる、完全な抽象クラスを提供します。 ただし、C# では複数の継承がサポートされていないため、異なる基本クラスが必要になる場合がありますが、インターフェイスを実装することもできます。

ここでは、生成された **インターフェイス定義** が含まれています。  これは、プロトコルから必要なすべてのメソッドを含むインターフェイスです。  これにより、開発者は、インターフェイスを実装するだけで、プロトコルを実装することができます。  ランタイムは、プロトコルを採用するときに型を自動的に登録します。

インターフェイスには、必要なメソッドが一覧表示されるだけで、オプションのメソッドが公開されることに注意してください。   これは、プロトコルを採用するクラスが必要なメソッドの完全な型チェックを取得することを意味しますが、省略可能なプロトコルメソッドに対しては、弱い型指定 (エクスポート属性を使用して手動で署名を使用すること) を行う必要があります。

プロトコルを使用する API を簡単に使用できるようにするために、バインディングツールは、すべての省略可能なメソッドを公開する拡張メソッドクラスも生成します。   これは、API を使用している限り、すべてのメソッドを含むプロトコルとして扱うことができることを意味します。

API でプロトコル定義を使用する場合は、API 定義にスケルトン空のインターフェイスを記述する必要があります。   API で MyProtocol を使用する場合は、次の手順を実行する必要があります。

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}

interface IMyProtocol {}

[BaseType (typeof(NSObject))]
interface MyTool {
    [Export ("getProtocol")]
    IMyProtocol GetProtocol ();
}
```

のバインド時には、が存在しないため、空のインターフェイスを提供する必要があるため、上記の手順が必要になり `IMyProtocol` ます。

### <a name="adopting-protocol-generated-interfaces"></a>プロトコルによって生成されたインターフェイスの導入

プロトコル用に生成されたインターフェイスの1つを実装する場合は、次のようになります。

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

必須のインターフェイスメソッドの実装は、適切な名前を使用してエクスポートされるため、次のようになります。

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

これは必要なすべてのプロトコルメンバーに対して機能しますが、オプションのセレクターを認識する特別なケースがあります。

基本クラスを使用する場合、オプションのプロトコルメンバーは同じように扱われます。

```
public class UrlSessionDelegate : NSUrlSessionDownloadDelegate {
    public override void DidWriteData (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, long bytesWritten, long totalBytesWritten, long totalBytesExpectedToWrite)
```

ただし、プロトコルインターフェイスを使用する場合は、[Export] を追加する必要があります。 上書きを使用して追加すると、IDE によってオートコンプリートによって追加されます。 

```
public class UrlSessionDelegate : NSObject, INSUrlSessionDownloadDelegate {
    [Export ("URLSession:downloadTask:didWriteData:totalBytesWritten:totalBytesExpectedToWrite:")]
    public void DidWriteData (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, long bytesWritten, long totalBytesWritten, long totalBytesExpectedToWrite)
```

実行時の2つの動作の違いはわずかです。

- 基本クラスのユーザー (Nの Lsessiondownloaddelegate など) では、必須のセレクターとオプションのセレクターがすべて提供され、妥当な既定値が返されます。
- インターフェイスのユーザー (INSUrlSessionDownloadDelegate など) は、指定されたセレクターにのみ応答します。

いくつかのまれなクラスは、ここでは動作が異なる場合があります。 ただし、ほとんどの場合は、どちらも安全に使用できます。

### <a name="protocol-inlining"></a>プロトコルインライン展開

プロトコルの導入として宣言されている既存の目標 C 型をバインドするときに、プロトコルを直接インライン化する必要があります。 これを行うには、単に属性を指定せずにインターフェイスとしてプロトコルを宣言 [`[BaseType]`](#BaseTypeAttribute) し、インターフェイスの基本インターフェイスの一覧にプロトコルを列挙します。

例:

```csharp
interface SpeakProtocol {
    [Export ("say:")]
    void Say (string msg);
}

[BaseType (typeof (NSObject))]
interface Robot : SpeakProtocol {
    [Export ("awake")]
    bool Awake { get; set; }
}
```

## <a name="member-definitions"></a>メンバーの定義

このセクションの属性は、型の個々のメンバー (プロパティとメソッド宣言) に適用されます。

### <a name="alignattribute"></a>AlignAttribute

プロパティの戻り値の型のアラインメント値を指定するために使用します。 特定のプロパティでは、特定の境界に沿って配置する必要があるアドレスへのポインターを取得します (Xamarin では、16バイトでアラインする必要があるいくつかのプロパティを使用する場合などに発生 `GLKBaseEffect` します)。 このプロパティを使用して getter を装飾し、alignment 値を使用できます。 これは、通常、および型と共に使用して、 `OpenTK.Vector4` `OpenTK.Matrix4` 目的の C api と統合する場合に使用します。

例:

```csharp
public interface GLKBaseEffect {
    [Export ("constantColor")]
    Vector4 ConstantColor { [Align (16)] get; set;  }
}
```

### <a name="appearanceattribute"></a>外観属性

属性は、 `[Appearance]` 外観マネージャーが導入された iOS 5 に限定されています。

この `[Appearance]` 属性は、フレームワークに参加する任意のメソッドまたはプロパティに適用でき `UIAppearance` ます。 この属性がクラスのメソッドまたはプロパティに適用されると、このクラスのすべてのインスタンスのスタイルを設定するために使用される厳密に型指定された外観クラス、または特定の条件に一致するインスタンスを作成するために、バインディングジェネレーターによって指示されます。

例:

```csharp
public interface UIToolbar {
    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);
}
```

上記の例では、UIToolbar に次のコードが生成されます。

```csharp
public partial class UIToolbar {
    public partial class UIToolbarAppearance : UIView.UIViewAppearance {
        public virtual void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);
        public virtual UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics)
    }
    public static new UIToolbarAppearance Appearance { get; }
    public static new UIToolbarAppearance AppearanceWhenContainedIn (params Type [] containers);
}
```

### <a name="autoreleaseattribute-xamarinios-54"></a>AutoReleaseAttribute (Xamarin 5.4)

メソッドとプロパティを使用して、 `[AutoReleaseAttribute]` メソッドの呼び出しを内のメソッドにラップし `NSAutoReleasePool` ます。

目的 C では、既定値に追加される値を返すメソッドがいくつかあり `NSAutoReleasePool` ます。 既定では、これらはスレッドに移動し `NSAutoReleasePool` ますが、Xamarin. iOS は、マネージオブジェクトが存在する限り、オブジェクトへの参照を保持するため、 `NSAutoReleasePool` スレッドが次のスレッドに制御を返すか、メインループに戻るまで、ドレインされないように追加の参照を保持しないようにすることもできます。

この属性は `UIImage.FromFile` 、既定値に追加されたオブジェクトを返す、高負荷なプロパティ (など) に適用され `NSAutoReleasePool` ます。 この属性がない場合、スレッドがメインループに制御を返さない限り、画像は保持されます。 Uf スレッドが、常に生きていて作業を待機しているバックグラウンドダウンローダーの一種であった場合、イメージは解放されません。

### <a name="forcedtypeattribute"></a>ForcedTypeAttribute

`[ForcedTypeAttribute]`返されるアンマネージオブジェクトがバインディング定義で記述されている型と一致しない場合でも、は、マネージ型の作成を強制するために使用されます。

これは、ヘッダーに記述されている型がネイティブメソッドの戻り値の型と一致しない場合に便利です。たとえば、から次のような C の定義を取得し `NSURLSession` ます。

`- (NSURLSessionDownloadTask *)downloadTaskWithRequest:(NSURLRequest *)request`

これは、インスタンスを返すことを明確に示してい `NSURLSessionDownloadTask` ますが、が **返さ** れます。これは、 `NSURLSessionTask` スーパークラスであり、に変換できないことを示し `NSURLSessionDownloadTask` ます。 これはタイプセーフなコンテキストであるため、が `InvalidCastException` 発生します。

ヘッダーの説明に従ってを回避するには、を `InvalidCastException` `[ForcedTypeAttribute]` 使用します。

```csharp
[BaseType (typeof (NSObject), Name="NSURLSession")]
interface NSUrlSession {

    [Export ("downloadTaskWithRequest:")]
    [return: ForcedType]
    NSUrlSessionDownloadTask CreateDownloadTask (NSUrlRequest request);
}
```

は、 `[ForcedTypeAttribute]` 既定ではという名前のブール値も受け入れ `Owns` `false` `[ForcedType (owns: true)]` ます。 所有しているパラメーターは、 **Core Foundation** オブジェクトの [所有権ポリシー](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html)に従うために使用されます。

は、 `[ForcedTypeAttribute]` パラメーター、プロパティ、および戻り値でのみ有効です。

<a name="BindAsAttribute"></a>

### <a name="bindasattribute"></a>BindAsAttribute

では、 `[BindAsAttribute]` `NSNumber` `NSValue` より正確な C# 型へのバインドと (列挙) が許可されて `NSString` います。 属性を使用すると、ネイティブ API でより正確で正確な .NET API を作成できます。

メソッド (戻り値)、パラメーター、およびプロパティは、を使用して装飾でき `BindAs` ます。 唯一の制限は、メンバーがインターフェイスまたはインターフェイスの内側に配置さ **れていない** ことです `[Protocol]` [`[Model]`](#ModelAttribute) 。

次に例を示します。

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

出力:

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

内部的には、との変換を行い `bool?`  <->  `NSNumber` `CGRect`  <->  `NSValue` ます。

現在サポートされているカプセル化の種類は次のとおりです。

* `NSValue`
* `NSNumber`
* `NSString`

#### <a name="nsvalue"></a>NSValue

次の C# のデータ型は、との間でカプセル化することがサポートされてい `NSValue` ます。

* Cgの Inetransform 変換
* NSRange
* CGVector
* SCNMatrix4
* CLLocationCoordinate2D
* SCNVector3
* SCNVector4
* CGPoint/PointF
* CGRect/RectangleF
* CGSize/SizeF
* UIEdgeInsets
* UIOffset
* MKCoordinateSpan
* CMTimeRange
* CMTime
* CMTimeMapping
* CATransform3D

#### <a name="nsnumber"></a>NSNumber

次の C# のデータ型は、との間でカプセル化することがサポートされてい `NSNumber` ます。

* [bool]
* byte
* double
* float
* short
* INT
* long
* sbyte
* ushort
* uint
* ulong
* nfloat
* nint
* nuint
* 列挙型

#### <a name="nsstring"></a>NSString

[`[BindAs]`](#BindAsAttribute)[NSString 定数によって列挙](#enum-attributes)された conjuntion で機能します。これにより、次のように、より優れた .net API を作成できます。

```csharp
[BindAs (typeof (CAScroll))]
[Export ("supportedScrollMode")]
NSString SupportedScrollMode { get; set; }
```

出力:

```csharp
[Export ("supportedScrollMode")]
CAScroll SupportedScrollMode { get; set; }
```

変換は、 `enum`  <->  `NSString` 指定された列挙型 [`[BindAs]`](#BindAsAttribute) が[nsstring 定数によってサポート](#enum-attributes)されている場合にのみ処理されます。

#### <a name="arrays"></a>配列

[`[BindAs]`](#BindAsAttribute) では、サポートされる任意の型の配列もサポートされています。例として、次の API 定義を使用できます。

```csharp
[return: BindAs (typeof (CAScroll []))]
[Export ("getScrollModesAt:")]
NSString [] GetScrollModes ([BindAs (typeof (CGRect []))] NSValue [] rects);
```

出力:

```csharp
[Export ("getScrollModesAt:")]
CAScroll? [] GetScrollModes (CGRect [] rects) { ... }
```

パラメーターは、それぞれのを含むにカプセル化され、返されたの `rects` `NSArray` 値を `NSValue` `CGRect` `CAScroll?` 使用して作成されたの配列を取得し `NSArray` `NSStrings` ます。

<a name="BindAttribute"></a>

### <a name="bindattribute"></a>BindAttribute

この属性には、 `[Bind]` メソッドまたはプロパティの宣言に適用されるときに2つのが使用されます。また、プロパティの getter または setter に個別に適用される場合もあります。

メソッドまたはプロパティに対して使用する場合、属性の効果 `[Bind]` は、指定されたセレクターを呼び出すメソッドを生成することです。 ただし、生成されたメソッドは属性で修飾されていません。これは、 [`[Export]`](#ExportAttribute) メソッドのオーバーライドに参加できないことを意味します。 これは通常、 `[Target]` 目的の C 拡張メソッドを実装するための属性と組み合わせて使用されます。

次に例を示します。

```csharp
public interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

Getter または setter で使用した場合、 `[Bind]` 属性を使用して、プロパティの getter および Setter 目標-C セレクターの名前を生成するときに、コードジェネレーターによって推論される既定値を変更します。 既定では、という名前のプロパティにフラグを設定すると、ジェネレーターによって `fooBar` getter と setter のエクスポートが生成され `fooBar` `setFooBar:` ます。 場合によっては、目的の C がこの規則に従わないことがあります。通常は、getter 名をに変更 `isFooBar` します。
この属性を使用して、こののジェネレーターを通知します。

次に例を示します。

```csharp
// Default behavior
[Export ("active")]
bool Active { get; set; }

// Custom naming with the Bind attribute
[Export ("visible")]
bool Visible { [Bind ("isVisible")] get; set; }
```

<a name="AsyncAttribute"></a>

### <a name="asyncattribute"></a>AsyncAttribute

Xamarin. iOS 6.3 以降でのみ使用できます。

この属性は、最後の引数として完了ハンドラーを受け取るメソッドに適用できます。

属性は、 `[Async]` 最後の引数がコールバックであるメソッドで使用できます。  メソッドにこのを適用すると、バインドジェネレーターは、サフィックスを使用して、そのメソッドのバージョンを生成し `Async` ます。  コールバックがパラメーターを受け取らない場合、戻り値はになり `Task` ます。コールバックがパラメーターを受け取ると、結果はになり `Task<T>` ます。

```csharp
[Export ("upload:complete:")]
[Async]
void LoadFile (string file, NSAction complete)
```

次の例では、この非同期メソッドが生成されます。

```csharp
Task LoadFileAsync (string file);
```

コールバックが複数のパラメーターを受け取る場合は、またはを設定して、すべてのプロパティを保持する、 `ResultType` `ResultTypeName` 生成される型の目的の名前を指定する必要があります。

```csharp
delegate void OnComplete (string [] files, nint byteCount);

[Export ("upload:complete:")]
[Async (ResultTypeName="FileLoading")]
void LoadFiles (string file, OnComplete complete)
```

次の例では、この非同期メソッドが生成されます `FileLoading` 。には、との両方にアクセスするプロパティが含まれてい `files` `byteCount` ます。

```csharp
Task<FileLoading> LoadFile (string file);
```

コールバックの最後のパラメーターがの場合、生成されたメソッドは、 `NSError` `Async` 値が null でないかどうかを確認します。その場合、生成された非同期メソッドはタスクの例外を設定します。

```csharp
[Export ("upload:onComplete:")]
[Async]
void Upload (string file, Action<string,NSError> onComplete);
```

上の例では、次の非同期メソッドが生成されます。

```csharp
Task<string> UploadAsync (string file);
```

また、エラーが発生した場合、結果として得られるをラップするに例外が設定され `NSErrorException` `NSError` ます。

#### <a name="asyncattributeresulttype"></a>AsyncAttribute。 ResultType

このプロパティを使用して、返されるオブジェクトの値を指定し `Task` ます。   このパラメーターは既存の型を受け取ります。そのため、コア api 定義の1つで定義する必要があります。

#### <a name="asyncattributeresulttypename"></a>AsyncAttribute. ResultTypeName

このプロパティを使用して、返されるオブジェクトの値を指定し `Task` ます。   このパラメーターは、目的の型名の名前を受け取ります。ジェネレーターは、コールバックによって実行されるパラメーターごとに1つずつ、一連のプロパティを生成します。

#### <a name="asyncattributemethodname"></a>AsyncAttribute. MethodName

このプロパティを使用して、生成される非同期メソッドの名前をカスタマイズします。   既定では、メソッドの名前を使用して "Async" というテキストを追加します。これを使用すると、この既定値を変更できます。

<a name="DesignatedInitializerAttribute"></a>

### <a name="designatedinitializerattribute"></a>Designated初期化 Erattribute

この属性がコンストラクターに適用されると、最終的なプラットフォームアセンブリでも同じものが生成され `[DesignatedInitializer]` ます。 これは、IDE がサブクラスでどのコンストラクターを使用する必要があるかを示すのに役立ちます。

これは、の目的の C/clang の使用にマップされる必要があり `__attribute__((objc_designated_initializer))` ます。

<a name="DisableZeroCopyAttribute"></a>

### <a name="disablezerocopyattribute"></a>Disableゼロ Copyattribute

この属性は、文字列パラメーターまたは文字列プロパティに適用され、このパラメーターに対してゼロコピー文字列のマーシャリングを使用しないようにコードジェネレーターに指示します。代わりに、C# 文字列から新しい NSString インスタンスを作成します。
この属性は、コマンドラインオプションを使用して、 `--zero-copy` またはアセンブリレベルの属性を設定して、ゼロコピーの文字列のマーシャリングを使用するようジェネレーターに指示する場合にのみ、文字列に必要です `ZeroCopyStringsAttribute` 。

これは、プロパティが `retain` プロパティではなくプロパティまたはプロパティであることを目的とする場合に必要です `assign` `copy` 。 これらは通常、開発者が誤って "最適化" したサードパーティのライブラリで発生します。 一般に、または `retain` `assign` `NSString` `NSMutableString` のユーザー派生クラスは `NSString` ライブラリコードを知らずに文字列の内容を変更する可能性があるため、またはプロパティは正しくありません。 通常、これは、最適化が早すぎることが原因で発生します。

次に、このような2つのプロパティを示します。

```csharp
@property(nonatomic,retain) NSString *name;
@property(nonatomic,assign) NSString *name2;
```

<a name="DisposeAttribute"></a>

### <a name="disposeattribute"></a>DisposeAttribute

をクラスに適用する場合は、 `[DisposeAttribute]` クラスのメソッド実装に追加されるコードスニペットを指定し `Dispose()` ます。

メソッドは `Dispose` ツールとツールによって自動的に生成されるため、属性を使用して、 `bmac-native` `btouch-native` 生成されたメソッドの実装にコードを挿入する必要があり `[Dispose]` `Dispose` ます。

次に例を示します。

```csharp
[BaseType (typeof (NSObject))]
[Dispose ("if (OpenConnections > 0) CloseAllConnections ();")]
interface DatabaseConnection {
}
```

<a name="ExportAttribute"></a>

### <a name="exportattribute"></a>ExportAttribute

属性は、 `[Export]` 目的の C ランタイムに公開されるメソッドまたはプロパティにフラグを設定するために使用されます。 この属性は、バインドツールと、実際の Xamarin、iOS、および Xamarin. Mac ランタイム間で共有されます。 メソッドの場合、パラメーターは生成されたコードに逐語的に渡されます。プロパティの場合は、基本宣言に基づいて getter および setter のエクスポートが生成され [`[BindAttribute]`](#BindAttribute) ます (バインディングツールの動作を変更する方法の詳細については、「」のセクションを参照してください)。

構文:

```csharp
public enum ArgumentSemantic {
    None, Assign, Copy, Retain.
}

[AttributeUsage (AttributeTargets.Method | AttributeTargets.Constructor | AttributeTargets.Property)]
public class ExportAttribute : Attribute {
    public ExportAttribute();
    public ExportAttribute (string selector);
    public ExportAttribute (string selector, ArgumentSemantic semantic);
    public string Selector { get; set; }
    public ArgumentSemantic ArgumentSemantic { get; set; }
}
```

[セレクター](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Selector.html)は、バインドされている、基になる目標 C メソッドまたはプロパティの名前を表します。

#### <a name="exportattributeargumentsemantic"></a>ExportAttribute. ArgumentSemantic

<a name="FieldAttribute"></a>

### <a name="fieldattribute"></a>FieldAttribute

この属性は、必要に応じて読み込まれ、C# コードに公開されるフィールドとして C グローバル変数を公開するために使用されます。 通常、これは、C または目的 C で定義されている定数の値を取得するために必要です。また、一部の Api で使用されるトークンであるか、または値が非透過的であり、ユーザーコードによってそのように使用される必要があります。

構文:

```csharp
public class FieldAttribute : Attribute {
    public FieldAttribute (string symbolName);
    public FieldAttribute (string symbolName, string libraryName);
    public string SymbolName { get; set; }
    public string LibraryName { get; set; }
}
```

`symbolName`は、リンク先の C 記号です。 既定では、このは、型が定義されている名前空間から推論される名前を持つライブラリから読み込まれます。 シンボルが検索されるライブラリでない場合は、パラメーターを渡す必要があり `libraryName` ます。 スタティックライブラリをリンクする場合は、 `__Internal` パラメーターとしてを使用し `libraryName` ます。

生成されるプロパティは常に static です。

Field 属性でフラグが設定されたプロパティには、次の種類があります。

* `NSString`
* `NSArray`
* `nint` / `int` / `long`
* `nuint` / `uint` / `ulong`
* `nfloat` / `float`
* `double`
* `CGSize`
* `System.IntPtr`
* 列挙型

Setter は、 [NSString 定数によっ](#enum-attributes)てサポートされる列挙型ではサポートされていませんが、必要に応じて手動でバインドできます。

例:

```csharp
[Static]
interface CameraEffects {
     [Field ("kCameraEffectsZoomFactorKey", "CameraLibrary")]
     NSString ZoomFactorKey { get; }
}
```

<a name="InternalAttribute"></a>

### <a name="internalattribute"></a>InternalAttribute

`[Internal]`この属性は、メソッドまたはプロパティに適用できます。また、生成されたコードに C# のキーワードを使用してフラグを設定すると、 `internal` 生成されたアセンブリ内のコードにのみコードからアクセスできるようになります。 これは通常、低レベルの Api を非表示にしたり、改善する必要のある最適ではないパブリック API を提供したり、ジェネレーターでサポートされておらず、何らかのハンドコーディングを必要とする api を提供したりするために使用されます。

バインディングをデザインするときは、通常、この属性を使用してメソッドまたはプロパティを非表示にし、メソッドまたはプロパティに別の名前を指定します。次に、C# 相補的なサポートファイルで、基になる機能を公開する厳密に型指定されたラッパーを追加します。

次に例を示します。

```csharp
[Internal]
[Export ("setValue:forKey:")]
void _SetValueForKey (NSObject value, NSObject key);

[Internal]
[Export ("getValueForKey:")]
NSObject _GetValueForKey (NSObject key);
```

次に、サポートファイルに次のようなコードを記述します。

```csharp
public NSObject this [NSObject idx] {
    get {
        return _GetValueForKey (idx);
    }
    set {
        _SetValueForKey (value, idx);
    }
}
```

<a name="IsThreadStaticAttribute"></a>

### <a name="isthreadstaticattribute"></a>IsThreadStaticAttribute

この属性は、プロパティのバッキングフィールドに、.NET 属性で注釈を付けるようにフラグを設定し `[ThreadStatic]` ます。 これは、フィールドがスレッドの静的変数である場合に便利です。

### <a name="marshalnativeexceptions-xamarinios-606"></a>Marshalの例外 (Xamarin 6.0.6)

この属性は、メソッドがネイティブ (目的 C) の例外をサポートするようにします。
呼び出しは、直接呼び出すのではなく、特定の `objc_msgSend` Trampoline ec 例外をキャッチし、それらをマネージ例外にマーシャリングするカスタムのを経由します。

現在のところ、サポートされているシグネチャはごくわずかです `objc_msgSend` (バインディングを使用するアプリのネイティブリンクが、不足している monotouch_ *_objc_msgSend* シンボルで失敗した場合、署名がサポートされていないかどうかは確認できます) が、要求に追加することができます。

### <a name="newattribute"></a>NewAttribute

この属性は、ジェネレーターが `new` 宣言の前にキーワードを生成するように、メソッドとプロパティに適用されます。

基底クラスに既に存在するサブクラスで同じメソッドまたはプロパティ名が導入された場合に、コンパイラの警告を回避するために使用されます。

<a name="NotificationAttribute"></a>

### <a name="notificationattribute"></a>NotificationAttribute

この属性をフィールドに適用して、ジェネレーターが厳密に型指定されたヘルパー通知クラスを生成するようにすることができます。

この属性は、ペイロードを含まない通知の引数なしで使用できます。また、API 定義で別のインターフェイスを参照するを指定することもでき `System.Type` ます。この場合、通常は "EventArgs" で終わる名前が使用されます。 ジェネレーターは、サブクラスを持つクラスにインターフェイスを変換し、そこに一覧表示されている `EventArgs` すべてのプロパティを含めます。 [`[Export]`](#ExportAttribute)属性をクラスで使用して、値を `EventArgs` 取得する目的の C 辞書を検索するために使用するキーの名前を一覧表示する必要があります。

次に例を示します。

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

上記のコードでは、 `MyClass.Notifications` 次のメソッドを使用して、入れ子になったクラスを生成します。

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
      public static NSObject ObserveDidStart (NSObject objectToObserve, EventHandler<NSNotificationEventArgs> handler)
   }
}
```

コードのユーザーは、次のようなコードを使用して、 [Nsdefaultcenter](xref:Foundation.NSNotificationCenter.DefaultCenter) にポストされた通知を簡単にサブスクライブできます。

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

または、監視する特定のオブジェクトを設定します。 `null`このメソッドにを渡すと `objectToObserve` 、その他のピアと同様に動作します。

```csharp
var token = MyClass.Notifications.ObserverDidStart (objectToObserve, (notification) => {
    Console.WriteLine ("Observed the 'DidStart' event on objectToObserve!");
});
```

から返された値を使用して、 `ObserveDidStart` 通知の受信を簡単に停止できます。次に例を示します。

```csharp
token.Dispose ();
```

または、 [Nsnotification](xref:Foundation.NSNotificationCenter.RemoveObserver(Foundation.NSObject)) を呼び出して、トークンを渡すこともできます。 通知にパラメーターが含まれている場合は、次のようにヘルパーインターフェイスを指定する必要があり `EventArgs` ます。

```csharp
interface MyClass {
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}

// The helper EventArgs declaration
interface MyScreenChangedEventArgs {
    [Export ("ScreenXKey")]
    nint ScreenX { get; set; }

    [Export ("ScreenYKey")]
    nint ScreenY { get; set; }

    [Export ("DidGoOffKey")]
    [ProbePresence]
    bool DidGoOff { get; }
}
```

上の例では、プロパティとプロパティを持つクラスが生成されます。これらのプロパティは、 `MyScreenChangedEventArgs` `ScreenX` `ScreenY` それぞれキーを使用して [nsnotification. UserInfo](xref:Foundation.NSNotification.UserInfo) ディクショナリからデータをフェッチし、 `ScreenXKey` 適切な変換を適用し `ScreenYKey` ます。 属性は、 `[ProbePresence]` 値を抽出するのではなく、でキーが設定されているかどうかを調べるジェネレーターに使用され `UserInfo` ます。 これは、キーの存在が値である場合 (通常はブール値の場合) に使用されます。

これにより、次のようなコードを記述できます。

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

場合によっては、ディクショナリで渡された値に定数が関連付けられていないことがあります。  Apple はパブリックシンボル定数を使用することがあり、場合によっては文字列定数を使用します。  既定で [`[Export]`](#ExportAttribute) は、指定したクラスの属性は、 `EventArgs` 実行時に検索されるパブリックシンボルとして指定された名前を使用します。  そうでない場合は、文字列定数として検索されるので、 `ArgumentSemantic.Assign` エクスポート属性に値を渡します。

**Xamarin の新。 iOS 8.4**

場合によっては、通知の有効期間が引数なしで開始されることがあるため、引数を指定せずに使用することも [`[Notification]`](#NotificationAttribute) 可能です。  しかし、通知のパラメーターが導入されることもあります。  このシナリオをサポートするには、属性を複数回適用できます。

バインドを開発していて、既存のユーザーコードが破損しないようにするには、次のようにして既存の通知を有効にします。

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

次のように、通知属性が2回表示されるバージョンにします。

```csharp
interface MyClass {
    [Notification]
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

<a name="NullAllowedAttribute"></a>

### <a name="nullallowedattribute"></a>NullAllowedAttribute

これがプロパティに適用されると、値が割り当てられるようにプロパティにフラグが設定され `null` ます。 これは、参照型に対してのみ有効です。

これがメソッドシグネチャのパラメーターに適用されると、指定されたパラメーターが null になる可能性があり、値を渡すためにチェックを実行する必要がないことを示し `null` ます。

参照型にこの属性がない場合、バインディングツールは、割り当てられている値のチェックを生成してから目的の C に渡し、 `ArgumentNullException` 割り当てられた値がの場合にをスローするチェックを生成します `null` 。

次に例を示します。

```csharp
// In properties

[NullAllowed]
UIImage IconFile { get; set; }

// In methods
void SetImage ([NullAllowed] UIImage image, State forState);
```

<a name="OverrideAttribute"></a>

### <a name="overrideattribute"></a>OverrideAttribute

この属性を使用して、この特定のメソッドのバインディングにキーワードでフラグを設定する必要があることをバインディングジェネレーターに指示し `override` ます。

### <a name="presnippetattribute"></a>PreSnippetAttribute

この属性を使用すると、入力パラメーターが検証された後、コードが目的の C を呼び出す前に挿入されるコードを挿入できます。

例:

```csharp
[Export ("demo")]
[PreSnippet ("var old = ViewController;")]
void Demo ();
```

### <a name="prologuesnippetattribute"></a>PrologueSnippetAttribute

この属性を使用すると、生成されたメソッドでパラメーターが検証される前に挿入されるコードを挿入できます。

例:

```csharp
[Export ("demo")]
[Prologue ("Trace.Entry ();")]
void Demo ();
```

### <a name="postgetattribute"></a>PostGetAttribute

このクラスから指定されたプロパティを呼び出して値をフェッチするように、バインディングジェネレーターに指示します。

このプロパティは、通常、オブジェクトグラフを参照している参照オブジェクトを参照するキャッシュを更新するために使用されます。 通常は、追加/削除などの操作を含むコードに表示されます。 このメソッドは、内部キャッシュを追加または削除した後で、実際に使用されているオブジェクトへのマネージ参照を保持するために使用されます。 バインディングツールは、指定されたバインディング内のすべての参照オブジェクトのバッキングフィールドを生成するため、これが可能です。

例:

```csharp
[BaseType (typeof (NSObject))]
[Since (4,0)]
public interface NSOperation {
    [Export ("addDependency:")][PostGet ("Dependencies")]
    void AddDependency (NSOperation op);

    [Export ("removeDependency:")][PostGet ("Dependencies")]
    void RemoveDependency (NSOperation op);

    [Export ("dependencies")]
    NSOperation [] Dependencies { get; }
}
```

この場合、オブジェクトの `Dependencies` 依存関係を追加または削除した後に、プロパティが呼び出されます。これにより、実際に読み込まれたオブジェクトを表すグラフが作成され、メモリ `NSOperation` リークとメモリの破損が両方とも防止されます。

### <a name="postsnippetattribute"></a>PostSnippetAttribute

この属性を使用して、コードが基になる目的の C メソッドを呼び出した後に挿入される C# ソースコードを挿入できます。

例:

```csharp
[Export ("demo")]
[PostSnippet ("if (old != null) old.DemoComplete ();")]
void Demo ();
```

### <a name="proxyattribute"></a>ProxyAttribute

この属性は、プロキシオブジェクトとしてフラグを設定する戻り値に適用されます。 一部の目的 C Api は、ユーザーバインドと区別できないプロキシオブジェクトを返します。 この属性の効果は、オブジェクトをオブジェクトとしてフラグを付けることです `DirectBinding` 。 Xamarin. Mac のシナリオでは、 [このバグに](https://bugzilla.novell.com/show_bug.cgi?id=670844)ついての説明を参照できます。

### <a name="retainlistattribute"></a>RetainListAttribute

パラメーターへのマネージ参照を保持するか、パラメーターへの内部参照を削除するようジェネレーターに指示します。 これは、オブジェクトを参照するために使用されます。

構文:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

の値が true の場合、 `doAdd` パラメーターはに追加され `__mt_{0}_var List<NSObject>;` ます。 ここ `{0}` で、は指定されたに置き換えられ `listName` ます。 相補的な部分クラスでこのバッキングフィールドを API に宣言する必要があります。

例については、「 [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) and [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs) 」を参照してください。

### <a name="releaseattribute-xamarinios-60"></a>ReleaseAttribute (Xamarin. iOS 6.0)

これは、を返す前に、ジェネレーターがオブジェクトに対してを呼び出す必要があることを示すために、戻り値の型に適用でき `Release` ます。 これは、メソッドが保持されたオブジェクトを提供する場合にのみ必要です。これは、最も一般的なシナリオである autoreleased オブジェクトとは対照的です。

例:

```csharp
[Export ("getAndRetainObject")]
[return: Release ()]
NSObject GetAndRetainObject ();
```

さらに、この属性は生成されたコードに反映されるため、Xamarin の iOS ランタイムは、このような関数から目的の C に戻るときにオブジェクトを保持する必要があることを認識します。

### <a name="sealedattribute"></a>SealedAttribute

生成されたメソッドにシールとしてフラグを付けるようジェネレーターに指示します。 この属性が指定されていない場合、既定では、仮想メソッド (仮想メソッド、抽象メソッド、または他の属性の使用方法に応じたオーバーライド) が生成されます。

<a name="StaticAttribute"></a>

### <a name="staticattribute"></a>StaticAttribute

`[Static]`メソッドまたはプロパティに属性を適用すると、静的メソッドまたは静的プロパティが生成されます。 この属性が指定されていない場合、ジェネレーターはインスタンスメソッドまたはプロパティを生成します。

### <a name="transientattribute"></a>遷移属性

この属性を使用して、一時的に値が設定されているプロパティ、つまり iOS によって一時的に作成され、有効期間の短いオブジェクトにフラグを設定します。 この属性がプロパティに適用されている場合、ジェネレーターはこのプロパティのバッキングフィールドを作成しません。つまり、マネージクラスはオブジェクトへの参照を保持しません。

<a name="WrapAttribute"></a>

### <a name="wrapattribute"></a>WrapAttribute

Xamarin. iOS/Xamarin. Mac バインドの設計では、属性を `[Wrap]` 使用して、厳密に型指定されたオブジェクトを使用して、弱い型指定のオブジェクトをラップします。 これは、主に、型または型として宣言されている目的 C デリゲートオブジェクトを使用して動作 `id` `NSObject` します。 Xamarin. iOS と Xamarin. Mac で使用される規則は、これらのデリゲートまたはデータソースを型として公開し、 `NSObject` 公開される名前という規則を使用して名前を付けます。 `id delegate`目的の C のプロパティは、API コントラクトファイルのプロパティとして公開さ `NSObject WeakDelegate { get; set; }` れます。

ただし、通常、このデリゲートに割り当てられる値は厳密な型であるため、厳密な型を使用して属性を適用します `[Wrap]` 。これにより、ユーザーは、細かい制御が必要な場合、または低レベルのテクニックを使用する必要がある場合や、ほとんどの作業に厳密に型指定されたプロパティを使用できる場合に、弱い

例:

```csharp
[BaseType (typeof (NSObject))]
interface Demo {
     [Export ("delegate"), NullAllowed]
     NSObject WeakDelegate { get; set; }

     [Wrap ("WeakDelegate")]
     DemoDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
interface DemoDelegate {
    [Export ("doDemo")]
    void DoDemo ();
}
```

これは、ユーザーが弱い型指定バージョンのデリゲートを使用する方法です。

```csharp
// The weak case, user has to roll his own
class SomeObject : NSObject {
    [Export ("doDemo")]
    void CallbackForDoDemo () {}

}

var demo = new Demo ();
demo.WeakDelegate = new SomeObject ();
```

これは、ユーザーが厳密に型指定されたバージョンを使用する方法であり、ユーザーが C# の型システムを利用し、その意図を宣言するために override キーワードを使用していること、および `[Export]` ユーザーのバインドでその作業を行ったため、でメソッドを手動で修飾する必要がないことに注意してください。

```csharp
// This is the strong case,
class MyDelegate : DemoDelegate {
   override void Demo DoDemo () {}
}

var strongDemo = new Demo ();
demo.Delegate = new MyDelegate ();
```

属性のもう1つの用途 `[Wrap]` は、厳密に型指定されたメソッドのバージョンをサポートすることです。  次に例を示します。

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

属性を使用して修飾された `[Wrap]` 型内のメソッドに属性が適用される場合は `[Category]` 、 `This` 拡張メソッドが生成されるため、を最初の引数として含める必要があります。 次に例を示します。

```csharp
[Wrap ("Write (This, image, options?.Dictionary, out error)")]
bool Write (CIImage image, CIImageRepresentationOptions options, out NSError error);
```

によって生成されるメンバー `[Wrap]` は、既定では使用されません `virtual` 。メンバーが必要な場合は、 `virtual` 省略可能なパラメーターにを設定でき `true` `isVirtual` ます。

```csharp
[BaseType (typeof (NSObject))]
interface FooExplorer {
    [Export ("fooWithContentsOfURL:")]
    void FromUrl (NSUrl url);

    [Wrap ("FromUrl (NSUrl.FromString (url))", isVirtual: true)]
    void FromUrl (string url);
}
```

`[Wrap]` は、プロパティの getter および setter で直接使用することもできます。
これにより、に対してフルコントロールを設定し、必要に応じてコードを調整できます。
たとえば、スマートな列挙型を使用する次の API 定義を考えてみます。

```csharp
// Smart enum.
enum PersonRelationship {
        [Field (null)]
        None,

        [Field ("FMFather", "__Internal")]
        Father,

        [Field ("FMMother", "__Internal")]
        Mother
}
```

インターフェイスの定義:

```csharp
// Property definition.

[Export ("presenceType")]
NSString _PresenceType { get; set; }

PersonRelationship PresenceType {
    [Wrap ("PersonRelationshipExtensions.GetValue (_PresenceType)")]
    get;
    [Wrap ("_PresenceType = value.GetConstant ()")]
    set;
}
```

## <a name="parameter-attributes"></a>パラメーター属性

ここでは、メソッド定義のパラメーターに適用できる属性と、プロパティ全体に適用される属性について説明し `[NullAttribute]` ます。

<a name="BlockCallback"></a>

### <a name="blockcallback"></a>BlockCallback

この属性は、問題のパラメーターが目的の C ブロック呼び出し規約に準拠していることをバインダーに通知するために、C# デリゲート宣言のパラメーター型に適用され、この方法でマーシャリングする必要があります。

これは通常、次のように定義されているコールバックで使用されます。

```objc
typedef returnType (^SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

参照: [Ccallback](#CCallback)

<a name="CCallback"></a>

### <a name="ccallback"></a>CCallback

この属性は、問題のパラメーターが C ABI 関数ポインターの呼び出し規約に準拠していることをバインダーに通知するために、C# デリゲート宣言のパラメーター型に適用され、この方法でマーシャリングする必要があります。

これは通常、次のように定義されているコールバックで使用されます。

```objc
typedef returnType (*SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

「 [Blockcallback](#BlockCallback)」も参照してください。

### <a name="params"></a>パラメーター

`[Params]`メソッド定義の最後の配列パラメーターに属性を使用して、ジェネレーターが定義に "params" を挿入することができます。   これにより、バインドで省略可能なパラメーターを簡単に使用できるようになります。

たとえば、次のような定義があるとします。

```csharp
[Export ("loadFiles:")]
void LoadFiles ([Params]NSUrl [] files);
```

では、次のコードを記述できます。

```csharp
foo.LoadFiles (new NSUrl (url));
foo.LoadFiles (new NSUrl (url1), new NSUrl (url2), new NSUrl (url3));
```

これには、ユーザーが要素を渡すためだけに配列を作成する必要がないという利点もあります。

<a name="plainstring"></a>

### <a name="plainstring"></a>PlainString

`[PlainString]`文字列パラメーターの前に属性を使用すると、パラメーターをとして渡す代わりに、文字列を C 文字列として渡すようにバインドジェネレーターに指示でき `NSString` ます。

ほとんどの目的 C Api で `NSString` はパラメーターが使用されますが、いくつかの api は、バリエーションでは `char *` なく文字列を渡すための api を公開してい `NSString` ます。
そのような場合は、を使用 `[PlainString]` します。

たとえば、次のような C の宣言があります。

```csharp
- (void) setText: (NSString *) theText;
- (void) logMessage: (char *) message;
```

次のようにバインドする必要があります。

```csharp
[Export ("setText:")]
void SetText (string theText);

[Export ("logMessage:")]
void LogMessage ([PlainString] string theText);
```

### <a name="retainattribute"></a>RetainAttribute

指定されたパラメーターへの参照を保持するようジェネレーターに指示します。 ジェネレーターによって、このフィールドのバッキングストアが提供されます。また、値を格納する名前 () を指定することもでき `WrapName` ます。 これは、目的の C にパラメーターとして渡されるマネージオブジェクトへの参照を保持するのに便利です。また、C はこのオブジェクトのコピーのみを保持することがわかっている場合に便利です。 たとえば、のような API では、 `SetDisplay (SomeObject)` SetDisplay で一度に1つのオブジェクトしか表示できない可能性があるため、この属性が使用されます。 複数のオブジェクト (たとえば、スタックに似た API) を追跡する必要がある場合は、属性を使用し `[RetainList]` ます。

構文:

```csharp
public class RetainAttribute {
    public RetainAttribute ();
    public RetainAttribute (string wrapName);
    public string WrapName { get; }
}
```

### <a name="retainlistattribute"></a>RetainListAttribute

パラメーターへのマネージ参照を保持するか、パラメーターへの内部参照を削除するようジェネレーターに指示します。 これは、オブジェクトを参照するために使用されます。

構文:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

の値が true の場合、 `doAdd` パラメーターはに追加され `__mt_{0}_var List<NSObject>` ます。 ここ `{0}` で、は指定されたに置き換えられ `listName` ます。 相補的な部分クラスでこのバッキングフィールドを API に宣言する必要があります。

例については、「 [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) and [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs) 」を参照してください。

### <a name="transientattribute"></a>遷移属性

この属性はパラメーターに適用され、目標 C から C# に移行する場合にのみ使用されます。  これらの遷移では、さまざまな目的 C `NSObject` パラメーターがオブジェクトのマネージ表現にラップされます。

ランタイムは、ネイティブオブジェクトへの参照を受け取り、オブジェクトへの最後に管理された参照がなくなるまで参照を保持し、GC は実行される可能性があります。

場合によっては、C# ランタイムがネイティブオブジェクトへの参照を保持しないようにすることが重要です。  これは、基になるネイティブコードがパラメーターのライフサイクルに特別な動作をアタッチした場合に発生することがあります。  例: パラメーターのデストラクターはクリーンアップアクションを実行するか、貴重なリソースを破棄します。

この属性は、上書きされたメソッドから目的の C に戻るときに、可能であればオブジェクトを破棄する必要があることをランタイムに通知します。

規則は単純です。ランタイムがネイティブオブジェクトから新しいマネージ表現を作成する必要がある場合、関数の末尾で、ネイティブオブジェクトの保持カウントが削除され、マネージオブジェクトの Handle プロパティがクリアされます。   つまり、マネージオブジェクトへの参照を保持した場合、その参照は役に立たなくなります (メソッドを呼び出すと例外がスローされます)。

渡されたオブジェクトが作成されなかった場合、またはオブジェクトの未処理のマネージ表現が既に存在していた場合、強制破棄は行われません。 

## <a name="property-attributes"></a>[プロパティ属性]

<a name="NotImplementedAttribute"></a>

### <a name="notimplementedattribute"></a>NotImplementedAttribute

この属性は、getter を持つプロパティが基底クラスで導入され、変更可能なサブクラスが setter を導入する、目的 C の表現をサポートするために使用されます。

C# ではこのモデルがサポートされていないため、基底クラスは setter と getter の両方を持つ必要があり、サブクラスは [Overrideattribute](#OverrideAttribute)を使用できます。

この属性は、プロパティ setter でのみ使用され、目標 C で変更可能な表現をサポートするために使用されます。

例:

```csharp
[BaseType (typeof (NSObject))]
interface MyString {
    [Export ("initWithValue:")]
    IntPtr Constructor (string value);

    [Export ("value")]
    string Value {
        get;

    [NotImplemented ("Not available on MyString, use MyMutableString to set")]
        set;
    }
}

[BaseType (typeof (MyString))]
interface MyMutableString {
    [Export ("value")]
    [Override]
    string Value { get; set; }
}
```

<a name="enum-attributes"></a>

## <a name="enum-attributes"></a>列挙型の属性

`NSString`定数を列挙値にマップすると、より適切な .NET API を簡単に作成できます。 それでは次のことが行われます。

* API に対して正しい値 **のみ** を表示することにより、コード補完をより効果的にすることができます。
* タイプセーフを追加し、間違ったコンテキストで別の定数を使用することはできません `NSString` 。
* では、一部の定数を非表示にすることができます

例:

```csharp
enum NSRunLoopMode {

    [DefaultEnumValue]
    [Field ("NSDefaultRunLoopMode")]
    Default,

    [Field ("NSRunLoopCommonModes")]
    Common,

    [Field (null)]
    Other = 1000
}
```

上記のバインディング定義から、ジェネレーターは `enum` それ自体を作成し、 `*Extensions` 列挙値と定数間の2つの方法の変換メソッドを含む静的な型も作成します `NSString` 。 つまり、API に含まれていない場合でも、開発者は定数を使用できます。

例 :

```csharp
// using the NSString constant in a different API / framework / 3rd party code
CallApiRequiringAnNSString (NSRunLoopMode.Default.GetConstant ());
```

```csharp
// converting the constants from a different API / framework / 3rd party code
var constant = CallApiReturningAnNSString ();
// back into an enum value
CallApiWithEnum (NSRunLoopModeExtensions.GetValue (constant));
```

### <a name="defaultenumvalueattribute"></a>DefaultEnumValueAttribute

この属性を使用して、 **1 つ** の列挙値を装飾できます。 これは、列挙値が不明な場合に返される定数になります。

上記の例から:

```csharp
var x = (NSRunLoopMode) 99;
Call (x.GetConstant ()); // NSDefaultRunLoopMode will be used
```

列挙値が装飾されていない場合は、が `NotSupportedException` スローされます。

### <a name="errordomainattribute"></a>ErrorDomainAttribute

エラーコードは列挙値としてバインドされます。 一般的に、これらのエラードメインは存在していますが、適用されているもの (存在する場合) を見つけるのは簡単ではありません。

この属性を使用して、エラードメインを列挙型自体に関連付けることができます。

例:

```csharp
[Native]
[ErrorDomain ("AVKitErrorDomain")]
public enum AVKitError : nint {
    None = 0,
    Unknown = -1000,
    PictureInPictureStartFailed = -1001
}
```

次に、拡張メソッドを呼び出して、 `GetDomain` 任意のエラーのドメイン定数を取得できます。

### <a name="fieldattribute"></a>FieldAttribute

これは、 `[Field]` 型の中の定数に使用される属性と同じです。 また、特定の定数に値をマップするために、列挙型の内部で使用することもできます。

値は、 `null` `null` `NSString` 定数が指定されている場合に返される列挙値を指定するために使用できます。

上記の例から:

```csharp
var constant = NSRunLoopMode.NewInWatchOS3; // will be null in watchOS 2.x
Call (NSRunLoopModeExtensions.GetValue (constant)); // will return 1000
```

`null`値が存在しない場合は、が `ArgumentNullException` スローされます。

## <a name="global-attributes"></a>グローバル属性

グローバル属性は `[assembly:]` 、やのような属性修飾子を使用して適用され [`[LinkWithAttribute]`](#LinkWithAttribute) ます。また、属性や属性など、任意の場所で使用することもでき [`[Lion]`](#SinceAndLionAttributes) [`[Since]`](#SinceAndLionAttributes) ます。

<a name="LinkWithAttribute"></a>

### <a name="linkwithattribute"></a>LinkWithAttribute

これはアセンブリレベルの属性であり、開発者はライブラリのコンシューマーに対して、ライブラリに渡された gcc_flags および追加の mtouch 引数を手動で構成することなく、バインドされたライブラリを再利用するために必要なリンクフラグを指定できます。

構文:

```csharp
// In properties
[Flags]
public enum LinkTarget {
    Simulator    = 1,
    ArmV6    = 2,
    ArmV7    = 4,
    Thumb    = 8,
}

[AttributeUsage(AttributeTargets.Assembly, AllowMultiple=true)]
public class LinkWithAttribute : Attribute {
    public LinkWithAttribute ();
    public LinkWithAttribute (string libraryName);
    public LinkWithAttribute (string libraryName, LinkTarget target);
    public LinkWithAttribute (string libraryName, LinkTarget target, string linkerFlags);
    public bool ForceLoad { get; set; }
    public string Frameworks { get; set; }
    public bool IsCxx { get; set;  }
    public string LibraryName { get; }
    public string LinkerFlags { get; set; }
    public LinkTarget LinkTarget { get; set; }
    public bool NeedsGccExceptionHandling { get; set; }
    public bool SmartLink { get; set; }
    public string WeakFrameworks { get; set; }
}
```

この属性はアセンブリレベルで適用されます。たとえば、 [Coreplot バインディング](https://github.com/mono/monotouch-bindings/tree/master/CorePlot) で使用されるのは次のようになります。

```csharp
[assembly: LinkWith ("libCorePlot-CocoaTouch.a", LinkTarget.ArmV7 | LinkTarget.ArmV7s | LinkTarget.Simulator, Frameworks = "CoreGraphics QuartzCore", ForceLoad = true)]
```

属性を使用すると、指定されたが結果のアセンブリに埋め込まれます。これにより、ユーザーは、 `[LinkWith]` `libraryName` アンマネージ依存関係と、Xamarin のライブラリを適切に使用するために必要なコマンドラインフラグの両方を含む単一の DLL を出荷できるようになります。

を提供しないこともできます `libraryName` 。その場合、属性を使用して `LinkWith` 追加のリンカーフラグを指定するだけで済みます。

``` csharp
[assembly: LinkWith (LinkerFlags = "-lsqlite3")]
```

#### <a name="linkwithattribute-constructors"></a>LinkWithAttribute コンストラクター

これらのコンストラクターを使用すると、ライブラリを指定して、結果のアセンブリにリンクして埋め込むことができます。また、ライブラリでサポートされているサポート対象のターゲット、およびライブラリとリンクするために必要な任意のライブラリフラグを指定することもできます。

`LinkTarget`引数は Xamarin. iOS によって推論されるため、設定する必要はありません。

例 :

```csharp
// Specify additional linker:
[assembly: LinkWith (LinkerFlags = "-sqlite3")]

// Specify library name for the constructor:
[assembly: LinkWith ("libDemo.a");

// Specify library name, and link target for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator);

// Specify only the library name, link target and linker flags for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator, SmartLink = true, ForceLoad = true, IsCxx = true);
```

#### <a name="linkwithattributeforceload"></a>LinkWithAttribute. ForceLoad

プロパティは、 `ForceLoad` `-force_load` ネイティブライブラリのリンクにリンクフラグを使用するかどうかを決定するために使用されます。 現時点では、これは常に true である必要があります。

#### <a name="linkwithattributeframeworks"></a>LinkWithAttribute. フレームワーク

バインドされているライブラリが、および以外の任意のフレームワークに対してハード要件を持っている場合は、プロパティを、 `Foundation` `UIKit` `Frameworks` 必要なプラットフォームフレームワークの空白で区切られた一覧を含む文字列に設定する必要があります。 たとえば、およびを必要とするライブラリをバインドする場合は、 `CoreGraphics` `CoreText` `Frameworks` プロパティをに設定し `"CoreGraphics CoreText"` ます。

#### <a name="linkwithattributeiscxx"></a>LinkWithAttribute. IsCxx

生成された実行可能ファイルを、C コンパイラの既定値ではなく、C++ コンパイラを使用してコンパイルする必要がある場合は、このプロパティを true に設定します。 バインドするライブラリが C++ で記述されている場合は、これを使用します。

#### <a name="linkwithattributelibraryname"></a>LinkWithAttribute. LibraryName

バンドルするアンマネージライブラリの名前。 これは拡張子が ". a" のファイルで、複数のプラットフォーム (たとえば、シミュレーターの場合は ARM、x86) のオブジェクトコードを含むことができます。

以前のバージョンの Xamarin では、プロパティをチェックし `LinkTarget` てライブラリがサポートされているプラットフォームを特定しましたが、これは自動検出され、 `LinkTarget` プロパティは無視されるようになりました。

#### <a name="linkwithattributelinkerflags"></a>LinkWithAttribute. LinkerFlags

`LinkerFlags`文字列を使用すると、作成者はネイティブライブラリをアプリケーションにリンクするときに必要な追加のリンカーフラグを指定することができます。

たとえば、ネイティブライブラリに libxml2 と zlib が必要な場合は、 `LinkerFlags` 文字列をに設定し `"-lxml2 -lz"` ます。

#### <a name="linkwithattributelinktarget"></a>LinkWithAttribute. LinkTarget

以前のバージョンの Xamarin では、プロパティをチェックし `LinkTarget` てライブラリがサポートされているプラットフォームを特定しましたが、これは自動検出され、 `LinkTarget` プロパティは無視されるようになりました。

#### <a name="linkwithattributeneedsgccexceptionhandling"></a>LinkWithAttribute. NeedsGccExceptionHandling

リンクするライブラリが GCC 例外処理ライブラリ (gcc_eh) を必要とする場合は、このプロパティを true に設定します。

#### <a name="linkwithattributesmartlink"></a>LinkWithAttribute. SmartLink

`SmartLink`プロパティを true に設定して、Xamarin でが必須かどうかを判断する必要があります。 `ForceLoad`

#### <a name="linkwithattributeweakframeworks"></a>LinkWithAttribute. このフレームワーク

プロパティは、 `WeakFrameworks` プロパティと同じように動作し `Frameworks` ます。ただし、リンク時には、 `-weak_framework` 指定された各フレームワークの gcc に指定子が渡されます。

`WeakFrameworks` では、ライブラリとアプリケーションがプラットフォームフレームワークに対して弱いリンクを作成できるようになりました。使用可能な場合は、必要に応じてそれらを使用できます。ただし、ライブラリが新しいバージョンの iOS に追加機能を追加する場合に便利です。 弱いリンクの詳細については、 [弱リンク](https://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WeakLinking.html)に関する Apple のドキュメントを参照してください。

脆弱なリンクの候補としては、 `Frameworks` アカウント、、、 `CoreBluetooth` `CoreImage` `GLKit` 、 `NewsstandKit` および `Twitter` が iOS 5 でのみ利用可能であるというものがあります。

<a name="SinceAndLionAttributes"></a>

### <a name="sinceattribute-ios-and-lionattribute-macos"></a>SinceAttribute (iOS) と LionAttribute (macOS)

属性を使用し `[Since]` て、特定の時点で導入されているものとして api にフラグを付けることができます。 属性は、基になるクラス、メソッド、またはプロパティが使用できない場合に、ランタイムの問題を引き起こす可能性のある型およびメソッドにフラグを設定するためにのみ使用する必要があります。

構文:

```csharp
public SinceAttribute : Attribute {
     public SinceAttribute (byte major, byte minor);
     public byte Major, Minor;
}
```

以前のバージョンのオペレーティングシステムを使用しているデバイスで実行された場合、ランタイムエラーが発生しないため、列挙型、制約、または新しい構造体には一般的に適用されません。

型に適用される場合の例:

```csharp
// Type introduced with iOS 4.2
[Since (4,2)]
[BaseType (typeof (UIPrintFormatter))]
interface UIViewPrintFormatter {
    [Export ("view")]
    UIView View { get; }
}
```

新しいメンバーに適用する場合の例:

```csharp
[BaseType (typeof (UIViewController))]
public interface UITableViewController {
    [Export ("tableView", ArgumentSemantic.Retain)]
    UITableView TableView { get; set; }

    [Since (3,2)]
    [Export ("clearsSelectionOnViewWillAppear")]
    bool ClearsSelectionOnViewWillAppear { get; set; }
```

属性は、 `[Lion]` ライオンで導入された型に対しても同じように適用されます。 Ios で使用されるより具体的なバージョン番号と比較して使用する理由として、iOS は非常に頻繁に変更されますが `[Lion]` 、主要な OS X のリリースはめったに発生しません。また、オペレーティングシステムは、バージョン番号よりもコードネームによって簡単に覚えることができます。

### <a name="adviceattribute"></a>AdviceAttribute

この属性は、開発者が使用するのに便利な他の Api に関するヒントを開発者に提供するために使用します。   たとえば、厳密に型指定された API のバージョンを指定した場合、弱い型指定の属性でこの属性を使用して、開発者により優れた API を提供できます。

この属性の情報は、ドキュメントに記載されています。ツールを開発して、

### <a name="requiressuperattribute"></a>RequiresSuperAttribute

これは、属性の特化されたサブクラスであり、メソッドをオーバーライドする場合に、 `[Advice]` 基本 (オーバーライドされた) メソッドを呼び出す **必要** があることを開発者にヒントに使用できます。

これはに対応し `clang` ます。 [`__attribute__((objc_requires_super))`](https://clang.llvm.org/docs/AttributeReference.html#objc-requires-super)

### <a name="zerocopystringsattribute"></a>ZeroCopyStringsAttribute

Xamarin 5.4 以降でのみ使用できます。

この属性は、この特定のライブラリ (で適用されている場合 `[assembly:]` ) または型のバインディングが、高速ゼロコピー文字列のマーシャリングを使用する必要があることをジェネレーターに指示します。 この属性は、コマンドラインオプションをジェネレーターに渡すことと同じです `--zero-copy` 。

文字列にゼロコピーを使用する場合、ジェネレーターは、新しいオブジェクトを作成せず `NSString` に、c# 文字列から目的の c 文字列へのデータのコピーを回避して、目的の c 文字列と同じ C# 文字列を効果的に使用します。 0個のコピー文字列を使用する場合の唯一の欠点は、ラップする文字列プロパティに、 `retain` または属性が設定されているかどうかを確認する必要があることです `copy` `[DisableZeroCopy]` 。 これは、ゼロコピー文字列のハンドルがスタックに割り当てられており、関数の戻り値では無効であるために必要です。

例:

```csharp
[ZeroCopyStrings]
[BaseType (typeof (NSObject))]
interface MyBinding {
    [Export ("name")]
    string Name { get; set; }

    [Export ("domain"), NullAllowed]
    string Domain { get; set; }

    [DisablZeroCopy]
    [Export ("someRetainedNSString")]
    string RetainedProperty { get; set; }
}

```

アセンブリレベルで属性を適用することもできます。この属性は、アセンブリのすべての型に適用されます。

```csharp
[assembly:ZeroCopyStrings]
```

## <a name="strongly-typed-dictionaries"></a>厳密に型指定されたディクショナリ

Xamarin. iOS 8.0 では、をラップする厳密に型指定されたクラスを簡単に作成できるようになりました `NSDictionaries` 。

常に、 [Dictionarycontainer](xref:Foundation.DictionaryContainer) データ型を手動の API と共に使用できるようになりましたが、これははるかに簡単になりました。  詳細については、「 [厳密な型の提示](~/cross-platform/macios/binding/objective-c-libraries.md#Surfacing_Strong_Types)」を参照してください。

<a name="StrongDictionary"></a>

### <a name="strongdictionary"></a>StrongDictionary

この属性がインターフェイスに適用されると、ジェネレーターは、 [Dictionarycontainer](xref:Foundation.DictionaryContainer) から派生したインターフェイスと同じ名前のクラスを生成し、インターフェイスに定義されている各プロパティを、そのディクショナリの厳密に型指定された getter および setter に変換します。

これにより、新しいを作成した既存のまたはからインスタンス化できるクラスが自動的に生成され `NSDictionary` ます。

この属性は、1つのパラメーターを受け取ります。これは、ディクショナリの要素にアクセスするために使用されるキーを含むクラスの名前です。   既定では、属性を持つインターフェイスの各プロパティは、"Key" というサフィックスが付いた名前の指定された型のメンバーを参照します。

次に例を示します。

```csharp
[StrongDictionary ("MyOptionKeys")]
interface MyOption {
    string Name { get; set; }
    nint    Age  { get; set; }
}

[Static]
interface MyOptionKeys {
    // In Objective-C this is "NSString *MYOptionNameKey;"
    [Field ("MYOptionNameKey")]
    NSString NameKey { get; }

    // In Objective-C this is "NSString *MYOptionAgeKey;"
    [Field ("MYOptionAgeKey")]
    NSString AgeKey { get; }
}

```

上記の場合、クラスは `MyOption` `Name` 、 `MyOptionKeys.NameKey` 文字列を取得するためにディクショナリのキーとしてを使用するの文字列プロパティを生成します。   とは、を `MyOptionKeys.AgeKey` キーとしてディクショナリに使用して、int を含むを取得し `NSNumber` ます。

別のキーを使用する場合は、プロパティで export 属性を使用できます。次に例を示します。

```csharp
[StrongDictionary ("MyColoringKeys")]
interface MyColoringOptions {
    [Export ("TheName")]  // Override the default which would be NameKey
    string Name { get; set; }

    [Export ("TheAge")] // Override the default which would be AgeKey
    nint    Age  { get; set; }
}

[Static]
interface MyColoringKeys {
    // In Objective-C this is "NSString *MYColoringNameKey"
    [Field ("MYColoringNameKey")]
    NSString TheName { get; }

    // In Objective-C this is "NSString *MYColoringAgeKey"
    [Field ("MYColoringAgeKey")]
    NSString TheAge { get; }
}
```

#### <a name="strong-dictionary-types"></a>強い辞書型

定義では、次のデータ型がサポートされてい `StrongDictionary` ます。

|C# インターフェイス型|`NSDictionary` ストレージの種類|
|---|---|
|`bool`|`Boolean` に格納されます。 `NSNumber`|
|列挙値|に格納されている整数 `NSNumber`|
|`int`|32-に格納されているビット整数 `NSNumber`|
|`uint`|32-に格納されたビット符号なし整数 `NSNumber`|
|`nint`|`NSInteger` に格納されます。 `NSNumber`|
|`nuint`|`NSUInteger` に格納されます。 `NSNumber`|
|`long`|64-に格納されているビット整数 `NSNumber`|
|`float`|32-として格納されるビット整数 `NSNumber`|
|`double`|64-として格納されるビット整数 `NSNumber`|
|`NSObject` およびサブクラス|`NSObject`|
|`NSDictionary`|`NSDictionary`|
|`string`|`NSString`|
|`NSString`|`NSString`|
|C# `Array``NSObject`|`NSArray`|
|`Array`列挙型の C#|`NSArray` 格納する `NSNumber` 値|
