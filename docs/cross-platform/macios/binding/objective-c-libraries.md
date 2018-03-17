---
title: "Objective C ライブラリのバインド"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 8674a8b846573c27e54660ae3bc065e07561f411
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2018
---
# <a name="binding-objective-c-libraries"></a>Objective C ライブラリのバインド

Xamarin.iOS または Xamarin.Mac を処理するときは、サード パーティ製 Objective C のライブラリを使用する場合が発生する可能性があります。 ような状況では、ネイティブの Objective C ライブラリを c# バインディングを作成するのに Xamarin バインド プロジェクトを使用できます。 プロジェクトでは、C# の場合に、iOS と Mac の Api を使用する同じツールを使用します。

Objective C Api をバインドする方法について説明 C Api のみをバインドする場合は、これは、標準の .NET メカニズムを使用する必要があります[P/invoke framework](http://mono-project.com/Dllimport)です。
C ライブラリを静的にリンクする方法の詳細についてで使用できる、[ネイティブ ライブラリのリンク](~/ios/platform/native-interop.md)ページ。

当社ガイド 』 を参照してください[バインドの種類のリファレンス ガイド](~/cross-platform/macios/binding/binding-types-reference.md)です。
また場合は、内部での動作の詳細を確認するには、確認、[バインディングの概要](~/cross-platform/macios/binding/overview.md)ページ。

バインディングは、iOS と Mac のライブラリの両方を構築できます。
このページでは、バインディング、Mac のバインドはよく似ていますが、iOS で操作する方法について説明します。

**IOS 用のサンプル コード**

使用することができます、 [iOS のバインドのサンプル](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample)バインドをテストするプロジェクトです。

<a name="Getting_Started" />

## <a name="getting-started"></a>作業の開始

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

バインディングを作成する最も簡単な方法では、Xamarin.iOS バインド プロジェクトを作成します。
これを行う Visual Studio から Mac 用プロジェクトの種類を選択して**iOS > ライブラリ > バインディング ライブラリ**:

[![](objective-c-libraries-images/00-sml.png "これは Visual Studio for Mac、プロジェクトの種類、iOS ライブラリのバインドを選択")](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

バインディングを作成する最も簡単な方法では、Xamarin.iOS バインド プロジェクトを作成します。
これを行う Windows 上の Visual Studio から、プロジェクトの種類を選択して**Visual c# > iOS > バインディング ライブラリ (iOS)**:

[![](objective-c-libraries-images/00vs-sml.png "iOS のバインドのライブラリの iOS")](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> 注: バインド プロジェクト**Xamarin.Mac** for mac に Visual Studio でのみサポートされて

-----

生成されたプロジェクトが編集できる小規模のテンプレートを含む、2 つのファイルが含まれています:`ApiDefinition.cs`と`StructsAndEnums.cs`です。

`ApiDefinition.cs` API コントラクトを定義した場合は、これは、C# の場合に、基になる Objective C API を射影する方法を説明するファイル。 構文およびこのファイルの内容は、このドキュメントの説明のトピックとその内容は c# インターフェイスとデリゲートの宣言 (C#) に制限されます。 `StructsAndEnums.cs`ファイルは、インターフェイスやデリゲートで必要なすべての定義を入力するファイル。 これには、列挙値と、コードを使用する構造体が含まれます。

<a name="Binding_an_API" />

## <a name="binding-an-api"></a>API のバインド

包括的なバインドには、する Objective C API の定義を理解し、.NET Framework デザイン ガイドラインを理解しておいてください。

API 定義ファイルから始めるは通常、ライブラリをバインドします。 API 定義ファイルは、単なる c# ソース ファイルをバインディングには、いくつかのドライブに役立つ属性で注釈が付けられている c# インターフェイスが含まれています。  このファイルは、c# と OBJECTIVE-C 間のコントラクトは、どのような新機能を定義します。

たとえば、これは、ライブラリの単純な api ファイルです。

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

上記のサンプルと呼ばれるクラスを定義する`Cocos2D.Camera`から派生した、`NSObject`基本データ型 (この型に由来`Foundation.NSObject`) 静的なプロパティを定義して (`ZEye`)、引数なしとメソッドを実行する 2 つのメソッドは、3 つ引数。

API ファイルと使用できる属性の形式の詳しい説明については、 [API 定義ファイル](~/cross-platform/macios/binding/objective-c-libraries.md)以下のセクションです。

完全なバインドを作成するには、通常、4 つのコンポーネントが処理されます。

-  API 定義ファイル (`ApiDefinition.cs`テンプレートで)。
-  省略可能: 任意の列挙型、型、API 定義ファイルで必要な構造体 (`StructsAndEnums.cs`テンプレートで)。
-  省略可能: 余分なソースを生成されたバインディングを展開または API を提供する複数 c# フレンドリ (c# ファイルをプロジェクトに追加する) ことがあります。
-  ネイティブ ライブラリにバインドします。

このグラフは、ファイル間の関係を示しています。

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png "このグラフは、ファイル間の関係を示しています")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

API 定義ファイル: 名前空間とインターフェイス定義で (と共にインターフェイスを含めることができるメンバー) のみが含まれ、クラス、列挙体、デリゲート、または構造体を含めることはできません。 API 定義ファイルは、API の生成に使用されるコントラクトだけです。

列挙体と同様に必要なこと、または"CameraMode"上の例で、別のファイルでホストされるクラスをサポートする必要がありますの余分なコードは、CS ファイルに存在しないため、たとえば別のファイルでホストされる必要がある列挙値`StructsAndEnums.cs`:

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

`APIDefinition.cs`ファイルと組み合わせて、`StructsAndEnum`クラスを使用して、ライブラリのコア バインドを生成します。 として生成されたライブラリを使用することができます、結果として得られる、ユーザーのため一部の c# 機能を追加するライブラリをチューニングしたいが、通常はします。 いくつかの例には、実装が含まれて、`ToString()`メソッド、c# のインデクサーを提供、なんらかのネイティブ型との間の暗黙的な変換を追加またはいくつかの方法の厳密に型指定されたバージョンを提供します。 これらの機能強化は、余分な c# ファイルに格納されます。 単なる c# ファイルをプロジェクトに追加し、それらはこのビルド プロセスに含まれます。

これでコードを実装する方法を示しています、`Extra.cs`ファイル。 使用することは部分クラスの組み合わせから生成される部分クラスを拡張したりこれらとに注意してください、`ApiDefinition.cs`と`StructsAndEnums.cs`コア バインド。

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

ライブラリのビルドと、ネイティブなバインディングが生成されます。

このバインドを完了するには、プロジェクトにネイティブ ライブラリを追加する必要があります。  プロジェクトを右クリックして、ドラッグ アンド ドロップ ネイティブ ライブラリ Finder から、ソリューション エクスプ ローラーでプロジェクトをするか、プロジェクトにネイティブ ライブラリを追加することでこれを行う**追加** > **ファイルの追加**ネイティブ ライブラリを選択します。
慣例によりネイティブ ライブラリでは、"lib"という単語で始まるし、末尾に拡張子".a"です。 Visual Studio for Mac が 2 つのファイルを追加した場合、:`.a`ファイルと、自動的に設定されている c# ファイル ネイティブ ライブラリに含まれる新機能に関する情報を格納します。

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png "慣例によりネイティブ ライブラリ word lib で開始および終了拡張子 .a")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

内容、`libMagicChord.linkwith.cs`ファイルはこのライブラリの使用方法については、生成された DLL ファイルにこのバイナリをパッケージ化、IDE に指示します。

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

ソースコードファ属性を使用する方法の詳細についてに記載されて、[バインドの種類のリファレンス ガイド](~/cross-platform/macios/binding/binding-types-reference.md)です。

これで、/compile には、プロジェクトをビルドするときに、`MagicChords.dll`バインディングと、ネイティブ ライブラリの両方を含むファイルです。 このプロジェクトを配布したり、結果の他の開発者が独自の DLL を使用します。

必要があるいくつかの列挙値、デリゲートの定義、またはその他の種類があります。 配置しないでください API 定義ファイルのコントラクトだけでは

 <a name="The_API_definition_file" />

## <a name="the-api-definition-file"></a>API 定義ファイル

API 定義ファイルは、インターフェイスの数で構成されます。 API 定義内のインターフェイスをクラス宣言になり、使用する装飾する必要があります、 [[BaseType]](~/cross-platform/macios/binding/binding-types-reference.md)属性をクラスの基本クラスを指定します。

だろう、なぜお使用しなかったクラス インターフェイスではなく定義については、コントラクトです。 API 定義ファイル内のメソッド本体を指定することがまたは例外をスローまたは意味のある値を取得する必要があった本文を指定することがなく、メソッドのコントラクトを記述することが許可されているためにインターフェイスを選択しました。

おを使用しているインターフェイス スケルトンとしてクラスを生成するため、バインドをドライブに属性を持つコントラクトのさまざまな部分を修飾することに頼る必要があります。

<a name="Binding_Methods" />

### <a name="binding-methods"></a>メソッドのバインディング

行うことができます最も単純なバインディングでは、メソッドをバインドします。 だけ、c# の名前付け規則を持つインターフェイスのメソッドを宣言しを使用してメソッドを装飾、 [[エクスポート]](~/cross-platform/macios/binding/binding-types-reference.md)属性。 [エクスポート] 属性は、リンク、Xamarin.iOS ランタイムで Objective C の名前の c# 名前です。 エクスポート属性のパラメーターは、OBJECTIVE-C セレクター、例をいくつかの名前です。

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

上記のサンプルでは、インスタンス メソッドをバインドする方法を示しています。 静的メソッドをバインドするに使用する必要があります、`[Static]`属性には、次のようにします。

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

これが必要なコントラクトが、インターフェイスの一部でされインターフェイスには、静的ポートとインスタンスの宣言の概念がない属性をもう一度する必要があるためです。 使用してメソッドを装飾できますのバインドからの特定のメソッドを非表示にする場合、 [[内部]](~/cross-platform/macios/binding/binding-types-reference.md)属性。

`btouch-native`コマンドで導入する必要ありません、null にする参照パラメーターをチェックします。 特定のパラメーターに対して null 値を許可する場合を使用して、 [[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md)次のように、パラメーターに属性。

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

参照型をエクスポートするときに、`[Export]`キーワードの割り当てのセマンティクスを指定することもできます。 これは、データがリークせずことを確認する必要があります。

<a name="Binding_Properties" />

### <a name="binding-properties"></a>バインドのプロパティ

使用して、メソッドと同じように OBJECTIVE-C プロパティがバインドされて、 [[エクスポート]](~/cross-platform/macios/binding/binding-types-reference.md)属性および c# のプロパティに直接マップします。 使用方法と同じようにプロパティを装飾できます、 [[静的]](~/cross-platform/macios/binding/binding-types-reference.md)と[[内部]](~/cross-platform/macios/binding/binding-types-reference.md)属性。

使用すると、`[Export]`カバー btouch ネイティブの下で、プロパティの属性は、2 つのメソッドを実際にはバインド: getter と setter です。 エクスポートするのには、指定した名前、 **basename**と、set アクセス操作子が「セット」の最初の文字にすると、単語を付加することによって計算された、 **basename**大文字に変換したうえでかかるセレクターに、引数。 つまり、`[Export ("label")]`に適用されるプロパティは、"label"を実際にはバインドと"setLabel:"OBJECTIVE-C メソッドです。

Objective C のプロパティが上記のパターンを示さないして、名前は手動で上書きされる場合があります。 そのような場合は、バインディングを使用して生成する方法を制御できます、 `[Bind]` get アクセス操作子または set アクセス操作子、属性の例を示します。

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

Get アクセス操作子および set アクセス操作子が明示的に定義されているように、`name`と`setName`上のバインド。

使用して静的プロパティのサポートに加えて`[Static]`、装飾できるは、スレッド内静的プロパティを[[IsThreadStatic]](~/cross-platform/macios/binding/binding-types-reference.md)、例を示します。

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

メソッドにフラグを設定するいくつかのパラメーターを使用するのと同じように[[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md)、適用できる[[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md)そのは null は、プロパティの有効な値などを示すためにプロパティに。

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

[[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md)パラメーターは、setter 上で直接も指定することができます。

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>カスタム コントロールのバインドの注意事項があります。

カスタム コントロールのバインドを設定するときは、次の注意事項を検討してください。

1. **バインドのプロパティは、静的にする必要があります**、プロパティのバインドを定義するとき、`Static`属性を使用する必要があります。
2. **プロパティ名が正確に一致する必要があります**-プロパティのバインドに使用する名前では、カスタム コントロールのプロパティの名前を正確に一致する必要があります。
3. **プロパティの型が正確に一致する必要があります**-プロパティをバインドするために使用する変数の型では、カスタム コントロールのプロパティの型を完全に一致する必要があります。
4. **ブレークポイントと getter または setter** - getter にブレークポイントを設定またはプロパティの setter メソッドがヒットすることはありません。
5. **コールバックを観察**-通知を受けるカスタム コントロールのプロパティの値の変更の監視のコールバックを使用する必要があります。

上記の一覧の注意事項のいずれかを確認するエラーは、サイレント モードで実行時に失敗しているバインドになります。

<a name="MutablePattern" />

#### <a name="objective-c-mutable-pattern-and-properties"></a>Objective C の変更可能なパターンおよびプロパティ

Objective C フレームワークは、表現を使用して、一部のクラスは変更可能なサブクラスで変更できません。   たとえば`NSString`不変のバージョンでは、中に`NSMutableString`変異できるサブクラスです。

それらのクラスに共通するプロパティで、get アクセス操作子がセッターがありませんが含まれる変更できない基本クラスを参照してください。   Set アクセス操作子を導入する変更可能なバージョンについてはします。   これができないため実際に c# を使用して、この表現方法と c# が連携は表現にマップするありました。

これは c# にマップする方法は、基本クラスでは、getter と setter の両方を追加することがで set アクセス操作子にフラグを設定、`[NotImplemented]`属性。

次に、変更可能なサブクラスでは、使用する、`[Override]`プロパティが実際には、親の動作をオーバーライドすることを確認するプロパティの属性です。

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

**Btouch ネイティブ**ツールがクラスでは、特定のクラスの 4 つのコンス トラクターを自動的に生成します。 `Foo`、が生成されます。

-  `Foo ()`: 既定のコンス トラクター (maps OBJECTIVE-C の"init"のコンス トラクターを)
-  `Foo (NSCoder)`: NIB ファイルの逆シリアル化中に使用するコンス トラクター (OBJECTIVE-C のマップ"initWithCoder:"のコンス トラクター)。
-  `Foo (IntPtr handle)`: コンス トラクターは、ハンドル ベースの作成では、これが呼び出される、ランタイムによって、ランタイムはアンマネージ オブジェクトからマネージ オブジェクトを公開する必要がある場合。
-  `Foo (NSEmptyFlag)`: これは、二重の初期化を防ぐために、派生クラスによって使用がします。

インターフェイス定義内の次の署名を使用して宣言する必要が定義されたコンス トラクター、: 返す必要がある、`IntPtr`値とメソッドの名前は、コンス トラクターをする必要があります。 バインドする例については、`initWithFrame:`コンス トラクターを使用して、これは。

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

 <a name="Binding_Protocols" />

### <a name="binding-protocols"></a>バインディング プロトコル

API デザイン ドキュメントのセクションで」の説明に従って[モデルとプロトコルについて説明する](~/ios/internals/api-design/index.md)、Xamarin.iOS とフラグ付けされているクラスに Objective C のプロトコルをマップする、 [[モデル]](~/cross-platform/macios/binding/binding-types-reference.md)属性。 Objective C デリゲート クラスを実装するときに通常使用されます。

通常のバインドされたクラスと、デリゲート クラスの大きな違いは、デリゲート クラスは 1 つまたは複数のオプションのメソッドである可能性があります。

たとえば、`UIKit`クラス`UIAccelerometerDelegate`、これは、Xamarin.iOS 内のバインド方法。

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

これは省略可能なメソッドの定義でため`UIAccelerometerDelegate`を行うにはそれ以外のものを使用する必要があります。 追加する必要があります、プロトコルに必要なメソッドがあった場合は、 [[抽象]](~/cross-platform/macios/binding/binding-types-reference.md)属性をメソッドにします。 これにより、実際には、メソッドの本体を提供する実装のユーザーが強制的です。

一般に、プロトコルは、メッセージに応答するクラスで使用されます。 これは、通常 Objective C の「代理」プロパティを割り当てると、プロトコルのメソッドに応答するオブジェクトのインスタンス。

Xamarin.iOS の規則は、疎両方、OBJECTIVE-C をサポートするために結合されたスタイル任意のインスタンス、`NSObject`割り当てることができる、デリゲートとも公開する厳密に型指定されたバージョンです。 このため、マイクロソフト通常提供する厳密に型指定された「代理」プロパティと弱い型指定されている"WeakDelegate"の両方。 厳密でないバージョンのエクスポートと通常バインドし、使用して、 [[折り返し]](~/cross-platform/macios/binding/binding-types-reference.md)属性が厳密に型指定のバージョンを提供します。

これはバインドする方法を示しています、`UIAccelerometer`クラス。

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

新規および改良されたプロトコルが機能をバインディング MonoTouch 7.0 以降が組み込まれました。  この新しいサポートが簡単に特定のクラスの 1 つまたは複数のプロトコルを採用する Objective C の表現方法を使用します。

すべてのプロトコル定義に`MyProtocol`OBJECTIVE-C であるようになりました、`IMyProtocol`省略可能なすべてのメソッドを提供する拡張機能クラスと同様に、プロトコルからのすべての必要なメソッドを一覧表示するインターフェイスです。  組み合わせるエディターにより、開発者が以前の抽象型のモデル クラスの別のサブクラスを使用することがなくプロトコル メソッドを実装する Xamarin Studio の新しいサポート。

含まれている任意の定義、`[Protocol]`属性は、プロトコルを使用する方法を大幅に改善する 3 つのサポート クラスに実際に生成されます。

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

**クラスの実装**完全な抽象クラスの個々 のメソッドをオーバーライドおよび完全な型の安全性を取得できることを指定します。  により c# 多重継承をサポートしていないが存在する場合がありますのシナリオが別の基本クラスでは、する必要がありますが、場所であるインターフェイスを実装するが、

生成された**インターフェイス定義**で提供します。  これは、プロトコルからのすべての必要なメソッドを持つインターフェイスです。  これにより、開発者が単なるインターフェイスを実装するプロトコルを実装します。  ランタイムは、プロトコルを採用すると、型を自動的に登録されます。

インターフェイスがのみ、必要なメソッドを一覧表示し、省略可能なメソッドは公開ことに注意してください。  つまり、プロトコルを採用するクラス全体の型チェック、必要なメソッドが表示されますが、(手動でエクスポート属性の使用と、署名と一致する)、弱い型指定に頼る必要がメソッドでは省略可能なプロトコルです。

プロトコルを使用する API を使用すると便利なするためにをバインド ツールも生成されますをすべての省略可能なメソッドを公開する拡張メソッド クラス。  これは、API を使用している限りができるプロトコルをすべてのメソッドを持つものとして扱うことを意味します。

API でプロトコルの定義を使用する場合は、API の定義で空のスケルトンのインターフェイスを記述する必要があります。  API で、MyProtocol を使用する場合は、これを行う必要があります。

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

#### <a name="adopting-protocol-generated-interfaces"></a>プロトコルが生成したインターフェイスを採用します。

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

<a name="Binding_Class_Extensions" />

### <a name="binding-class-extensions"></a>クラスのバインディング拡張

<!--In Objective-C it is possible to extend classes with new methods,
similar in spirit to C#'s extension methods. When one of these methods
is present, you can use the `[Target]` attribute to flag the first
parameter of a method as being the receiver of the Objective-C
message.

For example, in Xamarin.iOS we bound the extension methods that are defined on
`NSString` when `UIKit` is imported as methods in the `UIView`, like this:

```csharp
[BaseType (typeof (UIResponder))]
interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

-->

Objective c には、# の拡張メソッドの基本のような新しいメソッドは、クラスを拡張することができます。 これらのメソッドのいずれかが存在する場合を使用できます、 `BaseType` OBJECTIVE-C メッセージの受信者としてメソッドにフラグを設定する属性。

たとえば、Xamarin.iOS でバインドされた拡張メソッドで定義されている`NSString`とき`UIKit`内のメソッドとしてインポートされます、 `NSStringDrawingExtensions`、次のように。

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

 <a name="Binding_Objective-C_Argument_Lists" />

### <a name="binding-objective-c-argument-lists"></a>Objective C の引数リストのバインディング

Objective C の可変個引数をサポートしているでよりけり Gris で説明されている、次の手法を使用する[この投稿](http://forums.monotouch.net/yaf_postst311_SOLVED-Binding-ObjectiveC-Argument-Lists.aspx)です。

Objective C メッセージは、次のようになります。

```csharp
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

次のように署名を作成する c# からこのメソッドを呼び出す。

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

これは、internal として、ユーザーから、上記の API を非表示にするが、公開することで、ライブラリにメソッドを宣言します。 次のようにメソッドを記述できます。

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

### <a name="binding-fields"></a>フィールドのバインド

ライブラリの中で宣言されたパブリック フィールドにアクセスする場合があります。

通常これらのフィールドには、参照する必要がある文字列または整数の値が含まれます。 特定の通知を表す文字列とはディクショナリ内のキーとしてよく使用されます。

バインドするフィールド、インターフェイス定義ファイルにプロパティを追加しを使用してプロパティを装飾、 [[Field]](~/cross-platform/macios/binding/binding-types-reference.md)属性。 この属性は 1 つのパラメーターを受け取ります。 参照する記号の C の名前。 例えば:

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

派生していないの静的クラスでさまざまなフィールドをラップする場合は、 `NSObject`、使用することができます、`[Static]`次のように、クラスの属性。

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

上記が生成されます、`LonelyClass`から派生していませんが`NSObject`へのバインドを含めると、 `NSSomeEventNotification` 
 `NSString`として公開されている、`NSString`です。

`[Field]`属性は、次のデータ型に適用できます。

-  `NSString` 参照 (読み取り専用プロパティのみ)
-  `NSArray` 参照 (読み取り専用プロパティのみ)
-  32 ビット整数値 (`System.Int32`)
-  64 ビット整数値 (`System.Int64`)
-  32 ビットの浮動小数点値 (`System.Single`)
-  64 ビットの浮動小数点値 (`System.Double`)
-  `System.Drawing.SizeF`
-  `CGSize`

だけでなく、ネイティブのフィールド名のフィールドの場所、ライブラリ名を渡すことによって、ライブラリの名前を指定できます。

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

### <a name="binding-enums"></a>列挙型のバインド

追加することができます`enum`バインディングに直接ファイルをしやすく使用 API 定義の内部が別のソース ファイル (つまり、バインドと完成したプロジェクトの両方でコンパイルする必要があります) を使用します。

例:

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

置換する独自の列挙型を作成することも`NSString`定数。 ここでは、ジェネレーターが**自動的に**の列挙型値と NSString 定数を変換するメソッドを作成します。

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

上記の例では、装飾することが`void Perform (NSString mode);`で、`[Internal]`属性。 これは**を非表示に**バインドのコンシューマーから定数ベースの API です。

ただしこれは制限としてより良い API 代替では、型をサブクラス化、`[Wrap]`属性。 生成されたこれらのメソッドではありません`virtual`、つまり、won ' t になる可能性がありますかを変更することが、適切な選択をします。

代わりに、元のマークが`NSString`-ベース、定義として`[Protected]`です。 サブクラス化するために、必要な場合、これにより、wrap'ed バージョンはまだ作業とし、オーバーライドされたメソッドを呼び出します。

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>NSValue、NSNumber、および NSString を向上させる型にバインドします。

[[BindAs]](~/cross-platform/macios/binding/binding-types-reference.md)属性では、バインディング`NSNumber`、`NSValue`と`NSString`(列挙) より正確な c# の型にします。 属性を作成も優れたより正確に使用できる、ネイティブの API 経由での .NET API です。

装飾できるは、(戻り値) のメソッドやパラメーターを持つプロパティ[[BindAs]](~/cross-platform/macios/binding/binding-types-reference.md)です。 唯一の制限は、メンバー**いない必要があります**内にある、`[Protocol]`または`[Model]`インターフェイスです。

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

[[BindAs]](~/cross-platform/macios/binding/binding-types-reference.md)もの配列をサポート`NSNumber``NSValue`と`NSString`(列挙)。

例えば:

```csharp
[BindAs (typeof (CAScroll []))]
[Export ("supportedScrollModes")]
NSString [] SupportedScrollModes { get; set; }
```

出力します。

```csharp
[Export ("supportedScrollModes")]
CAScroll [] SupportedScrollModes { get; set; }
```

`CAScroll` `NSString`列挙型、バックアップが、右側をフェッチお`NSString`値し、型変換を処理します。

参照してください[[BindAs] ドキュメント](~/cross-platform/macios/binding/binding-types-reference.md)を表示するには、換算の種類をサポートします。

 <a name="Binding_Notifications" />

### <a name="binding-notifications"></a>通知のバインド

通知に送信されるメッセージ、`NSNotificationCenter.DefaultCenter`メカニズムとして、別のアプリケーションへの 1 つの部分からメッセージをブロードキャストするために使用します。 開発者は、通常を使用して通知をサブスクライブ、 [NSNotificationCenter](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/)の[AddObserver](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/M/AddObserver/)メソッドです。 アプリケーションは、通知センターにメッセージをポストするとき、に格納されているペイロードを格納、通常、 [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/)ディクショナリ。 このディクショナリが厳密に型指定され、ユーザーが通常どのキーがディクショナリとディクショナリに格納できる値の型に使用可能なドキュメントを読み取る必要がありますと間違いやすくでは、外の情報を取得します。 場合によってキーが存在するがもブール値として使用されます。

Xamarin.iOS バインディング ジェネレーターは、通知をバインドする開発者向けのサポートを提供します。 設定するには、 [[通知]](~/cross-platform/macios/binding/binding-types-reference.md)もされているプロパティの属性でタグ付け、 [[Field]](~/cross-platform/macios/binding/binding-types-reference.md) (できますパブリックまたはプライベート) プロパティです。

なし、運ぶ通知の引数を指定せず、この属性を使用できますかを指定することができます、 `System.Type` API の定義で別のインターフェイスを通常"EventArgs"で終わる名前で参照します。 ジェネレーターは、インターフェイスに変換クラスをサブクラス化する`EventArgs`すべてのプロパティが一覧表示が含まれます。 `[Export]`属性は、値がフェッチ OBJECTIVE-C ディクショナリの検索に使用するキーの名前を一覧表示を EventArgs クラスで使用する必要があります。

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
   }
}
```

ポストされた通知に、コードのユーザーが簡単にサブスクライブできますし、 [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/)次のようにコードを使用しています。

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

返された値`ObserveDidStart`を簡単に次のように、通知の受信を停止するために使用できます。

```csharp
token.Dispose ();
```

呼び出すか、または[NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject/)トークンを渡します。 パラメーターが、通知に含まれる場合は、ヘルパーを指定する必要があります`EventArgs`インターフェイスは、次のようにします。

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

上記が生成されます、`MyScreenChangedEventArgs`クラス、`ScreenX`と`ScreenY`からデータをフェッチするプロパティ、 [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) "ScreenXKey"と"ScreenYKey"キーを使用してディクショナリそれぞれの適切な変換を適用します。 `[ProbePresence]`属性はプローブ、キーが設定されている場合に、ジェネレーターの使用、`UserInfo`値を抽出しようとしています。 代わりにします。 これは、キーの存在が (通常はブール値) の値の場合に使用されます。

次のようにコードを記述できます。

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

 <a name="Binding_Categories" />

### <a name="binding-categories"></a>バインディングのカテゴリ

カテゴリは、メソッドとクラスで使用可能なプロパティのセットを拡張するために使用 OBJECTIVE-C メカニズムです。   実際には、それらを使用するか、基底クラスの機能を拡張する (たとえば`NSObject`) で特定のフレームワークがリンクされている場合 (たとえば`UIKit`)、使用できますが、新しいフレームワークがでリンクされている場合にのみ、そのメソッドを作成します。   その他のいくつかの場合、機能によって、クラスの機能を整理する使用されます。   これらは、c# 拡張メソッドを基本に似ています。これは、カテゴリがでどのように目標 c:

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

上の例場合で検出されたインスタンスの拡張ライブラリ`UIView`メソッドを使用して`makeBackgroundRed`です。

これらをバインドするに使用することができます、`[Category]`インターフェイス定義の属性です。  ときに、カテゴリを使用して属性の意味、`[BaseType]`から基本クラスを拡張する、拡張する型であることを指定するために使用されている属性を変更します。

次に示す方法、`UIView`拡張機能がバインドされているし、c# 拡張メソッドに変換します。

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

上記の作成、`MyUIViewExtension`を格納するクラス、`MakeBackgroundRed`拡張メソッド。  つまり、呼び出すことができるようになりました"MakeBackgroundRed"のいずれかの`UIView`サブクラスは目標 c が表示される同じ機能を提供するので、 その他のいくつかの場合、カテゴリはシステム クラス拡張はなく、機能、純粋な装飾の目的での整理に使用されます。  以下に例を示します。

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

使用できますが、`Category`属性もこの装飾スタイルの宣言するには、追加することがも同様に、クラス定義の一部をします。  同じこれらの両方の達成は。

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

### <a name="binding-blocks"></a>バインディングのブロック

ブロックは、目標 C になります。 c# の匿名メソッドと同等の機能を Apple によって導入された新しいコンス トラクターです。 たとえば、`NSSet`クラスでは、このメソッドを公開します。

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

上記の説明と呼ばれるメソッドを宣言する"*enumerateObjectsUsingBlock:*"という 1 つの引数を取る*ブロック*です。 このブロックは、現在の環境 ("this"ポインターをローカル変数とパラメーターへのアクセス) をキャプチャするためのサポートがある点で、c# の匿名メソッドに似ています。 上記の方法で`NSSet`を 2 つのパラメーターを使用してブロックを呼び出す、 `NSObject` ("id obj"の部分) とブール値へのポインター (、"BOOL * 停止") 部分です。

このような API を btouch をバインドするには、ように、c# を委任し、次のように、API エントリ ポイントから参照最初ブロック型のシグネチャを宣言する必要。

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

今すぐ、コードは c# から、関数を呼び出すことができます。

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

バインディング ジェネレーターは、非同期メソッドにメソッドの特定のクラスを有効にできます (タスクまたはタスクを返すメソッド&lt;T&gt;)。

使用することができます、 `[Async]` void を返すし、最後の引数は、コールバック メソッドの属性です。  バインディング ジェネレーターがサフィックスを持つメソッドのバージョンを生成はこれをメソッドに適用すると、ときに`Async`です。  コールバックがパラメーターをとらない場合、戻り値になります、 `Task`、コールバックがパラメーターを受け取る場合、結果になります、`Task<T>`です。  設定する必要があります、コールバックが複数のパラメーターを受け取る場合、`ResultType`または`ResultTypeName`に必要なすべてのプロパティを保持する、生成された型の名前を指定します。

例:

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

上記のコードが両方、LoadFile メソッドを生成するだけでなく。

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types" />

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>弱い NSDictionary パラメーターの厳密な型の表示

Objective C API でさまざまな場所にパラメーターが渡されると厳密に型指定`NSDictionary`で特定のキーと値がこれらの Api はエラー (無効なキーを渡すし、警告が表示されないことができます以外の場合は、無効な値を渡すし、警告が表示されないことができます) が発生しやすい、苛立つ使用可能なキーの名前と値を照合するドキュメントを複数のトリップが必要なのでを使用します。

ソリューションは、API およびバック グラウンドで厳密に型指定のバージョンを提供する厳密に型指定されたバージョン、さまざまな基になるキーと値のマッピングを提供します。

Objective C API が受け入れられた場合などは、 `NSDictionary` "XyzVolumeKey"は、キーの取得として記載されていて、`NSNumber`ボリューム値を 0.0 から 1.0 および、文字列を受け取る"XyzCaptionKey"が先にユーザーが、良い次のような API:

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

`Volume` Objective C の規則には、これらのディクショナリ値が設定される可能性がありますいないシナリオがあるために、値を持つことは不要とプロパティが null 許容の浮動小数点数として定義されています。

これを行うには、いくつかの作業を行う必要があります。

* 厳密に型指定されたクラスをサブクラス化を作成[DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/)し、各プロパティのさまざまな get アクセス操作子および set アクセス操作子を提供します。
* 受け取るメソッド オーバー ロードを宣言`NSDictionary`新しい厳密に型指定されたバージョンを実行します。

厳密型クラスをするか、手動で作成またはジェネレーターを使用するための作業を行うことができます。  まず、何が起こってを理解するように手動で行う方法、および自動のアプローチをについて説明します。

このサポート ファイルを作成する必要があります、API のコントラクトになりません。  これは、書き込み、XyzOptions クラスを作成する必要があります。

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

低水準の API の上に、高度な API を表示するラッパー メソッドを指定する必要があります。

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

場合は、API は、上書きする必要はありません、安全に非表示にできます NSDictionary ベースの API を使用して、[内部](~/cross-platform/macios/binding/binding-types-reference.md)属性。

ご覧のように、使用して、`[Wrap]`新しい API エントリ ポイントを表示する属性を厳密に型指定の XyzOptions クラスを使用して画面します。
ラッパー メソッドは null を渡すこともできます。

ここが記述されていないことの 1 つは、where、`XyzOptionsKeys`元の値。  通常は、API サーフェス XyzOptionsKeys と同様に、静的クラスで次のように、キー、グループは。

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

これらの厳密に型指定されたディクショナリを作成するための自動サポートを見てみましょう。  十分なボイラー プレート、これを回避でき、外部ファイルを使用する代わりに、API コントラクト内で直接、ディクショナリを定義することができます。

厳密に型指定されたディクショナリを作成する、API でインターフェイスを導入しで装飾、 [StrongDictionary](~/cross-platform/macios/binding/binding-types-reference.md)属性。  こうこと、ジェネレーターから派生は、インターフェイスと同じ名前のクラスを作成する必要があります`DictionaryContainer`を厳密に型指定されたアクセサーを提供します。

`StrongDictionary`属性が 1 つのパラメーターであり、これは、ディクショナリのキーを格納する静的クラスの名前。  インターフェイスの各プロパティには、厳密に型指定されたアクセサーになります。  既定では、コードは、プロパティの名前を使用、静的クラスにサフィックス「キー」アクセサーを作成します。

つまり、厳密に型指定されたアクセサーの作成不要になったが必要に外部のファイルも get アクセス操作子および set アクセス操作子のすべてのプロパティを手動で作成することも、キーを手動で探す必要自分でします。

これは、全体のバインディングはようになります。

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

内で参照する必要がある場合に、`XyzOption`メンバー、別のフィールド (つまり名ではなく、サフィックスを持つプロパティの`Key`)、装飾できるは、そのプロパティを`Export`を使用する名前の属性です。

 <a name="Type_mappings" />

## <a name="type-mappings"></a>型のマッピング

このセクションでは、c# の型を Objective C 型をマップする方法について説明します。

<a name="Simple_Types" />

### <a name="simple-types"></a>単純型

次の表は、OBJECTIVE-C と CocoaTouch world から Xamarin.iOS 世界への型をマップする方法を示しています。

|Objective C 型名|Xamarin.iOS Unified API type|
|---|---|
|`BOOL`, `GLboolean`|`bool`|
|`NSInteger`|`nint`|
|`NSUInteger`|`nuint`|
|`CFTimeInterval` / `NSTimeInterval`|`double`|
|`NSString` ([バインドの詳細`NSString` ](~/ios/internals/api-design/nsstring.md))|`string`|
|`char *`|`string` (も参照してください: [PlainString 属性](~/cross-platform/macios/binding/binding-types-reference.md#plainstring))|
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

Xamarin.iOS ランタイムが自動的に処理 (C#) への配列に変換する`NSArrays`し、これを行う変換を元に戻すためたとえば虚数部の OBJECTIVE-C メソッドを返します、`NSArray`の`UIViews`:

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

概念では、これにより、推測し、または配列に含まれるオブジェクトの実際の型を調べるには、ドキュメントを検索するユーザーを強制することがなく、実際の型との適切なコード補完機能を提供するための IDE と C# の場合、厳密に型指定された配列を使用します。

使用することがないを追跡できますを配列に含まれている実際の最派生型の場合、`NSObject []`の戻り値にします。

 <a name="Selectors" />

### <a name="selectors"></a>セレクター

セレクターは、OBJECTIVE-C API で、特殊な"SEL"として表示されます。 型をマップするセレクターをバインドするときに`ObjCRuntime.Selector`します。  通常のセレクターは、ターゲット オブジェクトで呼び出すには、オブジェクト、ターゲット オブジェクトとセレクターの両方の API で公開されます。 基本的にこれらの両方を提供する c# デリゲートに対応しています: オブジェクトでメソッドを呼び出すだけでなく、呼び出されるメソッドをカプセル化するものです。

これは、バインディングはのようになります。

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

これとは、メソッドは通常で使用する方法、アプリケーション。

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

C# の開発者に、バインドをより良いさせるためには、通常を提供しますを受け取るメソッド、`NSAction`パラメーターの代わりに使用するには、c# のデリゲートとラムダ、`Target+Selector`です。 これを行うには通常非表示にする"SetTarget"メソッド「内部」属性を持つフラグを設定し、次のように、新しいヘルパー メソッドを公開する、します。

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

受け取るメソッドにバインドするときに、`NSString`文字列型 (C#) の両方で返すこと型およびパラメーターを置き換えることができます。

場合のみ使用するときに、`NSString`直接文字列をトークンとして使用する場合は、します。 文字列の詳細については、 `NSString`、読み取り、 [NSString で API の設計](~/ios/internals/api-design/nsstring.md)ドキュメント。

まれなケースでは、API が C のような文字列を公開可能性があります (`char *`) Objective C 文字列ではなく (`NSString *`)。 ような場合、パラメーターの注釈を表示できます、 [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring)属性。

 <a name="outref_parameters" />

### <a name="outref-parameters"></a>out または ref パラメーター

いくつかの Api は、参照によってパラメーターを渡すことも、パラメーターの値を返します。

次のように、署名が通常は。

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

最初の例では、エラー コードへのポインターを返す共通 Objective C の表現形式、`NSError`ポインターが渡され、関数が戻るときに値が設定されます。   2 番目のメソッドは、OBJECTIVE-C メソッドがオブジェクトの取得、およびその内容を変更方法を示します。   純粋な出力値ではなく、参照渡しになります。

バインディングは、次のようになります。

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

 <a name="Memory_management_attributes" />

### <a name="memory-management-attributes"></a>メモリ管理属性

使用すると、`[Export]`属性とするが、呼び出されたメソッドで保持されるデータを渡すことは、たとえば 2 番目のパラメーターとして渡すによって引数のセマンティクスを指定できます。

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

上記の値「保持」のセマンティクスを持つものとしてはフラグです。 使用可能なセマンティクスは次のとおりです。

-  割り当てます。
-  コピー:
-  保持されます。

 <a name="Style_Guidelines" />

### <a name="style-guidelines"></a>スタイルのガイドライン

 <a name="Using_[Internal]" />

#### <a name="using-internal"></a>[内部] を使用します。

使用することができます、 [[内部]](~/cross-platform/macios/binding/binding-types-reference.md)パブリック API からメソッドを非表示にする属性。 これは、公開されている API が低すぎる、このメソッドに基づく別のファイルに高レベルの実装を提供する場合で実行する可能性があります。

バインディング ジェネレーターの制限事項が発生する、たとえば、一部の高度なシナリオはバインドされていない型を公開する可能性があります、独自の方法でバインドして、独自の方法で自分でそれらの型をラップするときに、これを行うもこともできます。

 <a name="Event_Handlers_and_Callbacks" />

## <a name="event-handlers-and-callbacks"></a>イベント ハンドラーとコールバック

一般に、OBJECTIVE-C クラスでは、通知をブロードキャストまたはデリゲート クラス (OBJECTIVE-C デリゲート) でメッセージを送信して情報を要求します。

このモデルでは、完全にサポートされている、Xamarin.iOS 側に表示されることがあります厄介で面倒です。 Xamarin.iOS は、c# イベント パターンとこれらの状況で使用できるクラスのメソッドのコールバック システムを公開します。 これにより、コードを実行する次のようにします。

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

バインディング ジェネレーターは c# パターン Objective C のパターンにマップする必要な入力量を削減できます。

Xamarin.iOS 1.4 で始まることは、ジェネレーター特定 OBJECTIVE-C デリゲートのバインドを作成し、c# イベントとホスト型のプロパティとして、デリゲートを公開するように指示することになります。

この処理に関連する 2 つのクラスは、ホスト クラスが現在のイベントを出力し、送信に 1 つ、`Delegate`または`WeakDelegate`と実際のデリゲート クラスです。

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

-  ホスト クラスを追加、`[BaseType]`宣言、デリゲートと c# 名として機能する型が公開します。 前の例では"typeof (MyClassDelegate)"と"WeakDelegate"それぞれします。
-  デリゲート クラスで、3 つ以上のパラメーターには、各メソッドには、自動的に生成された EventArgs クラスを使用する型を指定する必要があります。

バインディング ジェネレーター、折り返しの 1 つのイベントの保存先のみに限定されません、可能であればことを複数のメッセージを出力するには、いくつか Objective C クラスを委任するため、この設定をサポートするために配列を指定する必要があります。 ほとんどの設定では、その必要はありませんが、ジェネレーターは、このような場合に対応しています。

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

`EventArgs`の名前を指定に使用される、`EventArgs`クラスを生成します。 シグネチャごとに 1 つを使用する必要があります (この例では、`EventArgs`型 nint の「に」プロパティを含む)。

上記の定義を含む、ジェネレーターに生成された MyClass で次のイベントが生成されます。

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

したがってこのようなコードを今すぐを使用できます。

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

コールバックは、イベントの呼び出しと同じように、その違いは複数の潜在的なサブスクライバーではなく (たとえば、複数のメソッドから"Clicked"イベントまたはイベントを「のダウンロードが完了しました」にフックできます) のコールバックでは、単一のサブスクライバーを持つことができますのみです。

プロセスは同じです、唯一の違いは、生成される EventArgs クラスの名前を公開するのではなく、EventArgs 実際を使用する名前を結果として得られる c# デリゲート。

デリゲート クラスのメソッドが値を返す場合バインディング ジェネレーターはマップこのイベントではなく、親クラスのデリゲート メソッドにします。 このような場合は、返される、メソッドによって、ユーザーがフックいない場合、デリゲートに既定値を指定する必要があります。 これを行うを使用して、`[DefaultValue]`または`[DefaultValueFromArgument]`属性。

DefaultValue はハードコード戻り値の場合は、中に`[DefaultValueFromArgument]`返される入力引数を指定するために使用します。

 <a name="Enumerations_and_Base_Types" />

## <a name="enumerations-and-base-types"></a>列挙型および基本型

列挙型または btouch インターフェイス定義システムによって直接サポートされていない基本データ型を参照することもできます。 これを行うには、別のファイルに、列挙体と主要な型を配置して、btouch に提供する余分なファイルのいずれかの一部として含まれます。

 <a name="Linking_the_Dependencies" />

## <a name="linking-the-dependencies"></a>依存関係のリンク

アプリケーションの一部ではない Api をバインドする場合、これらのライブラリに対して、実行可能ファイルがリンクされていることを確認する必要があります。

Xamarin.iOS、ライブラリにリンクする方法を通知する必要があることができますで行う必要がいずれかを使用して、新しいライブラリとリンクする方法を指定するいくつかの追加のビルド引数を持つ mtouch コマンドを呼び出す、ビルド構成を変更することにより、"-gcc_flags"の後に、オプション引用符で囲まれた文字列では、次のように、プログラムに必要なすべての余分なライブラリを含みます。

```csharp
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

上記の例ではリンク`libMyLibrary.a`、`libSystemLibrary.dylib`と`CFNetwork`フレームワーク ライブラリ、最終的な実行可能ファイルにします。

アセンブリ レベルの活用、または`LinkWithAttribute`、契約ファイルに埋め込むことができます (など`AssemblyInfo.cs`)。 使用すると、 `LinkWithAttribute`、ためするには、バインド、アプリケーションを使用して、ネイティブ ライブラリ埋め込む時に使用できる、ネイティブ ライブラリが存在する必要があります。 例えば:

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

だろう、なぜ必要がある"-force_load"コマンド、および理由をあるメッセージが - ObjC フラグが、内のコードをコンパイルする必要があります (コンパイラ/リンカーされないコードの削除は、それを切り離します) カテゴリをサポートするために必要なメタデータは保持されません。Xamarin.iOS の実行時にします。

 <a name="Assisted_References" />

## <a name="assisted-references"></a>手動の参照

アクション シートとアラートのボックスのように一時的なオブジェクトがいくつかは開発者向けの追跡を厄介で面倒とバインディング ジェネレーターがここでは少し役立つことができます。

メッセージを表示し、「完了」のイベント、この処理の従来の方法は、生成されるクラスがあった場合の例は次のようになります。

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

上記のシナリオでは、開発者は自分自身して、いずれかの例外オブジェクトへの参照を保持またはアクティブには自分のボックスのリファレンスをオフにする必要があります。  バインド コード ジェネレーターをサポートしますがの参照の追跡になり、チェック ボックスをオフに特殊なメソッドが呼び出されたときに、上記のコードは。

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

どのローカル変数と共に動作して、死んだ状態になったときに参照をクリアする必要がないのインスタンスに変数を保持する必要はなくなりましたに注意してください。

活用するためにこれをクラスは、イベント プロパティが設定されて、`[BaseType]`宣言とも、`KeepUntilRef`変数オブジェクトには、次のように、作業が完了したときに呼び出されるメソッドの名前に設定します。

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

 <a name="Inheriting_Protocols" />

## <a name="inheriting-protocols"></a>プロトコルを継承します。

Xamarin.iOS v3.2 以降がサポートされていますが付いているプロトコルからの継承、`[Model]`プロパティです。 これは、特定の API パターン内などに便利です`MapKit`場所、`MKOverlay`プロトコル、継承、`MKAnnotation`プロトコル、およびから継承するクラスの数によって採用された`NSObject`です。

すべての実装へのプロトコルのコピーを必要お従来ですが、このような場合のようになりましたがあっても、`MKShape`クラスから継承する、`MKOverlay`プロトコルとそれが必要なすべてのメソッドを自動的に生成します。

## <a name="related-links"></a>関連リンク

- [バインディングのサンプル](https://developer.xamarin.com/samples/BindingSample/)
