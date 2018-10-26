---
title: バインドの種類のリファレンス ガイド
description: このリファレンス ガイドは、さまざまな属性とを作成するときに理解しておくために必要な概念について説明します。 C# Objective C ライブラリへのバインド。
ms.prod: xamarin
ms.assetid: C6618E9D-07FA-4C84-D014-10DAC989E48D
author: conceptdev
ms.author: crdun
ms.date: 03/06/2018
ms.openlocfilehash: 369e1a37cc75bb4d10cc71d8f79ed1dd473378ba
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119437"
---
# <a name="binding-types-reference-guide"></a>バインドの種類のリファレンス ガイド

このドキュメントは、API コントラクトのファイル、バインドをドライブに注釈を設定する際、コードを生成する属性の一覧を説明します

Xamarin.iOS および Xamarin.Mac API コントラクトがで記述されたC#Objective C コードを明らかにする方法を定義するインターフェイス定義として主C#します。 プロセスではさまざまなインターフェイスの宣言といくつかの基本的な種類の定義、API コントラクトが必要な場合があります。 バインドの種類の概要については、当社の必携ガイドを参照してください。 [Objective-c ライブラリのバインド](~/cross-platform/macios/binding/objective-c-libraries.md)します。

## <a name="type-definitions"></a>型定義

構文:

```csharp
[BaseType (typeof (BTYPE))
interface MyType [: Protocol1, Protocol2] {
     IntPtr Constructor (string foo);
}
```

コントラクトが定義されているすべてのインターフェイス、 [ `[BaseType]` ](#BaseTypeAttribute)生成されたオブジェクトの基本型を宣言する属性。 上記の宣言で、`MyType`クラスC#Objective C 型へのバインドがという型が生成される`MyType`します。

型名の後に任意の種類を指定する場合 (上記のサンプルで`Protocol1`と`Protocol2`) インターフェイスの継承の構文を使用してこれらのインターフェイスの内容はインラインになりますのコントラクトの一部であった場合、`MyType`します。
Xamarin.iOS サーフェスの型が、プロトコルを採用して方法メソッドと型自体に、プロトコルの中で宣言されたプロパティのすべてのインライン化します。

次に示す方法の Objective C 宣言`UITextField`Xamarin.iOS コントラクトで定義されます。

```objc
@interface UITextField : UIControl <UITextInput> {

}
```

としてこのような記述は、 C# API コントラクト。

```csharp
[BaseType (typeof (UIControl))]
interface UITextField : UITextInput {
}
```

インターフェイスにその他の属性を適用するよう構成して、コード生成の他の多くの側面を制御できます、 [ `[BaseType]` ](#BaseTypeAttribute)属性。


### <a name="generating-events"></a>イベントを生成します。

Objective C のデリゲート クラスをマップする、Xamarin.iOS および Xamarin.Mac API デザインの機能の 1 つは、C#イベントとコールバック。 などのプロパティに割り当てることで、Objective C のプログラミング パターンを採用するかどうかのインスタンスごとに選択できるユーザー `Delegate` Objective C のランタイムを呼び出すと、さまざまなメソッドを実装するクラスのインスタンス選択、 C#-イベントおよびプロパティをスタイル設定します。

お知らせ、OBJECTIVE-C でモデルを使用する方法の 1 つの例を参照してください。

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

上記の例では、2 つのメソッドを上書きして選択したを通知するスクロール イベントが取得され、2 つ目を 1 つのコールバックを指示するブール値を返す必要がありますを確認できます、`scrollView`をスクロールするかどうか、かどうかを上位します。

C#モデルでは、ライブラリのユーザーを使用して通知をリッスンする、C#予想される値を返すコールバックをフックするには、イベントの構文とプロパティ構文です。

これは、方法、C#コードの同じ機能がラムダを使用するようになります。

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

イベントは (void 戻り値の型がある) の値を返さないために、複数のコピーを接続できます。 `ShouldScrollToTop` 、イベントではない型を持つプロパティではなく`UIScrollViewCondition`このシグネチャを持ちます。

```csharp
public delegate bool UIScrollViewCondition (UIScrollView scrollView);
```

返します、`bool`値、この場合、ラムダ構文によりから値を返すだけ、`MakeDecision`関数。

バインディング ジェネレーターは、イベントとなどのクラスをリンクしているプロパティの生成をサポート`UIScrollView`でその`UIScrollViewDelegate`(もこれらのモデル クラス) を呼び出すこれは、注釈を付けることで、 [ `[BaseType]` ](#BaseTypeAttribute)定義を`Events`と`Delegates`パラメーター (後述)。 注釈を付けるだけでなく、 [ `[BaseType]` ](#BaseTypeAttribute)これらのパラメーターでは詳細のいくつかのコンポーネントのジェネレーターを通知するために必要です。

1 つ以上のパラメーターを受け取るイベント (Objective C で、規則は、デリゲート クラスの最初のパラメーターは、送信元オブジェクトのインスタンスを)、生成されたの希望する名前を指定する必要があります`EventArgs`クラスを作成できます。 これは、 [ `[EventArgs]` ](#EventArgsAttribute)モデル クラス内のメソッド宣言の属性。 例えば:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

上記の宣言が生成されます、`UIImagePickerImagePickedEventArgs`クラスから派生した`EventArgs`パックの両方のパラメーターと、 `UIImage` 、`NSDictionary`します。 ジェネレーターでは、これが生成されます。

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

次を公開し、`UIImagePickerController`クラス。

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

モデルの値を返すメソッドは、異なる方法でバインドされます。 両方の名前を生成されたが必要とC#デリゲート (メソッドのシグネチャ) とも、既定値を返す場合は、ユーザーが自分自身実装を提供していません。 たとえば、`ShouldScrollToTop`定義は。

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIScrollViewDelegate {
    [Export ("scrollViewShouldScrollToTop:"), DelegateName ("UIScrollViewCondition"), DefaultValue ("true")]
    bool ShouldScrollToTop (UIScrollView scrollView);
}
```

上記が作成されます、`UIScrollViewCondition`シグネチャを持つデリゲートが上記し、ユーザーが、実装を提供していない場合、戻り値が true になります。

加え、 [ `[DefaultValue]` ](#DefaultValueAttribute)属性を使用することも、 [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute)属性を呼び出し、または、で指定されたパラメーターの値を返すジェネレーターに指示する[ `[NoDefaultValue]` ](#NoDefaultValueAttribute)パラメーターの既定値はありません、ジェネレーターに指示します。

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

使用する、`Name`プロパティをこの型は、OBJECTIVE-C で世界中にバインドされる名前を制御します。 通常、使用するため、 C# 、.NET Framework デザイン ガイドラインに準拠しているが、その規則に従わない Objective C での名前にマップする名前を入力します。

例では、OBJECTIVE-C で次の例でマップ`NSURLConnection`型`NSUrlConnection`ように、.NET Framework デザイン ガイドラインでは、"Url"を使用して、"URL"ではなく。

```csharp
[BaseType (typeof (NSObject), Name="NSURLConnection")]
interface NSUrlConnection {
}
```

指定した名前が生成される値として使用`[Register]`バインディング属性。 場合`Name`が指定されていない型の短い名前がの値として使用される、`[Register]`生成された出力内の属性。

#### <a name="basetypeevents-and-basetypedelegates"></a>BaseType.Events と BaseType.Delegates

これらのプロパティを使用して、ドライブの生成C#-生成されたクラスでイベントをスタイル設定します。 リンクの Objective C のデリゲート クラスを使用して特定のクラスに使用されます。 多くの場合、クラスはデリゲート クラスを使用して、通知やイベントを送信する場所になります。 たとえば、`BarcodeScanner`コンパニオンが`BardodeScannerDelegate`クラス。 `BarcodeScanner`クラスが、通常、`Delegate`のインスタンスを割り当てるプロパティ`BarcodeScannerDelegate`に、この動作を while をユーザーに公開する可能性があります、 C#-、を使用するスタイルのイベントインターフェイスのように、そのような場合に`Events`と`Delegates`のプロパティ、 [ `[BaseType]` ](#BaseTypeAttribute)属性。

これらのプロパティは、常に一緒に設定されますと同じ数の要素が必要し、同期を維持します。`Delegates`配列をラップする各の厳密に型指定されたデリゲートの 1 つの文字列に含まれる、`Events`配列には、それに関連付ける各型の 1 つの型が含まれています。

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


#### <a name="basetypekeeprefuntil"></a>BaseType.KeepRefUntil

によって参照されるメソッドまでをそのオブジェクトのインスタンスが保持されますがこのクラスの新しいインスタンスを作成するときにこの属性を適用する場合、`KeepRefUntil`が呼び出されています。 これは、ユーザーをコードを使用して、オブジェクトの周囲への参照を保持したくない場合は、独自の Api の使いやすさを向上させるために役立ちます。 このプロパティの値は、メソッドの名前、`Delegate`クラスと組み合わせて、これを使用する必要がありますので、`Events`と`Delegates`プロパティもします。

次の例は、これでの使用方法を示します`UIActionSheet`Xamarin.iOS で。

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


### <a name="disabledefaultctorattribute"></a>DisableDefaultCtorAttribute

この属性をインターフェイス定義に適用すると、ジェネレーターから既定のコンス トラクターを生成できなくなります。

オブジェクト クラスの他のコンス トラクターのいずれかで初期化する必要がある場合は、この属性を使用します。


### <a name="privatedefaultctorattribute"></a>PrivateDefaultCtorAttribute

インターフェイス定義にこの属性が適用されるときに、既定のコンス トラクターを private としてフラグです。 つまり、拡張ファイルから内部的にこのクラスのオブジェクトをインスタンスできますが、クラスのユーザーにアクセスできるだけしません。

<a name="CategoryAttribute" />

### <a name="categoryattribute"></a>CategoryAttribute

Objective C カテゴリをバインドするものとして公開して、型定義でこの属性を使用してC#方法 Objective C を反映する拡張メソッドは、機能を公開します。

カテゴリは、メソッドとクラスで使用できるプロパティのセットを拡張するために使用する OBJECTIVE-C メカニズムです。   実際には、基底クラスの機能を拡張するかに使用されます (たとえば`NSObject`) で特定のフレームワークがリンクされている場合 (たとえば`UIKit`)、使用できますが、新しいフレームワークにリンクされている場合にのみ、そのメソッドを作成します。   その他の場合、機能によって、クラスの機能を整理するが使用されます。   似ていますが、C#拡張メソッド。

これは目標 c: なりますが、カテゴリです。

```objc
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

上記の例がのインスタンスを拡張するライブラリで見つかった`UIView`メソッドを使用して`makeBackgroundRed`します。

これらをバインドするに使用することができます、 [ `[Category]` ](#CategoryAttribute)インターフェイス定義の属性。   使用する場合、 [ `[Category]` ](#CategoryAttribute)属性の意味、 [ `[BaseType]` ](#BaseTypeAttribute)に拡張する型を拡張する基底クラスを指定するために使用されているから属性を変更します。

次に示す方法、`UIView`拡張機能のバインドし、なってC#拡張メソッド。

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

上記は作成、`MyUIViewExtension`を含むクラス、`MakeBackgroundRed`拡張メソッド。   つまり、呼び出すことができるようになりました`MakeBackgroundRed`いずれかで`UIView`サブクラスでは、OBJECTIVE-C で表示されますが、同じ機能を提供します。

一部のケースでは紹介**静的**カテゴリ内のメンバーが次の例のようにします。

```objc
@interface FooObject (MyFooObjectExtension)
+ (BOOL)boolMethod:(NSRange *)range;
@end
```

これにより、**が正しくない**カテゴリC#インターフェイス定義。

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

これが正しくないためにを使用する、`BoolMethod`拡張機能のインスタンスを必要`FooObject`、ObjC をバインドしているが、**静的**拡張機能では、これは方法のための副作用C#拡張メソッドが実装されています.

上記の定義を使用する唯一の方法は、厄介次のコードでは。

```csharp
(null as FooObject).BoolMethod (range);
```

これを回避する推奨事項をインラインには、`BoolMethod`内で定義、`FooObject`インターフェイス定義自体、これによりなどのものでは、この拡張機能を呼び出すことができます`FooObject.BoolMethod (range)`します。

```csharp
[BaseType (typeof (NSObject))]
interface FooObject {

    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

見つかるたびに、警告 (BI1117) を発行しますが、 [ `[Static]` ](#StaticAttribute)内でメンバーを[ `[Category]` ](#CategoryAttribute)定義。 実際がしたい場合[ `[Static]` ](#StaticAttribute)内のメンバー、 [ `[Category]` ](#CategoryAttribute)定義を使用して、警告を無音できます`[Category (allowStaticMembers: true)]`またはメンバーを修飾することによって、または[`[Category]` ](#CategoryAttribute)インターフェイスで定義[ `[Internal]`](#InternalAttribute)します。

<a name="StaticAttribute_Class" />

### <a name="staticattribute"></a>StaticAttribute

静的クラスから派生していない 1 つが生成されますだけこの属性がクラスに適用される`NSObject`であり、 [ `[BaseType]` ](#BaseTypeAttribute)属性は無視されます。 静的クラスは、ホストに公開する C のパブリック変数に使用されます。

例えば:

```csharp
[Static]
interface CBAdvertisement {
    [Field ("CBAdvertisementDataServiceUUIDsKey")]
    NSString DataServiceUUIDsKey { get; }
```

生成されます、C#次の API を使用したクラス。

```csharp
public partial class CBAdvertisement  {
    public static NSString DataServiceUUIDsKey { get; }
}
```

## <a name="protocolmodel-definitions"></a>プロトコル/モデルの定義

通常、モデルは、プロトコルの実装が使用します。
ランタイムのみを登録する OBJECTIVE-C メソッドを実際には、上書きされている点が違います。
それ以外の場合、メソッドは登録されていません。

つまりする場合、一般にフラグを付けられたクラスをサブクラス化する、 `ModelAttribute`、基本メソッドを呼び出す必要はありません。   このメソッドを呼び出すと、例外がスローされます、全体の動作をオーバーライドするメソッドに対して、サブクラスを実装することになっています。

<a name="AbstractAttribute" />

### <a name="abstractattribute"></a>AbstractAttribute

既定では、プロトコルの一部であるメンバーの使用は必須ではありません。 サブクラスを作成することができます、`Model`オブジェクト内のクラスから派生するだけでC#関心のあるメソッドだけをオーバーライドします。 場合によって、Objective C コントラクトでは、ユーザーがこのメソッドの実装を提供することが必要です (フラグが付いたもの、`@required`ディレクティブ Objective C で)。 その場合、これらのメソッドとのフラグを設定する必要があります、`[Abstract]`属性。

`[Abstract]`属性 methods または properties に適用できるし、ジェネレーターに抽象クラス、およびクラスを抽象クラスとして生成されたメンバーのフラグを設定します。

以下は、Xamarin.iOS からの抜粋です。

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

ユーザーがこの特定のメソッドで、モデル オブジェクトのメソッドを提供していない場合は、モデルのメソッドによって返される既定値を指定します

構文:

```csharp
public class DefaultValueAttribute : Attribute {
        public DefaultValueAttribute (object o);
        public object Default { get; set; }
}
```

などの次の虚数部のデリゲート クラスで、`Camera`クラスを提供しています、`ShouldUploadToServer`これで、プロパティとして公開は、`Camera`クラス。 場合のユーザー、`Camera`クラスは明示的に設定されていない、true または false に応答できるラムダに値を戻り値の既定値ここでは false の場合で指定した値、`DefaultValue`属性。

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("camera:shouldPromptForAction:"), DefaultValue (false)]
    bool ShouldUploadToServer (Camera camera, CameraAction action);
}
```

虚数部のクラス ハンドラーを設定すると場合は、この値は無視されます。

```csharp
var camera = new Camera ();
camera.ShouldUploadToServer = (camera, action) => return SomeDecision ();
```

参照してください: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute)、 [ `[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)します。

<a name="DefaultValueFromArgumentAttribute" />

### <a name="defaultvaluefromargumentattribute"></a>DefaultValueFromArgumentAttribute

構文:

```csharp
public class DefaultValueFromArgumentAttribute : Attribute {
    public DefaultValueFromArgumentAttribute (string argument);
    public string Argument { get; }
}
```

モデル クラスで、値を返すメソッドで提供されている場合は、この属性は、ユーザーが自分のメソッドまたはラムダを指定しない場合、指定されたパラメーターの値を返すジェネレーターように指示されます。

例:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, nfloat progress);
}
```

上記の場合でのユーザー、`NSAnimation`クラスのいずれかを使用することを選択、C#イベント/プロパティを設定していないと`NSAnimation.ComputeAnimationCurve`メソッドまたはラムダでは、戻り値が進行中のパラメーターで渡される値になります。

参照してください: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute)、 [`[DefaultValue]`](#DefaultValueAttribute)

### <a name="ignoredindelegateattribute"></a>IgnoredInDelegateAttribute

状況に合理的しないイベントを公開またはそので修飾された任意のメソッドの生成を回避するために、ジェネレーターに指示がこの属性を追加するためのホスト クラスに、モデル クラスからのプロパティを委任します。

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

この属性は、使用するデリゲートのシグネチャの名前を設定する値を返すモデルのメソッドで使用されます。

例:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, float progress);
}
```

上記の定義には、次のパブリックの宣言をジェネレーターが生成されます。

```csharp
public delegate float NSAnimationProgress (MonoMac.AppKit.NSAnimation animation, float progress);
```

### <a name="delegateapinameattribute"></a>DelegateApiNameAttribute

この属性は、ホスト クラスで生成されたプロパティの名前を変更するジェネレーターの許可に使用されます。 便利です FooDelegate クラスのメソッドの名前は、デリゲート クラスに適したが奇数をプロパティとしてホスト クラスでになります。

これは本当に便利です (および必要な) も場合がある 2 つまたはに意味のある複数のオーバー ロード メソッドでは、という FooDelegate クラスでは、引き続きがそれらを指定した名前を使用して、ホスト クラスに公開します。

例:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateApiName ("ComputeAnimationCurve"), DelegateName ("Func<NSAnimation, float, float>"), DefaultValueFromArgument ("progress")]
    float GetValueForProgress (NSAnimation animation, float progress);
}
```

上記の定義に、ジェネレーター ホスト クラスで次のパブリックの宣言が生成されます。

```csharp
public Func<NSAnimation, float, float> ComputeAnimationCurve { get; set; }
```

<a name="EventArgsAttribute" />

### <a name="eventargsattribute"></a>EventArgsAttribute

1 つ以上のパラメーターを受け取るイベント (Objective C で、規則はデリゲート クラスの最初のパラメーターが送信元オブジェクトのインスタンス) に生成された EventArgs クラスの名前を指定する必要があります。 これは、`[EventArgs]`メソッド宣言で属性、`Model`クラス。

例えば:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

上記の宣言が生成されます、 `UIImagePickerImagePickedEventArgs` EventArgs から派生し、両方のパラメーターをパックするクラス、 `UIImage` 、`NSDictionary`します。 ジェネレーターでは、これが生成されます。

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

次を公開し、`UIImagePickerController`クラス。

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```


### <a name="eventnameattribute"></a>EventNameAttribute

この属性は、イベントまたはクラスで生成されたプロパティの名前を変更するジェネレーターの許可に使用されます。 便利です、モデル クラスのメソッドの名前、モデル クラスの意味がイベントまたはプロパティとして、元のクラスに奇数になります。

たとえば、`UIWebView`から次のビットを使用して、 `UIWebViewDelegate`:

```csharp
[Export ("webViewDidFinishLoad:"), EventArgs ("UIWebView"), EventName ("LoadFinished")]
void LoadingFinished (UIWebView webView);
```

上記の公開`LoadingFinished`方法として、`UIWebViewDelegate`が`LoadFinished`でフックのイベントとして、 `UIWebView`:

```csharp
var webView = new UIWebView (...);
webView.LoadFinished += delegate { Console.WriteLine ("done!"); }
```

<a name="ModelAttribute" />

### <a name="modelattribute"></a>ModelAttribute

適用すると、 `[Model]` API、ランタイム、コントラクトの型定義に属性は、ユーザーが、クラスのメソッドを上書きする場合、クラスのメソッドを呼び出しを表示のみが特別なコードを生成します。 この属性は通常、Objective C のデリゲート クラスをラップするすべての Api に適用されます。

<a name="NoDefaultValueAttribute" />

### <a name="nodefaultvalueattribute"></a>NoDefaultValueAttribute

モデルのメソッドが既定の戻り値を提供しないことを指定します。

これは応答することにより、OBJECTIVE-C ランタイムでは機能`false`Objective C ランタイムの要求を指定されたセレクターがこのクラスで実装されたかどうかを判断します。

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("shouldDisplayPopup"), NoDefaultValue]
    bool ShouldUploadToServer ();
}
```

参照してください: [ `[DefaultValue]` ](#DefaultValueAttribute)、 [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)  

<a name="ProtocolAttribute" />

## <a name="protocols"></a>プロトコル

OBJECTIVE-C プロトコルの概念はで実際に存在しませんC#します。 プロトコルはのようなC#インターフェイスが、そのすべてのメソッドと異なるプロトコルで宣言されたプロパティは、これを採用するクラスが実装する必要があります。 代わりにいくつかのメソッドとプロパティは省略可能です。

一部のプロトコルは、通常モデル クラスとして使用、それらを使用してバインドする必要があります、 [ `[Model]` ](#ModelAttribute)属性。

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

Xamarin.iOS 7.0 以降、新規および改良されたプロトコルのバインド機能が組み込まれています。  任意の定義を含む、`[Protocol]`属性プロトコルを使用する方法を大幅に向上する 3 つのサポート クラスが生成されます。

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

**クラスの実装**の個々 のメソッドをオーバーライドし、完全な型の安全性を取得できる完全な抽象クラスを提供します。 原因が、C#は多重継承をサポートしていない、シナリオ、異なる基底クラスが必要ですが、インターフェイスを実装する場合があります。

これは、ような場合、生成された**インターフェイス定義**が用意されています。  これは、プロトコルからのすべての必要なメソッドを持つインターフェイスです。  これにより、開発者が単なるインターフェイスを実装するプロトコルを実装します。  ランタイムは、プロトコルの導入として型を自動的に登録されます。

インターフェイスがのみ、必要なメソッドを一覧表示し、省略可能なメソッドは公開ことに注意してください。   つまり、プロトコルを採用するクラスを確認、必要なメソッドでは、完全な型を取得が (手動でエクスポート属性を使用してシグネチャの一致)、弱い型指定を使用する必要がありますの省略可能なプロトコル メソッド。

プロトコルを使用する API を使用すると便利にバインディング ツールも生成されます拡張メソッドのクラスのすべての省略可能なメソッドを公開します。   これは、ことを意味する API を使用している限りはしてプロトコルをすべてのメソッドを持つものとして扱うこと。

API でプロトコルの定義を使用する場合は、API の定義で空のインターフェイスはスケルトンを記述する必要があります。   API で、MyProtocol を使用する場合は、これを行う必要があります。

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

上記に必要なため、バインド時に、`IMyProtocol`が存在しない、つまり空のインターフェイスを提供する必要がある理由。

### <a name="adopting-protocol-generated-interfaces"></a>プロトコルによって生成されたインターフェイスを導入します。

たびにプロトコルは、次のように、生成されたインターフェイスのいずれかを実装します。

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

インターフェイス メソッドの実装は自動的に取得エクスポート、適切な名前を持つため、これと同等です。

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

これは、暗黙的または明示的にインターフェイスを実装する場合に関係ありません。

### <a name="protocol-inlining"></a>プロトコルのインライン展開

プロトコルの導入として宣言されている既存の OBJECTIVE-C で型をバインドするときにインライン化するプロトコル直接します。 これを行うことがなく、インターフェイスとしてだけで、プロトコルを宣言[ `[BaseType]` ](#BaseTypeAttribute)属性し、インターフェイスの基本インターフェイスの一覧で、プロトコルの一覧を表示します。

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

このセクションでは属性の型の個々 のメンバーに適用されます。 プロパティとメソッドの宣言。


### <a name="alignattribute"></a>AlignAttribute

プロパティの戻り値の型のアラインメント値を指定するために使用します。 特定のプロパティを使用する特定の境界に配置する必要がありますアドレスへのポインター (これは、たとえば一部の Xamarin.iOS で`GLKBaseEffect`16 バイトにする必要があるプロパティの配置)。 このプロパティを使用して、getter を装飾し、配置の値を使用できます。 これは通常使用、`OpenTK.Vector4`と`OpenTK.Matrix4`Objective C Api と統合した場合の型します。

例:

```csharp
public interface GLKBaseEffect {
    [Export ("constantColor")]
    Vector4 ConstantColor { [Align (16)] get; set;  }
}
```


### <a name="appearanceattribute"></a>AppearanceAttribute

`[Appearance]`外観マネージャーが導入された属性は iOS 5 に制限されます。

`[Appearance]`メソッドやプロパティに参加するには属性を適用できます、`UIAppearance`フレームワーク。 この属性がメソッドまたはクラスのプロパティに適用されると、このクラスのすべてのインスタンスまたは特定の条件に一致するインスタンスのスタイルを設定するために使用するクラスを厳密に型指定された外観を作成するバインド ジェネレーターを指示します。

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

上記は、次にコードを生成 UIToolbar:

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

### <a name="autoreleaseattribute-xamarinios-54"></a>AutoReleaseAttribute (Xamarin.iOS 5.4)

使用して、`[AutoReleaseAttribute]`メソッドにメソッドの呼び出しをラップするメソッドとプロパティで、`NSAutoReleasePool`します。

Objective C では、既定値に追加される値を返す方法をいくつか`NSAutoReleasePool`します。 既定では、スレッドに移動してこれらは`NSAutoReleasePool`、Xamarin.iOS では、オブジェクトへの参照も保持、管理対象のオブジェクトが存続する限り、ためにたくないに追加の参照を保持するが、`NSAutoReleasePool`まで、スレッドが、ドレインのみを取得次のスレッドに制御を返します。 または、メイン ループに戻ってください。

たとえば大量のプロパティでこの属性を適用 (たとえば`UIImage.FromFile`)、既定値に追加されているオブジェクトを返す`NSAutoReleasePool`します。 この属性がない場合、スレッドが、メイン ループに制御を返しません限り、イメージを保持するは。 何らかの動作は、常にバック グラウンド ダウンローダー Uf、スレッド、作業を待機しているイメージは解放されません。

### <a name="forcedtypeattribute"></a>ForcedTypeAttribute

`[ForcedTypeAttribute]`返されるアンマネージ オブジェクトで、バインディングの定義で示される型が一致しない場合でも、マネージ型の作成を強制するために使用します。

ヘッダーで示される型が、ネイティブ メソッドの戻り値の型と一致しない場合に便利ですが、たとえばから次の Objective C の定義がかかる`NSURLSession`:

`- (NSURLSessionDownloadTask *)downloadTaskWithRequest:(NSURLRequest *)request`

返すが明確に示すように、`NSURLSessionDownloadTask`インスタンスしますが、まだ、**を返します**、`NSURLSessionTask`はスーパークラスにないために変換できると`NSURLSessionDownloadTask`します。 タイプ セーフなコンテキストであるので、`InvalidCastException`は行われます。

ヘッダーの説明に従っているし、回避、 `InvalidCastException`、`[ForcedTypeAttribute]`使用されます。

```csharp
[BaseType (typeof (NSObject), Name="NSURLSession")]
interface NSUrlSession {

    [Export ("downloadTaskWithRequest:")]
    [return: ForcedType]
    NSUrlSessionDownloadTask CreateDownloadTask (NSUrlRequest request);
}
```

`[ForcedTypeAttribute]`もという名前のブール値を受け入れる`Owns`つまり`false`既定`[ForcedType (owns: true)]`します。 パラメーターを使用して以下を所有している、[所有権ポリシー](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html)の**Core Foundation**オブジェクト。

`[ForcedTypeAttribute]`パラメーター、プロパティ、および戻り値でのみ有効です。

<a name="BindAsAttribute" />

### <a name="bindasattribute"></a>BindAsAttribute

`[BindAsAttribute]`によりバインド`NSNumber`、`NSValue`と`NSString`(列挙型) に正確なC#型。 属性を使用してより優れたより正確に作成することができます、ネイティブ API 経由での .NET API。

装飾できるは、(戻り値) のメソッド、パラメーターおよびプロパティを`BindAs`します。 唯一の制限は、メンバー**は許可されません**内に、`[Protocol]`または[ `[Model]` ](#ModelAttribute)インターフェイス。

例えば:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

出力されます。

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

内部的にすべきことは、 `bool?`  <->  `NSNumber`と`CGRect`  <->  `NSValue`変換します。

現在のカプセル化がサポートされている種類は次のとおりです。

* `NSValue`
* `NSNumber`
* `NSString`

#### <a name="nsvalue"></a>NSValue

次C#との間にカプセル化するデータ型がサポートされている`NSValue`:

* CGAffineTransform
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

次C#との間にカプセル化するデータ型がサポートされている`NSNumber`:

* bool
* byte
* double
* float
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

[`[BindAs]`](#BindAsAttribute) と一緒に動作[列挙型は、NSString 定数によって支え](#enum-attributes)などのより優れた .NET API を作成できるように。

```csharp
[BindAs (typeof (CAScroll))]
[Export ("supportedScrollMode")]
NSString SupportedScrollMode { get; set; }
```

出力されます。

```csharp
[Export ("supportedScrollMode")]
CAScroll SupportedScrollMode { get; set; }
```

処理は、 `enum`  <->  `NSString`指定された列挙型を入力する場合にのみ変換[ `[BindAs]` ](#BindAsAttribute)は[NSString 定数に支え](#enum-attributes)。

#### <a name="arrays"></a>配列

[`[BindAs]`](#BindAsAttribute) 配列をサポートもサポートされている型のいずれかを例としては、次の API 定義を設定することが。

```csharp
[return: BindAs (typeof (CAScroll []))]
[Export ("getScrollModesAt:")]
NSString [] GetScrollModes ([BindAs (typeof (CGRect []))] NSValue [] rects);
```

出力されます。

```csharp
[Export ("getScrollModesAt:")]
CAScroll? [] GetScrollModes (CGRect [] rects) { ... }
```

`rects`にパラメーターをカプセル化されます、`NSArray`を格納している、`NSValue`各`CGRect`し、戻り値の配列を取得する`CAScroll?`が、返された値を使用して作成されたが`NSArray`含む`NSStrings`します。

<a name="BindAttribute" />

### <a name="bindattribute"></a>BindAttribute

`[Bind]`属性が 2 つの用途の 1 つのメソッドまたはプロパティの宣言と個々 の getter または setter プロパティ内に適用すると、もう 1 つに適用します。

メソッドやプロパティの効果に使用される場合、`[Bind]`属性は、指定されたセレクターが呼び出すメソッドを生成します。 その結果、生成されたメソッドが修飾されていないが、 [ `[Export]` ](#ExportAttribute)属性には、メソッドのオーバーライドで参加できるしないことを意味します。 これは通常と組み合わせて使用、 `[Target]` Objective C の拡張メソッドを実装するための属性。

例えば:

```csharp
public interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

Get アクセス操作子または set アクセス操作子を使用すると、`[Bind]`プロパティの getter および setter OBJECTIVE-C セレクター名を生成するときに、コード ジェネレーターによって推論される既定の設定を変更する属性を使用します。 名前のプロパティのフラグを設定すると、既定で`fooBar`、ジェネレーターを生成、 `fooBar` getter のエクスポートと`setFooBar:`setter 用。 いくつかの場合、OBJECTIVE-C に従わないこの規則、get アクセス操作子の名前を変更、通常は`isFooBar`します。
この属性は、このジェネレーターの通知に使用されます。

例えば:

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

のみ Xamarin.iOS 6.3 で使用できると新しいします。

この属性は、完了ハンドラーの最後の引数として取るメソッドに適用できます。

使用することができます、`[Async]`が最後の引数は、コールバック メソッドの属性。  バインディング ジェネレーターがサフィックスを持つメソッドのバージョンを生成はこれをメソッドに適用すると`Async`します。  コールバックがパラメーターを受け取らない場合、戻り値になります、 `Task`、コールバックがパラメーターを受け取る場合、結果になります、`Task<T>`します。

```csharp
[Export ("upload:complete:")]
[Async]
void LoadFile (string file, NSAction complete)
```

この非同期メソッドは、次の場合に生成するされます。

```csharp
Task LoadFileAsync (string file);
```

設定する必要があります、コールバックが複数のパラメーターを受け取る場合、`ResultType`または`ResultTypeName`に必要なすべてのプロパティを格納する生成された型の名前を指定します。

```csharp
delegate void OnComplete (string [] files, nint byteCount);

[Export ("upload:complete:")]
[Async (ResultTypeName="FileLoading")]
void LoadFiles (string file, OnComplete complete)
```

次はこの非同期メソッドを生成ここ`FileLoading`両方にアクセスするプロパティを格納`files`と`byteCount`:

```csharp
Task<FileLoading> LoadFile (string file);
```

コールバックの最後のパラメーターがある場合、 `NSError`、し、生成された`Async`かどうか、値が null でないと、生成された非同期メソッドは、タスクの例外を設定する場合は、メソッドは確認します。

```csharp
[Export ("upload:onComplete:")]
[Async]
void Upload (string file, Action<string,NSError> onComplete);
```

上記では、次の非同期メソッドが生成されます。

```csharp
Task<string> UploadAsync (string file);
```

エラーの場合、結果のタスク設定されている例外を必要とする`NSErrorException`、その結果をラップする`NSError`します。

#### <a name="asyncattributeresulttype"></a>AsyncAttribute.ResultType

このプロパティを使用して、返される値を指定する`Task`オブジェクト。   このパラメーターは、既存の型、したがって、コア api 定義のいずれかで定義する必要があります。

#### <a name="asyncattributeresulttypename"></a>AsyncAttribute.ResultTypeName

このプロパティを使用して、返される値を指定する`Task`オブジェクト。   このパラメーターは、目的の型名の名前、一連のコールバックを受け取る各パラメーターのいずれかのプロパティをジェネレーターが生成されます。

#### <a name="asyncattributemethodname"></a>AsyncAttribute.MethodName

このプロパティを使用すると、生成された非同期メソッドの名前をカスタマイズできます。   既定値は、メソッドの名前を使用し、テキスト"Async"を追加するのには、これを使用して、この既定を変更することができます。

### <a name="disablezerocopyattribute"></a>DisableZeroCopyAttribute

この属性が文字列パラメーターまたは文字列プロパティに適用され、このパラメーターのコピーなしの文字列のマーシャ リングを使用し、代わりにから新しい NSString インスタンスを作成するコード ジェネレーターの指示、C#文字列。
コピーなしの文字列のいずれかを使用してマーシャ リングを使用するジェネレーターに指示する場合、この属性は文字列で必要のみ、`--zero-copy`またはアセンブリ レベル属性を設定するコマンド ライン オプション`ZeroCopyStringsAttribute`します。

これは、プロパティにするには、OBJECTIVE-C で宣言されている場合に必要な`retain`または`assign`プロパティの代わりに、`copy`プロパティ。 これらは、通常、最適化されている誤って""開発者がサードパーティ製のライブラリで発生します。 一般に、`retain`または`assign``NSString`プロパティが誤っているか以降`NSMutableString`またはユーザーから派生したクラスの`NSString`微妙互換性に影響する、ライブラリ コードの知識なし文字列の内容が変わる可能性があります、アプリケーション。 通常これは、途中の最適化が原因で発生します。

C: の目的でこのような 2 つのプロパティを次に示します

```csharp
@property(nonatomic,retain) NSString *name;
@property(nonatomic,assign) NSString *name2;
```


### <a name="disposeattribute"></a>DisposeAttribute

適用すると、`[DisposeAttribute]`に追加されるコード スニペットを提供するクラスに、`Dispose()`クラスのメソッドの実装。

以降、`Dispose`メソッドはによって自動的に生成、`bmac-native`と`btouch-native`ツールを使用する必要があります、`[Dispose]`に生成されたコードを挿入する属性`Dispose`メソッドの実装。

例えば:

```csharp
[BaseType (typeof (NSObject))]
[Dispose ("if (OpenConnections > 0) CloseAllConnections ();")]
interface DatabaseConnection {
}
```

<a name="ExportAttribute" />

### <a name="exportattribute"></a>ExportAttribute

`[Export]`メソッドまたは OBJECTIVE-C ランタイムに公開されるプロパティのフラグを設定する属性を使用します。 この属性は、バインディング ツールと実際の Xamarin.iOS および Xamarin.Mac ランタイム間で共有されます。 メソッドの場合、パラメーターが渡される生成されたコードにそのままのプロパティ、get アクセス操作子および set アクセス操作子のエクスポートは基本宣言に基づいて生成されます (を参照してください、 [ `[BindAttribute]` ](#BindAttribute)を変更する方法については、ツールの動作、バインディング)。

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

[セレクター](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Selector.html)基になる OBJECTIVE-C メソッドまたはバインドされているプロパティの名前を表します。

#### <a name="exportattributeargumentsemantic"></a>ExportAttribute.ArgumentSemantic

<a name="FieldAttribute" />

### <a name="fieldattribute"></a>FieldAttribute

要求時に読み込まれに公開するフィールドとして、C のグローバル変数を公開するこの属性が使用されるC#コード。 C++、Objective C で定義されている、一部の Api で使用されるいずれかのトークンになる可能性があります、または値を持つは不透明ととして使用する必要があります定数の値を取得する、通常、これが必要、ユーザー コードでは、します。

構文:

```csharp
public class FieldAttribute : Attribute {
    public FieldAttribute (string symbolName);
    public FieldAttribute (string symbolName, string libraryName);
    public string SymbolName { get; set; }
    public string LibraryName { get; set; }
}
```

`symbolName`とリンクする C 記号。 既定では、これは、名前空間から名前が推論される型が定義されているライブラリから読み込まれます。 場合は、シンボルの検索するライブラリは、渡す必要があります、`libraryName`パラメーター。 スタティック ライブラリをリンクする場合を使用して、`__Internal`として、`libraryName`パラメーター。

生成されたプロパティは常に静的であります。

フィールドの属性を持つフラグが設定されたプロパティには、次の種類を使用できます。

* `NSString`
* `NSArray`
* `nint` / `int` / `long`
* `nuint` / `uint` / `ulong`
* `nfloat` / `float`
* `double`
* `CGSize`
* `System.IntPtr`
* 列挙体

Set アクセス操作子はサポートされていません[列挙型は、NSString 定数によって支え](#enum-attributes)が、手動でバインドできるために必要な場合。

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

`[Internal]`メソッドまたはプロパティに属性を適用できを使用して生成されたコードに対するフラグの設定の効果がある、 `internal` C#キーワードので、コードが生成されたアセンブリ コードにのみアクセスできます。 これは通常、すぎる低レベルの Api を非表示時またはジェネレーターでサポートされていない api を向上する最適ではないパブリック API を提供またはいくつかのハンド コーディングが必要に使用されます。

通常メソッドまたはプロパティがこの属性を使用して非表示にはメソッドまたはプロパティにし、別の名前を指定のバインディングを設計するとき、C#補完的なサポート ファイルを公開する厳密に型指定されたラッパーを追加すると、基になる機能です。

例えば:

```csharp
[Internal]
[Export ("setValue:forKey:")]
void _SetValueForKey (NSObject value, NSObject key);

[Internal]
[Export ("getValueForKey:")]
NSObject _GetValueForKey (NSObject key);
```

次に、サポート ファイルでこのようないくつかのコードがある。

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

この属性は、.NET で注釈を付けるプロパティのバッキング フィールドをフラグ`[ThreadStatic]`属性。 これは、フィールドが、スレッドの静的変数である場合に便利です。

### <a name="marshalnativeexceptions-xamarinios-606"></a>MarshalNativeExceptions (Xamarin.iOS 6.0.6)

この属性をメソッドのサポートにネイティブ (OBJECTIVE-C) 例外となります。
呼び出す代わりに`objc_msgSend`ObjectiveC 例外をキャッチし、それらをマネージ例外をマーシャ リングするカスタム トランポリンに呼び出しは通過を直接します。

現在ごく`objc_msgSend`署名はサポートされて (については、バインドを使用するアプリのリンクをネイティブで不足している monotouch_ が失敗した場合、署名はサポートされていませんかどうか *_objc_msgSend*記号)、詳細を指定できますが、要求に追加されます。


### <a name="newattribute"></a>NewAttribute

この属性がメソッドに適用され、プロパティにジェネレーターを生成、`new`宣言の前にキーワード。

同じメソッドまたはプロパティ名が基本クラスに既に存在していたサブクラスで導入されたときに、コンパイラの警告を回避するために使用されます。

<a name="NotificationAttribute" />

### <a name="notificationattribute"></a>NotificationAttribute

この属性は、ジェネレーターが厳密に型指定されたヘルパー Notifications クラスを生成するフィールドに適用できます。

通知ペイロードが保持されていませんで引数を指定せず、この属性を使用できますかを指定できます、 `System.Type` "EventArgs"で終わる名前を持つ API の定義で別のインターフェイスを通常参照します。 ジェネレーターに変換されます、インターフェイス、クラスをサブクラスとして持つ`EventArgs`されすべてのプロパティがそこに一覧が表示されます。 [ `[Export]` ](#ExportAttribute)で属性を使用する必要があります、 `EventArgs` Objective C ディクショナリの値をフェッチする検索に使用するキーの名前を一覧表示するクラス。

例えば:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

上記のコードを生成しても、入れ子になったクラスが生成されます`MyClass.Notifications`次のメソッド。

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
      public static NSObject ObserveDidStart (NSObject objectToObserve, EventHandler<NSNotificationEventArgs> handler)
   }
}
```

コードのユーザーにポストされた通知を簡単にサブスクライブできますし、 [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/)このようなコードを使用しています。

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

または、観察する特定のオブジェクトを設定します。 渡した場合`null`に`objectToObserve`このメソッドは、その他のピアと同じように動作します。

```csharp
var token = MyClass.Notifications.ObserverDidStart (objectToObserve, (notification) => {
    Console.WriteLine ("Observed the 'DidStart' event on objectToObserve!");
});
```

返された値`ObserveDidStart`を簡単に次のように、通知の受信を停止するために使用できます。

```csharp
token.Dispose ();
```

呼び出すか、または[NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject//)トークンに渡します。 通知にパラメーターが含まれている場合は、ヘルパーを指定する必要があります`EventArgs`インターフェイスは、次のようにします。

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

上記が生成されます、`MyScreenChangedEventArgs`クラス、`ScreenX`と`ScreenY`からデータをフェッチするプロパティ、 [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/)ディクショナリのキーを使用して`ScreenXKey`と`ScreenYKey`それぞれ適切な変換を適用します。 `[ProbePresence]`プローブ、キーが設定されている場合に、ジェネレーターに属性を使用、`UserInfo`値を抽出しようとしています。 代わりにします。 これは、キーの存在が (通常はブール値) の値である場合に使用されます。

このようなコードを記述できます。

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

場合によっては、ディクショナリに渡された値に関連付けられている定数はありません。  Apple では、場合によってパブリック シンボルの定数を使用しも文字列定数を使用します。  既定では、 [ `[Export]` ](#ExportAttribute) 、指定した属性`EventArgs`クラスは実行時に検索するパブリック シンボルとして指定した名前を使用するされます。  これは、ケースではありませんし、検索する文字列定数を渡す代わりに所定の場合、`ArgumentSemantic.Assign`エクスポート属性の値。

**Xamarin.iOS 8.4 の新機能**

場合によっては、通知は、引数を含まない有効期間をので開始の使用[ `[Notification]` ](#NotificationAttribute)含まない引数は許容されます。  場合によっては、通知のパラメーターが導入されます。  このシナリオをサポートするには、属性を複数回適用できます。

バインドを開発している既存のユーザー コードを壊さないようにしたい場合から既存の通知が有効になります。

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

次のように 2 回、通知の属性を一覧表示するバージョン。

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

このプロパティが適用されると、プロパティ値を許容するようにフラグを設定`null`をそれに割り当てられます。 これは、参照型の有効なのみです。

指定されたパラメーターを null にできることと、渡すことのチェックを実行しない必要がありますを示します。 メソッド シグネチャのパラメーターに適用されるこの`null`値。

バインディング ツールが OBJECTIVE-C に渡す前に割り当てられている値のチェックが生成され、スローするチェックが生成されます参照型がこの属性を持たない場合、`ArgumentNullException`割り当てられた値が場合`null`します。

例えば:

```csharp
// In properties

[NullAllowed]
UIImage IconFile { get; set; }

// In methods
void SetImage ([NullAllowed] UIImage image, State forState);
```

<a name="OverrideAttribute" />

### <a name="overrideattribute"></a>OverrideAttribute

この属性でこの特定のメソッドのバインドをフラグ必要があることをバインド ジェネレーターに指示を使用して、`override`キーワード。

### <a name="presnippetattribute"></a>PreSnippetAttribute

この属性を使用するには、入力パラメーターが検証されると後、OBJECTIVE-C にコードの呼び出しの前に挿入するのにコードを挿入するには

例:

```csharp
[Export ("demo")]
[PreSnippet ("var old = ViewController;")]
void Demo ();
```

### <a name="prologuesnippetattribute"></a>PrologueSnippetAttribute

この属性を使用すると、任意のパラメーターは、生成されたメソッドで検証する前に挿入するのにには、いくつかコードを挿入します。

例:

```csharp
[Export ("demo")]
[Prologue ("Trace.Entry ();")]
void Demo ();
```

### <a name="postgetattribute"></a>PostGetAttribute

そこから値を取得するには、このクラスから指定したプロパティを呼び出し、バインド ジェネレーターに指示します。

このプロパティは通常を参照するオブジェクト グラフを保持する参照オブジェクトを指すキャッシュの更新に使用します。 通常これコードを追加/削除などの操作を持つを示します。 このメソッドを使用するため、実際に使用されているオブジェクトへのマネージ参照を保持した後、要素が追加または削除されることを確認する、内部キャッシュに更新されます。 バインディング ツールは、特定のバインディングですべての参照オブジェクトのバッキング フィールドを生成するため、可能です。

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

ここで、`Dependencies`プロパティが追加または削除からの依存関係の後に呼び出される、`NSOperation`実際を表すグラフがあることを確認しているオブジェクトに読み込まれたオブジェクト、メモリの破損だけでなく、メモリ リークを防止します。

### <a name="postsnippetattribute"></a>PostSnippetAttribute

この属性を使用するにはいくらかC#ソース コードが基になる OBJECTIVE-C メソッドを呼び出した後に挿入するコード

例:

```csharp
[Export ("demo")]
[PostSnippet ("if (old != null) old.DemoComplete ();")]
void Demo ();
```

### <a name="proxyattribute"></a>ProxyAttribute

この属性は、プロキシ オブジェクトとそれらにフラグを設定する値を返すに適用されます。 ユーザーのバインドを区別しないできます Objective C Api 戻り値のプロキシ オブジェクト。 この属性の効果は、オブジェクトとフラグを設定する、`DirectBinding`オブジェクト。 Xamarin.Mac のシナリオでは、について確認できます、[このバグについて](https://bugzilla.novell.com/show_bug.cgi?id=670844)します。

### <a name="retainlistattribute"></a>RetainListAttribute

パラメーターへのマネージ参照を維持またはパラメーターの内部参照を削除するジェネレーターに指示します。 これは、参照されるオブジェクトの保持に使用されます。

構文:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

場合の値`doAdd`が true の場合に、パラメーターが追加、`__mt_{0}_var List<NSObject>;`します。 場所`{0}`に置き換えられますが、特定`listName`。 API を補完的な部分クラスでは、このバッキング フィールドを宣言する必要があります。

例については、次を参照してください[foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs)と[NSNotificationCenter.cs。](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)

### <a name="releaseattribute-xamarinios-60"></a>ReleaseAttribute (Xamarin.iOS 6.0)

これは、ジェネレーターを呼び出す必要がありますを指定する型を返すに適用できる`Release`に返す前にオブジェクト。 メソッドは、(自動発行オブジェクトは、最も一般的なシナリオ) ではなく、保持されているオブジェクトを提供する場合にのみ必要です。

例:

```csharp
[Export ("getAndRetainObject")]
[return: Release ()]
NSObject GetAndRetainObject ();
```

さらにこの属性は、Xamarin.iOS ランタイムは、このような関数から OBJECTIVE-C に戻ると、オブジェクトを保持する必要がありますを認識できるように、生成されたコードに反映されます。


### <a name="sealedattribute"></a>SealedAttribute

生成されたメソッドをシール済みとしてフラグを設定するジェネレーターに指示します。 この属性が指定されていない場合、既定では、(仮想メソッド、抽象メソッドまたはその他の属性の使用方法に応じて、上書き) の仮想メソッドを生成します。

<a name="StaticAttribute" />

### <a name="staticattribute"></a>StaticAttribute

ときに、`[Static]`メソッドまたはプロパティに属性が適用される、静的メソッドまたはプロパティが生成されます。 この属性が指定されていない場合、コード ジェネレーターはインスタンス メソッドまたはプロパティが生成されます。


### <a name="transientattribute"></a>TransientAttribute

フラグ プロパティの値は、一時的なものである iOS によって一時的に作成されますが、有効期間が長いオブジェクトには、この属性を使用します。 プロパティにこの属性が適用されると、ジェネレーターは、マネージ クラスが、オブジェクトへの参照を保持しないことを意味します。 このプロパティのバッキング フィールドを作成できません。

<a name="WrapAttribute" />

### <a name="wrapattribute"></a>WrapAttribute

Xamarin.iOS/Xamarin.Mac のバインドの設計では、`[Wrap]`属性は厳密に型指定されたオブジェクトを使用して、厳密に型指定されたオブジェクトをラップするために使用します。 これは、通常の型として宣言されるデリゲート オブジェクトを OBJECTIVE-C にほとんどの場合に`id`または`NSObject`します。 Xamarin.iOS および Xamarin.Mac で使用される規則は、それらの型としてのデリゲートまたはデータ ソースを公開する`NSObject`と規則の"Weak"+ 公開される名前を使用して名前付けします。 `id delegate` Objective C からのプロパティとして公開できる、 `NSObject WeakDelegate { get; set; }` API コントラクトのファイルのプロパティ。

厳密な型を表示して適用するための厳密な型では、このデリゲートに割り当てられている値が通常は、`[Wrap]`属性、つまり、ユーザーはいくつかの細かい制御が必要な場合、または低レベル tric に頼る必要がある場合は、弱い型を使用できますks、またはこれらは、作業のほとんどの厳密に型指定されたプロパティを使用することができます。

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

これは、ユーザーが、デリゲートの厳密に型指定されたバージョンを使用する方法です。

```csharp
// The weak case, user has to roll his own
class SomeObject : NSObject {
    [Export ("doDemo")]
    void CallbackForDoDemo () {}

}

var demo = new Demo ();
demo.WeakDelegate = new SomeObject ();
```

これは、ユーザーが、厳密に型指定されたバージョンを使用する方法と、ユーザーが活用を受け取ることに注意してくださいC#の型システムと彼の意図を宣言する、override キーワードを使用して、メソッドを装飾する手動でない彼`[Export]`、経過ユーザーのバインドでは、その作業を行いました。

```csharp
// This is the strong case,
class MyDelegate : DemoDelegate {
   override void Demo DoDemo () {}
}

var strongDemo = new Demo ();
demo.Delegate = new MyDelegate ();
```

別の使用、`[Wrap]`属性は、厳密に型指定されたバージョンのメソッドをサポートします。  例えば:

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

によって生成されたメンバー`[Wrap]`ない`virtual`必要がある場合、既定で、`virtual`メンバーを設定することができます`true`省略可能な`isVirtual`パラメーター。

```csharp
[BaseType (typeof (NSObject))]
interface FooExplorer {
    [Export ("fooWithContentsOfURL:")]
    void FromUrl (NSUrl url);

    [Wrap ("FromUrl (NSUrl.FromString (url))", isVirtual: true)]
    void FromUrl (string url);
}
```

## <a name="parameter-attributes"></a>パラメーター属性

メソッド定義のパラメーターに適用できる属性について説明だけでなく`[NullAttribute]`全体のプロパティに適用します。

<a name="BlockCallback" />

### <a name="blockcallback"></a>BlockCallback

この属性パラメーターの型に適用されますC#Objective C に準拠している対象のパラメーターをバインダーに通知するデリゲートの宣言は、呼び出し規約をブロックし、この方法でマーシャ リングする必要があります。

これは通常このような c: の目的で定義されているコールバックの使用します。

```objc
typedef returnType (^SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

参照してください: [CCallback](#CCallback)します。

<a name="CCallback" />

### <a name="ccallback"></a>CCallback

この属性パラメーターの型に適用されますC#デリゲートの宣言、バインダーに通知対象のパラメーターは、C ABI 関数ポインターの呼び出し規約に準拠しているし、この方法でマーシャ リングする必要があります。

これは通常このような c: の目的で定義されているコールバックの使用します。

```objc
typedef returnType (*SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

参照してください: [BlockCallback](#BlockCallback)します。

### <a name="params"></a>params

使用することができます、`[Params]`ジェネレーターの定義に"params"を挿入するメソッドの定義の最後の配列パラメーターの属性。   これにより、簡単には、省略可能なパラメーターをバインドします。

たとえば、次の定義:

```csharp
[Export ("loadFiles:")]
void LoadFiles ([Params]NSUrl [] files);
```

次のコードを記述するを使用できます。

```csharp
foo.LoadFiles (new NSUrl (url));
foo.LoadFiles (new NSUrl (url1), new NSUrl (url2), new NSUrl (url3));
```

これは、要素を渡すためには、純粋な配列を作成するユーザーが必要としない追加の利点があります。

<a name="plainstring" />

### <a name="plainstring"></a>PlainString

使用することができます、`[PlainString]`属性バインド ジェネレーターとしてパラメーターを渡す代わりに、C の文字列として文字列を渡すよう指示する文字列パラメーターの前に、`NSString`します。

ほとんどの Objective C Api を消費`NSString`パラメーターがいくつかの Api の公開、`char *`の代わりに、文字列を渡すための API、`NSString`バリエーションの 1 つ。
使用`[PlainString]`そのような場合。

たとえば、次のような Objective C 宣言します。

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

指定されたパラメーターの参照を保持するジェネレーターに指示します。 コード ジェネレーターはこのフィールドのバッキング ストアを指定または名前を指定することができます (、 `WrapName`) に値を格納します。 これは Objective C にパラメーターとして渡され、Objective C がこのオブジェクトのコピーを保持はのみを認識するマネージ オブジェクトへの参照を保持するために役立ちます。 などの API、`SetDisplay (SomeObject)`一度にの 1 つのオブジェクトを SetDisplay が表示のみすることが、この属性を使用して、します。 1 つ以上のオブジェクト (たとえば、スタックのような API) を追跡する必要がある場合は使用、`[RetainList]`属性。

構文:

```csharp
public class RetainAttribute {
    public RetainAttribute ();
    public RetainAttribute (string wrapName);
    public string WrapName { get; }
}
```


### <a name="retainlistattribute"></a>RetainListAttribute

パラメーターへのマネージ参照を維持またはパラメーターの内部参照を削除するジェネレーターに指示します。 これは、参照されるオブジェクトの保持に使用されます。

構文:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

場合の値`doAdd`が true の場合に、パラメーターが追加、`__mt_{0}_var List<NSObject>`します。 場所`{0}`に置き換えられますが、特定`listName`。 API を補完的な部分クラスでは、このバッキング フィールドを宣言する必要があります。

例については、次を参照してください[foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs)と[NSNotificationCenter.cs。](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)


### <a name="transientattribute"></a>TransientAttribute

この属性は、パラメーターに適用されするには、OBJECTIVE-C から移行する場合にのみ使用C#します。  さまざまな OBJECTIVE-C では、これらの遷移中に`NSObject`パラメーターは、オブジェクトのマネージ表現にラップされています。

ランタイムはネイティブ オブジェクトへの参照を取得し、オブジェクトへの最後のマネージ参照がないため、GC を実行する機会にまで参照を保持します。

いくつかの場合はの重要なC#ネイティブ オブジェクトへの参照を保持するランタイム。  これは、基になるネイティブ コードが特別な動作をパラメーターのライフ サイクルにアタッチされている場合にも発生します。  例: パラメーターのデストラクターがクリーンアップ アクションを実行またはいくつかの貴重なリソースを破棄します。

この属性は、可能であれば、上書きされたメソッドから OBJECTIVE-C に戻るときに破棄するオブジェクトをランタイムに通知します。

ルールは、単純な: かどうか、ランタイムは、ネイティブのオブジェクトから新しいマネージ表現を作成する必要があるし、関数の末尾には、ネイティブ オブジェクトの保持数は削除され、マネージ オブジェクトの Handle プロパティはクリアされます。   つまり、マネージ オブジェクトへの参照を保持する場合は、その参照が無駄になること (のメソッドを呼び出す例外がスローされます)。

渡されたオブジェクトが作成されていない場合、またはオブジェクトの未処理のマネージ表現が既に存在する場合は、強制 dispose は行われません。 


## <a name="property-attributes"></a>プロパティの属性

<a name="NotImplementedAttribute" />

### <a name="notimplementedattribute"></a>NotImplementedAttribute

この属性は、getter を持つプロパティが、基底クラスで導入された、変更可能なサブクラスには、set アクセス操作子が導入されています、OBJECTIVE-C の表現形式をサポートするために使用されます。

C#はこのモデルでは、基底クラスの setter と getter の両方がある必要がありますをサポートしません、サブクラスを使用できます、 [OverrideAttribute](#OverrideAttribute)します。

この属性は、プロパティの setter でのみ使用され、OBJECTIVE-C で変更可能な表現形式をサポートするために使用

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

マッピング`NSString`列挙値に定数が簡単に優れた .NET API を作成します。 これには:

* 表示することによって、便利であること、コード補完機能により、**のみ**API; に適切な値
* タイプ セーフでは、別に使用することはできません`NSString`; 正しくないコンテキストの定数と
* 短い API の一覧を表示する機能を失うことがなくコード補完を行ういくつかの定数を非表示にできます。

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

コード ジェネレーターを作成する上記のバインドの定義から、`enum`自体も作成し、`*Extensions`列挙型値間の 2 つの方法で変換メソッドを含む静的な型と`NSString`定数。 つまり、定数のまま、API の一部でない場合でも開発者が利用できます。

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

装飾できます**1 つ**この属性で列挙値。 これは、列挙型の値がわからないかどうかに返される定数になります。

上記の例では。

```csharp
var x = (NSRunLoopMode) 99;
Call (x.GetConstant ()); // NSDefaultRunLoopMode will be used
```

列挙型の値が修飾されていない場合、`NotSupportedException`がスローされます。

### <a name="errordomainattribute"></a>ErrorDomainAttribute

エラー コードは、列挙型の値としてバインドされます。 通常、それらのドメインのエラーがあるし、常に検索する 1 つが適用されます (またはも存在するかどうか) に簡単ではありません。

この属性を使用すると、エラー ドメイン列挙型自体を関連付けます。

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

拡張メソッドを呼び出して`GetDomain`ドメインのすべてのエラーの定数を取得します。

### <a name="fieldattribute"></a>FieldAttribute

これは、同じ`[Field]`定数型の内部で使用される属性です。 これは、こともできます列挙型の内部と特定の定数値をマップします。

A`null`値できますを指定する場合に返される必要があります列挙型の値を`null``NSString`定数を指定します。

上記の例では。

```csharp
var constant = NSRunLoopMode.NewInWatchOS3; // will be null in watchOS 2.x
Call (NSRunLoopModeExtensions.GetValue (constant)); // will return 1000
```

ない場合は`null`値が存在する、`ArgumentNullException`がスローされます。

## <a name="global-attributes"></a>グローバル属性

グローバル属性を使用して、適用、`[assembly:]`のような属性の修飾子、 [ `[LinkWithAttribute]` ](#LinkWithAttribute)またはできますなど、どこでも使用、 [ `[Lion]` ](#SinceAndLionAttributes)と[ `[Since]`](#SinceAndLionAttributes)属性。

<a name="LinkWithAttribute" />

### <a name="linkwithattribute"></a>LinkWithAttribute

これは、開発者が、gcc_flags を手動で構成するライブラリのコンシューマーを強制することがなく、バインドされたライブラリを再利用するために必要なリンクのフラグと、ライブラリに渡される追加 mtouch 引数を指定するアセンブリ レベル属性。

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

この属性がアセンブリ レベルで適用される、たとえば、これはどのような[CorePlot バインド](https://github.com/mono/monotouch-bindings/tree/master/CorePlot)を使用します。

```csharp
[assembly: LinkWith ("libCorePlot-CocoaTouch.a", LinkTarget.ArmV7 | LinkTarget.ArmV7s | LinkTarget.Simulator, Frameworks = "CoreGraphics QuartzCore", ForceLoad = true)]
```

使用すると、`[LinkWith]`属性は、指定した`libraryName`コマンド ライン フラグを正しく使用するために必要なだけでなく、非管理対象の依存関係を含む 1 つの DLL を出荷できるように、生成されたアセンブリに埋め込まれ、Xamarin.iOS ライブラリします。

提供することも、 `libraryName`、その場合、`LinkWith`属性を使用してのみ追加のリンカー フラグを指定することができます。

``` csharp
[assembly: LinkWith (LinkerFlags = "-lsqlite3")]
```

#### <a name="linkwithattribute-constructors"></a>LinkWithAttribute コンス トラクター

これらのコンス トラクターを使用すると、ライブラリとリンクし、サポートされているターゲット ライブラリをサポートして、ライブラリとリンクするために必要な任意の省略可能なライブラリ フラグ、結果として得られるアセンブリに埋め込むを指定できます。

なお、`LinkTarget`引数は、Xamarin.iOS によって推定され、設定する必要はありません。

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

#### <a name="linkwithattributeforceload"></a>LinkWithAttribute.ForceLoad

`ForceLoad`を決定するプロパティを使用するかどうか、`-force_load`リンク フラグは、ネイティブ ライブラリをリンクするために使用されます。 ここでは、これは常に true です。

#### <a name="linkwithattributeframeworks"></a>LinkWithAttribute.Frameworks

任意のフレームワークにバインドされているライブラリにハードという要件があるかどうか (以外の`Foundation`と`UIKit`)、設定する必要があります、`Frameworks`プロパティを必要なプラットフォーム、フレームワークのスペースで区切られた一覧を含む文字列。 たとえば、ライブラリにバインドしているかどうかを必要とする`CoreGraphics`と`CoreText`、設定、`Frameworks`プロパティを`"CoreGraphics CoreText"`します。

#### <a name="linkwithattributeiscxx"></a>LinkWithAttribute.IsCxx

結果として得られる実行可能ファイルは C コンパイラは、既定値ではなく C++ コンパイラを使用してコンパイルする必要がある場合は true には、このプロパティを設定します。 C++ で記述されたライブラリをバインドする場合は、これを使用します。

#### <a name="linkwithattributelibraryname"></a>LinkWithAttribute.LibraryName

バンドルするアンマネージ ライブラリの名前。 これは拡張子".a"ファイルであり、複数のプラットフォーム (ARM、x86、シミュレーターのなど) 用のオブジェクト コードを含めることができます。

以前のバージョンの Xamarin.iOS のチェック、 `LinkTarget` 、プラットフォーム、サポートされているライブラリを決定するプロパティが、これが、自動検出と`LinkTarget`プロパティは無視されます。

#### <a name="linkwithattributelinkerflags"></a>LinkWithAttribute.LinkerFlags

`LinkerFlags`文字列バインディングの作成者が、アプリケーションにネイティブ ライブラリをリンクするときに必要な追加のリンカー フラグを指定する方法を提供します。

たとえば、設定 libxml2 と zlib がネイティブ ライブラリが必要な場合、`LinkerFlags`文字列`"-lxml2 -lz"`します。

#### <a name="linkwithattributelinktarget"></a>LinkWithAttribute.LinkTarget

以前のバージョンの Xamarin.iOS のチェック、 `LinkTarget` 、プラットフォーム、サポートされているライブラリを決定するプロパティが、これが、自動検出と`LinkTarget`プロパティは無視されます。

#### <a name="linkwithattributeneedsgccexceptionhandling"></a>LinkWithAttribute.NeedsGccExceptionHandling

このプロパティをリンクするライブラリが GCC 例外処理 (gcc_eh) ライブラリが必要な場合は true に設定します。

#### <a name="linkwithattributesmartlink"></a>LinkWithAttribute.SmartLink

`SmartLink`プロパティは、Xamarin.iOS を決定できるようにする場合は true に設定する必要があるかどうか`ForceLoad`かどうかが必要です。

#### <a name="linkwithattributeweakframeworks"></a>LinkWithAttribute.WeakFrameworks

`WeakFrameworks`プロパティと同じように機能、`Frameworks`プロパティ、リンク時に、点を除いて、`-weak_framework`指定子は、各表示されているフレームワークの gcc に渡されます。

`WeakFrameworks` 使用するようできます必要に応じてそれらが使用可能なが受け取らない、ライブラリの機能を追加するものでは場合に便利ですがハードの依存関係に新しいライブラリおよびアプリケーション プラットフォーム フレームワークに対して弱くリンクすることによりiOS のバージョン。 脆弱なリンクの詳細については、Apple のドキュメントを参照して[弱いリンク](http://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WeakLinking.html)します。

脆弱なリンクに適した候補となります`Frameworks`などのアカウント、 `CoreBluetooth`、 `CoreImage`、 `GLKit`、`NewsstandKit`と`Twitter`iOS 5 で利用可能なだけであることから。

<a name="SinceAndLionAttributes" />

### <a name="sinceattribute-ios-and-lionattribute-macos"></a>SinceAttribute (iOS) および LionAttribute (macOS)

使用する、`[Since]`時刻に特定の時点で導入されるを持つものとしてフラグを Api に属性します。 属性は、型と基になるクラス、メソッドまたはプロパティが使用できない場合、ランタイムの問題を引き起こす可能性のあるメソッドにフラグを設定する場合にのみ使用する必要があります。

構文:

```csharp
public SinceAttribute : Attribute {
     public SinceAttribute (byte major, byte minor);
     public byte Major, Minor;
}
```

する必要があります一般にするため適用されません列挙型、制約、または構造体の新しいオペレーティング システムの以前のバージョンでのデバイスで実行された場合ランタイム エラーが発生ものはありません。

型に適用する場合の例:

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

`[Lion]`ライオンで導入された型が、同じ方法で属性を適用します。 使用する理由は、 `[Lion]` iOS で使用されている特定のバージョン番号とは、iOS は非常に多くの場合、OS X のメジャー リリースはほとんど発生し、オペレーティング システムのバージョン番号でよりも、コードネーム保存する方が簡単に改訂

### <a name="adviceattribute"></a>AdviceAttribute

この属性を使用して、使用するようの方が便利になる可能性があるその他の Api についてのヒントを開発者に提供します。   などの API の厳密に型指定されたバージョンを指定する場合、厳密に型指定された属性にこの属性を使用でしたより優れた API を開発者に知らせます。

この属性からの情報は、ドキュメントに表示されを向上させる方法については、ユーザーを提案するツールを開発できます。

### <a name="zerocopystringsattribute"></a>ZeroCopyStringsAttribute

のみ Xamarin.iOS 5.4 で使用できると新しいします。

この属性は、ジェネレーターを指示するこの特定のライブラリのバインド (で適用されている場合`[assembly:]`) または型が高速コピーのゼロの文字列のマーシャ リングを使用する必要があります。 この属性は、コマンド ライン オプションを渡すことに相当`--zero-copy`ジェネレーターにします。

コピーなしの文字列を使用する場合、ジェネレーター効果的に使用して同じC#Objective C を新しいの作成を発生させずに使用する文字列として文字列`NSString`オブジェクトおよびからデータをコピーせず、C#文字列をObjective C 文字列。 コピーのゼロの文字列を使用する唯一の欠点は、ことをラップする文字列のプロパティで、としてフラグを設定する点を確保`retain`または`copy`が、`[DisableZeroCopy]`属性に設定します。 コピーなしの文字列のハンドルがスタックに割り当てられているし、関数の戻り値に有効でないため、これが必要ですが。

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

アセンブリ レベル属性を適用することもでき、アセンブリのすべての型に適用されます。

```csharp
[assembly:ZeroCopyStrings]
```

## <a name="strongly-typed-dictionaries"></a>厳密に型指定されたディクショナリ

Xamarin.iOS 8.0 でラップする厳密に型指定されたクラスを簡単に作成するためのサポートが導入`NSDictionaries`します。

使用して常になったときに、 [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/)と共に、手動の API のデータ型になったこれを行うにははるかに簡単です。  詳細については、次を参照してください。[厳密な型の提示](~/cross-platform/macios/binding/objective-c-libraries.md#Surfacing_Strong_Types)します。

<a name="StrongDictionary" />

### <a name="strongdictionary"></a>StrongDictionary

インターフェイスにこの属性が適用されると、ジェネレーターでは、クラスから派生するインターフェイスと同じ名前で、生成[DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/)を厳密に型指定に、インターフェイスで定義されている各プロパティをオンにし、get アクセス操作子およびディクショナリの set アクセス操作子。

既存のインスタンス化できるクラスを自動的に生成この`NSDictionary`または新規作成されました。

この属性は、1 つのパラメーター ディクショナリの要素へのアクセスに使用されるキーを含むクラスの名前です。   既定では、属性を持つインターフェイス内の各プロパティは「キー」サフィックスが付いた名前の指定した型のメンバーを検索します。

例えば:

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

上記の場合、`MyOption`クラス用の文字列プロパティが生成されます`Name`を使用する、`MyOptionKeys.NameKey`文字列を取得する辞書にキーとして。   使用して、`MyOptionKeys.AgeKey`を取得するディクショナリ キーとして、 `NSNumber` int が含まれています

別のキーを使用する場合は、たとえば、プロパティのエクスポート属性を使用できます。

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

#### <a name="strong-dictionary-types"></a>ディクショナリの厳密な型

次のデータ型がでサポートされている、`StrongDictionary`定義。

|C#インターフェイスの種類|`NSDictionary` 記憶域の種類|
|---|---|
|`bool`|`Boolean` 格納されている、 `NSNumber`|
|列挙値|整数が格納されている、 `NSNumber`|
|`int`|32 ビットの整数に格納されている、 `NSNumber`|
|`uint`|32 ビット符号なし整数に格納されている、 `NSNumber`|
|`nint`|`NSInteger` 格納されている、 `NSNumber`|
|`nuint`|`NSUInteger` 格納されている、 `NSNumber`|
|`long`|64 ビットの整数に格納されている、 `NSNumber`|
|`float`|32 ビットの整数として格納されている、 `NSNumber`|
|`double`|64 ビットの整数として格納されている、 `NSNumber`|
|`NSObject` サブクラスと|`NSObject`|
|`NSDictionary`|`NSDictionary`|
|`string`|`NSString`|
|`NSString`|`NSString`|
|C#`Array`の `NSObject`|`NSArray`|
|C#`Array`の列挙型|`NSArray` 含む`NSNumber`値|
