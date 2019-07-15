---
title: OBJECTIVE-C ライブラリのバインド
description: このドキュメントを作成する方法の概要を提供するC#イベント、メソッド、カスタム コントロール、およびバインドする方法を説明する、OBJECTIVE-C コードへのバインド。
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
author: conceptdev
ms.author: crdun
ms.date: 03/06/2018
ms.openlocfilehash: 206379b162c7778663ee2baf64dfeb1d33666ab4
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67831465"
---
# <a name="binding-objective-c-libraries"></a>OBJECTIVE-C ライブラリのバインド

Xamarin.iOS または Xamarin.Mac を使用する場合は、サード パーティ製の Objective C ライブラリを使用する必要がある場合があります。 これらの状況で作成する Xamarin バインド プロジェクトを使用することができます、C#ネイティブ OBJECTIVE-C ライブラリにバインドします。 IOS と Mac の Api を使用するのと同じツールを使用するプロジェクトC#します。

このドキュメントは、Objective C Api をバインドする方法を説明します。 C Api だけをバインドする場合は、これは、標準の .NET メカニズムを使用する必要があります[P/invoke framework](https://www.mono-project.com/docs/advanced/pinvoke/)します。
C ライブラリに静的にリンクする方法の詳細についてで使用できる、[ネイティブ ライブラリのリンク](~/ios/platform/native-interop.md)ページ。

当社ガイド 』 を参照してください。[バインド型のリファレンス ガイド](~/cross-platform/macios/binding/binding-types-reference.md)します。
さらに、内部的にはどうなるかについて詳細を表示する場合は、確認、[バインディングの概要](~/cross-platform/macios/binding/overview.md)ページ。

IOS と Mac ライブラリの両方のバインドを構築できます。
このページには、バインド、Mac のバインドはよく似ていますが、iOS で操作する方法について説明します。

**IOS 用のサンプル コード**

使用することができます、 [iOS バインディング サンプル](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample)バインドみたりする場合のプロジェクト。

<a name="Getting_Started" />

## <a name="getting-started"></a>作業の開始

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

バインディングを作成する最も簡単な方法では、Xamarin.iOS のバインド プロジェクトを作成します。
これを行うから Visual Studio for Mac プロジェクトの種類を選択して**iOS > ライブラリ > バインド ライブラリ**:

[![](objective-c-libraries-images/00-sml.png "これは Visual Studio for Mac を iOS バインド ライブラリ プロジェクトの種類を選択")](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

バインディングを作成する最も簡単な方法では、Xamarin.iOS のバインド プロジェクトを作成します。
これを行う Windows 上の Visual Studio から、プロジェクトの種類を選択して**Visual C# > iOS > バインド ライブラリ (iOS)** :

[![](objective-c-libraries-images/00vs-sml.png "iOS バインド ライブラリ iOS")](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> メモ:バインドのプロジェクト**Xamarin.Mac** for mac Visual Studio でのみサポートされます。

-----

生成されたプロジェクトには、編集可能な小規模のテンプレートが含まれている、2 つのファイルが含まれています:`ApiDefinition.cs`と`StructsAndEnums.cs`します。

`ApiDefinition.cs` 、API コントラクトを定義するにはこれは、ファイルに、基になる Objective C API を射影する方法を説明するC#します。 構文とこのファイルの内容は、このドキュメントの説明の主なトピックとその内容に制限されますC#インターフェイスとC#デリゲートの宣言。 `StructsAndEnums.cs`ファイルはファイルのインターフェイスとデリゲートで必要なすべての定義を入力する場所です。 これには、列挙値と、コードを使用する構造体が含まれます。

<a name="Binding_an_API" />

## <a name="binding-an-api"></a>API のバインド

包括的なバインドには、する Objective C API の定義を理解し、.NET Framework デザイン ガイドラインについて理解します。

API 定義ファイルを使用した通常開始は、ライブラリをバインドします。 API の定義ファイルは単なる、C#を含むソース ファイルC#、いくつかのバインディングを体験していただける属性付けするインターフェイス。  このファイルは、定義の間で、どのようなコントラクトC#OBJECTIVE-C と。

たとえば、ライブラリの単純な api ファイルです。

```csharp
using Foundation;

namespace Cocos2D {
  [BaseType (typeof (NSObject))]
  interface Camera {
    [Static, Export ("getZEye")]
    nfloat ZEye { get; }

    [Export ("restore")]
    void Restore ();

    [Export ("locate")]
    void Locate ();

    [Export ("setEyeX:eyeY:eyeZ:")]
    void SetEyeXYZ (nfloat x, nfloat y, nfloat z);

    [Export ("setMode:")]
    void SetMode (CameraMode mode);
  }
}
```

上記のサンプルと呼ばれるクラスを定義する`Cocos2D.Camera`から派生した、`NSObject`基本データ型 (この型に由来`Foundation.NSObject`) 静的プロパティを定義して (`ZEye`)、引数なしとメソッドを取得する 2 つのメソッドは、3 つ引数。

については、API のファイルと使用できる属性の形式の詳細については、 [API 定義ファイル](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file)以下のセクション。

完全なバインドを作成するには、通常 4 つのコンポーネントに対応するか。

-  API 定義ファイル (`ApiDefinition.cs`します)。
-  省略可能: 任意の列挙型、型、API 定義ファイルで必要な構造体 (`StructsAndEnums.cs`します)。
-  省略可能: 可能性があります、生成されたバインディングを展開したり、詳細を提供する余分なソースC#フレンドリ API (任意C#をプロジェクトに追加するファイル)。
-  ネイティブ ライブラリは、バインドです。

このグラフには、ファイル間のリレーションシップが表示されます。

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png "このグラフは、ファイル間の関係を示しています")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

API 定義ファイル (任意のメンバーがインターフェイスに含めることができます)、名前空間とインターフェイスの定義の内容はクラス、列挙、デリゲート、または構造体を含めることはできません。 API 定義ファイルは、API の生成に使用されるコントラクトだけです。

余分なコードなどの列挙体が必要か、サポート クラスを"CameraMode"上の例で、個別のファイルをホストすることは、CS ファイルに存在しないため、別のファイルにホストする必要がある列挙値`StructsAndEnums.cs`:

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

`APIDefinition.cs`ファイルを組み合わせて、`StructsAndEnum`クラスを使用して、ライブラリのコア バインディングが生成されます。 結果としてライブラリを使用することができます-チューニング結果のライブラリを追加することが通常はC#、ユーザーのための機能。 いくつかの例では、実装して、`ToString()`メソッドを提供C#といくつかのネイティブ型の間の暗黙的な変換を追加、インデクサー、またはいくつかのメソッドの厳密に型指定されたバージョンを提供します。 これらの機能強化が追加で格納されているC#ファイル。 追加するだけです、C#をプロジェクトにファイルと、このビルド プロセスが含まれます。

これでコードを実装する方法を示しています、`Extra.cs`ファイル。 使用する部分クラスとして、これらの組み合わせから生成される部分クラスを拡張に注意してください、 `ApiDefinition.cs` 、`StructsAndEnums.cs`バインドをコアします。

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

ライブラリのビルドと、ネイティブのバインドが生成されます。

このバインドを完了するには、プロジェクトにネイティブ ライブラリを追加する必要があります。  ドラッグして、ソリューション エクスプ ローラーでプロジェクトに、Finder からネイティブ ライブラリをドロップするか、プロジェクトにプロジェクトを右クリックして、ネイティブ ライブラリを追加することでこれを行う**追加** > **ファイルの追加**ネイティブ ライブラリを選択します。
規約によってネイティブ ライブラリは、"lib"という単語で起動し、拡張子".a"で終わます。 Visual Studio for Mac が 2 つのファイルを追加した場合: .a ファイルと、自動的に設定されたC#ネイティブ ライブラリに含まれる内容に関する情報を含むファイル。

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png "ネイティブ ライブラリの規則により word ライブラリで開始し、拡張子 '.a' で終わる")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

内容、`libMagicChord.linkwith.cs`ファイルはこのライブラリの使用方法については、結果の DLL ファイルにこのバイナリをパッケージ化、IDE に指示します。

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

完全な詳細を使用する方法については、 [`[LinkWith]`](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) 
属性が記載されて、[バインド型のリファレンス ガイド](~/cross-platform/macios/binding/binding-types-reference.md)します。

これで、最終的には、プロジェクトをビルドするときに、`MagicChords.dll`バインドおよびネイティブ ライブラリの両方を含むファイル。 このプロジェクトを配布するまたは他の開発者が独自の結果の DLL を使用します。

場合によって、いくつかの列挙値、デリゲートの定義またはその他の必要があります。 これは単なるコントラクトを API 定義ファイルの配置しません。

<a name="The_API_definition_file" />

## <a name="the-api-definition-file"></a>API 定義ファイル

API 定義ファイルは、さまざまなインターフェイスで構成されます。 API 定義のインターフェイスはクラス宣言に有効になりますで修飾する必要がありますが、 [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute)属性をクラスの基本クラスを指定します。

かもしれません。 なぜ使用しないクラス インターフェイスではなく、コントラクトの定義についてはします。 API 定義ファイルで、メソッド本体を指定することや、意味のある値を返したり、例外をスローする必要があった本文を指定することがなく、メソッドのコントラクトを作成することが許可されているために、インターフェイスを選択しました。

スケルトンとしてクラスを生成するインターフェイスを使用しますので、バインドをドライブに属性を持つコントラクトのさまざまな部分を装飾する手段がありますが。

<a name="Binding_Methods" />

### <a name="binding-methods"></a>バインド メソッド

行うことができます最も簡単なバインディングでは、メソッドをバインドします。 インターフェイスのメソッドを宣言するだけ、C#名前付け規則とを持つメソッドを装飾します [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)
属性。 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)属性は、リンク、 C# Xamarin.iOS ランタイムの Objective C の名前の名前。 パラメーター、 [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 
属性は、OBJECTIVE-C セレクターの名前です。 以下に、いくつかの例を示します。

```csharp
// A method, that takes no arguments
[Export ("refresh")]
void Refresh ();

// A method that takes two arguments and return the result
[Export ("add:and:")]
nint Add (nint a, nint b);

// A method that takes a string
[Export ("draw:atColumn:andRow:")]
void Draw (string text, nint column, nint row);
```

上記のサンプルでは、インスタンス メソッドにバインドする方法を示します。 静的メソッドをバインドするに使用する必要があります、 [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute)属性には、次のようにします。

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

これが必要なコントラクトがインターフェイスの一部されインターフェイスには、静的およびインスタンスの宣言の概念がないため、もう一度、属性を使用する必要があります。 持つメソッドを修飾することができます、バインドから特定のメソッドを非表示にする場合、 [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)属性。

`btouch-native`コマンドの null でない参照パラメーターのチェックが導入されます。 特定のパラメーターの null 値を許可する場合を使用して、 [`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)
次のように、パラメーターに属性:

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

参照型をエクスポートするときに、 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)キーワードの割り当てのセマンティクスを指定することもできます。 これは、データがリークしないことを確認する必要があります。

<a name="Binding_Properties" />

### <a name="binding-properties"></a>バインドのプロパティ

使用して OBJECTIVE-C プロパティをバインドのメソッドと同様、 [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)
属性し、に直接マップC#プロパティ。 プロパティは、メソッドと同様で修飾することができます、 [`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute)
および [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
属性。

使用すると、 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)カバー btouch ネイティブの下で、プロパティの属性は、2 つのメソッドを実際にはバインド: getter と setter します。 エクスポートには、指定した名前、 **basename** 「セット」の最初の文字にすると、単語を付けることによって、setter を計算し、 **basename**大文字に変換したうえでかかるセレクターに、引数。 つまり、`[Export ("label")]`に適用されるプロパティは、実際には"label"をバインドし、"setLabel:"OBJECTIVE-C メソッド。

Objective C プロパティは、上記で説明したパターンに従っていないと、名前は手動で上書きされる場合があります。 そのような場合は、バインディングを使用して生成する方法を制御できます、 [`[Bind]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAttribute) 
getter または setter の例では、属性:

```csharp
[Export ("menuVisible")]
bool MenuVisible { [Bind ("isMenuVisible")] get; set; }
```

このバインド"isMenuVisible"と"setMenuVisible:"です。 必要に応じて、次の構文を使用して、プロパティをバインドすることができます。

```csharp
[Category, BaseType(typeof(UIView))]
interface UIView_MyIn
{
  [Export ("name")]
  string Name();

  [Export("setName:")]
  void SetName(string name);
}
```

Getter と setter に明示的が定義されているように、`name`と`setName`上記のバインド。

使用して静的プロパティのサポートだけでなく[ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute)、スレッドの静的プロパティを修飾する[ `[IsThreadStatic]` ](~/cross-platform/macios/binding/binding-types-reference.md#IsThreadStaticAttribute)、たとえば。

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

メソッドでフラグを設定するいくつかのパラメーターを使用するのと同じよう[ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)、適用することができます [`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)
示すプロパティを null、たとえば、プロパティの有効な値には。

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

[ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)パラメーターは、setter で直接指定こともできます。

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>カスタム コントロールのバインドの注意事項

カスタム コントロールのバインドを設定する場合は、次の注意事項を検討してください。

1. **バインドのプロパティは、静的にする必要があります**- プロパティのバインドを定義する場合、 [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute)属性を使用する必要があります。
2. **プロパティ名が正確に一致する必要があります**-プロパティをバインドするための名前は、カスタム コントロールのプロパティの名前を正確に一致する必要があります。
3. **プロパティの型が正確に一致する必要があります**-プロパティをバインドするために使用する変数の型では、カスタム コントロールのプロパティの型を正確に一致する必要があります。
4. **ブレークポイントと getter と setter** - ブレークポイントは、get アクセス操作子に配置またはプロパティの setter メソッドに到達しません。
5. **コールバックを観察**-カスタム コントロールのプロパティの値の変更の通知を受け取る監視のコールバックを使用する必要があります。

上記の一覧表示されている警告のいずれかを観察する発生する可能性がサイレント モードで実行時に失敗しているバインディング。

<a name="MutablePattern" />

#### <a name="objective-c-mutable-pattern-and-properties"></a>Objective C の変更可能なパターンとプロパティ

Objective C フレームワークは、表現する形式を使用して、いくつかのクラスは変更可能なサブクラスに変更できません。 たとえば`NSString`変更不可のバージョンは、中に`NSMutableString`は変化できるサブクラスです。

それらのクラスでするが一般的で、get アクセス操作子がない setter プロパティが含まれる変更できない基本クラスを参照してください。 Setter を導入する変更可能なバージョンのとします。 これは本当に伴うではないためC#、表現と連携するようにこの手法をマップする必要があるC#します。

これにマップされている方法C#は、基本クラスでは、getter と setter の両方を追加すると set アクセス操作子にフラグを設定することです、 [`[NotImplemented]`](~/cross-platform/macios/binding/binding-types-reference.md#NotImplementedAttribute)
属性。

使用する、変更可能なサブクラスにし、 [`[Override]`](~/cross-platform/macios/binding/binding-types-reference.md#OverrideAttribute) 
プロパティが実際には、親の動作を上書きすることを確認するプロパティの属性です。

例:

```csharp
[BaseType (typeof (NSObject))]
interface MyTree {
    string Name { get; [NotImplemented] set; }
}

[BaseType (typeof (MyTree))]
interface MyMutableTree {
    [Override]
    string Name { get; set; }
}
```

<a name="Binding_Constructors" />

### <a name="binding-constructors"></a>バインディングのコンス トラクター

`btouch-native`ツールは、特定のクラス、クラスで 4 つのコンス トラクターを自動的に生成します。 `Foo`、が生成されます。

-  `Foo ()`: 既定のコンス トラクター (OBJECTIVE-C の"init"コンス トラクターにマップされます)
-  `Foo (NSCoder)`: NIB ファイルの逆シリアル化中に使用するコンス トラクター (objective-c のマップ"initWithCoder:"コンス トラクター)。
-  `Foo (IntPtr handle)`: コンス トラクターのハンドル ベースの作成では、このランタイムによってときに呼び出される、ランタイムは、アンマネージ オブジェクトからマネージ オブジェクトを公開する必要があります。
-  `Foo (NSEmptyFlag)`: これは、二重の初期化を防ぐために、派生クラスによって使用されます。

定義するコンス トラクターは、インターフェイス定義内で、次のシグネチャを使用して宣言する必要があります。: 返す必要がある、`IntPtr`値とメソッドの名前は、コンス トラクターをする必要があります。 バインドする例については、`initWithFrame:`コンス トラクターを使用するようになります。

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

<a name="Binding_Protocols" />

### <a name="binding-protocols"></a>バインディング プロトコル

API デザイン ドキュメントのセクションで」の説明に従って[モデルとプロトコルについて説明する](~/ios/internals/api-design/index.md#Models)Xamarin.iOS では、OBJECTIVE-C プロトコルをマップでのフラグが設定されたクラスに、 [`[Model]`](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute)
属性。 これは、OBJECTIVE-C でデリゲート クラスを実装する場合に通常使用されます。

通常のバインドされたクラスとデリゲート クラスの大きな違いは、デリゲート クラスは 1 つまたは複数のオプションのメソッドである可能性があります。

たとえば、`UIKit`クラス`UIAccelerometerDelegate`Xamarin.iOS でのバインド方法になります。

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

これは省略可能なメソッドの定義であるため`UIAccelerometerDelegate`他を行うにはありません。 追加する必要があります、プロトコルに必要なメソッドがあった場合は、 [`[Abstract]`](~/cross-platform/macios/binding/binding-types-reference.md#AbstractAttribute)
属性をメソッドにします。 これで、実装のユーザーが実際には、メソッドの本体を指定します。

一般に、プロトコルは、メッセージに応答するクラスで使用されます。 これは、通常 Objective C では、プロトコルのメソッドに応答するオブジェクトのインスタンスに"delegate"プロパティに割り当てることで。

Xamarin.iOS の規則はその両方、OBJECTIVE-C で疎結合スタイルをサポートするためのインスタンス、`NSObject`割り当てることができますも公開して、デリゲートにその厳密に型指定されたバージョン。 このため、通常提供両方、`Delegate`厳密に型指定されたプロパティと`WeakDelegate`緩く型指定されています。 弱い型指定のあるバージョンを通常、バインド[ `[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)を使用しています、 [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute)属性が厳密に型指定されたバージョンを提供します。

バインドされた方法を示します、`UIAccelerometer`クラス。

```csharp
[BaseType (typeof (NSObject))]
interface UIAccelerometer {
        [Static] [Export ("sharedAccelerometer")]
        UIAccelerometer SharedAccelerometer { get; }

        [Export ("updateInterval")]
        double UpdateInterval { get; set; }

        [Wrap ("WeakDelegate")]
        UIAccelerometerDelegate Delegate { get; set; }

        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }
}
```

<a name="iOS7ProtocolSupport" />

**MonoTouch 7.0 の新機能**

MonoTouch 7.0 以降、新規および改良されたプロトコルのバインド機能が組み込まれています。  この新しいサポートにより、簡単に特定のクラスで 1 つまたは複数のプロトコルを採用することの Objective C の表現方法を使用します。

すべてのプロトコルの定義の`MyProtocol`objective-c であるようになりました、`IMyProtocol`省略可能なすべてのメソッドを提供する拡張機能クラスと同様に、プロトコルからのすべての必要なメソッドを一覧表示するインターフェイス。  エディターでは、前の抽象モデル クラスの別のサブクラスを使用することがなくプロトコル メソッドを実装するために開発者を使用できます。 Xamarin Studio で新しいサポートと上記組み合わせます。

任意の定義を含む、 [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute)属性プロトコルを使用する方法を大幅に向上する 3 つのサポート クラスが生成されます。

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

**クラスの実装**の個々 のメソッドをオーバーライドし、完全な型の安全性を取得できる完全な抽象クラスを提供します。  原因が、C#別の基本クラスを用意する必要があるシナリオのことができますが、どこであるインターフェイスを実装する複数の継承をサポートしていないが、

生成された**インターフェイス定義**が用意されています。  これは、プロトコルからのすべての必要なメソッドを持つインターフェイスです。  これにより、開発者が単なるインターフェイスを実装するプロトコルを実装します。  ランタイムは、プロトコルの導入として型を自動的に登録されます。

インターフェイスがのみ、必要なメソッドを一覧表示し、省略可能なメソッドは公開ことに注意してください。  つまり、プロトコルを採用するクラスは、確認、必要なメソッドでは、完全な型を取得が (使用して手動で弱い型指定を使用する必要があります。 [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 
属性およびシグネチャの一致) の省略可能なプロトコル メソッド。

プロトコルを使用する API を使用すると便利にバインディング ツールも生成されます拡張メソッドのクラスのすべての省略可能なメソッドを公開します。  これは、ことを意味する API を使用している限りはしてプロトコルをすべてのメソッドを持つものとして扱うこと。

API でプロトコルの定義を使用する場合は、API の定義で空のインターフェイスはスケルトンを記述する必要があります。  API で、MyProtocol を使用する場合は、これを行う必要があります。

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

#### <a name="adopting-protocol-generated-interfaces"></a>プロトコルによって生成されたインターフェイスを導入します。

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

<a name="Binding_Class_Extensions" />

### <a name="binding-class-extensions"></a>バインディング クラスの拡張機能

Objective c を新しいメソッドに似たクラスを拡張することはC#の拡張メソッド。 これらのメソッドのいずれかが存在するときに使用できます、 [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 
OBJECTIVE-C でメッセージの受信者としてメソッドにフラグを設定する属性。

たとえば、Xamarin.iOS でバインドされたで定義されている拡張メソッド`NSString`とき`UIKit`メソッドとしてインポートされます、 `NSStringDrawingExtensions`、次のように。

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

<a name="Binding_Objective-C_Argument_Lists" />

### <a name="binding-objective-c-argument-lists"></a>引数リストを OBJECTIVE-C のバインド

Objective C では、可変個引数をサポートします。 例えば:

```objc
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

このメソッドからC#このような署名を作成します。

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

これは、ユーザーからの上記の API を非表示がライブラリに公開することとして、内部メソッドを宣言します。 このようなメソッドを記述できます。

```csharp
public void AppendWorkers(params Worker[] workers)
{
    if (workers == null)
         throw new ArgumentNullException ("workers");

    var pNativeArr = Marshal.AllocHGlobal(workers.Length * IntPtr.Size);
    for (int i = 1; i < workers.Length; ++i)
        Marshal.WriteIntPtr (pNativeArr, (i - 1) * IntPtr.Size, workers[i].Handle);

    // Null termination
    Marshal.WriteIntPtr (pNativeArr, (workers.Length - 1) * IntPtr.Size, IntPtr.Zero);

    // the signature for this method has gone from (IntPtr, IntPtr) to (Worker, IntPtr)
    WorkerManager.AppendWorkers(workers[0], pNativeArr);
    Marshal.FreeHGlobal(pNativeArr);
}
```

<a name="Binding_Fields" />

### <a name="binding-fields"></a>フィールドをバインド

ライブラリの中で宣言されたパブリック フィールドにアクセスすることがあります。

通常これらのフィールドには、参照する必要がある文字列または整数の値が含まれます。 ディクショナリ内のキーと、特定の通知を表す文字列としてよく使用されます。

フィールドをバインドするには、インターフェイス定義ファイルにプロパティを追加しを持つプロパティを装飾、 [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute)属性。 この属性は 1 つのパラメーターを受け取ります。 参照するシンボルの C の名前。 例えば:

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

静的クラスから派生していないのさまざまなフィールドをラップする場合`NSObject`、使用することができます、 [`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute_Class) 
次のように、クラスの属性:

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

上記が生成されます、`LonelyClass`から派生していませんが`NSObject`へのバインドが含まれますと、 `NSSomeEventNotification` 
 `NSString`として公開されている、`NSString`します。

[ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute)属性は、次のデータ型に適用できます。

-  `NSString` 参照 (読み取り専用プロパティのみ)
-  `NSArray` 参照 (読み取り専用プロパティのみ)
-  32 ビットの整数 (`System.Int32`)
-  64 ビットの整数 (`System.Int64`)
-  32 ビットの浮動小数点数 (`System.Single`)
-  64 ビットの浮動小数点数 (`System.Double`)
-  `System.Drawing.SizeF`
-  `CGSize`

だけでなく、ネイティブのフィールド名のライブラリ名を渡すことによって、フィールドの配置場所のライブラリ名を指定できます。

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

静的にリンクがない場合、バインドするライブラリを使用する必要があるため、`__Internal`名。

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums" />

### <a name="binding-enums"></a>バインディングの列挙型

追加することができます`enum`バインドに直接ファイルを簡単になります (つまり、バインドと、最終的なプロジェクトの両方でコンパイルする必要があります) 別のソース ファイルを使用してなしで API 定義の内部で使用します。

例:

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

置換する独自の列挙型を作成することも`NSString`定数。 コード ジェネレーターがここでは**自動的に**の列挙型の値と NSString 定数を変換するメソッドを作成します。

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

interface MyType {
    [Export ("performForMode:")]
    void Perform (NSString mode);

    [Wrap ("Perform (mode.GetConstant ())")]
    void Perform (NSRunLoopMode mode);
}
```

上記の例を装飾することでした`void Perform (NSString mode);`で、 [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)属性。 これが**を非表示に**定数ベースの API からコンシューマーをバインドします。

ただしこれは制限としてより良い API 代替では、型をサブクラス化、 [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute)属性。 生成されたメソッドがない`virtual`、つまり、それらの場合があります、かどうかを変更することが、適した選択肢であるしません。

代わりに、元のマークが`NSString`-ベースの定義として`[Protected]`します。 これにより、サブクラス化するために、必要に応じて、および wrap'ed バージョンは引き続き動作し、オーバーライドされたメソッドを呼び出します。

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>バインド`NSValue`、 `NSNumber`、および`NSString`より優れた型

[ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute)属性により、バインド`NSNumber`、`NSValue`と`NSString`(列挙型) に正確なC#型。 属性を使用してより優れたより正確に作成することができます、ネイティブ API 経由での .NET API。

装飾できるは、(戻り値) のメソッド、パラメーターおよびプロパティを[ `[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute)します。 唯一の制限は、メンバー**は許可されません**内に、 [`[Protocol]`](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) 
または[ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute)インターフェイス。

例:

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

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) 配列をサポートも`NSNumber``NSValue`と`NSString`(列挙型)。

例:

```csharp
[BindAs (typeof (CAScroll []))]
[Export ("supportedScrollModes")]
NSString [] SupportedScrollModes { get; set; }
```

出力されます。

```csharp
[Export ("supportedScrollModes")]
CAScroll [] SupportedScrollModes { get; set; }
```

`CAScroll` `NSString`列挙型でのバックアップは右側をフェッチしました`NSString`値し、型変換を処理します。

参照してください、 [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute)ドキュメントをサポートされている変換の種類を参照してください。

<a name="Binding_Notifications" />

### <a name="binding-notifications"></a>通知のバインド

通知は、メッセージに送信できる、`NSNotificationCenter.DefaultCenter`を別のアプリケーションの 1 つの部分からメッセージをブロードキャストするメカニズムとして使用されます。 開発者は通常を使用して通知をサブスクライブ、 [NSNotificationCenter](xref:Foundation.NSNotificationCenter)の[AddObserver](xref:Foundation.NSNotificationCenter.AddObserver(Foundation.NSString,System.Action{Foundation.NSNotification}))メソッド。 アプリケーションでは、通知センターにメッセージをポスト、ときに格納されているペイロードを格納、通常、 [NSNotification.UserInfo](xref:Foundation.NSNotification.UserInfo)ディクショナリ。 このディクショナリは弱く型指定された、ユーザーが通常どのキーがディクショナリとディクショナリに格納できる値の型で使用可能なドキュメントの読み取りが必要とエラーの原因は、その情報を取得します。 キーの存在もブール値として使用されます。

Xamarin.iOS のバインディング ジェネレーターは、通知をバインドする開発者向けのサポートを提供します。 これには、設定、 [`[Notification]`](~/cross-platform/macios/binding/binding-types-reference.md#NotificationAttribute)
されているプロパティの属性でタグ付けします。 [`[Field]`](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute)
プロパティは (そのことでパブリックまたはプライベートの指定できます)。

通知ペイロードが保持されていませんで引数を指定せず、この属性を使用できますかを指定できます、 `System.Type` "EventArgs"で終わる名前を持つ API の定義で別のインターフェイスを通常参照します。 ジェネレーターに変換されます、インターフェイス、クラスをサブクラスとして持つ`EventArgs`されすべてのプロパティがそこに一覧が表示されます。 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Objective C ディクショナリの値をフェッチする検索に使用するキーの名前を一覧表示する EventArgs クラスで属性を使用する必要があります。

例:

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
   }
}
```

コードのユーザーにポストされた通知を簡単にサブスクライブできますし、 [NSDefaultCenter](xref:Foundation.NSNotificationCenter.DefaultCenter)このようなコードを使用しています。

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

返された値`ObserveDidStart`を簡単に次のように、通知の受信を停止するために使用できます。

```csharp
token.Dispose ();
```

呼び出すか、または[NSNotification.DefaultCenter.RemoveObserver](xref:Foundation.NSNotificationCenter.RemoveObserver(Foundation.NSObject))トークンに渡します。 通知にパラメーターが含まれている場合は、ヘルパーを指定する必要があります`EventArgs`インターフェイスは、次のようにします。

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

上記が生成されます、`MyScreenChangedEventArgs`クラス、`ScreenX`と`ScreenY`からデータをフェッチするプロパティ、 [NSNotification.UserInfo](xref:Foundation.NSNotification.UserInfo) "ScreenXKey"と"ScreenYKey"キーを使用してディクショナリそれぞれ適切な変換を適用します。 `[ProbePresence]`プローブ、キーが設定されている場合に、ジェネレーターに属性を使用、`UserInfo`値を抽出しようとしています。 代わりにします。 これは、キーの存在が (通常はブール値) の値である場合に使用されます。

このようなコードを記述できます。

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

<a name="Binding_Categories" />

### <a name="binding-categories"></a>バインディングのカテゴリ

カテゴリは、メソッドとクラスで使用できるプロパティのセットを拡張するために使用する OBJECTIVE-C メカニズムです。   実際には、基底クラスの機能を拡張するかに使用されます (たとえば`NSObject`) で特定のフレームワークがリンクされている場合 (たとえば`UIKit`)、使用できますが、新しいフレームワークにリンクされている場合にのみ、そのメソッドを作成します。   その他の場合、機能によって、クラスの機能を整理するが使用されます。   似ていますが、C#拡張メソッド。これは目標 c: なりますが、カテゴリです。

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

上記の例では、場合に、ライブラリのインスタンスを拡張`UIView`メソッドを使用して`makeBackgroundRed`。

これらをバインドするに使用することができます、 [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute)インターフェイス定義の属性。  使用する場合、 [`[Category]`](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute)
属性の意味、 [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 
属性は、基本クラスを拡張する、拡張する型であることを指定するために使用されてから変更します。

次に示す方法、`UIView`拡張機能のバインドし、なってC#拡張メソッド。

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

上記は作成、`MyUIViewExtension`を含むクラス、`MakeBackgroundRed`拡張メソッド。  つまり、呼び出すことができるようになりました"MakeBackgroundRed"いずれかで`UIView`サブクラスでは、OBJECTIVE-C で表示されますが、同じ機能を提供します。 その他のいくつかの場合は、システム クラスを拡張するされませんが、純粋に装飾の目的での機能を整理するカテゴリを使用します。  以下に例を示します。

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

使用できますが、 [`[Category]`](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute)
属性もこの装飾スタイルの宣言するには、可能性がありますも追加するだけクラスの定義にすべて。  同じ操作を行うこれらの両方の場合します。

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Twitter {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Facebook {
    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

このような場合、カテゴリをマージする短いだけです。

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);

    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

<a name="Binding_Blocks" />

### <a name="binding-blocks"></a>バインド要素

ブロックと同等の機能に Apple によって導入された新しいコンス トラクターは、 C# OBJECTIVE-C に匿名メソッド たとえば、`NSSet`クラスはこのメソッドを公開するようになりました。

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

上記の説明が呼び出されるメソッドを宣言します`enumerateObjectsUsingBlock:`という名前の 1 つの引数を受け取る`block`します。 このブロックと似ています、C#匿名メソッドで、現在の環境 ("this"ポインターをローカル変数とパラメーターへのアクセス) をキャプチャするためのサポートがあります。 上記の方法で`NSSet`2 つのパラメーターを持つブロックを呼び出す、 `NSObject` (、`id obj`パーツ) とブール値へのポインター (、 `BOOL *stop`) 一部。

このような API を btouch をバインドするには、最初に、ブロックの型とシグネチャを宣言する必要があります、C#を委任し、次のように、API エントリ ポイントから参照します。

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

これで、コードがから関数を呼び出すことができますC#:

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

たい場合は、ラムダを使用することもできます。

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

<a name="GeneratingAsync" />

### <a name="asynchronous-methods"></a>非同期メソッド

バインディング ジェネレーターは、非同期に適したメソッドにメソッドの特定のクラスを有効にできます (タスクまたはタスクを返すメソッドを&lt;T&gt;)。

使用することができます、 [`[Async]`](~/cross-platform/macios/binding/binding-types-reference.md#AsyncAttribute) 
void を返すし、最後の引数は、コールバック メソッドの属性です。  バインディング ジェネレーターがサフィックスを持つメソッドのバージョンを生成はこれをメソッドに適用すると`Async`します。  コールバックがパラメーターを受け取らない場合、戻り値になります、 `Task`、コールバックがパラメーターを受け取る場合、結果になります、`Task<T>`します。  設定する必要があります、コールバックが複数のパラメーターを受け取る場合、`ResultType`または`ResultTypeName`に必要なすべてのプロパティを格納する生成された型の名前を指定します。

例:

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

上記のコードは両方、LoadFile メソッドを生成として。

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types" />

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>弱い NSDictionary パラメーターの厳密な型の表示

Objective C API でさまざまな場所でパラメーターが渡される、厳密に型指定された`NSDictionary`特定のキーと値がこれらの Api は、エラー、無効なキーを渡すし、警告が表示されないことができます (無効な値を渡すし、警告が表示されないことができます) が発生しにくい使用可能なキーの名前と値の参照ドキュメントを複数のトリップが必要なときに使用します。

解決するには、厳密に型指定されたバージョンの API およびバック グラウンドで、さまざまな基になるキーと値のマップを提供する、厳密に型指定されたバージョンを提供します。

Objective C API が受け入れられた場合は、例については、その、`NSDictionary`し、それを文書化、キーの取得として`XyzVolumeKey`受け取り、 `NSNumber` 0.0 から 1.0 までのボリューム値で、`XyzCaptionKey`文字列を受け取る、する便利な API があるユーザー次に示します。

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

`Volume` Objective C での慣例にこれらのディクショナリ値が設定される可能性がありますいないシナリオがあるために、値を持つことが必要としないよう、プロパティが null 許容の浮動小数点数として定義されています。

これを行うには、いくつかの操作を行いますが必要です。

* サブクラスとして持つ、厳密に型指定されたクラスを作成[DictionaryContainer](xref:Foundation.DictionaryContainer)と各プロパティのさまざまな getter および setter を提供します。
* 受け取るメソッド オーバー ロードを宣言`NSDictionary`新しい厳密に型指定されたバージョンを実行します。

厳密に型指定されたクラスをか、手動で作成したり、ジェネレーターを使用するための作業を実行することができます。  まず、何が起こってを理解するように手動で行う方法と、自動のアプローチをについて説明します。

このサポート ファイルを作成する必要がある、API コントラクトには説明しません。  これは、書き込み、XyzOptions クラスを作成する必要があります。

```csharp
public class XyzOptions : DictionaryContainer {
# if !COREBUILD
    public XyzOptions () : base (new NSMutableDictionary ()) {}
    public XyzOptions (NSDictionary dictionary) : base (dictionary){}

    public nfloat? Volume {
       get { return GetFloatValue (XyzOptionsKeys.VolumeKey); }
       set { SetNumberValue (XyzOptionsKeys.VolumeKey, value); }
    }
    public string Caption {
       get { return GetStringValue (XyzOptionsKeys.CaptionKey); }
       set { SetStringValue (XyzOptionsKeys.CaptionKey, value); }
    }
# endif
}
```

低レベルの API の上に、高度な API サーフェスのラッパー メソッドを提供する必要があります。

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

使用して、NSDictionary に基づく API を安全に非 API が上書きされる必要がない場合、 [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
属性。

使用して、ご覧のとおり、 [`[Wrap]`](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute)
新しい API エントリ ポイントを表示する属性し、厳密に型指定を使用して明示`XyzOptions`クラス。  ラッパー メソッドは null を渡すこともできます。

ここで、伝えがないことの 1 つは、where、`XyzOptionsKeys`値の取得元します。  グループ化キーのように、静的クラスでの API サーフェスを通常`XyzOptionsKeys`、次のようにします。

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

これらの厳密に型指定されたディクショナリを作成するための自動サポートについて見てみましょう。  これにより、定型の十分なと直接外部のファイルを使用する代わりに、API コントラクトのディクショナリを定義できます。

厳密に型指定されたディクショナリを作成する API のインターフェイスを紹介しで装飾、 [StrongDictionary](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary)属性。  クラスから派生しているインターフェイスと同じ名前で作成するようにジェネレーターこの指示`DictionaryContainer`の厳密な型指定されたアクセサーを提供します。

[ `[StrongDictionary]` ](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary)属性では、1 つのパラメーターを dictionary のキーを含む静的クラスの名前です。  インターフェイスの各プロパティには、厳密に型指定されたアクセサーになります。  既定では、コードは、プロパティの名前サフィックスを持つ使用「キー」静的クラスで、アクセサーを作成します。

これは、厳密に型指定されたアクセサーを作成できなくが必要である外部のファイルも getter および setter のすべてのプロパティを手動で作成することも、キーを手動で参照することは意味自分でします。

これは、全体のバインドはのようになります。

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
[StrongDictionary ("XyzOptionKeys")]
interface XyzOptions {
    nfloat Volume { get; set; }
    string Caption { get; set; }
}

[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

参照する必要がある場合に、`XyzOption`メンバーを別のフィールド (つまり名ではなく、サフィックスを持つプロパティの`Key`) のプロパティを修飾することができます、 [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 
使用する名前の属性です。

<a name="Type_mappings" />

## <a name="type-mappings"></a>型のマッピング

このセクションでは、OBJECTIVE-C の種類をマップする方法について説明しますC#型。

<a name="Simple_Types" />

### <a name="simple-types"></a>単純型

次の表は、OBJECTIVE-C と CocoaTouch 世界から Xamarin.iOS 世界への型をマップする方法を示しています。

|Objective C の型名|Xamarin.iOS Unified API の種類|
|---|---|
|`BOOL`, `GLboolean`|`bool`|
|`NSInteger`|`nint`|
|`NSUInteger`|`nuint`|
|`CFTimeInterval` / `NSTimeInterval`|`double`|
|`NSString` ([NSString のバインドについて](~/ios/internals/api-design/nsstring.md))|`string`|
|`char *`|`string` (も参照してください: [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring))|
|`CGRect`|`CGRect`|
|`CGPoint`|`CGPoint`|
|`CGSize`|`CGSize`|
|`CGFloat`, `GLfloat`|`nfloat`|
|CoreFoundation 型 (`CF*`)|`CoreFoundation.CF*`|
|`GLint`|`nint`|
|`GLfloat`|`nfloat`|
|Foundation 型 (`NS*`)|`Foundation.NS*`|
|`id`|`Foundation`.`NSObject`|
|`NSGlyph`|`nint`|
|`NSSize`|`CGSize`|
|`NSTextAlignment`|`UITextAlignment`|
|`SEL`|`ObjCRuntime.Selector`|
|`dispatch_queue_t`|`CoreFoundation.DispatchQueue`|
|`CFTimeInterval`|`double`|
|`CFIndex`|`nint`|
|`NSGlyph`|`nuint`|

<a name="Arrays" />

### <a name="arrays"></a>配列

Xamarin.iOS ランタイムは、変換の処理は自動的にC#に配列`NSArrays`返しますコードの変換、虚数部の OBJECTIVE-C メソッドではそのため、たとえば、`NSArray`の`UIViews`:

```csharp
// Get the peer views - untyped
- (NSArray *)getPeerViews ();

// Set the views for this container
- (void) setViews:(NSArray *) views
```

次のようにバインドします。

```csharp
[Export ("getPeerViews")]
UIView [] GetPeerViews ();

[Export ("setViews:")]
void SetViews (UIView [] views);
```

考え方は、厳密に型を使用するC#これにより、推測、または配列に含まれるオブジェクトの実際の型を確認するドキュメントを検索するユーザーを強制することがなく、実際の型との適切なコード補完機能を提供する IDE としての配列。

使用することができます、配列に含まれる実際の大多数の派生型を追跡できますしない場合、`NSObject []`戻り値として。

<a name="Selectors" />

### <a name="selectors"></a>セレクター

特殊な種類として、Objective C API にセレクターが表示される`SEL`します。 型はマップのセレクターをバインドするときに`ObjCRuntime.Selector`します。  通常のセレクターは、ターゲット オブジェクトを呼び出すには、両方のオブジェクト、ターゲット オブジェクト、およびセレクターで API で公開されます。 基本的に提供するこれらの両方に対応する、C#デリゲート: オブジェクトでメソッドを呼び出すだけでなく、呼び出すメソッドをカプセル化するものです。

これは、バインディングがどのようにです。

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

これは、方法、メソッドは通常、使用するアプリケーションで.

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        b.SetTarget (this, new Selector ("print"));
    }

    [Export ("print")]
    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

してバインドをするC#開発者は、通常は入力を受け取るメソッドを`NSAction`パラメーターC#の代わりに使用するには、デリゲートとラムダ、`Target+Selector`します。 非表示には通常、これを行う、`SetTarget`メソッドでフラグを設定します。 [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
属性と、次のように、新しいヘルパー メソッドを公開は。

```csharp
// API.cs
interface Button {
   [Export ("setTarget:selector:"), Internal]
   void SetTarget (NSObject target, Selector sel);
}

// Extensions.cs
public partial class Button {
     public void SetTarget (NSAction callback)
     {
         SetTarget (new NSActionDispatcher (callback), NSActionDispatcher.Selector);
     }
}
```

これで、ユーザー コードは、次のように記述できます。

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        // First Style
        b.SetTarget (ThePrintMethod);

        // Lambda style
        b.SetTarget (() => {  /* print here */ });
    }

    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

<a name="Strings" />

### <a name="strings"></a>文字列

受け取るメソッドをバインドする場合、`NSString`で置き換えることができます、C#文字列型、戻り値の型とパラメーターの両方でします。

唯一のケースを使用するときに、`NSString`直接は、文字列をトークンとして使用する場合。 文字列の詳細については、 `NSString`、読み取り、 [NSString で API の設計](~/ios/internals/api-design/nsstring.md)ドキュメント。

まれなケースでは、API が C のような文字列を公開可能性があります (`char *`)、Objective C 文字列ではなく (`NSString *`)。 その場合、パラメーターの注釈を表示できます、 [`[PlainString]`](~/cross-platform/macios/binding/binding-types-reference.md#plainstring)
属性。

<a name="outref_parameters" />

### <a name="outref-parameters"></a>out または ref パラメーター

一部の Api は、それらのパラメーターに値を返すか、参照によってパラメーターを渡します。

通常、署名は、ようになります。

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

最初の例では、エラー コードへのポインターを返すに一般的な Objective C の表現形式、`NSError`ポインターが渡され、戻り時に、値が設定されます。   2 番目のメソッドは、可能性があります、OBJECTIVE-C メソッドのオブジェクトを受け取るし、その内容を変更する方法を示します。   純粋な出力値ではなく、参照によって、パスになります。

バインドは、次のようになります。

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

<a name="Memory_management_attributes" />

### <a name="memory-management-attributes"></a>メモリ管理の属性

使用すると、 [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)属性とする呼び出されたメソッドで保持されるデータを受け渡し、たとえば 2 番目のパラメーターとして渡す、引数のセマンティクスを指定できます。

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

上記の「保持」セマンティクスを持つものとして値はフラグです。 使用可能なセマンティクスは次のとおりです。

-  Assign
-  コピー
-  保持

<a name="Style_Guidelines" />

### <a name="style-guidelines"></a>スタイルのガイドライン

<a name="Using_[Internal]" />

#### <a name="using-internal"></a>[内部] を使用してください。

使用することができます、 [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
パブリック API からのメソッドを非表示にする属性です。 これは、公開されている API が低レベルも、このメソッドに基づく別のファイルに高レベルの実装を提供する場合でする可能性があります。

バインディング ジェネレーターの制限事項が発生する、たとえば、高度なシナリオはバインドされていない型を公開する可能性があります、独自の方法でバインドして、独自の方法で自分でこれらの型をラップするときにすることも使用できます。

<a name="Event_Handlers_and_Callbacks" />

## <a name="event-handlers-and-callbacks"></a>イベント ハンドラーとコールバック

OBJECTIVE-C のクラスは、通常の通知をブロードキャストまたはデリゲート クラス (Objective C のデリゲート) でメッセージを送信して情報を要求します。

このモデルでは、完全にサポートされているし、Xamarin.iOS 側に表示される場合があります煩雑です。 Xamarin.iOS を公開、C#イベント パターンと、このような状況で使用できるクラスのメソッドのコールバック システム。 これにより、実行するこのようなコードです。

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

バインディング ジェネレーターに Objective C のパターンをマッピングするために必要な型指定の量を減らすことのできる、C#パターン。

Xamarin.iOS 1.4 で始まることはも特定 Objective C のデリゲートのバインドを作成し、そのデリゲートを公開するジェネレーターに指示することになりますC#イベントとホスト型のプロパティ。

このプロセスに関連する 2 つのクラスがあります、これは、ホスト クラスは現在のイベントを出力し、これらに送信する 1 つ、`Delegate`または`WeakDelegate`と実際のデリゲート クラス。

次の設定を検討します。

```csharp
[BaseType (typeof (NSObject))]
interface MyClass {
    [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
    NSObject WeakDelegate { get; set; }

    [Wrap ("WeakDelegate")][NullAllowed]
    MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
    [Export ("loaded:bytes:")]
    void Loaded (MyClass sender, int bytes);
}
```

クラスをラップする必要があります。

-  ホスト クラスを追加、 [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute)  
   宣言型のデリゲートとして機能していると、C#公開された名前。 上記の例では`typeof (MyClassDelegate)`と`WeakDelegate`それぞれします。
-  デリゲート クラスを複数の 2 つのパラメーターを持つ各メソッドでは、自動的に生成された、EventArgs クラスを使用する種類を指定する必要があります。

バインディング ジェネレーターは 1 つのイベントの保存先のみをラップする限定されませんを 1 つ以上のメッセージを出力するいくつかの Objective C クラスがデリゲート可能なため、この設定をサポートするために配列を提供する必要があります。 ほとんどの設定では、その必要はありませんが、ジェネレーターは、そのような場合に対応しています。

結果のコードになります。

```csharp
[BaseType (typeof (NSObject),
    Delegates=new string [] {"WeakDelegate"},
    Events=new Type [] { typeof (MyClassDelegate) })]
interface MyClass {
        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }

        [Wrap ("WeakDelegate")][NullAllowed]
        MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
        [Export ("loaded:bytes:"), EventArgs ("MyClassLoaded")]
        void Loaded (MyClass sender, int bytes);
}
```

`EventArgs`の名前を指定するために使用、`EventArgs`クラスを生成します。 シグネチャごとに 1 つを使用する必要があります (この例で、`EventArgs`が含まれます、`With`型 nint のプロパティ)。

上記の定義と、ジェネレーターに生成された MyClass で次のイベントが生成されます。

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

そのためこのようなコードを今すぐ使用できます。

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

コールバックは、イベントの呼び出しと同じように、その違いはなく、複数の潜在的なサブスクライバー (に複数のメソッドをフックできるなど、`Clicked`イベントまたは`DownloadFinished`イベント) のコールバックでは、単一のサブスクライバーを持つことができますのみです。

プロセスは同じ、唯一の違いの名前を公開するのではなく、`EventArgs`が生成されます、EventArgs クラスは、結果を名前に使用されます実際にC#デリゲート名。

デリゲート クラスのメソッドが値を返す場合、バインディング ジェネレーターはマップこのイベントではなく、親クラスのデリゲート メソッドにします。 このような場合は、返される、メソッドによって、ユーザーはフックしていない場合、デリゲートに既定値を指定する必要があります。 これを行うを使用して、 [`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute)
または[ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute)属性。

[`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) 戻り値をハードコーディングには、 [`[DefaultValueFromArgument]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute)
入力引数が返されますの指定に使用されます。

<a name="Enumerations_and_Base_Types" />

## <a name="enumerations-and-base-types"></a>列挙型と基本データ型

列挙型または btouch インターフェイス定義のシステムによって直接サポートされていない基本データ型を参照することもできます。 これを行うには、別のファイルに、列挙型と中核となる型を配置して、btouch に提供する余分なファイルのいずれかの一部として、これが含まれます。

<a name="Linking_the_Dependencies" />

## <a name="linking-the-dependencies"></a>依存関係のリンク

アプリケーションの一部ではない Api をバインドする場合は、これらのライブラリに対して、実行可能ファイルがリンクされていることを確認する必要があります。

Xamarin.iOS ライブラリにリンクする方法を通知する必要があります、これを行うを呼び出すビルド構成を変更することにより、`mtouch`いくつか余分にコマンドを使用して、新しいライブラリとリンクする方法を指定する引数をビルドする、"-gcc_flags"オプション次のように、プログラムに必要なすべての余分なライブラリが含まれている引用符で囲まれた文字列が続きます。

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

上記の例ではリンク`libMyLibrary.a`、`libSystemLibrary.dylib`と`CFNetwork`フレームワーク ライブラリ、最終的な実行可能ファイルにします。

アセンブリ レベルの利用または[ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute)、コントラクト ファイルに埋め込むことができますが (など`AssemblyInfo.cs`)。
使用すると、 [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute)、際に、バインド、ネイティブ ライブラリを埋め込むアプリケーションではこの時点で、ネイティブ ライブラリが存在する必要があります。 例:

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

思うかもしれません、実行が必要な理由`-force_load`コマンド、およびその理由は、- ObjC フラグでコードをコンパイルするが、カテゴリ (リンカーとコンパイラ実行されないコードの削除は、それを除去) をサポートするために必要なメタデータは保持されませんがXamarin.iOS の実行時に必要です。

<a name="Assisted_References" />

## <a name="assisted-references"></a>支援付きの参照

アクション シート、警告ボックスなどのいくつかの一時的なオブジェクトは、開発者向けを追跡する煩雑とバインディング ジェネレーターがここで少しに役立ちます。

たとえば、メッセージ、その後、生成されるクラスがある場合、`Done`イベント、これを処理の従来の方法があります。

```csharp
class Demo {
    MessageBox box;

    void ShowError (string msg)
    {
        box = new MessageBox (msg);
        box.Done += { box = null; ... };
    }
}
```

上記のシナリオでは、開発者は自身といずれかのリーク、オブジェクトへの参照を保持または積極的に独力でのボックスのリファレンスをオフにする必要があります。  サポートされますが、バインド コード ジェネレーターの参照の追跡し、オフ、特殊なメソッドが呼び出されたときに、上記のコードになる場合が。

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

どのインスタンスに保持する変数をローカル変数で動作して、オブジェクトが実行されていないときに、参照をオフにする必要はありません必要はなくなりましたに注意してください。

活用するためにこれをクラスにイベント プロパティ設定をする必要がありますが、 [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute)宣言しても、`KeepUntilRef`変数、オブジェクトがその作業を完了したようなときに呼び出されるメソッドの名前に設定これ：

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

<a name="Inheriting_Protocols" />

## <a name="inheriting-protocols"></a>プロトコルを継承します。

Xamarin.iOS の v3.2 時点でサポートされていますが付いているプロトコルからの継承、 [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute)プロパティ。 API、特定のパターンとしてのような場合に便利です`MapKit`場所、`MKOverlay`プロトコル、継承、`MKAnnotation`プロトコル、およびから継承するクラスの数が採用`NSObject`。

従来、プロトコルをすべての実装にコピーする必要が、このような場合のようになりましたがあっても、`MKShape`クラスから継承する、`MKOverlay`プロトコルと、必要なメソッドをすべて自動的に生成されます。

## <a name="related-links"></a>関連リンク

- [バインドのサンプル](https://developer.xamarin.com/samples/monotouch/BindingSample/)
