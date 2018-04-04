---
title: バインディングの種類のリファレンス ガイド
ms.prod: xamarin
ms.assetid: C6618E9D-07FA-4C84-D014-10DAC989E48D
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 9364b4c56951ac9ebd3870e4afe41a40f9e1f455
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="binding-types-reference-guide"></a>バインディングの種類のリファレンス ガイド

このドキュメントには、バインディングをドライブにファイルを API コントラクトに注釈を付けるに使用して、コードが生成される属性の一覧がについて説明します

Xamarin.iOS と Xamarin.Mac API コントラクトは c# で記述された方法を定義するインターフェイス定義としてほとんどの場合 Objective C コードは、c# に提示されます。 プロセスには、さまざまなインターフェイスの宣言と API コントラクトを必要とする一部の基本的な型定義が含まれます。 バインドの種類の概要については、この必携ガイドを参照してください。[バインディング Objective C ライブラリ](~/cross-platform/macios/binding/objective-c-libraries.md)です。

## <a name="type-definitions"></a>型定義

構文:

```csharp
[BaseType (typeof (BTYPE))
interface MyType [: Protocol1, Protocol2] {
     IntPtr Constructor (string foo);
}
```

すべてのインターフェイスを持つコントラクト定義で、 [ `[BaseType]` ](#BaseTypeAttribute)生成されたオブジェクトの基本型を宣言する属性。 上記の宣言で、 `MyType` Objective C 型にバインドという名前の c# 型のクラスが生成される`MyType`です。

型名の後に任意の種類を指定する場合 (上記のサンプルで`Protocol1`と`Protocol2`) インターフェイスの継承の構文を使用してこれらのインターフェイスの内容はインラインになりますのコントラクトの一部であった場合、`MyType`です。
型は、プロトコルが採用する Xamarin.iOS サーフェスでは、どのようにインライン展開のすべてのメソッドと型自体に、プロトコルの中で宣言されたプロパティ。

次に示す方法の Objective C 宣言`UITextField`が Xamarin.iOS コントラクトで定義されます。

```objc
@interface UITextField : UIControl <UITextInput> {

}
```

書き込まれる次のように c# API コントラクトとして。

```csharp
[BaseType (typeof (UIControl))]
interface UITextField : UITextInput {
}
```

インターフェイスにその他の属性を適用するよう構成して、コード生成の他の多くの側面を制御できます、 [ `[BaseType]` ](#BaseTypeAttribute)属性。


### <a name="generating-events"></a>イベントを生成します。

Xamarin.iOS および Xamarin.Mac API の設計の 1 つの機能は、c# イベントやコールバックと OBJECTIVE-C デリゲート クラスをマップおです。 ユーザーを選択できますのインスタンスごとのようなプロパティに割り当てることによって、Objective C のプログラミング パターンを採用するかどうか`Delegate`Objective C のランタイムを呼び出すと、さまざまなメソッドを実装するクラスのインスタンスこのオプションを選択すると、c#-イベントおよびプロパティのスタイルを設定します。

お知らせ Objective C のモデルを使用する方法の 1 つの例を参照してください。

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

上記の例では、2 つのメソッドを上書きすることを決めました、指示するブール値を返すコールバックが通知をスクロール イベントが取得し、2 つ目を 1 つであるを確認できます、`scrollView`にスクロールするかどうか、有無の上位します。

C# のモデル、ライブラリの値を返すと予想されるコールバックをフックするために、c# イベントの構文またはプロパティの構文を使用して通知をリッスンするようにできます。

これは、ラムダを使用するように、同じ機能の c# コードの検索です。

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

イベント (void の戻り値の型がある) の値を返さないために、複数のコピーを接続することができます。 `ShouldScrollToTop` 、イベントではない型を持つプロパティでは代わりに`UIScrollViewCondition`この署名を持ちます。

```csharp
public delegate bool UIScrollViewCondition (UIScrollView scrollView);
```

返します、`bool`値、ここでは、ラムダの構文によりだけから値を返す、`MakeDecision`関数。

バインディング ジェネレーターが生成のイベントおよびリンクのようにクラス プロパティをサポートしている`UIScrollView`でその`UIScrollViewDelegate`(もこれらのモデル クラス) を呼び出すに注釈を付けるこれを行う、 [ `[BaseType]` ](#BaseTypeAttribute)と定義、`Events`と`Delegates`パラメーター (下記参照)。 注釈を付けるだけでなく、 [ `[BaseType]` ](#BaseTypeAttribute)これらのパラメーターがいくつかのより多くのコンポーネントのジェネレーターを通知するために必要です。

パラメーターを 1 つ以上のイベント (OBJECTIVE-C、規則は、デリゲート クラスの最初のパラメーターが送信元オブジェクトのインスタンスである) か名前を使用して生成される必要があります`EventArgs`クラスを使用します。 これは、 [ `[EventArgs]` ](#EventArgsAttribute)モデル クラスのメソッドの宣言での属性です。 例えば:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

上記の宣言が生成されます、`UIImagePickerImagePickedEventArgs`から派生したクラス`EventArgs`パックの両方のパラメーターと、`UIImage`と`NSDictionary`です。 ジェネレーターには、これが生成されます。

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

次の項目を公開し、`UIImagePickerController`クラス。

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

値を返すモデルのメソッドは、異なる方法でバインドされます。 これらには、生成されたデリゲート (C#) (メソッドのシグネチャ) とも、既定値を返す、ユーザーが自分自身実装を提供していない場合に、名の両方が必要があります。 たとえば、`ShouldScrollToTop`定義であります。

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIScrollViewDelegate {
    [Export ("scrollViewShouldScrollToTop:"), DelegateName ("UIScrollViewCondition"), DefaultValue ("true")]
    bool ShouldScrollToTop (UIScrollView scrollView);
}
```

上記が作成されます、`UIScrollViewCondition`シグネチャを持つデリゲート上に示したがいると、ユーザーが、実装を提供しない場合、戻り値が true になります。

加え、 [ `[DefaultValue]` ](#DefaultValueAttribute)属性を使用することも、 [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute)属性を通話や、で指定されたパラメーターの値を返すジェネレーターを誘導する[ `[NoDefaultValue]` ](#NoDefaultValueAttribute)既定値はありません、ジェネレーターに指示するパラメーターです。

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

使用する、 `Name` Objective C の世界でこの型にバインドされる名前を制御するプロパティです。 これは通常、c# 型は、.NET Framework デザイン ガイドラインに準拠しているが、その規則に従っていない Objective C の名前に対応する名前の指定を使用します。

例では、Objective C を割り当てる場合は、次の`NSURLConnection`型`NSUrlConnection`、.NET Framework デザイン ガイドラインは、"URL"ではなく"Url"を使用します。

```csharp
[BaseType (typeof (NSObject), Name="NSURLConnection")]
interface NSUrlConnection {
}
```

指定した名前が生成される値として使用`[Register]`バインディング内の属性です。 場合`Name`が指定されていない、型の短い名前がの値として使用される、`[Register]`生成された出力内の属性です。

#### <a name="basetypeevents-and-basetypedelegates"></a>BaseType.Events と BaseType.Delegates

これらのプロパティを使用して、ドライブの C# コードの生成-生成されたクラスでイベントのスタイルを設定します。 その OBJECTIVE-C デリゲート クラスで指定されたクラスをリンクに使用されます。 多くの場合、クラスは、デリゲート クラスを使用して、通知およびイベントを送信する場所が発生します。 たとえば、`BarcodeScanner`コンパニオンが`BardodeScannerDelegate`クラスです。 `BarcodeScanner`クラスには通常、`Delegate`のインスタンスを割り当てられるプロパティ`BarcodeScannerDelegate`すると共に、このしくみ、可能性があります公開する、ユーザーに、c#-使用するスタイルのイベント インターフェイスのように、このような場合、 `Events`と`Delegates`のプロパティ、 [ `[BaseType]` ](#BaseTypeAttribute)属性。

これらのプロパティは、常に一緒に設定されますと同じ数の要素が必要し、の同期を維持します。`Delegates`配列には、ラップする弱い型指定の各デリゲートの 1 つの文字列が含まれています、`Events`に関連付けるとする種類ごとに 1 つの型が配列に含まれています。

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

によって参照されるメソッドまで周囲そのオブジェクトのインスタンスで保持されるこのクラスの新しいインスタンスを作成するときにこの属性を適用する場合、`KeepRefUntil`呼び出されています。 これは、ユーザーのコードを使用するオブジェクトの周囲への参照を保持するしたくない場合に、独自の Api の使いやすさを向上させるために役立ちます。 このプロパティの値は内のメソッドの名前、`Delegate`クラスと組み合わせて、これを使用する必要がありますので、`Events`と`Delegates`プロパティもします。

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

この属性をインターフェイス定義に適用すると、ジェネレーターを既定のコンス トラクターを生成できなくなります。

オブジェクトのクラスの他のコンス トラクターのいずれかで初期化する必要がある場合は、この属性を使用します。


### <a name="privatedefaultctorattribute"></a>PrivateDefaultCtorAttribute

インターフェイス定義にこの属性が適用されるときに、フラグが設定される既定のコンス トラクターにプライベートです。 つまり、拡張ファイルから内部的にこのクラスのオブジェクトをインスタンスことができますが、クラスのユーザーにアクセスできるようにしてだけしないことです。

<a name="CategoryAttribute" />

### <a name="categoryattribute"></a>CategoryAttribute

Objective C のカテゴリをバインドして OBJECTIVE-C 機能を公開する方法を反映する c# 拡張メソッドとしてのこれらの公開を型定義でこの属性を使用します。

カテゴリは、メソッドとクラスで使用可能なプロパティのセットを拡張するために使用 OBJECTIVE-C メカニズムです。   実際には、それらを使用するか、基底クラスの機能を拡張する (たとえば`NSObject`) で特定のフレームワークがリンクされている場合 (たとえば`UIKit`)、使用できますが、新しいフレームワークがでリンクされている場合にのみ、そのメソッドを作成します。   その他のいくつかの場合、機能によって、クラスの機能を整理する使用されます。   これらは、c# 拡張メソッドを基本に似ています。

これは、カテゴリがでどのように目標 c:

```objc
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

上記の例がのインスタンスを拡張するためのライブラリで見つかった`UIView`メソッドを使用して`makeBackgroundRed`です。

これらをバインドするに使用することができます、 [ `[Category]` ](#CategoryAttribute)インターフェイス定義の属性です。   使用する場合、 [ `[Category]` ](#CategoryAttribute)属性の意味、 [ `[BaseType]` ](#BaseTypeAttribute)から拡張する型になるように、拡張する基本クラスを指定するために使用されている属性を変更します。

次に示す方法、`UIView`拡張機能がバインドされているし、c# 拡張メソッドに変換します。

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

上記の作成、`MyUIViewExtension`を格納するクラス、`MakeBackgroundRed`拡張メソッド。   つまり、呼び出すことができるようになりました`MakeBackgroundRed`の`UIView`サブクラスは目標 c が表示される同じ機能を提供するので、

場合によってはで見つかります**静的**カテゴリ内のメンバーは、次の例と同様にします。

```objc
@interface FooObject (MyFooObjectExtension)
+ (BOOL)boolMethod:(NSRange *)range;
@end
```

結果には、**誤った**カテゴリ c# インターフェイス定義。

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

これが正しくないためにを使用する、`BoolMethod`拡張機能のインスタンスを必要`FooObject`、ObjC をバインドするが、**静的**拡張機能では、これは、c# 拡張メソッドを実装する方法の原因による副作用です。

上記の定義を使用する唯一の方法は、次のような厄介コードによっては。

```csharp
(null as FooObject).BoolMethod (range);
```

この問題を回避するという推奨設定が、インライン、`BoolMethod`内の定義、`FooObject`インターフェイス定義自体、こうと、目的としているように、この拡張機能を呼び出すことができます`FooObject.BoolMethod (range)`です。

```csharp
[BaseType (typeof (NSObject))]
interface FooObject {

    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

発行される警告 (BI1117) おを見つける場合に、 [ `[Static]` ](#StaticAttribute)内のメンバー、 [ `[Category]` ](#CategoryAttribute)定義します。 実際にする場合[ `[Static]` ](#StaticAttribute)内のメンバー、 [ `[Category]` ](#CategoryAttribute)定義を使用して、警告を無音できます`[Category (allowStaticMembers: true)]`またはメンバーまたはのいずれかで修飾[`[Category]` ](#CategoryAttribute)インターフェイスで定義[ `[Internal]`](#InternalAttribute)です。

<a name="StaticAttribute_Class" />

### <a name="staticattribute"></a>StaticAttribute

静的クラスから派生していないものが生成されますだけこの属性がクラスに適用されるときに`NSObject`ので、 [ `[BaseType]` ](#BaseTypeAttribute)属性は無視されます。 静的クラスは、ホストに公開するパブリック変数の C に使用されます。

例えば:

```csharp
[Static]
interface CBAdvertisement {
    [Field ("CBAdvertisementDataServiceUUIDsKey")]
    NSString DataServiceUUIDsKey { get; }
```

次の API と c# クラスが生成されます。

```csharp
public partial class CBAdvertisement  {
    public static NSString DataServiceUUIDsKey { get; }
}
```

## <a name="protocolmodel-definitions"></a>プロトコル/モデルの定義

モデルは、プロトコルの実装で用いられます。
ランタイムのみに登録される OBJECTIVE-C メソッドを実際には、上書きされている点で異なります。
それ以外の場合、メソッドは登録されていません。

一般つまりサブクラスとしてマークされているクラス、 `ModelAttribute`、基本メソッドを呼び出す必要はありません。   例外をスローするメソッドを呼び出すと、オーバーライドするメソッドに対して、サブクラスに全体の動作を実装することになっています。

<a name="AbstractAttribute" />

### <a name="abstractattribute"></a>AbstractAttribute

既定では、プロトコルの一部であるメンバーの使用は必須ではありません。 これにより、ユーザーのサブクラスを作成する、`Model`オブジェクトだけを実行 (C#) クラスから派生するメソッドをオーバーライドのみ気をします。 Objective C コントラクトに、ユーザーがこのメソッドの実装を提供する必要な場合があります (フラグが付いたもの、`@required`ディレクティブ Objective C で)。 ような場合、これらのメソッドでのフラグを設定する必要があります、`[Abstract]`属性。

`[Abstract]`属性 methods または properties に適用でき、ジェネレーターを抽象クラスを抽象クラスを使用すると、生成されたメンバーのフラグを設定します。

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

ユーザーがモデル オブジェクトでこの特定のメソッドのメソッドを指定しない場合、モデルのメソッドによって返される既定値を指定します

構文:

```csharp
public class DefaultValueAttribute : Attribute {
        public DefaultValueAttribute (object o);
        public object Default { get; set; }
}
```

たとえば、次の虚数部のデリゲート クラスの`Camera`クラスを提供する、`ShouldUploadToServer`これで、プロパティとして公開は、`Camera`クラスです。 場合のユーザー、`Camera`クラスが明示的に設定されていない、true または false に応答できる lambda 値、戻り値の既定値ここでは false なりますで指定する値、`DefaultValue`属性。

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("camera:shouldPromptForAction:"), DefaultValue (false)]
    bool ShouldUploadToServer (Camera camera, CameraAction action);
}
```

ユーザーが虚数部のクラスのハンドラーを設定すると場合は、この値は無視されます。

```csharp
var camera = new Camera ();
camera.ShouldUploadToServer = (camera, action) => return SomeDecision ();
```

関連項目: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute)、 [ `[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)です。

<a name="DefaultValueFromArgumentAttribute" />

### <a name="defaultvaluefromargumentattribute"></a>DefaultValueFromArgumentAttribute

構文:

```csharp
public class DefaultValueFromArgumentAttribute : Attribute {
    public DefaultValueFromArgumentAttribute (string argument);
    public string Argument { get; }
}
```

モデル クラスの値を返すメソッドで提供されている場合は、この属性は、ジェネレーターを自分のメソッドまたはラムダ、ユーザーは提供されなかった場合に指定されたパラメーターの値を返すように指示されます。

例:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, nfloat progress);
}
```

上記の場合でのユーザー、`NSAnimation`クラス、c# イベント/プロパティのいずれかを使用することを選択し、設定しなかった`NSAnimation.ComputeAnimationCurve`メソッドまたはラムダでは、戻り値になります、進行状況のパラメーターに渡された値。

関連項目: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute)、 [`[DefaultValue]`](#DefaultValueAttribute)

### <a name="ignoredindelegateattribute"></a>IgnoredInDelegateAttribute

場合もありますが合理的イベントを公開またはため、この属性を追加するように指示することで、修飾された任意のメソッドの生成を回避するコード ジェネレーターは、ホスト クラスに、モデル クラスからプロパティをデリゲートにありません。

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

この属性は、モデルのメソッドを使用するデリゲートのシグネチャの名前を設定する値を返すことで使用されます。

例:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, float progress);
}
```

上記の定義に、ジェネレーターに、次のパブリック宣言が生成されます。

```csharp
public delegate float NSAnimationProgress (MonoMac.AppKit.NSAnimation animation, float progress);
```

### <a name="delegateapinameattribute"></a>DelegateApiNameAttribute

この属性は、ホスト クラスで生成されるプロパティの名前を変更するコード ジェネレーターを許可する使用されます。 場合があります便利 FooDelegate クラスのメソッドの名前は、デリゲート クラスの意味をなさなくが不自然プロパティとして、ホスト クラスになります。

またこれは非常に便利です (および必要な) と 2 つがある場合またはに意味のある複数のオーバー ロード メソッド保持 FooDelegate クラスでは、という名前にしますが、それらを向上させる特定の名前のホスト クラスで公開します。

例:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateApiName ("ComputeAnimationCurve"), DelegateName ("Func<NSAnimation, float, float>"), DefaultValueFromArgument ("progress")]
    float GetValueForProgress (NSAnimation animation, float progress);
}
```

上記の定義には、ホスト クラスでは、次のパブリック宣言をジェネレーターが生成されます。

```csharp
public Func<NSAnimation, float, float> ComputeAnimationCurve { get; set; }
```

<a name="EventArgsAttribute" />

### <a name="eventargsattribute"></a>EventArgsAttribute

パラメーターを 1 つ以上のイベント (OBJECTIVE-C、規則は、デリゲート クラスの最初のパラメーターが送信元オブジェクトのインスタンスである) に使用する生成された EventArgs クラスを使用する名前を指定する必要があります。 これは、`[EventArgs]`メソッド宣言での属性、`Model`クラスです。

例えば:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

上記の宣言が生成されます、 `UIImagePickerImagePickedEventArgs` EventArgs から派生し、両方のパラメーターをパックするクラス、`UIImage`と`NSDictionary`です。 ジェネレーターには、これが生成されます。

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

次の項目を公開し、`UIImagePickerController`クラス。

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```


### <a name="eventnameattribute"></a>EventNameAttribute

この属性はイベント メッセージまたはクラスで生成されるプロパティの名前を変更するジェネレーターを使用します。 場合があります便利モデル クラスのメソッドの名前は、モデル クラスの意味をなさなくが、イベントまたはプロパティとして、元のクラスに奇数になります。

たとえば、`UIWebView`から次のビットを使用して、 `UIWebViewDelegate`:

```csharp
[Export ("webViewDidFinishLoad:"), EventArgs ("UIWebView"), EventName ("LoadFinished")]
void LoadingFinished (UIWebView webView);
```

上記の公開`LoadingFinished`内のメソッドとして、`UIWebViewDelegate`が`LoadFinished`内にフックするためのイベントとして、 `UIWebView`:

```csharp
var webView = new UIWebView (...);
webView.LoadFinished += delegate { Console.WriteLine ("done!"); }
```

<a name="ModelAttribute" />

### <a name="modelattribute"></a>ModelAttribute

適用すると、 `[Model]` API、ランタイム、コントラクトの型定義を属性では、のみ現れます呼び出し、クラスのメソッドに、ユーザーが、クラスのメソッドを上書きされた場合の特殊なコードを生成します。 この属性は通常、OBJECTIVE-C デリゲート クラスをラップするすべての Api に適用されます。

<a name="NoDefaultValueAttribute" />

### <a name="nodefaultvalueattribute"></a>NoDefaultValueAttribute

モデルのメソッドが既定の戻り値を提供しないことを指定します。

Objective C ランタイムで応答することによって動作`false`Objective C のランタイム要求を指定されたセレクターがこのクラスで実装されたかどうかを判断します。

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("shouldDisplayPopup"), NoDefaultValue]
    bool ShouldUploadToServer ();
}
```

関連項目: [ `[DefaultValue]` ](#DefaultValueAttribute)、 [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)  

<a name="ProtocolAttribute" />

## <a name="protocols"></a>プロトコル

Objective C プロトコルの概念が本当にありません (C#)。 プロトコルは c# のインターフェイスに似ていますが、それらとは異なり、すべてのメソッドおよびプロトコルで宣言されたプロパティは、これを採用するクラスによって実装する必要があります。 代わりにメソッドおよびプロパティの一部は省略できます。

一部のプロトコルは通常、モデルのクラスとして使用、それらを使用してバインドする必要があります、 [ `[Model]` ](#ModelAttribute)属性。

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

新規および改良されたプロトコルが機能をバインディング Xamarin.iOS 7.0 以降が組み込まれました。  含まれている任意の定義、`[Protocol]`属性は、プロトコルを使用する方法を大幅に改善する 3 つのサポート クラスに実際に生成されます。

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

**クラスの実装**完全な抽象クラスの個々 のメソッドをオーバーライドおよび完全な型の安全性を取得できることを指定します。 C# 多重継承をサポートしていないためシナリオ別の基本クラスを必要とは、インターフェイスを実装することが必要です。

これは、ような場合、生成された**インターフェイス定義**で提供します。  これは、プロトコルからのすべての必要なメソッドを持つインターフェイスです。  これにより、開発者が単なるインターフェイスを実装するプロトコルを実装します。  ランタイムは、プロトコルを採用すると、型を自動的に登録されます。

インターフェイスがのみ、必要なメソッドを一覧表示し、省略可能なメソッドは公開ことに注意してください。   つまり、プロトコルを採用するクラス全体の型チェック、必要なメソッドが表示されますが、(手動でエクスポート属性の使用と、署名と一致する)、弱い型指定に頼る必要がメソッドでは省略可能なプロトコルです。

プロトコルを使用する API を使用すると便利なするためにをバインド ツールも生成されますをすべての省略可能なメソッドを公開する拡張メソッド クラス。   これは、API を使用している限りができるプロトコルをすべてのメソッドを持つものとして扱うことを意味します。

API でプロトコルの定義を使用する場合は、API の定義で空のスケルトンのインターフェイスを記述する必要があります。   API で、MyProtocol を使用する場合は、これを行う必要があります。

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

上記の必要なためにバインド時に、`IMyProtocol`が存在しない、つまり理由を空のインターフェイスを提供する必要があります。

### <a name="adopting-protocol-generated-interfaces"></a>プロトコルが生成したインターフェイスを採用します。

ときに実装するプロトコルは、次のように、生成されたインターフェイスのいずれか。

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

インターフェイス メソッドの実装は自動的に取得エクスポート、適切な名前を持つこのと等価であるため。

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

インターフェイスを実装すると、暗黙的または明示的には、関係ありません。

### <a name="protocol-inlining"></a>プロトコルのインライン展開

プロトコルを採用するとして宣言されている既存の Objective C 型をバインドするときにするインライン化、プロトコル直接できます。 これを行うことがなく、インターフェイスとしてだけで、プロトコルを宣言[ `[BaseType]` ](#BaseTypeAttribute)属性し、インターフェイスの基本インターフェイスのリストにプロトコルを一覧表示します。

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

このセクションの属性の型の個々 のメンバーに適用されます: プロパティとメソッドの宣言。


### <a name="alignattribute"></a>AlignAttribute

プロパティの戻り値の型のアラインメント値を指定するために使用します。 特定のプロパティが特定の境界に配置する必要がありますアドレスへのポインターを受け取る (この例で起こります一部 Xamarin.iOS で`GLKBaseEffect`16 バイトにする必要があるプロパティの配置) します。 Get アクセス操作子の装飾にこのプロパティを使用して、配置の値を使用できます。 これは通常使用、`OpenTK.Vector4`と`OpenTK.Matrix4`Objective C Api と統合されたときの型します。

例:

```csharp
public interface GLKBaseEffect {
    [Export ("constantColor")]
    Vector4 ConstantColor { [Align (16)] get; set;  }
}
```


### <a name="appearanceattribute"></a>AppearanceAttribute

`[Appearance]`属性は、外観マネージャーが導入された iOS 5 に制限されます。

`[Appearance]`メソッドまたはプロパティに参加している属性を適用できる、`UIAppearance`フレームワークです。 この属性を適用するメソッドまたはクラスのプロパティににより、このクラスのすべてのインスタンスまたは特定の条件に一致するインスタンスのスタイルを設定するために使用する外観の厳密に型指定されたクラスを作成するバインディングのジェネレーターが転送されます。

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

上記は、次のコードを生成 UIToolbar:

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

使用して、`[AutoReleaseAttribute]`内のメソッドにメソッドの呼び出しをラップするメソッドとプロパティに、`NSAutoReleasePool`です。

Objective C では、既定値に追加される値を返す方法をいくつか`NSAutoReleasePool`です。 既定では、これらには、スレッド`NSAutoReleasePool`、Xamarin.iOS さらに、維持、オブジェクトへの参照をマネージ オブジェクトに存在する限り、のでたくないに余分な参照を保持するが、`NSAutoReleasePool`は、スレッドまでドレイン取得のみ次のスレッドに制御を返します。 または、メイン ループに戻る。

たとえば大量のプロパティでこの属性は適用 (たとえば`UIImage.FromFile`)、既定値に追加されているオブジェクトを返す`NSAutoReleasePool`です。 この属性を持たないイメージは保持されますとしてメイン ループにコントロールが、スレッドでは返されませんでした。 Uf、スレッドが有効では常にバック グラウンド ダウンローダーのいくつかの並べ替えと、作業を待機しているイメージは解放されません。

### <a name="forcedtypeattribute"></a>ForcedTypeAttribute

`[ForcedTypeAttribute]`返されるアンマネージ オブジェクトでバインディングの定義で示される型が一致しない場合でも、マネージ型の作成を強制するために使用します。

ヘッダーで示される型が、ネイティブ メソッドの戻り値の型と一致しない場合に便利ですが、たとえば、次の OBJECTIVE-C 定義からを受け取る`NSURLSession`:

`- (NSURLSessionDownloadTask *)downloadTaskWithRequest:(NSURLRequest *)request`

返されることを明確に示す、`NSURLSessionDownloadTask`インスタンスが、まだその**を返します**、 `NSURLSessionTask`、スーパークラスにないために変換できると`NSURLSessionDownloadTask`です。 タイプ セーフなコンテキストられているので、`InvalidCastException`は行われます。

ヘッダーの説明に従っているを回避するため、 `InvalidCastException`、`[ForcedTypeAttribute]`を使用します。

```csharp
[BaseType (typeof (NSObject), Name="NSURLSession")]
interface NSUrlSession {

    [Export ("downloadTaskWithRequest:")]
    [return: ForcedType]
    NSUrlSessionDownloadTask CreateDownloadTask (NSUrlRequest request);
}
```

`[ForcedTypeAttribute]`もという名前のブール値を受け入れる`Owns`つまり`false`既定`[ForcedType (owns: true)]`です。 パラメーターは次に使用を所有している、[所有権ポリシー](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html)の**基盤**オブジェクト。

`[ForcedTypeAttribute]`パラメーター、プロパティ、および戻り値でのみ有効です。

<a name="BindAsAttribute" />

### <a name="bindasattribute"></a>BindAsAttribute

`[BindAsAttribute]`により、バインド`NSNumber`、`NSValue`と`NSString`(列挙) より正確な c# の型にします。 属性を作成も優れたより正確に使用できる、ネイティブの API 経由での .NET API です。

装飾できるは、(戻り値) のメソッドやパラメーターを持つプロパティ`BindAs`です。 唯一の制限は、メンバー**いない必要があります**内にある、`[Protocol]`または[ `[Model]` ](#ModelAttribute)インターフェイスです。

例えば:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

出力します。

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

内部的に処理を行います、 `bool?`  <->  `NSNumber`と`CGRect`  <->  `NSValue`変換します。

現在のカプセル化のサポートされている型は次のとおりです。

* `NSValue`
* `NSNumber`
* `NSString`

#### <a name="nsvalue"></a>NSValue

次の c# のデータ型はから/にカプセル化するサポート`NSValue`:

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

次の c# のデータ型はから/にカプセル化するサポート`NSNumber`:

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

[`[BindAs]`](#BindAsAttribute) と一緒に動作[列挙型に支えられて NSString 定数](#enum-attributes)のため、たとえば向上の .NET API を作成することができます。

```csharp
[BindAs (typeof (CAScroll))]
[Export ("supportedScrollMode")]
NSString SupportedScrollMode { get; set; }
```

出力します。

```csharp
[Export ("supportedScrollMode")]
CAScroll SupportedScrollMode { get; set; }
```

処理されるため、 `enum`  <->  `NSString`変換指定の列挙型を入力する場合にのみ[ `[BindAs]` ](#BindAsAttribute)は[NSString 定数によってバックアップ](#enum-attributes)です。

#### <a name="arrays"></a>配列

[`[BindAs]`](#BindAsAttribute) 配列をサポートもサポートされている型のいずれか、例としては、次の API 定義を持つことができます。

```csharp
[return: BindAs (typeof (CAScroll []))]
[Export ("getScrollModesAt:")]
NSString [] GetScrollModes ([BindAs (typeof (CGRect []))] NSValue [] rects);
```

出力します。

```csharp
[Export ("getScrollModesAt:")]
CAScroll? [] GetScrollModes (CGRect [] rects) { ... }
```

`rects`にパラメーターをカプセル化されます、`NSArray`を格納している、`NSValue`各`CGRect`され、戻り値の配列を取得`CAScroll?`これが作成されて、返された値を使用して`NSArray`含む`NSStrings`です。

<a name="BindAttribute" />

### <a name="bindattribute"></a>BindAttribute

`[Bind]`属性が 2 つの用途メソッドまたはプロパティの宣言、および個々 の get アクセス操作子またはプロパティに setter を適用するときに、もう 1 つに適用されるときに 1 つです。

メソッドまたはプロパティの効果の使用時に、`[Bind]`属性は、指定されたセレクターを呼び出すメソッドを生成します。 その結果、生成されたメソッドがで修飾されていないが、 [ `[Export]` ](#ExportAttribute)属性は、メソッドのオーバーライドで参加できるいないことを意味します。 これは通常と組み合わせて使用、 `[Target]` Objective C の拡張メソッドを実装するための属性です。

例えば:

```csharp
public interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

Getter または setter で使用する場合、`[Bind]`プロパティの get アクセス操作子および set アクセス操作子 OBJECTIVE-C セレクター名を生成するときに、コード ジェネレーターによって推論される既定の設定を変更する属性を使用します。 既定では、名前のプロパティのフラグを設定すると`fooBar`、ジェネレーターが生成、 `fooBar` getter のエクスポートと`setFooBar:`set アクセス操作子のです。 いくつかの場合、OBJECTIVE-C 従わないこの規則でする get アクセス操作子の名前を変更、通常`isFooBar`です。
このコード ジェネレーターを通知するために、この属性を使用します。

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

のみ Xamarin.iOS 6.3 でご確認いただけます、新しいです。

この属性は、完了ハンドラーの最後の引数として受け取らないメソッドに適用できます。

使用することができます、`[Async]`最後の引数は、コールバック メソッドの属性です。  バインディング ジェネレーターがサフィックスを持つメソッドのバージョンを生成はこれをメソッドに適用すると、ときに`Async`です。  コールバックがパラメーターをとらない場合、戻り値になります、 `Task`、コールバックがパラメーターを受け取る場合、結果になります、`Task<T>`です。

```csharp
[Export ("upload:complete:")]
[Async]
void LoadFile (string file, NSAction complete)
```

この非同期メソッドは、次の場合に生成するされます。

```csharp
Task LoadFileAsync (string file);
```

設定する必要があります、コールバックが複数のパラメーターを受け取る場合、`ResultType`または`ResultTypeName`に必要なすべてのプロパティを保持する、生成された型の名前を指定します。

```csharp
delegate void OnComplete (string [] files, nint byteCount);

[Export ("upload:complete:")]
[Async (ResultTypeName="FileLoading")]
void LoadFiles (string file, OnComplete complete)
```

次はこの非同期メソッドを生成、`FileLoading`両方にアクセスするプロパティが含まれます`files`と`byteCount`:

```csharp
Task<FileLoading> LoadFile (string file);
```

コールバックの最後のパラメーターがある場合、 `NSError`、生成された`Async`メソッドは値が null でないかどうかと、生成される非同期メソッドは、タスクの例外を設定する場合を確認します。

```csharp
[Export ("upload:onComplete:")]
[Async]
void Upload (string file, Action<string,NSError> onComplete);
```

上記の例は、次の非同期メソッドを生成します。

```csharp
Task<string> UploadAsync (string file);
```

エラーの場合、結果のタスクが例外に設定し、 `NSErrorException` 、その結果をラップする`NSError`です。

#### <a name="asyncattributeresulttype"></a>AsyncAttribute.ResultType

このプロパティを使用して、返される値を指定する`Task`オブジェクト。   このパラメーターは、既存の種類、そのため、主要な api 定義のいずれかで定義する必要があります。

#### <a name="asyncattributeresulttypename"></a>AsyncAttribute.ResultTypeName

このプロパティを使用して、返される値を指定する`Task`オブジェクト。   このパラメーターには、目的の型名の名前、一連のコールバックを受け取るパラメーターごとに 1 つのプロパティをジェネレーターが生成されます。

#### <a name="asyncattributemethodname"></a>AsyncAttribute.MethodName

このプロパティを使用すると、生成される非同期メソッドの名前をカスタマイズできます。   既定値が、メソッドの名前を使用し、「Async」というテキストを追加するには、これを使用して、この既定を変更することができます。

### <a name="disablezerocopyattribute"></a>DisableZeroCopyAttribute

この属性は、文字列パラメーターまたは文字列のプロパティに適用されてとコード ジェネレーターは、このパラメーターのマーシャ リング 0 コピー文字列を使用するよう指示、代わりに、c# の文字列から新しい NSString インスタンスを作成します。
文字列をゼロ コピーのいずれかを使用してマーシャ リングを使用するコード ジェネレーターを指示する場合、この属性は文字列に必要のみ、`--zero-copy`またはアセンブリ レベル属性を設定するコマンド ライン オプション`ZeroCopyStringsAttribute`です。

これは、プロパティがある OBJECTIVE-C で宣言されている場合に必要な`retain`または`assign`プロパティの代わりに、`copy`プロパティです。 これらは、通常、最適化されている間違って""の開発者によってサード パーティ製のライブラリで発生します。 一般に、`retain`または`assign``NSString`プロパティが誤って以降`NSMutableString`のユーザーから派生したクラスまたは`NSString`微妙に互換性に影響する、ライブラリのコードに関する知識なし文字列の内容が変わる可能性があります、アプリケーション。 通常これは、不完全な最適化が原因で発生します。

C: の目的でこのような 2 つのプロパティを次に示します

```csharp
@property(nonatomic,retain) NSString *name;
@property(nonatomic,assign) NSString *name2;
```


### <a name="disposeattribute"></a>DisposeAttribute

適用すると、`[DisposeAttribute]`に追加されるコード スニペットを提供するクラスに、`Dispose()`クラスのメソッドの実装です。

以降、`Dispose`メソッドはによって自動的に生成、`bmac-native`と`btouch-native`ツールを使用する必要があります、 `[Dispose]` 、生成されたコードを挿入する属性`Dispose`メソッドの実装です。

例えば:

```csharp
[BaseType (typeof (NSObject))]
[Dispose ("if (OpenConnections > 0) CloseAllConnections ();")]
interface DatabaseConnection {
}
```

<a name="ExportAttribute" />

### <a name="exportattribute"></a>ExportAttribute

`[Export]`メソッドまたは Objective C のランタイムに公開するプロパティ フラグを設定する属性を使用します。 この属性は、バインド ツールと実際の Xamarin.Mac、Xamarin.iOS およびランタイムの間で共有されます。 メソッドの場合は渡されたパラメーター生成されたコードに逐語的プロパティ、get アクセス操作子および set アクセス操作子のエクスポートが基本宣言に基づいて生成されるため (を参照してください、 [ `[BindAttribute]` ](#BindAttribute)を変更する方法については、ツールの動作のバインディング)。

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

この属性は要求時に読み込まれ、c# コードに公開するフィールドとして C グローバル変数を公開に使用します。 通常これは C または OBJECTIVE-C で定義されているとなる可能性が一部の Api で使用されているいずれかのトークンや値を持つは不透明として使用する必要がありますの定数の値を取得する必要が、ユーザー コードでは、します。

構文:

```csharp
public class FieldAttribute : Attribute {
    public FieldAttribute (string symbolName);
    public FieldAttribute (string symbolName, string libraryName);
    public string SymbolName { get; set; }
    public string LibraryName { get; set; }
}
```

`symbolName`とリンクする C 記号。 既定では、これは、型が定義されている名前は、名前空間から推測ライブラリから読み込まれます。 ここで、シンボルは、参照が、ライブラリでない場合を渡す必要があります、`libraryName`パラメーター。 スタティック ライブラリをリンクする場合を使用して`__Internal`として、`libraryName`パラメーター。

生成されたプロパティは、常に静的です。

次の種類のフィールドの属性を持つフラグが設定されたプロパティになります。

* `NSString`
* `NSArray`
* `nint` / `int` / `long`
* `nuint` / `uint` / `ulong`
* `nfloat` / `float`
* `double`
* `CGSize`
* `System.IntPtr`
* 列挙体

Set アクセス操作子はサポートされていません[NSString 定数によってバックアップされた列挙型](#enum-attributes)、手動でバインドが必要な場合は。

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

`[Internal]`属性は、メソッドまたはプロパティに適用できで生成されたコードに対するフラグの設定の効果がある、 `internal` c# のキーワードが生成されたアセンブリでコードをコードにのみアクセスできるようにします。 これは通常、非常に低レベルの Api を非表示に、時またはジェネレーターでサポートされていない api を向上させる最適のパブリック API を提供またはいくつか手動コーディングを必要として使用します。

C# 補完的なサポート ファイルに追加し公開する厳密に型指定されたラッパーと、通常はメソッドまたはこの属性を使用してプロパティを非表示にし、メソッドまたはプロパティに別の名前を提供するバインドをデザインするときに、基になる機能です。

例えば:

```csharp
[Internal]
[Export ("setValue:forKey:")]
void _SetValueForKey (NSObject value, NSObject key);

[Internal]
[Export ("getValueForKey:")]
NSObject _GetValueForKey (NSObject key);
```

次に、サポート ファイルで次のようにいくつかのコードがある可能性があります。

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

この属性は、.NET を使用して注釈が付けられるプロパティのバッキング フィールドをフラグ`[ThreadStatic]`属性。 これは、フィールドが、スレッドの静的変数である場合に便利です。

### <a name="marshalnativeexceptions-xamarinios-606"></a>MarshalNativeExceptions (Xamarin.iOS 6.0.6)

この属性には、メソッドのサポートにネイティブ (OBJECTIVE-C) 例外となります。
呼び出し元ではなく`objc_msgSend`を直接呼び出しにも例外をキャッチし、それらをマネージ コードの例外をマーシャ リングするカスタム トランポリンです。

現在は少量のみ`objc_msgSend`署名はサポートされて (学びます不足している monotouch_ でバインディングを使用したアプリのネイティブのリンクが失敗した場合、署名はサポートされていませんかどうか*_objc_msgSend*シンボル)、詳細を指定できますが、要求に追加されます。


### <a name="newattribute"></a>NewAttribute

この属性がメソッドに適用され、ジェネレーターにプロパティが生成される、`new`宣言の前にキーワード。

同じメソッドまたはプロパティ名が基本クラスに既に存在していたサブクラスで導入されたときに、コンパイラの警告を避けるために使用されます。

<a name="NotificationAttribute" />

### <a name="notificationattribute"></a>NotificationAttribute

この属性は、ジェネレーター生成厳密に型指定されたヘルパー通知クラスを持つようにフィールドに適用できます。

なし、運ぶ通知の引数を指定せず、この属性を使用できますかを指定することができます、 `System.Type` API の定義で別のインターフェイスを通常"EventArgs"で終わる名前で参照します。 ジェネレーターは、インターフェイスに変換クラスをサブクラス化する`EventArgs`すべてのプロパティが一覧表示が含まれます。 [ `[Export]` ](#ExportAttribute)で属性を使用する必要があります、`EventArgs`値がフェッチ OBJECTIVE-C ディクショナリの検索に使用されるキーの名前を一覧表示するクラス。

例えば:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

上記のコードが入れ子になったクラスを生成する`MyClass.Notifications`以下の方法で。

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
      public static NSObject ObserveDidStart (NSObject objectToObserve, EventHandler<NSNotificationEventArgs> handler)
   }
}
```

ポストされた通知に、コードのユーザーが簡単にサブスクライブできますし、 [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/)次のようにコードを使用しています。

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

またはを観察する特定のオブジェクトを設定します。 渡す場合`null`に`objectToObserve`このメソッドがその他のピアと同じように動作します。

```csharp
var token = MyClass.Notifications.ObserverDidStart (objectToObserve, (notification) => {
    Console.WriteLine ("Observed the 'DidStart' event on objectToObserve!");
});
```

返された値`ObserveDidStart`を簡単に次のように、通知の受信を停止するために使用できます。

```csharp
token.Dispose ();
```

呼び出すか、または[NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject//)トークンを渡します。 パラメーターが、通知に含まれる場合は、ヘルパーを指定する必要があります`EventArgs`インターフェイスは、次のようにします。

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

上記が生成されます、`MyScreenChangedEventArgs`クラス、`ScreenX`と`ScreenY`からデータをフェッチするプロパティ、 [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/)ディクショナリのキーを使用して`ScreenXKey`と`ScreenYKey`それぞれの適切な変換を適用します。 `[ProbePresence]`属性はプローブ、キーが設定されている場合に、ジェネレーターの使用、`UserInfo`値を抽出しようとしています。 代わりにします。 これは、キーの存在が (通常はブール値) の値の場合に使用されます。

次のようにコードを記述できます。

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

場合によっては、ディクショナリに渡された値に関連付けられている定数はありません。  Apple では、場合によってパブリック シンボルの定数を使用し、場合もあります文字列定数を使用します。  既定では、 [ `[Export]` ](#ExportAttribute)を指定した属性`EventArgs`クラスが使用する、指定された名前、パブリック シンボルとして実行時に検索します。  これは、ケースではありませんし、代わりとしてサポートされている、文字列定数を検索しを渡す場合、`ArgumentSemantic.Assign`エクスポート属性の値。

**Xamarin.iOS 8.4 の新機能**

場合によっては、通知はの有効期間開始、引数を指定せずそのための使用[ `[Notification]` ](#NotificationAttribute)を含まない引数は許容されます。  場合によっては、その通知にパラメーターが導入される予定です。  このシナリオをサポートするためには、属性を複数回適用できます。

バインドを開発している既存のユーザー コードが壊れないようにする場合から既存の通知を有効にするとします。

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

次のように 2 回、通知の属性を一覧表示するためのバージョン。

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

プロパティ値を許可するようにフラグが設定のプロパティにこれを適用すると`null`をそれに割り当てられます。 これは参照型の有効なだけです。

これが適用されたとき、指定されたパラメーターを null にできることと、チェックを実行することなしに渡すことを示します。 メソッド シグネチャ内のパラメーターに`null`値。

参照型がこの属性を持たない場合バインディング Objective C に渡す前に割り当てられている値のチェックが生成されますされをスローするチェックが生成されます、`ArgumentNullException`割り当てられた値が場合`null`です。

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

この属性を使用して、この特定のメソッドのバインディングでフラグが付けられますがバインド ジェネレーターに指示する、`override`キーワード。

### <a name="presnippetattribute"></a>PreSnippetAttribute

この属性を使用して、目標 C. にコードの呼び出しの前に、入力パラメーターが検証されると後に、挿入するためのコードを注入することができます。

例:

```csharp
[Export ("demo")]
[PreSnippet ("var old = ViewController;")]
void Demo ();
```

### <a name="prologuesnippetattribute"></a>PrologueSnippetAttribute

この属性を使用して、生成されたメソッドのパラメーターのいずれかが検証する前に挿入するためのコードを挿入することができます。

例:

```csharp
[Export ("demo")]
[Prologue ("Trace.Entry ();")]
void Demo ();
```

### <a name="postgetattribute"></a>PostGetAttribute

値をフェッチするには、このクラスから指定したプロパティを呼び出し、バインド ジェネレーターに指示します。

このプロパティは通常を参照するオブジェクト グラフを保持する参照オブジェクトを指すキャッシュの更新に使用されます。 通常追加/削除などの操作のあるコードを示します。 要素が追加または削除、内部キャッシュを更新することを確認後にマネージ オブジェクトへの参照が実際に使用する保持できるように、このメソッドが使用されます。 これにはバインド ツールは、特定のバインディングですべての参照オブジェクトのバッキング フィールドを生成するためです。

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

ここで、`Dependencies`プロパティを追加またはから依存関係を削除後に呼び出される、`NSOperation`オブジェクトを表す実際グラフがあることを確認するには、メモリの破損だけでなく、メモリ リークの防止、オブジェクトが読み込まれています。

### <a name="postsnippetattribute"></a>PostSnippetAttribute

この属性を使用するには、コードが、基になる OBJECTIVE-C メソッドを呼び出した後に挿入するのに c# ソース コードを挿入するには

例:

```csharp
[Export ("demo")]
[PostSnippet ("if (old != null) old.DemoComplete ();")]
void Demo ();
```

### <a name="proxyattribute"></a>ProxyAttribute

この属性は、プロキシ オブジェクトとそれらにフラグを設定する値を返すに適用されます。 いくつか Objective C Api 戻りプロキシ オブジェクトにユーザーのバインディングとは区別されないことができます。 この属性の効果がオブジェクトに付けるのフラグを設定するには、`DirectBinding`オブジェクト。 Xamarin.Mac のシナリオを参照してください、[このバグのディスカッション](https://bugzilla.novell.com/show_bug.cgi?id=670844)です。

### <a name="retainlistattribute"></a>RetainListAttribute

パラメーターへの参照をマネージを残すか、パラメーターに、内部参照を削除するジェネレーターに指示します。 参照されるオブジェクトの保持に使用されます。

構文:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

場合の値`doAdd`が true の場合にパラメーターを追加、`__mt_{0}_var List<NSObject>;`です。 ここで`{0}`に置き換え、指定された`listName`です。 API には、補完的な部分クラスでは、このバッキング フィールドを宣言する必要があります。

例については、次を参照してください[foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs)と[NSNotificationCenter.cs。](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)

### <a name="releaseattribute-xamarinios-60"></a>ReleaseAttribute (Xamarin.iOS 6.0)

これが返すジェネレーターを呼び出す必要がありますを示す型を適用できる`Release`の返送前に、オブジェクトにします。 これが必要になるだけメソッドは、(最も一般的なシナリオでは、自動発行オブジェクト) ではなく保持されているオブジェクトを提供します。

例:

```csharp
[Export ("getAndRetainObject")]
[return: Release ()]
NSObject GetAndRetainObject ();
```

さらにこの属性は、Xamarin.iOS ランタイムは、このような関数から Objective C に戻ったときにオブジェクトを保持する必要がありますを認識できるように、生成されたコードに反映されます。


### <a name="sealedattribute"></a>SealedAttribute

ジェネレーターに、生成されたメソッドをシール済みとしてフラグを設定するように指示します。 この属性が指定されていない場合、既定では、(仮想メソッド、抽象メソッドまたはその他の属性の使用方法に応じて、上書き) の仮想メソッドを生成します。

<a name="StaticAttribute" />

### <a name="staticattribute"></a>StaticAttribute

ときに、`[Static]`メソッドまたはプロパティに属性が適用される、静的メソッドまたはプロパティが生成されます。 この属性が指定されていないインスタンス メソッドまたはプロパティ、ジェネレーターが生成されます。


### <a name="transientattribute"></a>TransientAttribute

このフラグは、値が一時的なものでは、オブジェクト プロパティである iOS によって一時的に作成されますが、長期間維持される属性を使用します。 この属性を適用するプロパティに、ジェネレーターはこのプロパティは、マネージ クラスが、オブジェクトへの参照を保持しないことを意味のバッキング フィールドを作成できません。

<a name="WrapAttribute" />

### <a name="wrapattribute"></a>WrapAttribute

Xamarin.iOS/Xamarin.Mac バインドの設計では、`[Wrap]`弱い型指定のオブジェクトと厳密に型指定されたオブジェクトをラップする属性を使用します。 型として宣言は、通常 OBJECTIVE-C デリゲート オブジェクトを使用するほとんどの場合に、この`id`または`NSObject`です。 Xamarin.iOS および Xamarin.Mac で使用される規則は、型としてのこれらのデリゲートまたはデータ ソースを公開する`NSObject`し、名前には、規則の"Weak"+ 公開されている名前を使用します。 `id delegate` Objective C からのプロパティとして公開できる、 `NSObject WeakDelegate { get; set; }` API コントラクト ファイルのプロパティです。

通常このデリゲートに割り当てられている値を厳密な型のため、厳密な型を提示して適用して、`[Wrap]`属性、つまり、ユーザーはいくつかの細かい制御が必要な場合、または低レベル tric に頼る必要がある場合は、弱い型を使用できますks、またはこれらは、作業のほとんどの厳密に型指定されたプロパティを使用することができます。

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

これは、ユーザーが、デリゲートの弱い型指定のバージョンを使用する方法です。

```csharp
// The weak case, user has to roll his own
class SomeObject : NSObject {
    [Export ("doDemo")]
    void CallbackForDoDemo () {}

}

var demo = new Demo ();
demo.WeakDelegate = new SomeObject ();
```

これは、ユーザーは、厳密に型指定されたバージョンを使用して、彼はないことを手動でと、ユーザーが # の型システムを活用し、彼の目的を宣言する、override キーワードを使用していることを確認する方法を装飾を使用してメソッドおよび`[Export]`ですので、ユーザーのバインディングで使用できます。

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

によって生成されたメンバー`[Wrap]`されない`virtual`既定では、必要がある場合、`virtual`メンバーを設定することができます`true`省略可能な`isVirtual`パラメーター。

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

このセクションには、メソッド定義のパラメーターに適用できる属性がについて説明しますだけでなく、`[NullAttribute]`全体のプロパティを適用します。

<a name="BlockCallback" />

### <a name="blockcallback"></a>BlockCallback

この属性は、対象のパラメーターが呼び出し規約、OBJECTIVE-C ブロックに準拠し、この方法でマーシャ リングする必要があります、バインダーに通知する (C#) デリゲートの宣言のパラメーターの型に適用されます。

これは通常 c: の目的で次のように定義されているコールバックの使用します。

```csharp
typedef returnType (^SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

関連項目: [CCallback](#CCallback)です。

<a name="CCallback" />

### <a name="ccallback"></a>CCallback

この属性は、対象のパラメーターは C ABI 関数ポインターの呼び出し規約に準拠しているし、この方法でマーシャ リングする必要があります、バインダーに通知する (C#) デリゲートの宣言のパラメーターの型に適用されます。

これは通常 c: の目的で次のように定義されているコールバックの使用します。

```objc
typedef returnType (*SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

関連項目: [BlockCallback](#BlockCallback)です。

### <a name="params"></a>params

使用することができます、`[Params]`定義で"params"を挿入ジェネレーターにメソッド定義の最後の配列パラメーターの属性です。   これにより、簡単にできるようにする省略可能なパラメーターにバインドできます。

たとえば、次のような定義があるとします。

```csharp
[Export ("loadFiles:")]
void LoadFiles ([Params]NSUrl [] files);
```

次のコードを記述するを使用できます。

```csharp
foo.LoadFiles (new NSUrl (url));
foo.LoadFiles (new NSUrl (url1), new NSUrl (url2), new NSUrl (url3));
```

これにより、要素を渡すためには、純粋な配列を作成するユーザーは必要ありません利点があります。

<a name="plainstring" />

### <a name="plainstring"></a>PlainString

使用することができます、`[PlainString]`属性、バインド ジェネレーターとしてパラメーターを渡す代わりに、C の文字列として文字列を渡すよう指示するための文字列パラメーターの前に、`NSString`です。

Objective C のほとんどの Api 消費`NSString`パラメーターが、いくつかの Api を公開、`char *`の代わりに、文字列を渡すための API、`NSString`バリエーションの 1 つです。
使用して`[PlainString]`そのような場合です。

たとえば、次の Objective C 宣言します。

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

指定されたパラメーターの参照を保持するジェネレーターに指示します。 ジェネレーターは、このフィールドをバッキング ストアを提供するか、名前を指定できます (、 `WrapName`) に値を格納します。 Objective C にパラメーターとして渡され、わかっている場合に Objective C でこのオブジェクトのコピーでのみ保持することは、マネージ オブジェクトへの参照を保持するために便利です。 ような API のインスタンス、`SetDisplay (SomeObject)`可能性のある、SetDisplay でしたのみ 1 つのオブジェクトを同時に表示されているこの属性を使用します。 複数のオブジェクト (たとえば、スタックのような API) の追跡に必要な場合は使用、`[RetainList]`属性。

構文:

```csharp
public class RetainAttribute {
    public RetainAttribute ();
    public RetainAttribute (string wrapName);
    public string WrapName { get; }
}
```


### <a name="retainlistattribute"></a>RetainListAttribute

パラメーターへの参照をマネージを残すか、パラメーターに、内部参照を削除するジェネレーターに指示します。 参照されるオブジェクトの保持に使用されます。

構文:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

場合の値`doAdd`が true の場合にパラメーターを追加、`__mt_{0}_var List<NSObject>`です。 ここで`{0}`に置き換え、指定された`listName`です。 API には、補完的な部分クラスでは、このバッキング フィールドを宣言する必要があります。

例については、次を参照してください[foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs)と[NSNotificationCenter.cs。](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)


### <a name="transientattribute"></a>TransientAttribute

この属性は、パラメーターに適用し、OBJECTIVE-C (C#) を移行する場合にのみ使用されます。  さまざまな OBJECTIVE-C その遷移中に`NSObject`パラメーターは、マネージ オブジェクトの表現にラップされます。

ランタイムはネイティブのオブジェクトへの参照を受け取るし、オブジェクトへの最後のマネージ参照が削除され、GC を実行する機会になるまで、参照を保持します。

いくつかの場合が c# ランタイムがないネイティブ オブジェクトへの参照を保持するため重要です。  場合、基になるネイティブ コードでは、特別な動作は、パラメーターのライフ サイクルにアタッチします。  例: パラメーターのデストラクターがクリーンアップ アクションを実行または貴重なリソースを破棄します。

この属性は、可能であれば Objective C に上書きされたメソッドから返されるときに破棄するオブジェクトを希望するランタイムを通知します。

ルールは、単純な: かどうか、ランタイムはネイティブのオブジェクトから新しいマネージ表現を作成する必要があるし、関数の最後に、ネイティブ オブジェクトの保持数は削除、され、マネージ オブジェクトのハンドル プロパティはクリアされます。   つまり、マネージ オブジェクトへの参照を保持する場合は、その参照が無駄になること (にメソッドを呼び出す例外がスローされます)。

渡されたオブジェクトが作成されなかった場合、またはオブジェクトの未処理のマネージ表現が既に存在する場合は、強制 dispose は行われません。 


## <a name="property-attributes"></a>プロパティの属性

<a name="NotImplementedAttribute" />

### <a name="notimplementedattribute"></a>NotImplementedAttribute

この属性は、get アクセス操作子を持つプロパティは、基底クラスで導入された、変更可能なサブクラスには、set アクセス操作子が導入されています、Objective C の表現形式をサポートするために使用されます。

C# の場合は、このモデルをサポートしていないため、基底クラスは、set アクセス操作子と、getter の両方がある必要がありますサブクラスが使用できる、 [OverrideAttribute](#OverrideAttribute)です。

この属性はプロパティの setter でのみ使用し、目標 C の変更可能な表現形式をサポートするために使用されます。

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

マッピング`NSString`列挙値に定数が簡単に向上 .NET API を作成します。 これには:

* 表示することによって、便利であるコード補完機能は、**のみ**API; に適切な値
* タイプ セーフの追加別に使用することはできません`NSString`正しくないコンテキストにある定数と
* コード補完機能を失うことがなく短い API の一覧を表示するため、一部の定数を非表示にできます。

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

コード ジェネレーターを作成する上記のバインディングの定義から、`enum`自体も作成し、`*Extensions`列挙型値間の 2 つの方法で変換メソッドが含まれる静的な型と`NSString`定数。 つまり、定数が利用する開発者 API の一部でない場合でもです。

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

装飾できます**1**この属性を持つ列挙型値。 これには、列挙型の値が不明でかどうかに返される定数になります。

上記の例では。

```csharp
var x = (NSRunLoopMode) 99;
Call (x.GetConstant ()); // NSDefaultRunLoopMode will be used
```

列挙値が修飾されていない場合、`NotSupportedException`がスローされます。

### <a name="errordomainattribute"></a>ErrorDomainAttribute

エラー コードは、列挙値としてバインドされます。 一般にそれらのドメインのエラーがあるし、常に検索のうち、どれが適用されます (またはも存在するかどうか) に簡単ではありません。

この属性を使用して、エラー ドメイン、その enum 自体を関連付けることができます。

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

拡張メソッドを呼び出すことができますし、`GetDomain`ドメインのすべてのエラーの定数を取得します。

### <a name="fieldattribute"></a>FieldAttribute

これは、同じ`[Field]`定数型の中に使用する属性。 これは、こともできます列挙型の内部と特定の定数値をマップします。

A`null`値できますを指定する場合に返される必要があります列挙型値を`null``NSString`定数を指定します。

上記の例では。

```csharp
var constant = NSRunLoopMode.NewInWatchOS3; // will be null in watchOS 2.x
Call (NSRunLoopModeExtensions.GetValue (constant)); // will return 1000
```

ない場合は`null`値が存在し、`ArgumentNullException`がスローされます。

## <a name="global-attributes"></a>グローバル属性

グローバル属性を使用して適用するか、`[assembly:]`属性の修飾子と同様に、 [ `[LinkWithAttribute]` ](#LinkWithAttribute)することもできますと同様に、どこでも使用、 [ `[Lion]` ](#SinceAndLionAttributes)と[ `[Since]`](#SinceAndLionAttributes)属性。

<a name="LinkWithAttribute" />

### <a name="linkwithattribute"></a>LinkWithAttribute

これは、開発者が、gcc_flags を手動で構成するライブラリのコンシューマーを強制することがなく、バインドされたライブラリを再利用するために必要なリンクのフラグとをライブラリに渡された mtouch 余分な引数を指定するアセンブリ レベル属性。

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

この属性がアセンブリ レベルで適用される、たとえば、これは、どのような[CorePlot バインド](https://github.com/mono/monotouch-bindings/tree/master/CorePlot)を使用します。

```csharp
[assembly: LinkWith ("libCorePlot-CocoaTouch.a", LinkTarget.ArmV7 | LinkTarget.ArmV7s | LinkTarget.Simulator, Frameworks = "CoreGraphics QuartzCore", ForceLoad = true)]
```

使用すると、`[LinkWith]`属性が指定された`libraryName`を正しく使用するために必要なコマンド ライン フラグだけでなく、アンマネージの依存関係を含む 1 つの DLL を出荷するように、生成されたアセンブリに埋め込まれ、Xamarin.iOS からライブラリです。

提供することも、 `libraryName`、その場合、`LinkWith`属性は、その他のリンカー フラグを指定だけを使用することができます。

``` csharp
[assembly: LinkWith (LinkerFlags = "-lsqlite3")]
```

#### <a name="linkwithattribute-constructors"></a>LinkWithAttribute コンス トラクター

これらのコンス トラクターを使用するとリンクし、結果として得られるアセンブリ、サポートされているターゲット ライブラリをサポートして、ライブラリとリンクするために必要な任意の省略可能なライブラリ フラグに埋め込むライブラリを指定できます。

なお、`LinkTarget`引数は、Xamarin.iOS によって推論され、設定する必要はありません。

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

`ForceLoad`を決定するプロパティを使用するかどうか、`-force_load`リンク フラグは、ネイティブ ライブラリをリンクするために使用します。 ここでは、これは常に true です。

#### <a name="linkwithattributeframeworks"></a>LinkWithAttribute.Frameworks

バインドされているライブラリがいずれのフレームワークでもハードの要件を持つかどうか (以外の`Foundation`と`UIKit`) を設定する必要があります、`Frameworks`プロパティを必要なプラットフォーム フレームワークのスペースで区切られた一覧を含む文字列にします。 たとえば、ライブラリにバインドするかどうかを必要とする`CoreGraphics`と`CoreText`、設定すると、`Frameworks`プロパティを`"CoreGraphics CoreText"`です。

#### <a name="linkwithattributeiscxx"></a>LinkWithAttribute.IsCxx

結果の実行可能ファイルを C コンパイラは、既定値ではなく、C++ コンパイラを使用してコンパイルする必要がある場合は true には、このプロパティを設定します。 バインドする、ライブラリは、C++ で記述された場合は、これを使用します。

#### <a name="linkwithattributelibraryname"></a>LinkWithAttribute.LibraryName

バンドルにアンマネージ ライブラリの名前。 これは".a"の拡張子を持つファイルであり、複数のプラットフォーム (例、ARM および x86、シミュレーターの場合) 用のオブジェクト コードを含めることができます。

Xamarin.iOS の以前のバージョンのチェック、`LinkTarget`プラットフォームをサポートするには、ライブラリを決定するプロパティが、これが、自動的に検出され、`LinkTarget`プロパティは無視されます。

#### <a name="linkwithattributelinkerflags"></a>LinkWithAttribute.LinkerFlags

`LinkerFlags`文字列は、アプリケーションにネイティブ ライブラリをリンクするときに、その他のリンカー フラグのいずれかが必要なを指定するバインディングの作成者の方法を提供します。

たとえば、libxml2 と zlib ネイティブ ライブラリを必要とする場合は設定する、`LinkerFlags`文字列`"-lxml2 -lz"`です。

#### <a name="linkwithattributelinktarget"></a>LinkWithAttribute.LinkTarget

Xamarin.iOS の以前のバージョンのチェック、`LinkTarget`プラットフォームをサポートするには、ライブラリを決定するプロパティが、これが、自動的に検出され、`LinkTarget`プロパティは無視されます。

#### <a name="linkwithattributeneedsgccexceptionhandling"></a>LinkWithAttribute.NeedsGccExceptionHandling

このプロパティは、リンク、ライブラリには、GCC 例外処理 (gcc_eh) ライブラリが必要な場合は、true に設定します。

#### <a name="linkwithattributesmartlink"></a>LinkWithAttribute.SmartLink

`SmartLink`プロパティは、Xamarin.iOS を決定できるようにする場合は true に設定する必要があるかどうか`ForceLoad`が必要か。

#### <a name="linkwithattributeweakframeworks"></a>LinkWithAttribute.WeakFrameworks

`WeakFrameworks`プロパティと同じように機能、`Frameworks`プロパティ、リンク時に、点を除いて、`-weak_framework`指定子が表示されているフレームワークの各 gcc に渡されます。

`WeakFrameworks` これによりライブラリおよびアプリケーション プラットフォーム フレームワークに対して弱いリンクをできます必要に応じてそれらを活用する、使用可能なのに受け取らない上余分な機能を追加するものでは、ライブラリの場合に便利であるハードコーディングによる依存関係に新しいようにiOS のバージョン。 脆弱なリンクの詳細については、Apple のドキュメントを参照して[弱いリンク](http://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WeakLinking.html)です。

脆弱なリンクの適切な候補になる`Frameworks`などのアカウント、 `CoreBluetooth`、 `CoreImage`、 `GLKit`、`NewsstandKit`と`Twitter`だけでは使用 iOS 5 以降。

<a name="SinceAndLionAttributes" />

### <a name="sinceattribute-ios-and-lionattribute-macos"></a>SinceAttribute (iOS) および LionAttribute (macOS)

使用する、`[Since]`時刻に特定の時点で導入されたを持つものとして属性フラグ Api にします。 属性は、フラグの型および基になるクラス、メソッドまたはプロパティが使用できない場合、実行時に問題が原因となるメソッドにのみ使用する必要があります。

構文:

```csharp
public SinceAttribute : Attribute {
     public SinceAttribute (byte major, byte minor);
     public byte Major, Minor;
}
```

これは、必要があります一般にするため適用されません列挙型、制約、または構造体の新しいものは発生しません、ランタイム エラー、オペレーティング システムの以前のバージョンでのデバイスで実行された場合。

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

新しいメンバーに適用される場合の例:

```csharp
[BaseType (typeof (UIViewController))]
public interface UITableViewController {
    [Export ("tableView", ArgumentSemantic.Retain)]
    UITableView TableView { get; set; }

    [Since (3,2)]
    [Export ("clearsSelectionOnViewWillAppear")]
    bool ClearsSelectionOnViewWillAppear { get; set; }
```

`[Lion]` Lion で導入された型のですが、同じ方法で属性を適用します。 使用する理由`[Lion]`iOS で使用されている特定のバージョン番号とは、その iOS が非常に多くの場合は、OS X のメジャー リリースはほとんど発生であり、そのバージョン番号でよりもそのコードネームによって、オペレーティング システムを覚えやすく中に改訂

### <a name="adviceattribute"></a>AdviceAttribute

この属性を使用して、使用するようの方が便利になる可能性があるその他の Api についてのヒントを開発者に提供します。   など、厳密に型指定されたバージョンの API を提供する場合、厳密に型指定された属性にこの属性を使用でしたより優れた API を開発者に知らせます。

ドキュメントのこの属性からの情報が表示され、向上する方法に関するユーザーの推奨事項を提供するツールを開発できます。

### <a name="zerocopystringsattribute"></a>ZeroCopyStringsAttribute

のみ Xamarin.iOS 5.4 で使用できると新しいです。

この属性は、ジェネレーターを指示するこの特定のライブラリのバインド (で適用されている場合`[assembly:]`) 型が高速 0 コピー文字列のマーシャ リングを使用する必要があります。 この属性は、コマンド ライン オプションを渡すことに相当`--zero-copy`ジェネレーターにします。

ジェネレーターが、新しいの作成を生じることがなく Objective C を使用する文字列として、C# の場合、同じ文字列を効果的に使用する文字列の 0 コピーを使用して、`NSString`オブジェクトと c# の文字列から Objective C の文字列に、データのコピーを回避します。 0 コピー文字列を使用する唯一の欠点があることを確認するをラップする文字列のプロパティとしてマークする発生した`retain`または`copy`が、`[DisableZeroCopy]`属性に設定します。 これには、0 コピー文字列のハンドルがスタックに割り当てられているし、関数の戻り値に有効ではありませんのでが必要です。

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

アセンブリ レベルで属性を適用することもでき、アセンブリのすべての種類に適用されます。

```csharp
[assembly:ZeroCopyStrings]
```

## <a name="strongly-typed-dictionaries"></a>厳密に型指定された辞書

Xamarin.iOS 8.0 でラップする厳密に型指定されたクラスを簡単に作成できるようになりました`NSDictionaries`です。

使用することが常にきましたが、 [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/)データ型の手動の API と共にこれでこれを行うはるかに簡単です。  詳細については、次を参照してください。[厳密な型の提示](~/cross-platform/macios/binding/objective-c-libraries.md#Surfacing_Strong_Types)です。

<a name="StrongDictionary" />

### <a name="strongdictionary"></a>StrongDictionary

ジェネレーターがから派生するインターフェイスと同じ名前のクラスを生成するインターフェイスにこの属性を適用[DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/)厳密に型指定に、インターフェイスで定義されている各プロパティをオンにし、get アクセス操作子と set アクセス操作子、辞書のです。

既存のインスタンス化できるクラスを自動生成この`NSDictionary`または新規作成されています。

この属性は、1 つのパラメーター ディクショナリの要素にアクセスするためのキーを含むクラスの名前です。   既定では、属性を持つインターフェイス内の各プロパティは「キー」サフィックスが付いた名前の指定した型のメンバーを検索します。

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

上記の場合、`MyOption`クラスの文字列プロパティが生成されます`Name`を使用する、`MyOptionKeys.NameKey`文字列を取得する辞書にキーとして。   使用して、`MyOptionKeys.AgeKey`を取得する辞書にキーとして、 `NSNumber` int が含まれています

異なるキーを使用する場合は、たとえば、プロパティのエクスポート属性を使用できます。

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

次のデータ型はサポートされて、`StrongDictionary`定義。

|インターフェイスの型 (C#)|`NSDictionary` 記憶域の種類|
|---|---|
|`bool`|`Boolean` 格納されている、 `NSNumber`|
|列挙値|格納されている整数、 `NSNumber`|
|`int`|32 ビット整数に格納されている、 `NSNumber`|
|`uint`|32 ビット符号なし整数に格納されている、 `NSNumber`|
|`nint`|`NSInteger` 格納されている、 `NSNumber`|
|`nuint`|`NSUInteger` 格納されている、 `NSNumber`|
|`long`|64 ビット整数に格納されている、 `NSNumber`|
|`float`|32 ビット整数として格納されている、 `NSNumber`|
|`double`|64 ビット整数として格納されている、 `NSNumber`|
|`NSObject` サブクラスと|`NSObject`|
|`NSDictionary`|`NSDictionary`|
|`string`|`NSString`|
|`NSString`|`NSString`|
|C#`Array`の `NSObject`|`NSArray`|
|C#`Array`列挙型の|`NSArray` 含む`NSNumber`値|
