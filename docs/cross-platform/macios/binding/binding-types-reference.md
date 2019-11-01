---
title: バインディングの種類のリファレンスガイド
description: このリファレンスガイドでは、目的の C ライブラリへのバインドを作成C#するときに理解しておく必要があるさまざまな属性と概念について説明します。
ms.prod: xamarin
ms.assetid: C6618E9D-07FA-4C84-D014-10DAC989E48D
author: davidortinau
ms.author: daortin
ms.date: 03/06/2018
ms.openlocfilehash: e89cbf98dbaf5a96fdfa51069f580b914ba5ff76
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016298"
---
# <a name="binding-types-reference-guide"></a>バインディングの種類のリファレンスガイド

このドキュメントでは、API コントラクトファイルに注釈を付けて、バインディングと生成されたコードを駆動するために使用できる属性の一覧について説明します。

Xamarin と Xamarin の API コントラクトは、ほとんどのC#場合、目的の C コードがどのようにC#表示されるかを定義するインターフェイス定義として記述されています。 このプロセスには、インターフェイス宣言と、API コントラクトが必要とする基本的な型定義の組み合わせが含まれます。 バインディングの種類の概要については、「関連ガイド」バインドの「[目標 C ライブラリ](~/cross-platform/macios/binding/objective-c-libraries.md)」を参照してください。

## <a name="type-definitions"></a>型定義

構文:

```csharp
[BaseType (typeof (BTYPE))
interface MyType : [Protocol1, Protocol2] {
     IntPtr Constructor (string foo);
}
```

生成されたオブジェクトの基本型を宣言する[`[BaseType]`](#BaseTypeAttribute)属性を持つ、コントラクト定義内のすべてのインターフェイス。 上記の宣言では、`MyType`C#と呼ばれる目標 C 型にバインドする `MyType` クラス型が生成されます。

インターフェイスの継承構文を使用して、typename の後に型を指定した場合 (上記のサンプル `Protocol1` および `Protocol2`)、これらのインターフェイスの内容は `MyType`のコントラクトに含まれているかのようにインライン化されます。
Xamarin の iOS では、型がプロトコルを採用する方法として、プロトコルで宣言されたすべてのメソッドとプロパティを型自体にインライン展開します。

次に、`UITextField` の目的 C の宣言を Xamarin. iOS コントラクトで定義する方法を示します。

```objc
@interface UITextField : UIControl <UITextInput> {

}
```

は、次のようにC# API コントラクトとして記述されます。

```csharp
[BaseType (typeof (UIControl))]
interface UITextField : UITextInput {
}
```

他の属性をインターフェイスに適用したり、 [`[BaseType]`](#BaseTypeAttribute)属性を構成したりすることにより、コード生成の他の多くの側面を制御できます。

### <a name="generating-events"></a>生成 (イベントを)

Xamarin および Xamarin API の設計の1つの機能として、目的の C デリゲートクラスをイベントとC#コールバックとしてマップすることが挙げられます。 ユーザーは、目標 C プログラミングパターンを採用するかどうかをインスタンス単位で選択できます。そのためには、目的の C ランタイムが呼び出すさまざまなメソッドを実装するクラスのインスタンス `Delegate` などのプロパティに割り当てるか、またはC#-スタイルのイベントとプロパティ。

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

上の例では、2つのメソッドを上書きすることを選択したことがわかります。これは、スクロールイベントが発生したことを示す通知です。2番目のメソッドは、トップまたは n にスクロールする必要があるかどうか `scrollView` を指示するブール値を返すコールバックであることを示しています。ot.

C#モデルを使用すると、ライブラリのユーザーは、 C#イベント構文またはプロパティ構文を使用して通知をリッスンし、値を返すことが想定されているコールバックをフックできます。

次に示すのC#は、ラムダを使用して同じ機能のコードがどのように見えるかを示しています。

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

イベントは値を返さないため (戻り値の型が void であるため)、複数のコピーを接続できます。 `ShouldScrollToTop` はイベントではなく、次のシグネチャを持つ `UIScrollViewCondition` 型のプロパティです。

```csharp
public delegate bool UIScrollViewCondition (UIScrollView scrollView);
```

この例では `bool` 値が返されます。この場合、ラムダ構文を使用すると、`MakeDecision` 関数から値を返すだけで済みます。

バインディングジェネレーターは、`UIScrollView` のようなクラスを `UIScrollViewDelegate` とリンクするイベントおよびプロパティの生成をサポートしています (これらのモデルクラスを呼び出すこともよくあります)。これは、 [`[BaseType]`](#BaseTypeAttribute)定義に `Events` と `Delegates` のパラメーターで注釈を付けることによって行われます (次に説明します)。 これらのパラメーターを使用して[`[BaseType]`](#BaseTypeAttribute)に注釈を付けるだけでなく、いくつかのコンポーネントをジェネレーターに通知する必要があります。

複数のパラメーターを受け取るイベントの場合 (目的 C では、デリゲートクラスの最初のパラメーターが sender オブジェクトのインスタンスであること)、生成された `EventArgs` クラスの名前を指定する必要があります。 これを行うには、モデルクラスのメソッド宣言の[`[EventArgs]`](#EventArgsAttribute)属性を使用します。 (例:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

上記の宣言は、`EventArgs` から派生し、パラメーター、`UIImage`、および `NSDictionary`の両方をパックする `UIImagePickerImagePickedEventArgs` クラスを生成します。 ジェネレーターは次のものを生成します。

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

その後、`UIImagePickerController` クラスで次のものを公開します。

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

値を返すモデルメソッドは、異なる方法でバインドされます。 これには、生成さC#れたデリゲートの名前 (メソッドのシグネチャ) と、ユーザーが実装を提供しない場合に返す既定値が必要です。 たとえば、`ShouldScrollToTop` 定義は次のようになります。

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIScrollViewDelegate {
    [Export ("scrollViewShouldScrollToTop:"), DelegateName ("UIScrollViewCondition"), DefaultValue ("true")]
    bool ShouldScrollToTop (UIScrollView scrollView);
}
```

上の例では、上に示したシグネチャを使用して `UIScrollViewCondition` デリゲートを作成します。ユーザーが実装を提供していない場合、戻り値は true になります。

[`[DefaultValue]`](#DefaultValueAttribute)属性に加えて、 [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)属性を使用して、呼び出しで指定されたパラメーターの値を返すようにジェネレーターに指示することもできます。また、 [`[NoDefaultValue]`](#NoDefaultValueAttribute)パラメーターを使用して、存在することをジェネレーターに指示することもできます。既定値はありません。

<a name="BaseTypeAttribute" />

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

この型が目的の C 言語でバインドされる名前を制御するには、`Name` プロパティを使用します。 これは通常、.NET Framework 設計ガイドラインC#に準拠している名前を型に指定するために使用されますが、この規則に従っていない目的で C の名前にマップされます。

たとえば、次の例では、.NET Framework 設計ガイドラインでは "URL" ではなく "Url" を使用しているので、目的の C `NSURLConnection` 型を `NSUrlConnection`にマップします。

```csharp
[BaseType (typeof (NSObject), Name="NSURLConnection")]
interface NSUrlConnection {
}
```

指定された名前は、バインディングで生成される `[Register]` 属性の値として使用されます。 `Name` が指定されていない場合は、生成された出力の `[Register]` 属性の値として、型の短い名前が使用されます。

#### <a name="basetypeevents-and-basetypedelegates"></a>BaseType. イベントと BaseType

これらのプロパティは、生成されたC#クラスでスタイルの生成イベントを駆動するために使用されます。 これらは、特定のクラスとその目的の C デリゲートクラスをリンクするために使用されます。 クラスがデリゲートクラスを使用して通知とイベントを送信する多くのケースが発生します。 たとえば、`BarcodeScanner` には、コンパニオン `BardodeScannerDelegate` クラスがあります。 `BarcodeScanner` クラスには、通常、`BarcodeScannerDelegate` のインスタンスを割り当てる `Delegate` のプロパティがあります。これは、ユーザーに似たスタイルのイベントインターフェイスを公開C#し、そのような場合には`Events`と @no__t_5 を使用することをお勧めします。[`[BaseType]`](#BaseTypeAttribute)属性のプロパティ。

これらのプロパティは常に一緒に設定され、同じ数の要素を持ち、同期されたままにする必要があります。`Delegates` 配列には、ラップする弱い型指定デリゲートごとに1つの文字列が含まれています。また、`Events` 配列には、関連付ける型ごとに1つの型が含まれています。

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

このクラスの新しいインスタンスが作成されたときにこの属性を適用すると、そのオブジェクトのインスタンスは、`KeepRefUntil` によって参照されるメソッドが呼び出されるまで保持されます。 これは、ユーザーがコードを使用するためにオブジェクトへの参照を保持する必要がない場合に、Api の使いやすさを向上させるために役立ちます。 このプロパティの値は `Delegate` クラスのメソッドの名前であるため、`Events` と `Delegates` プロパティと組み合わせて使用する必要があります。

次の例では、Xamarin の `UIActionSheet` によってこれがどのように使用されるかを示します。

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

<a name="DesignatedDefaultCtorAttribute" />

### <a name="designateddefaultctorattribute"></a>DesignatedDefaultCtorAttribute

この属性がインターフェイス定義に適用されると、既定の (生成される) コンストラクターに `[DesignatedInitializer]` 属性が生成され、`init` セレクターにマップされます。

<a name="DisableDefaultCtorAttribute" />

### <a name="disabledefaultctorattribute"></a>DisableDefaultCtorAttribute

この属性がインターフェイス定義に適用されると、ジェネレーターが既定のコンストラクターを生成できなくなります。

クラスの他のコンストラクターの1つを使用してオブジェクトを初期化する必要がある場合は、この属性を使用します。

<a name="PrivateDefaultCtorAttribute" />

### <a name="privatedefaultctorattribute"></a>PrivateDefaultCtorAttribute

この属性がインターフェイス定義に適用されると、既定のコンストラクターにプライベートとしてフラグが設定されます。 これは、このクラスのオブジェクトを拡張ファイルから内部的にインスタンス化できることを意味しますが、クラスのユーザーがアクセスすることはできません。

<a name="CategoryAttribute" />

### <a name="categoryattribute"></a>カテゴリの属性

この属性を型定義で使用して、目的の C カテゴリをバインドし、 C#それらを拡張メソッドとして公開し、目的の c が機能を公開する方法をミラー化します。

カテゴリは、クラスで使用可能なメソッドとプロパティのセットを拡張するために使用される、目的の C の機構です。   実際には、特定のフレームワークがリンクされている (たとえば `UIKit`) ときに基本クラスの機能を拡張するために使用されます (たとえば、`NSObject`)。新しいフレームワークがリンクされている場合にのみ、メソッドを使用できるようにします。   場合によっては、機能によってクラスの特徴を整理するために使用されます。   これらは、スピリットの拡張C#メソッドに似ています。

このカテゴリは、次のようになります。

```objc
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

上の例は、メソッド `makeBackgroundRed`を使用して `UIView` のインスタンスを拡張するライブラリにあります。

これらをバインドするには、インターフェイス定義で[`[Category]`](#CategoryAttribute)属性を使用します。   [`[Category]`](#CategoryAttribute)属性を使用すると、 [`[BaseType]`](#BaseTypeAttribute)属性の意味が、拡張する基底クラスを指定するために使用されるように変更され、拡張する型になります。

`UIView` の拡張機能がどのようにバインドされ、 C#拡張メソッドに変換されるかを次に示します。

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

上の例では、`MakeBackgroundRed` 拡張メソッドを含むクラス `MyUIViewExtension` を作成します。   つまり、任意の `UIView` サブクラスで `MakeBackgroundRed` を呼び出すことができるので、目的の C でも同じ機能を使用できます。

場合によっては、次の例のように、**静的**メンバーがカテゴリ内に存在することがあります。

```objc
@interface FooObject (MyFooObjectExtension)
+ (BOOL)boolMethod:(NSRange *)range;
@end
```

これにより、**正しくない**カテゴリC#インターフェイス定義が発生します。

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

`BoolMethod` 拡張機能を使用して `FooObject` のインスタンスが必要ですが、ObjC**静的**拡張をバインドしているため、これは正しくありません。これは、 C#拡張メソッドの実装方法の事実による副作用です。

上記の定義を使用する唯一の方法は、次のような厄介なコードです。

```csharp
(null as FooObject).BoolMethod (range);
```

これを回避するための推奨事項は、`FooObject` インターフェイス定義内で `BoolMethod` 定義をインライン化することです。これにより、`FooObject.BoolMethod (range)`意図したように、この拡張機能を呼び出すことができます。

```csharp
[BaseType (typeof (NSObject))]
interface FooObject {

    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

[`[Category]`](#CategoryAttribute)定義内に[`[Static]`](#StaticAttribute)メンバーが見つかるたびに、警告 (BI1117) が発行されます。 [`[Category]`](#CategoryAttribute)定義内にメンバーを[`[Static]`](#StaticAttribute)する場合は、`[Category (allowStaticMembers: true)]` を使用するか、メンバーまたは[`[Category]`](#CategoryAttribute)インターフェイス定義を[`[Internal]`](#InternalAttribute)で修飾することにより、警告を無音にすることができます。

<a name="StaticAttribute_Class" />

### <a name="staticattribute"></a>StaticAttribute

この属性がクラスに適用されると、静的クラス (`NSObject`から派生したものではない) が生成されます。そのため、 [`[BaseType]`](#BaseTypeAttribute)属性は無視されます。 静的クラスは、公開する C パブリック変数をホストするために使用されます。

(例:

```csharp
[Static]
interface CBAdvertisement {
    [Field ("CBAdvertisementDataServiceUUIDsKey")]
    NSString DataServiceUUIDsKey { get; }
```

では、 C#次の API を使用してクラスが生成されます。

```csharp
public partial class CBAdvertisement  {
    public static NSString DataServiceUUIDsKey { get; }
}
```

## <a name="protocolmodel-definitions"></a>プロトコル/モデルの定義

モデルは、通常、プロトコルの実装によって使用されます。
これは、ランタイムが、実際に上書きされたメソッドのみを目的の C に登録する点が異なります。
それ以外の場合、メソッドは登録されません。

これは一般に、`ModelAttribute`でフラグが設定されたクラスをサブクラス化する場合、基本メソッドを呼び出さないことを意味します。   このメソッドを呼び出すと、例外がスローされます。オーバーライドするメソッドについては、サブクラスに対して動作全体を実装することになります。

<a name="AbstractAttribute" />

### <a name="abstractattribute"></a>AbstractAttribute

既定では、プロトコルの一部であるメンバーは必須ではありません。 これにより、ユーザーは、単にのC#クラスから派生させ、関心のあるメソッドだけをオーバーライドすることによって、`Model` オブジェクトのサブクラスを作成できます。 場合によっては、ユーザーがこのメソッドの実装を提供しなければならないことがあります (これは、目的の `@required` ディレクティブでフラグが付けられます)。 そのような場合は、`[Abstract]` 属性を使用してこれらのメソッドにフラグを付ける必要があります。

`[Abstract]` 属性は、メソッドまたはプロパティのいずれかに適用でき、生成されたメンバーに abstract としてフラグを設定し、クラスを抽象クラスにします。

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

<a name="DefaultValueAttribute" />

### <a name="defaultvalueattribute"></a>DefaultValueAttribute

モデルオブジェクトでこの特定のメソッドに対してユーザーがメソッドを提供しない場合に、モデルメソッドによって返される既定値を指定します。

構文:

```csharp
public class DefaultValueAttribute : Attribute {
        public DefaultValueAttribute (object o);
        public object Default { get; set; }
}
```

たとえば、`Camera` クラスの次の虚数デリゲートクラスでは、`Camera` クラスのプロパティとして公開される `ShouldUploadToServer` を提供します。 `Camera` クラスのユーザーが、true または false に応答できるラムダに値を明示的に設定していない場合、この場合の既定値は false になり、`DefaultValue` 属性で指定した値になります。:

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

関連項目: [`[NoDefaultValue]`](#NoDefaultValueAttribute)、 [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)。

<a name="DefaultValueFromArgumentAttribute" />

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

上記の例では、`NSAnimation` クラスのユーザーがいずれかのC#イベント/プロパティを使用するように選択し、`NSAnimation.ComputeAnimationCurve`をメソッドまたはラムダに設定しなかった場合、戻り値は progress パラメーターに渡された値になります。

関連項目: [`[NoDefaultValue]`](#NoDefaultValueAttribute)、 [`[DefaultValue]`](#DefaultValueAttribute)

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

<a name="EventArgsAttribute" />

### <a name="eventargsattribute"></a>EventArgsAttribute

複数のパラメーターを受け取るイベントの場合 (目的 C では、デリゲートクラスの最初のパラメーターが sender オブジェクトのインスタンスであること)、生成された EventArgs クラスの名前をに指定する必要があります。 これを行うには、`Model` クラスのメソッド宣言の `[EventArgs]` 属性を使用します。

(例:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

上記の宣言は、EventArgs から派生した `UIImagePickerImagePickedEventArgs` クラスを生成し、パラメーター、`UIImage`、および `NSDictionary`の両方をパックします。 ジェネレーターは次のものを生成します。

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

その後、`UIImagePickerController` クラスで次のものを公開します。

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

### <a name="eventnameattribute"></a>EventNameAttribute

この属性は、クラスで生成されたイベントまたはプロパティの名前をジェネレーターが変更できるようにするために使用されます。 モデルクラスのメソッドの名前がモデルクラスにとって意味があるものの、元のクラスではイベントまたはプロパティとして奇妙に見える場合に役立つことがあります。

たとえば、`UIWebView` は `UIWebViewDelegate`の次のビットを使用します。

```csharp
[Export ("webViewDidFinishLoad:"), EventArgs ("UIWebView"), EventName ("LoadFinished")]
void LoadingFinished (UIWebView webView);
```

上の例では、`UIWebViewDelegate`のメソッドとして `LoadingFinished` を公開していますが、`UIWebView`でフックするイベントとして `LoadFinished` しています。

```csharp
var webView = new UIWebView (...);
webView.LoadFinished += delegate { Console.WriteLine ("done!"); }
```

<a name="ModelAttribute" />

### <a name="modelattribute"></a>ModelAttribute

コントラクト API の型定義に `[Model]` 属性を適用すると、ユーザーがクラスのメソッドを上書きした場合に、クラスのメソッドへの呼び出しのみを実行する特殊なコードがランタイムによって生成されます。 この属性は、通常、目的の C デリゲートクラスをラップするすべての Api に適用されます。

<a name="NoDefaultValueAttribute" />

### <a name="nodefaultvalueattribute"></a>NoDefaultValueAttribute

モデルのメソッドが既定の戻り値を提供しないことを指定します。

これは、指定したセレクターがこのクラスに実装されているかどうかを判断するために、目的の C ランタイム要求に `false` 応答することによって、目的 C ランタイムで動作します。

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("shouldDisplayPopup"), NoDefaultValue]
    bool ShouldUploadToServer ();
}
```

関連項目: [`[DefaultValue]`](#DefaultValueAttribute)、 [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)  

<a name="ProtocolAttribute" />

## <a name="protocols"></a>プロトコル

「」では、C プロトコルの概念は実際C#には存在しません。 プロトコルはインターフェイスにC#似ていますが、プロトコルで宣言されているすべてのメソッドとプロパティが、それを採用するクラスによって実装されている必要はありません。 代わりに、一部のメソッドとプロパティは省略可能です。

一部のプロトコルは通常、モデルクラスとして使用されます。これらは、 [`[Model]`](#ModelAttribute)属性を使用してバインドする必要があります。

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

Xamarin 以降では、新たに強化されたプロトコルバインド機能が組み込まれてい7.0 ます。  `[Protocol]` 属性を含む定義では、次の3つのサポートクラスを実際に生成して、プロトコルの使用方法を大幅に向上させます。

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

**クラス実装**は、の個々のメソッドをオーバーライドし、完全なタイプセーフを取得できる、完全な抽象クラスを提供します。 ただし、複数C#の継承がサポートされていないため、異なる基本クラスが必要になる場合もありますが、インターフェイスを実装する必要がある場合があります。

ここでは、生成された**インターフェイス定義**が含まれています。  これは、プロトコルから必要なすべてのメソッドを含むインターフェイスです。  これにより、開発者は、インターフェイスを実装するだけで、プロトコルを実装することができます。  ランタイムは、プロトコルを採用するときに型を自動的に登録します。

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

`IMyProtocol` が存在しない場合はバインド時に、空のインターフェイスを提供する必要があるため、上記が必要です。

### <a name="adopting-protocol-generated-interfaces"></a>プロトコルによって生成されたインターフェイスの導入

プロトコル用に生成されたインターフェイスの1つを実装する場合は、次のようになります。

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

インターフェイスメソッドの実装は、適切な名前を使用して自動的にエクスポートされるため、次のようになります。

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

インターフェイスが暗黙的または明示的に実装されているかどうかは関係ありません。

### <a name="protocol-inlining"></a>プロトコルインライン展開

プロトコルの導入として宣言されている既存の目標 C 型をバインドするときに、プロトコルを直接インライン化する必要があります。 これを行うには、単に、 [`[BaseType]`](#BaseTypeAttribute)属性のないインターフェイスとしてプロトコルを宣言し、インターフェイスの基本インターフェイスの一覧でプロトコルを一覧表示します。

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

プロパティの戻り値の型のアラインメント値を指定するために使用します。 特定のプロパティでは、特定の境界に沿って配置する必要があるアドレスへのポインターを取得します (Xamarin では、たとえば、16バイトでアラインする必要があるいくつかの `GLKBaseEffect` プロパティを使用します)。 このプロパティを使用して getter を装飾し、alignment 値を使用できます。 これは通常、目的の C Api と統合するときに、`OpenTK.Vector4` および `OpenTK.Matrix4` 型と共に使用されます。

例:

```csharp
public interface GLKBaseEffect {
    [Export ("constantColor")]
    Vector4 ConstantColor { [Align (16)] get; set;  }
}
```

### <a name="appearanceattribute"></a>外観属性

`[Appearance]` 属性は、外観マネージャーが導入された iOS 5 に限定されています。

`[Appearance]` 属性は、`UIAppearance` framework に参加する任意のメソッドまたはプロパティに適用できます。 この属性がクラスのメソッドまたはプロパティに適用されると、このクラスのすべてのインスタンスのスタイルを設定するために使用される厳密に型指定された外観クラス、または特定の条件に一致するインスタンスを作成するために、バインディングジェネレーターによって指示されます。

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

メソッドとプロパティの `[AutoReleaseAttribute]` を使用して、`NSAutoReleasePool`内のメソッドへのメソッド呼び出しをラップします。

目的 C では、既定の `NSAutoReleasePool`に追加される値を返すメソッドがいくつかあります。 既定では、これらはスレッド `NSAutoReleasePool`に移動しますが、Xamarin. iOS はマネージオブジェクトが存在する限り、オブジェクトへの参照を保持するため、`NSAutoReleasePool` には追加の参照を保持しないようにします。これは、スレッドが戻るまでドレインされません。次のスレッドに制御を戻すか、メインループに戻ります。

この属性は、既定の `NSAutoReleasePool`に追加されたオブジェクトを返す、高負荷なプロパティ (`UIImage.FromFile`など) の例に適用されます。 この属性がない場合、スレッドがメインループに制御を返さない限り、画像は保持されます。 Uf スレッドが、常に生きていて作業を待機しているバックグラウンドダウンローダーの一種であった場合、イメージは解放されません。

### <a name="forcedtypeattribute"></a>ForcedTypeAttribute

`[ForcedTypeAttribute]` は、返されたアンマネージオブジェクトがバインディング定義で記述されている型と一致しない場合でも、マネージ型の作成を強制するために使用されます。

これは、ヘッダーに記述されている型が、ネイティブメソッドの戻り値の型と一致しない場合に便利です。たとえば、`NSURLSession`から次のような C の定義を使用します。

`- (NSURLSessionDownloadTask *)downloadTaskWithRequest:(NSURLRequest *)request`

これは `NSURLSessionDownloadTask` インスタンスを返すことを明確に示していますが、`NSURLSessionTask`を**返し**ます。これはスーパークラスであるため、`NSURLSessionDownloadTask`に変換できません。 これはタイプセーフなコンテキストであるため、`InvalidCastException` が発生します。

ヘッダーの説明に従って `InvalidCastException`を回避するには、`[ForcedTypeAttribute]` を使用します。

```csharp
[BaseType (typeof (NSObject), Name="NSURLSession")]
interface NSUrlSession {

    [Export ("downloadTaskWithRequest:")]
    [return: ForcedType]
    NSUrlSessionDownloadTask CreateDownloadTask (NSUrlRequest request);
}
```

`[ForcedTypeAttribute]` は、既定の `[ForcedType (owns: true)]`で `false` される `Owns` という名前のブール値も受け入れます。 所有しているパラメーターは、 **Core Foundation**オブジェクトの[所有権ポリシー](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html)に従うために使用されます。

`[ForcedTypeAttribute]` は、パラメーター、プロパティ、および戻り値でのみ有効です。

<a name="BindAsAttribute" />

### <a name="bindasattribute"></a>BindAsAttribute

`[BindAsAttribute]` を使用すると、`NSNumber`、`NSValue`、および `NSString`(列挙型C# ) をより正確な型にバインドできます。 属性を使用すると、ネイティブ API でより正確で正確な .NET API を作成できます。

メソッド (戻り値)、パラメーター、およびプロパティを `BindAs`で装飾できます。 唯一の制限は、メンバーが `[Protocol]` インターフェイスまたは[`[Model]`](#ModelAttribute)インターフェイス内に存在してはいけ**ない**ことです。

(例:

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

内部的には、`bool?` <-> `NSNumber` および `CGRect` <-> `NSValue` 変換を行います。

現在サポートされているカプセル化の種類は次のとおりです。

* `NSValue`
* `NSNumber`
* `NSString`

#### <a name="nsvalue"></a>NSValue

次C#のデータ型は、`NSValue`からのカプセル化がサポートされています。

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

次C#のデータ型は、`NSNumber`からのカプセル化がサポートされています。

* bool
* byte
* 二重線
* フローティング
* short
* int
* long
* sbyte
* ushort
* uint
* ulong
* nfloat
* nint
* nuint
* 列挙体

#### <a name="nsstring"></a>NSString

[`[BindAs]`](#BindAsAttribute)は、 [nsstring 定数によって列挙](#enum-attributes)された conjuntion で動作するため、より優れた .net API を作成できます。次に例を示します。

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

[`[BindAs]`](#BindAsAttribute)に指定された列挙型が[nsstring 定数によってサポート](#enum-attributes)されている場合にのみ、`enum` <-> `NSString` 変換を処理します。

#### <a name="arrays"></a>配列

また、サポートされている任意の型の配列もサポートしているため、次のような API 定義を例として使用できます。 [`[BindAs]`](#BindAsAttribute)

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

`rects` パラメーターは `CGRect` ごとに `NSValue` を含む `NSArray` にカプセル化され、返された `CAScroll?` を含む返された `NSArray` の値を使用して作成された `NSStrings`の配列を取得します。

<a name="BindAttribute" />

### <a name="bindattribute"></a>BindAttribute

`[Bind]` 属性には、メソッドまたはプロパティの宣言に適用する場合は2つ、プロパティの getter または setter に適用する場合は2つのを使用します。

メソッドまたはプロパティに対して使用する場合、`[Bind]` 属性の効果は、指定されたセレクターを呼び出すメソッドを生成することです。 ただし、生成されたメソッドは、 [`[Export]`](#ExportAttribute)属性では修飾されません。これは、メソッドのオーバーライドに参加できないことを意味します。 これは通常、目的の C 拡張メソッドを実装するために、`[Target]` 属性と組み合わせて使用されます。

(例:

```csharp
public interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

Getter または setter で使用した場合、`[Bind]` 属性を使用して、プロパティの getter および setter 目標-C セレクター名を生成するときに、コードジェネレーターによって推論される既定値を変更します。 既定では、`fooBar`という名前のプロパティにフラグを設定すると、ジェネレーターによって、セッターの getter および `setFooBar:` の `fooBar` エクスポートが生成されます。 場合によっては、目的の C がこの規則に従わないことがあります。通常は、getter 名を `isFooBar`に変更します。
この属性を使用して、こののジェネレーターを通知します。

(例:

```csharp
// Default behavior
[Export ("active")]
bool Active { get; set; }

// Custom naming with the Bind attribute
[Export ("visible")]
bool Visible { [Bind ("isVisible")] get; set; }
```

<a name="AsyncAttribute" />

### <a name="asyncattribute"></a>AsyncAttribute

Xamarin. iOS 6.3 以降でのみ使用できます。

この属性は、最後の引数として完了ハンドラーを受け取るメソッドに適用できます。

最後の引数がコールバックであるメソッドでは、`[Async]` 属性を使用できます。  メソッドにこのを適用すると、バインドジェネレーターは `Async`サフィックスを付けて、そのメソッドのバージョンを生成します。  コールバックがパラメーターを受け取らない場合、戻り値は `Task`になります。コールバックがパラメーターを受け取る場合、結果は `Task<T>`になります。

```csharp
[Export ("upload:complete:")]
[Async]
void LoadFile (string file, NSAction complete)
```

次の例では、この非同期メソッドが生成されます。

```csharp
Task LoadFileAsync (string file);
```

コールバックが複数のパラメーターを受け取る場合は、`ResultType` または `ResultTypeName` を設定して、すべてのプロパティを保持する、生成される型の目的の名前を指定する必要があります。

```csharp
delegate void OnComplete (string [] files, nint byteCount);

[Export ("upload:complete:")]
[Async (ResultTypeName="FileLoading")]
void LoadFiles (string file, OnComplete complete)
```

次の例では、`files` と `byteCount`の両方にアクセスするためのプロパティが `FileLoading` に含まれています。この非同期メソッドが生成されます。

```csharp
Task<FileLoading> LoadFile (string file);
```

コールバックの最後のパラメーターが `NSError`の場合、生成された `Async` メソッドは、値が null でないかどうかを確認します。その場合は、生成された非同期メソッドによってタスクの例外が設定されます。

```csharp
[Export ("upload:onComplete:")]
[Async]
void Upload (string file, Action<string,NSError> onComplete);
```

上の例では、次の非同期メソッドが生成されます。

```csharp
Task<string> UploadAsync (string file);
```

また、エラーが発生した場合は、結果の `NSError`をラップする `NSErrorException` に例外が設定されます。

#### <a name="asyncattributeresulttype"></a>AsyncAttribute。 ResultType

このプロパティを使用して、返される `Task` オブジェクトの値を指定します。   このパラメーターは既存の型を受け取ります。そのため、コア api 定義の1つで定義する必要があります。

#### <a name="asyncattributeresulttypename"></a>AsyncAttribute. ResultTypeName

このプロパティを使用して、返される `Task` オブジェクトの値を指定します。   このパラメーターは、目的の型名の名前を受け取ります。ジェネレーターは、コールバックによって実行されるパラメーターごとに1つずつ、一連のプロパティを生成します。

#### <a name="asyncattributemethodname"></a>AsyncAttribute. MethodName

このプロパティを使用して、生成される非同期メソッドの名前をカスタマイズします。   既定では、メソッドの名前を使用して "Async" というテキストを追加します。これを使用すると、この既定値を変更できます。

<a name="DesignatedInitializerAttribute" />

### <a name="designatedinitializerattribute"></a>Designated初期化 Erattribute

この属性がコンストラクターに適用されると、最終的なプラットフォームアセンブリで同じ `[DesignatedInitializer]` が生成されます。 これは、IDE がサブクラスでどのコンストラクターを使用する必要があるかを示すのに役立ちます。

これは、`__attribute__((objc_designated_initializer))`の目的、C、clang の使用にマップする必要があります。

<a name="DisableZeroCopyAttribute" />

### <a name="disablezerocopyattribute"></a>Disableゼロ Copyattribute

この属性は、文字列パラメーターまたは文字列プロパティに適用され、このパラメーターに対してゼロコピーの文字列のマーシャリングを使用しないようにコードジェネレーターに指示しますC# 。代わりに、文字列から新しい nsstring インスタンスを作成します。
この属性は、`--zero-copy` のコマンドラインオプションを使用するか、アセンブリレベルの属性 `ZeroCopyStringsAttribute`を設定することによって、ゼロコピーの文字列のマーシャリングを使用するようジェネレーターに指示する場合にのみ、文字列に必要です。

これは、プロパティが、`copy` プロパティではなく、`retain` または `assign` プロパティとして使用される場合に必要です。 これらは通常、開発者が誤って "最適化" したサードパーティのライブラリで発生します。 一般に、`NSMutableString` またはユーザーによって派生した `NSString` では、ライブラリコードに関する知識がなくても文字列の内容を変更する可能性があるため、`retain` または `assign` の `NSString` のプロパティは正しくありません。 通常、これは、最適化が早すぎることが原因で発生します。

次に、このような2つのプロパティを示します。

```csharp
@property(nonatomic,retain) NSString *name;
@property(nonatomic,assign) NSString *name2;
```

<a name="DisposeAttribute" />

### <a name="disposeattribute"></a>DisposeAttribute

クラスに `[DisposeAttribute]` を適用する場合は、クラスの `Dispose()` メソッドの実装に追加されるコードスニペットを指定します。

`Dispose` メソッドは `bmac-native` ツールと `btouch-native` ツールによって自動的に生成されるため、`[Dispose]` の属性を使用して、生成された `Dispose` メソッドの実装にコードを挿入する必要があります。

(例:

```csharp
[BaseType (typeof (NSObject))]
[Dispose ("if (OpenConnections > 0) CloseAllConnections ();")]
interface DatabaseConnection {
}
```

<a name="ExportAttribute" />

### <a name="exportattribute"></a>ExportAttribute

`[Export]` 属性は、目的の C ランタイムに公開されるメソッドまたはプロパティにフラグを設定するために使用されます。 この属性は、バインドツールと、実際の Xamarin、iOS、および Xamarin. Mac ランタイム間で共有されます。 メソッドの場合、パラメーターは生成されたコードに逐語的に渡されます。プロパティの場合は、基本宣言に基づいて getter および setter のエクスポートが生成されます (バインディングツールの動作を変更する方法の詳細については、 [`[BindAttribute]`](#BindAttribute)の「」を参照してください)。

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

<a name="FieldAttribute" />

### <a name="fieldattribute"></a>FieldAttribute

この属性は、必要に応じて読み込まれ、コードにC#公開されるフィールドとして C グローバル変数を公開するために使用されます。 通常、これは、C または目的 C で定義されている定数の値を取得するために必要です。また、一部の Api で使用されるトークンであるか、または値が非透過的であり、ユーザーコードによってそのように使用される必要があります。

構文:

```csharp
public class FieldAttribute : Attribute {
    public FieldAttribute (string symbolName);
    public FieldAttribute (string symbolName, string libraryName);
    public string SymbolName { get; set; }
    public string LibraryName { get; set; }
}
```

`symbolName` は、リンク先の C 記号です。 既定では、このは、型が定義されている名前空間から推論される名前を持つライブラリから読み込まれます。 シンボルが検索されるライブラリでない場合は、`libraryName` パラメーターを渡す必要があります。 スタティックライブラリをリンクしている場合は、`libraryName` パラメーターとして `__Internal` を使用します。

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
* 列挙体

Setter は、 [NSString 定数によっ](#enum-attributes)てサポートされる列挙型ではサポートされていませんが、必要に応じて手動でバインドできます。

例:

```csharp
[Static]
interface CameraEffects {
     [Field ("kCameraEffectsZoomFactorKey", "CameraLibrary")]
     NSString ZoomFactorKey { get; }
}
```

<a name="InternalAttribute" />

### <a name="internalattribute"></a>InternalAttribute

`[Internal]` 属性は、メソッドまたはプロパティに適用できます。また、生成されたコードに対してC# 、生成されたアセンブリ内のコードのみがアクセスできるようにするために、`internal` キーワードを使用してフラグを設定するという効果があります。 これは通常、低レベルの Api を非表示にしたり、改善する必要のある最適ではないパブリック API を提供したり、ジェネレーターでサポートされておらず、何らかのハンドコーディングを必要とする api を提供したりするために使用されます。

バインディングをデザインするときは、通常、この属性を使用してメソッドまたはプロパティを非表示にし、メソッドまたはプロパティに別のC#名前を指定します。次に、補完的なサポートファイルで、を公開する厳密に型指定されたラッパーを追加します。基になる機能。

(例:

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

<a name="IsThreadStaticAttribute" />

### <a name="isthreadstaticattribute"></a>IsThreadStaticAttribute

この属性は、.NET `[ThreadStatic]` 属性で注釈を付けるプロパティのバッキングフィールドにフラグを設定します。 これは、フィールドがスレッドの静的変数である場合に便利です。

### <a name="marshalnativeexceptions-xamarinios-606"></a>Marshalの例外 (Xamarin 6.0.6)

この属性は、メソッドがネイティブ (目的 C) の例外をサポートするようにします。
`objc_msgSend` を直接呼び出すのではなく、呼び出しはカスタム trampoline を通じて実行され、その例外をキャッチし、それらをマネージ例外にマーシャリングします。

現在サポートされている `objc_msgSend` 署名はごくわずかです (バインディングを使用するアプリのネイティブリンクが monotouch_ の*送信*シンボルで失敗した場合、署名がサポートされていないかどうかを確認できます) が、要求に追加することができます。

### <a name="newattribute"></a>NewAttribute

この属性は、ジェネレーターが宣言の前に `new` キーワードを生成するように、メソッドとプロパティに適用されます。

基底クラスに既に存在するサブクラスで同じメソッドまたはプロパティ名が導入された場合に、コンパイラの警告を回避するために使用されます。

<a name="NotificationAttribute" />

### <a name="notificationattribute"></a>NotificationAttribute

この属性をフィールドに適用して、ジェネレーターが厳密に型指定されたヘルパー通知クラスを生成するようにすることができます。

この属性は、ペイロードを含まない通知の引数なしで使用できます。また、API 定義で別のインターフェイスを参照する `System.Type` を指定することもできます。この場合、通常は "EventArgs" で終わる名前が使用されます。 ジェネレーターは、`EventArgs` サブクラスを持つクラスにインターフェイスを変換し、そこに一覧表示されているすべてのプロパティを含みます。 `EventArgs` クラスでは、 [`[Export]`](#ExportAttribute)属性を使用して、値を取得する目的の C 辞書を検索するために使用するキーの名前を一覧表示する必要があります。

(例:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

上記のコードでは、次のメソッドを使用して、入れ子になったクラス `MyClass.Notifications` が生成されます。

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
      public static NSObject ObserveDidStart (NSObject objectToObserve, EventHandler<NSNotificationEventArgs> handler)
   }
}
```

コードのユーザーは、次のようなコードを使用して、 [Nsdefaultcenter](xref:Foundation.NSNotificationCenter.DefaultCenter)にポストされた通知を簡単にサブスクライブできます。

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

または、監視する特定のオブジェクトを設定します。 `objectToObserve` に `null` を渡すと、このメソッドは他のピアと同様に動作します。

```csharp
var token = MyClass.Notifications.ObserverDidStart (objectToObserve, (notification) => {
    Console.WriteLine ("Observed the 'DidStart' event on objectToObserve!");
});
```

次のように、`ObserveDidStart` から返された値を使用して、通知の受信を簡単に停止できます。

```csharp
token.Dispose ();
```

または、 [Nsnotification](xref:Foundation.NSNotificationCenter.RemoveObserver(Foundation.NSObject))を呼び出して、トークンを渡すこともできます。 通知にパラメーターが含まれている場合は、次のように、ヘルパー `EventArgs` インターフェイスを指定する必要があります。

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

上の例では、`ScreenX` プロパティと `ScreenY` プロパティを使用して `MyScreenChangedEventArgs` クラスが生成され[ます。](xref:Foundation.NSNotification.UserInfo)これらのプロパティは、それぞれキー `ScreenXKey` および `ScreenYKey` を使用して、適切な変換を適用します。 `[ProbePresence]` 属性は、値を抽出するのではなく、`UserInfo`でキーが設定されているかどうかを検査するためにジェネレーターで使用されます。 これは、キーの存在が値である場合 (通常はブール値の場合) に使用されます。

これにより、次のようなコードを記述できます。

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

場合によっては、ディクショナリで渡された値に定数が関連付けられていないことがあります。  Apple はパブリックシンボル定数を使用することがあり、場合によっては文字列定数を使用します。  既定では、指定した `EventArgs` クラスの[`[Export]`](#ExportAttribute)属性は、実行時に検索されるパブリックシンボルとして指定された名前を使用します。  そうでない場合は、文字列定数として検索されるので、`ArgumentSemantic.Assign` 値を Export 属性に渡します。

**Xamarin の新。 iOS 8.4**

場合によっては、通知の有効期間が引数なしで開始されることがあるため、引数を指定せずに[`[Notification]`](#NotificationAttribute)を使用することは許容されます。  しかし、通知のパラメーターが導入されることもあります。  このシナリオをサポートするには、属性を複数回適用できます。

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

<a name="NullAllowedAttribute" />

### <a name="nullallowedattribute"></a>NullAllowedAttribute

これがプロパティに適用されると、値 `null` を割り当てることができるようにプロパティにフラグが設定されます。 これは、参照型に対してのみ有効です。

これがメソッドシグネチャのパラメーターに適用されると、指定されたパラメーターが null になる可能性があること、および `null` 値を渡すためにチェックを実行する必要がないことを示します。

参照型にこの属性がない場合、バインディングツールは、割り当てられている値のチェックを生成してから目的の C に渡します。割り当てられた値が `null`場合は、`ArgumentNullException` をスローするチェックが生成されます。

(例:

```csharp
// In properties

[NullAllowed]
UIImage IconFile { get; set; }

// In methods
void SetImage ([NullAllowed] UIImage image, State forState);
```

<a name="OverrideAttribute" />

### <a name="overrideattribute"></a>OverrideAttribute

この属性を使用して、この特定のメソッドのバインディングに `override` キーワードでフラグを設定する必要があることをバインディングジェネレーターに指示します。

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

この場合、`Dependencies` プロパティは、`NSOperation` オブジェクトの依存関係を追加または削除した後に呼び出され、実際に読み込まれたオブジェクトを表すグラフがあることを確認します。これにより、メモリリークだけでなく、メモリの破損を防ぐことができます。

### <a name="postsnippetattribute"></a>PostSnippetAttribute

この属性を使用して、コードC#が基になる目標 C メソッドを呼び出した後に挿入されるソースコードを挿入できます。

例:

```csharp
[Export ("demo")]
[PostSnippet ("if (old != null) old.DemoComplete ();")]
void Demo ();
```

### <a name="proxyattribute"></a>ProxyAttribute

この属性は、プロキシオブジェクトとしてフラグを設定する戻り値に適用されます。 一部の目的 C Api は、ユーザーバインドと区別できないプロキシオブジェクトを返します。 この属性の効果は、オブジェクトを `DirectBinding` オブジェクトとしてフラグを付けることです。 Xamarin. Mac のシナリオでは、[このバグに](https://bugzilla.novell.com/show_bug.cgi?id=670844)ついての説明を参照できます。

### <a name="retainlistattribute"></a>RetainListAttribute

パラメーターへのマネージ参照を保持するか、パラメーターへの内部参照を削除するようジェネレーターに指示します。 これは、オブジェクトを参照するために使用されます。

構文:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

`doAdd` の値が true の場合、パラメーターが `__mt_{0}_var List<NSObject>;`に追加されます。 ここで `{0}` は、指定した `listName`に置き換えられます。 相補的な部分クラスでこのバッキングフィールドを API に宣言する必要があります。

例については、「 [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) and [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs) 」を参照してください。

### <a name="releaseattribute-xamarinios-60"></a>ReleaseAttribute (Xamarin. iOS 6.0)

これは、を返す前に、ジェネレーターがオブジェクトの `Release` を呼び出す必要があることを示すために、戻り値の型に適用できます。 これは、メソッドが保持されたオブジェクトを提供する場合にのみ必要です。これは、最も一般的なシナリオである autoreleased オブジェクトとは対照的です。

例:

```csharp
[Export ("getAndRetainObject")]
[return: Release ()]
NSObject GetAndRetainObject ();
```

さらに、この属性は生成されたコードに反映されるため、Xamarin の iOS ランタイムは、このような関数から目的の C に戻るときにオブジェクトを保持する必要があることを認識します。

### <a name="sealedattribute"></a>SealedAttribute

生成されたメソッドにシールとしてフラグを付けるようジェネレーターに指示します。 この属性が指定されていない場合、既定では、仮想メソッド (仮想メソッド、抽象メソッド、または他の属性の使用方法に応じたオーバーライド) が生成されます。

<a name="StaticAttribute" />

### <a name="staticattribute"></a>StaticAttribute

`[Static]` 属性がメソッドまたはプロパティに適用されると、静的メソッドまたは静的プロパティが生成されます。 この属性が指定されていない場合、ジェネレーターはインスタンスメソッドまたはプロパティを生成します。

### <a name="transientattribute"></a>遷移属性

この属性を使用して、一時的に値が設定されているプロパティ、つまり iOS によって一時的に作成され、有効期間の短いオブジェクトにフラグを設定します。 この属性がプロパティに適用されている場合、ジェネレーターはこのプロパティのバッキングフィールドを作成しません。つまり、マネージクラスはオブジェクトへの参照を保持しません。

<a name="WrapAttribute" />

### <a name="wrapattribute"></a>WrapAttribute

Xamarin. iOS/Xamarin. Mac バインドの設計では、厳密に型指定されたオブジェクトを使用して、弱い型指定のオブジェクトをラップするために、`[Wrap]` 属性が使用されます。 これは、主に、`id` または `NSObject`型として宣言されている目的 C のデリゲートオブジェクトを使用して動作します。 Xamarin. iOS と Xamarin. Mac で使用される規則は、これらのデリゲートまたはデータソースを `NSObject` 型として公開し、公開される名前という規則を使用して名前を付けます。 目標 C からの `id delegate` プロパティは、API コントラクトファイルの `NSObject WeakDelegate { get; set; }` プロパティとして公開されます。

ただし、通常、このデリゲートに割り当てられる値は厳密な型であるため、厳密な型を使用して `[Wrap]` 属性を適用します。これは、ユーザーが細かい制御が必要な場合、または低レベルのテクニックに頼る必要がある場合に、弱い型を使用することを選択できることを意味します。または、ほとんどの作業に対して厳密に型指定されたプロパティを使用できます。

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

これは、ユーザーが厳密に型指定されたバージョンを使用する方法であり、ユーザー C#が型システムを利用し、その意図を宣言するために override キーワードを使用しており、メソッドを手動で装飾する必要がないことに注意してください `[Export]`これは、ユーザーのバインドで動作しているためです。

```csharp
// This is the strong case,
class MyDelegate : DemoDelegate {
   override void Demo DoDemo () {}
}

var strongDemo = new Demo ();
demo.Delegate = new MyDelegate ();
```

`[Wrap]` 属性のもう1つの用途は、厳密に型指定されたメソッドのバージョンをサポートすることです。  (例:

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

`[Category]` 属性で修飾された型内のメソッドに `[Wrap]` 属性が適用される場合、拡張メソッドが生成されるため、`This` を最初の引数として含める必要があります。 (例:

```csharp
[Wrap ("Write (This, image, options?.Dictionary, out error)")]
bool Write (CIImage image, CIImageRepresentationOptions options, out NSError error);
```

`[Wrap]` によって生成されるメンバーは、既定では `virtual` されません。 `virtual` メンバーが必要な場合は、オプションの `isVirtual` パラメーターを `true` に設定できます。

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

ここでは、メソッド定義のパラメーターに適用できる属性と、プロパティ全体に適用される `[NullAttribute]` について説明します。

<a name="BlockCallback" />

### <a name="blockcallback"></a>BlockCallback

この属性は、問題のパラメーターがC#目的の C ブロック呼び出し規約に準拠していることをバインダーに通知するために、デリゲート宣言のパラメーターの型に適用され、この方法でマーシャリングする必要があります。

これは通常、次のように定義されているコールバックで使用されます。

```objc
typedef returnType (^SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

参照: [Ccallback](#CCallback)

<a name="CCallback" />

### <a name="ccallback"></a>CCallback

この属性は、問題のパラメーターがC# C ABI 関数ポインターの呼び出し規約に準拠していることをバインダーに通知するために、デリゲート宣言のパラメーターの型に適用され、この方法でマーシャリングする必要があります。

これは通常、次のように定義されているコールバックで使用されます。

```objc
typedef returnType (*SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

「 [Blockcallback](#BlockCallback)」も参照してください。

### <a name="params"></a>Params

メソッド定義の最後の配列パラメーターで `[Params]` 属性を使用して、ジェネレーターが定義に "params" を挿入できるようにすることができます。   これにより、バインドで省略可能なパラメーターを簡単に使用できるようになります。

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

<a name="plainstring" />

### <a name="plainstring"></a>PlainString

文字列パラメーターの前に `[PlainString]` 属性を使用して、パラメーターを `NSString`として渡すのではなく、文字列を C 文字列として渡すようにバインドジェネレーターに指示できます。

ほとんどの目的 C Api は `NSString` パラメーターを使用しますが、いくつかの Api は `NSString` のバリエーションではなく、文字列を渡すための `char *` API を公開します。
そのような場合は、`[PlainString]` を使用します。

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

指定されたパラメーターへの参照を保持するようジェネレーターに指示します。 ジェネレーターによって、このフィールドのバッキングストアが提供されます。また、値を格納する名前 (`WrapName`) を指定することもできます。 これは、目的の C にパラメーターとして渡されるマネージオブジェクトへの参照を保持するのに便利です。また、C はこのオブジェクトのコピーのみを保持することがわかっている場合に便利です。 たとえば、`SetDisplay (SomeObject)` のような API では、SetDisplay で一度に1つのオブジェクトしか表示できない可能性があるため、この属性が使用されます。 複数のオブジェクトを追跡する必要がある場合 (たとえば、スタックに似た API の場合) は、`[RetainList]` 属性を使用します。

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

`doAdd` の値が true の場合、パラメーターが `__mt_{0}_var List<NSObject>`に追加されます。 ここで `{0}` は、指定した `listName`に置き換えられます。 相補的な部分クラスでこのバッキングフィールドを API に宣言する必要があります。

例については、「 [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) and [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs) 」を参照してください。

### <a name="transientattribute"></a>遷移属性

この属性はパラメーターに適用され、目的の C からにC#移行する場合にのみ使用されます。  これらの遷移では、さまざまな目的 C `NSObject` パラメーターがオブジェクトのマネージ表現にラップされます。

ランタイムは、ネイティブオブジェクトへの参照を受け取り、オブジェクトへの最後に管理された参照がなくなるまで参照を保持し、GC は実行される可能性があります。

場合によっては、ランタイムがC#ネイティブオブジェクトへの参照を保持しないようにすることが重要です。  これは、基になるネイティブコードがパラメーターのライフサイクルに特別な動作をアタッチした場合に発生することがあります。  例: パラメーターのデストラクターはクリーンアップアクションを実行するか、貴重なリソースを破棄します。

この属性は、上書きされたメソッドから目的の C に戻るときに、可能であればオブジェクトを破棄する必要があることをランタイムに通知します。

規則は単純です。ランタイムがネイティブオブジェクトから新しいマネージ表現を作成する必要がある場合、関数の末尾で、ネイティブオブジェクトの保持カウントが削除され、マネージオブジェクトの Handle プロパティがクリアされます。   つまり、マネージオブジェクトへの参照を保持した場合、その参照は役に立たなくなります (メソッドを呼び出すと例外がスローされます)。

渡されたオブジェクトが作成されなかった場合、またはオブジェクトの未処理のマネージ表現が既に存在していた場合、強制破棄は行われません。 

## <a name="property-attributes"></a>プロパティ属性

<a name="NotImplementedAttribute" />

### <a name="notimplementedattribute"></a>NotImplementedAttribute

この属性は、getter を持つプロパティが基底クラスで導入され、変更可能なサブクラスが setter を導入する、目的 C の表現をサポートするために使用されます。

でC#はこのモデルがサポートされていないため、基底クラスは setter と getter の両方を持つ必要があり、サブクラスは[overrideattribute](#OverrideAttribute)を使用できます。

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

<a name="enum-attributes" />

## <a name="enum-attributes"></a>列挙型の属性

`NSString` 定数を列挙値にマッピングすると、より適切な .NET API を簡単に作成できます。 し

* API に対して正しい値**のみ**を表示することにより、コード補完をより効果的にすることができます。
* タイプセーフを追加します。間違ったコンテキストで別の `NSString` 定数を使用することはできません。そして
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

上記のバインディング定義から、ジェネレーターは `enum` 自体を作成します。また、列挙値と `NSString` 定数間の2つの方法の変換メソッドを含む静的な型 `*Extensions` も作成します。 つまり、API に含まれていない場合でも、開発者は定数を使用できます。

次に例を示します。

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

この属性を使用して、 **1 つ**の列挙値を装飾できます。 これは、列挙値が不明な場合に返される定数になります。

上記の例から:

```csharp
var x = (NSRunLoopMode) 99;
Call (x.GetConstant ()); // NSDefaultRunLoopMode will be used
```

修飾されている列挙値がない場合は、`NotSupportedException` がスローされます。

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

次に、拡張メソッド `GetDomain` を呼び出して、任意のエラーのドメイン定数を取得できます。

### <a name="fieldattribute"></a>FieldAttribute

これは、型の中の定数に使用される `[Field]` 属性と同じです。 また、特定の定数に値をマップするために、列挙型の内部で使用することもできます。

`null` 値を使用して、`null` `NSString` 定数が指定されている場合に返される列挙値を指定できます。

上記の例から:

```csharp
var constant = NSRunLoopMode.NewInWatchOS3; // will be null in watchOS 2.x
Call (NSRunLoopModeExtensions.GetValue (constant)); // will return 1000
```

`null` 値が存在しない場合は、`ArgumentNullException` がスローされます。

## <a name="global-attributes"></a>グローバル属性

グローバル属性は、 [`[LinkWithAttribute]`](#LinkWithAttribute)のような `[assembly:]` 属性修飾子を使用して適用されるか、 [`[Lion]`](#SinceAndLionAttributes)や[`[Since]`](#SinceAndLionAttributes)属性のように任意の場所で使用できます。

<a name="LinkWithAttribute" />

### <a name="linkwithattribute"></a>LinkWithAttribute

これはアセンブリレベルの属性であり、開発者はライブラリのコンシューマーに対して、ライブラリに渡された gcc_flags および extra mtouch 引数を手動で構成することなく、バインドされたライブラリを再利用するために必要なリンクフラグを指定できます。

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

この属性はアセンブリレベルで適用されます。たとえば、 [Coreplot バインディング](https://github.com/mono/monotouch-bindings/tree/master/CorePlot)で使用されるのは次のようになります。

```csharp
[assembly: LinkWith ("libCorePlot-CocoaTouch.a", LinkTarget.ArmV7 | LinkTarget.ArmV7s | LinkTarget.Simulator, Frameworks = "CoreGraphics QuartzCore", ForceLoad = true)]
```

`[LinkWith]` 属性を使用すると、指定された `libraryName` が生成されたアセンブリに埋め込まれます。これにより、ユーザーは、アンマネージ依存関係と、からライブラリを適切に使用するために必要なコマンドラインフラグの両方を含む単一の DLL を出荷できるようになります。Xamarin. iOS.

また、`libraryName`を提供しないことも可能です。この場合、`LinkWith` 属性を使用して、追加のリンカーフラグのみを指定できます。

``` csharp
[assembly: LinkWith (LinkerFlags = "-lsqlite3")]
```

#### <a name="linkwithattribute-constructors"></a>LinkWithAttribute コンストラクター

これらのコンストラクターを使用すると、ライブラリを指定して、結果のアセンブリにリンクして埋め込むことができます。また、ライブラリでサポートされているサポート対象のターゲット、およびライブラリとリンクするために必要な任意のライブラリフラグを指定することもできます。

`LinkTarget` 引数は Xamarin. iOS によって推論されるため、設定する必要はありません。

次に例を示します。

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

`ForceLoad` プロパティは、ネイティブライブラリのリンクに `-force_load` リンクフラグを使用するかどうかを決定するために使用されます。 現時点では、これは常に true である必要があります。

#### <a name="linkwithattributeframeworks"></a>LinkWithAttribute. フレームワーク

バインドされているライブラリが、(`Foundation` および `UIKit`を除く) どのフレームワークでもハード要件を持つ場合は、`Frameworks` プロパティを、必要なプラットフォームフレームワークのスペース区切りの一覧を含む文字列に設定する必要があります。 たとえば、`CoreGraphics` と `CoreText`を必要とするライブラリをバインドする場合は、`Frameworks` プロパティを `"CoreGraphics CoreText"`に設定します。

#### <a name="linkwithattributeiscxx"></a>LinkWithAttribute. IsCxx

生成された実行可能ファイルを、既定値ではなくC++コンパイラを使用してコンパイルする必要がある場合は、このプロパティを true に設定します。 バインドしているライブラリがにC++記述されている場合は、これを使用します。

#### <a name="linkwithattributelibraryname"></a>LinkWithAttribute. LibraryName

バンドルするアンマネージライブラリの名前。 これは拡張子が ". a" のファイルで、複数のプラットフォーム (たとえば、シミュレーターの場合は ARM、x86) のオブジェクトコードを含むことができます。

以前のバージョンの Xamarin。 iOS は、ライブラリがサポートされているプラットフォームを特定するために `LinkTarget` プロパティをチェックしましたが、これは自動検出され、`LinkTarget` プロパティは無視されます。

#### <a name="linkwithattributelinkerflags"></a>LinkWithAttribute. LinkerFlags

`LinkerFlags` 文字列は、作成者がネイティブライブラリをアプリケーションにリンクするときに必要な追加のリンカーフラグを指定するための方法を提供します。

たとえば、ネイティブライブラリに libxml2 と zlib が必要な場合は、`LinkerFlags` 文字列を `"-lxml2 -lz"`に設定します。

#### <a name="linkwithattributelinktarget"></a>LinkWithAttribute. LinkTarget

以前のバージョンの Xamarin。 iOS は、ライブラリがサポートされているプラットフォームを特定するために `LinkTarget` プロパティをチェックしましたが、これは自動検出され、`LinkTarget` プロパティは無視されます。

#### <a name="linkwithattributeneedsgccexceptionhandling"></a>LinkWithAttribute. NeedsGccExceptionHandling

リンクするライブラリに GCC 例外処理ライブラリ (gcc_eh) が必要な場合は、このプロパティを true に設定します。

#### <a name="linkwithattributesmartlink"></a>LinkWithAttribute. SmartLink

`SmartLink` プロパティは、`ForceLoad` が必要かどうかを判断するために true に設定する必要があります。

#### <a name="linkwithattributeweakframeworks"></a>LinkWithAttribute. このフレームワーク

`WeakFrameworks` プロパティは、`Frameworks` プロパティと同じように動作しますが、リンク時には、一覧表示されている各フレームワークの gcc に `-weak_framework` 指定子が渡される点が異なります。

`WeakFrameworks` を使用すると、ライブラリとアプリケーションをプラットフォームフレームワークに対して脆弱な状態にすることが可能になり、使用可能な場合には必要に応じて使用できます。また、ライブラリが機能を追加することを意図している場合に便利です。新しいバージョンの iOS。 弱いリンクの詳細については、[弱リンク](https://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WeakLinking.html)に関する Apple のドキュメントを参照してください。

脆弱なリンクの候補は、iOS 5 でのみ利用可能であるため、アカウント、`CoreBluetooth`、`CoreImage`、`GLKit`、`NewsstandKit`、`Twitter` のように `Frameworks` ます。

<a name="SinceAndLionAttributes" />

### <a name="sinceattribute-ios-and-lionattribute-macos"></a>SinceAttribute (iOS) と LionAttribute (macOS)

`[Since]` 属性を使用して、特定の時点で導入されているものとして Api にフラグを付けることができます。 属性は、基になるクラス、メソッド、またはプロパティが使用できない場合に、ランタイムの問題を引き起こす可能性のある型およびメソッドにフラグを設定するためにのみ使用する必要があります。

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

`[Lion]` 属性は、ライオンで導入された型に対して同じ方法で適用されます。 IOS で使用されている特定のバージョン番号と `[Lion]` 使用する理由は、iOS は非常に頻繁に変更されますが、主要な OS X のリリースはめったに発生せず、オペレーティングシステムのバージョン番号よりもコードネームによってオペレーティングシステムを覚える方が簡単であるということです。

### <a name="adviceattribute"></a>AdviceAttribute

この属性は、開発者が使用するのに便利な他の Api に関するヒントを開発者に提供するために使用します。   たとえば、厳密に型指定された API のバージョンを指定した場合、弱い型指定の属性でこの属性を使用して、開発者により優れた API を提供できます。

この属性の情報は、ドキュメントに記載されています。ツールを開発して、

### <a name="requiressuperattribute"></a>RequiresSuperAttribute

これは、`[Advice]` 属性の特化されたサブクラスであり、メソッドをオーバーライドする場合に、基本 (オーバーライドされた) メソッドを呼び出す**必要**があることを開発者にヒントに使用できます。

これは `clang` に対応[`__attribute__((objc_requires_super))`](https://clang.llvm.org/docs/AttributeReference.html#objc-requires-super)

### <a name="zerocopystringsattribute"></a>ZeroCopyStringsAttribute

Xamarin 5.4 以降でのみ使用できます。

この属性は、この特定のライブラリ (`[assembly:]`に適用されている場合) または型のバインドが高速ゼロコピー文字列のマーシャリングを使用する必要があることをジェネレーターに指示します。 この属性は、コマンドラインオプション `--zero-copy` をジェネレーターに渡すことと同じです。

文字列にゼロコピーを使用する場合、ジェネレーターは、新しい`NSString`C#オブジェクトを作成せずに、文字列からC#目的のデータをコピーすることを避けるために、目的の c で使用される文字列と同じ文字列を効果的に使用します。文字列. 0個のコピー文字列を使用する唯一の欠点は、ラップする文字列プロパティに `retain` または `[DisableZeroCopy]` `copy` としてフラグが設定されているかどうかを確認する必要があることです。 これは、ゼロコピー文字列のハンドルがスタックに割り当てられており、関数の戻り値では無効であるために必要です。

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

Xamarin. iOS 8.0 では、`NSDictionaries`をラップする厳密に型指定されたクラスを簡単に作成できるようになりました。

常に、 [Dictionarycontainer](xref:Foundation.DictionaryContainer)データ型を手動の API と共に使用できるようになりましたが、これははるかに簡単になりました。  詳細については、「[厳密な型の提示](~/cross-platform/macios/binding/objective-c-libraries.md#Surfacing_Strong_Types)」を参照してください。

<a name="StrongDictionary" />

### <a name="strongdictionary"></a>StrongDictionary

この属性がインターフェイスに適用されると、ジェネレーターは、 [Dictionarycontainer](xref:Foundation.DictionaryContainer)から派生したインターフェイスと同じ名前のクラスを生成し、インターフェイスで定義されている各プロパティを、厳密に型指定された getter とに対する setter に変換します。バイナリ.

これにより、既存の `NSDictionary` からインスタンス化したり、新しいを作成したりできるクラスが自動的に生成されます。

この属性は、1つのパラメーターを受け取ります。これは、ディクショナリの要素にアクセスするために使用されるキーを含むクラスの名前です。   既定では、属性を持つインターフェイスの各プロパティは、"Key" というサフィックスが付いた名前の指定された型のメンバーを参照します。

(例:

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

上記の例では、`MyOption` クラスによって、文字列を取得するためにディクショナリにキーとして `MyOptionKeys.NameKey` を使用する `Name` の文字列プロパティが生成されます。   とは、`MyOptionKeys.AgeKey` をキーとしてディクショナリに使用して、int を含む `NSNumber` を取得します。

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

`StrongDictionary` 定義では、次のデータ型がサポートされています。

|C#インターフェイスの種類|`NSDictionary` ストレージの種類|
|---|---|
|`bool`|`NSNumber` に格納されている `Boolean`|
|列挙値|`NSNumber` に格納されている整数|
|`int`|32-`NSNumber` に格納されているビット整数|
|`uint`|`NSNumber` に格納されている32ビット符号なし整数|
|`nint`|`NSNumber` に格納されている `NSInteger`|
|`nuint`|`NSNumber` に格納されている `NSUInteger`|
|`long`|64-`NSNumber` に格納されているビット整数|
|`float`|`NSNumber` として格納された32ビット整数|
|`double`|`NSNumber` として格納された64ビット整数|
|`NSObject` およびサブクラス|`NSObject`|
|`NSDictionary`|`NSDictionary`|
|`string`|`NSString`|
|`NSString`|`NSString`|
|C#`NSObject`の`Array`|`NSArray`|
|C#列挙型の`Array`|`NSNumber` 値を含む `NSArray`|
