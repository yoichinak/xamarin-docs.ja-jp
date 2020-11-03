---
title: バインディングの目的 C ライブラリ
description: このドキュメントでは、C 言語のコードに C# バインドを作成する方法の概要を説明します。イベント、メソッド、カスタムコントロールなどをバインドする方法について説明します。
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
author: davidortinau
ms.author: daortin
ms.date: 03/06/2018
ms.openlocfilehash: d7f66d9bda014337ae6108ab42158faa856632bb
ms.sourcegitcommit: 4f0223cf13e14d35c52fa72a026b1c7696bf8929
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2020
ms.locfileid: "93278339"
---
# <a name="binding-objective-c-libraries"></a>バインディングの目的 C ライブラリ

Xamarin. iOS または Xamarin. Mac を使用する場合、サードパーティの目的 C ライブラリを使用することが必要になる場合があります。 このような状況では、Xamarin バインドプロジェクトを使用して、ネイティブの目的 C ライブラリへの C# バインドを作成できます。 このプロジェクトでは、iOS および Mac Api を C# に持ち込むために使用するのと同じツールを使用します。

このドキュメントでは、C Api のみをバインドしている場合に、C Api をバインドする方法について説明します。そのためには、 [P/Invoke フレームワーク](https://www.mono-project.com/docs/advanced/pinvoke/)の標準の .net 機構を使用する必要があります。
C ライブラリを静的にリンクする方法の詳細については、「 [ネイティブライブラリのリンク](~/ios/platform/native-interop.md) 」ページを参照してください。

関連する [バインディングの種類のリファレンスガイド](~/cross-platform/macios/binding/binding-types-reference.md)を参照してください。
また、内部で何が起こるかについて詳しく知りたい場合は、「 [バインドの概要」](~/cross-platform/macios/binding/overview.md) ページをご覧ください。

バインドは、iOS と Mac の両方のライブラリ用に構築できます。
このページでは、iOS バインドでの作業方法について説明しますが、Mac バインドはよく似ています。

**IOS 用のサンプルコード**

[IOS のバインドサンプル](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample)プロジェクトを使用して、バインドを試すことができます。

<a name="Getting_Started"></a>

## <a name="getting-started"></a>作業の開始

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

バインディングを作成する最も簡単な方法は、Xamarin の iOS バインドプロジェクトを作成することです。
これを行うには、[プロジェクトの種類]、[ **iOS > ライブラリ > バインドライブラリ** ] の順に選択し Visual Studio for Mac します。

[![これを行うには、[プロジェクトの種類]、[iOS ライブラリバインドライブラリ] の順に選択し Visual Studio for Mac します。](objective-c-libraries-images/00-sml.png)](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

バインディングを作成する最も簡単な方法は、Xamarin の iOS バインドプロジェクトを作成することです。
これを行うには、Windows の Visual Studio で、プロジェクトの種類、 **visual C# > iOS > Binding Library (ios)** の順に選択します。

[![iOS バインドライブラリ iOS](objective-c-libraries-images/00vs-sml.png)](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> 注: **Xamarin. Mac** のバインドプロジェクトは、Visual Studio for Mac でのみサポートされています。

-----

生成されたプロジェクトには、編集できる小さなテンプレートが含まれており、との2つのファイルが含まれてい `ApiDefinition.cs` `StructsAndEnums.cs` ます。

は、 `ApiDefinition.cs` API コントラクトを定義する場所です。これは、基になる目標 C API が C# にどのように射影されるかを記述するファイルです。 このファイルの構文と内容は、このドキュメントの説明の主要なトピックであり、その内容は C# のインターフェイスと C# のデリゲート宣言に限定されています。 `StructsAndEnums.cs`ファイルは、インターフェイスおよびデリゲートで必要とされる定義を入力するファイルです。 これには、コードで使用される可能性がある列挙値と構造体が含まれます。

<a name="Binding_an_API"></a>

## <a name="binding-an-api"></a>API のバインド

包括的なバインドを行うには、目的 C の API 定義を理解し、.NET Framework 設計ガイドラインについて理解する必要があります。

ライブラリをバインドするには、通常、API 定義ファイルから開始します。 API 定義ファイルは c# のソースファイルにすぎません。 c# のインターフェイスには、バインドを駆動するためのいくつかの属性で注釈が付けられています。  このファイルは、C# と目標 C の間のコントラクトを定義するものです。

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

上のサンプルでは、基本型から派生するというクラスを定義して `Cocos2D.Camera` `NSObject` います (この型はから取得 `Foundation.NSObject` します)。また、静的プロパティ ( `ZEye` )、引数を受け取らない2つのメソッド、3つの引数を受け取るメソッドを定義します。

API ファイルの形式と使用できる属性の詳細については、後述の「 [api 定義ファイル](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) 」を参照してください。

完全なバインドを作成するには、通常、次の4つのコンポーネントを処理します。

- ( `ApiDefinition.cs` テンプレート内の) API 定義ファイル。
- 省略可能: API 定義ファイル (テンプレート内) に必要な列挙型、型、構造体 `StructsAndEnums.cs` 。
- 省略可能: 生成されたバインディングを拡張する可能性のある追加のソース。または、c# に適した API (プロジェクトに追加するすべての C# ファイル) を提供します。
- バインドするネイティブライブラリ。

このグラフは、次のファイル間の関係を示しています。

 [![このグラフは、ファイル間の関係を示します。](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png)](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

API 定義ファイルには、(インターフェイスが含むことのできるすべてのメンバーを含む) 名前空間とインターフェイスの定義のみが含まれます。クラス、列挙体、デリゲート、または構造体を含めることはできません。 API 定義ファイルは、API の生成に使用されるコントラクトにすぎません。

列挙やサポートクラスのように必要な追加のコードは、別のファイルでホストする必要があります。上記の例では、"CameraMode" は CS ファイルに存在しない列挙値です。たとえば、次のように別のファイルでホストする必要があり `StructsAndEnums.cs` ます。

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

`APIDefinition.cs`ファイルはクラスと組み合わされ、 `StructsAndEnum` ライブラリのコアバインドを生成するために使用されます。 生成されたライブラリをそのまま使用できますが、通常は、結果として得られるライブラリを調整して、ユーザーの利点を得るために C# の機能を追加する必要があります。 例としては、メソッドの実装 `ToString()` 、C# インデクサーの提供、ネイティブ型との間での暗黙的な変換の追加、一部のメソッドの厳密に型指定されたバージョンの提供などがあります。 これらの機能強化は、追加の C# ファイルに格納されています。 C# ファイルをプロジェクトに追加するだけで、このビルドプロセスに含まれるようになります。

これは、ファイルにコードを実装する方法を示して `Extra.cs` います。 部分クラスを使用することに注意してください。これにより、とのコアバインドの組み合わせから生成される部分クラスが拡張され `ApiDefinition.cs` `StructsAndEnums.cs` ます。

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

ライブラリをビルドすると、ネイティブバインドが生成されます。

このバインディングを完了するには、ネイティブライブラリをプロジェクトに追加する必要があります。  これを行うには、ソリューションエクスプローラーでネイティブライブラリを Finder からプロジェクトにドラッグアンドドロップするか、プロジェクトを右クリックして [ **追加** ] [ファイルの追加] の順に選択し、ネイティブライブラリを  >  **Add Files** 選択します。
ネイティブライブラリは、"lib" という語で始まり、拡張子 ". a" で終わります。 これを行うと、Visual Studio for Mac によって2つのファイルが追加されます。このファイルには、ネイティブライブラリに含まれる内容に関する情報を含む、自動的に設定された C# ファイルが追加されます。

 [![ネイティブライブラリは慣例により、"lib" から始まり、拡張子で終わります。](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png)](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

ファイルの内容に `libMagicChord.linkwith.cs` は、このライブラリの使用方法に関する情報が含まれており、このバイナリを結果の DLL ファイルにパッケージ化するように IDE に指示します。

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

の使用方法に関する完全な詳細情報 [`[LinkWith]`](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) 
属性の説明については、「 [バインドの種類リファレンスガイド](~/cross-platform/macios/binding/binding-types-reference.md)」を参照してください。

プロジェクトをビルドすると、 `MagicChords.dll` バインドとネイティブライブラリの両方を含むファイルが作成されるようになります。 このプロジェクトまたは生成された DLL を他の開発者に配布して、独自に使用することができます。

場合によっては、いくつかの列挙値、デリゲート定義、またはその他の型が必要になることがあります。 API 定義ファイルには配置しないでください。これは単なるコントラクトであるためです。

<a name="The_API_definition_file"></a>

## <a name="the-api-definition-file"></a>API 定義ファイル

API 定義ファイルは、さまざまなインターフェイスで構成されています。 API 定義内のインターフェイスはクラス宣言に変換され、 [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) クラスの基底クラスを指定するために属性で装飾する必要があります。

コントラクト定義のインターフェイスではなく、クラスを使用していない理由があるかもしれません。 インターフェイスを選択しました。これは、API 定義ファイルにメソッド本体を指定したり、例外をスローしたり、意味のある値を返したりする必要があった本体を提供したりすることなく、メソッドのコントラクトを記述できるようにするためです。

しかし、ここでは、クラスを生成するためにインターフェイスをスケルトンとして使用しているため、コントラクトのさまざまな部分を属性で装飾してバインディングを駆動する必要がありました。

<a name="Binding_Methods"></a>

### <a name="binding-methods"></a>バインドメソッド

最も簡単なバインドは、メソッドをバインドすることです。 C# の名前付け規則を使用してインターフェイス内のメソッドを宣言し、メソッドを [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)
属性. [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)属性は、C# 名と、Xamarin. iOS ランタイムの C の名前をリンクします。 のパラメーター [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 
属性は、目的の C セレクターの名前です。 次に例をいくつか示します。

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

上のサンプルでは、インスタンスメソッドをバインドする方法を示しています。 静的メソッドをバインドするには、次のように属性を使用する必要があり [`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) ます。

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

これは、コントラクトがインターフェイスの一部であり、インターフェイスに静的な vs インスタンス宣言の概念がないために必要です。したがって、属性を再指定するには、もう一度必要となります。 バインディングから特定のメソッドを非表示にする場合は、属性を使用してメソッドを装飾でき [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) ます。

コマンドでは、 `btouch-native` 参照パラメーターが null でないかどうかのチェックが行われます。 特定のパラメーターに null 値を許可する場合は、 [`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)
次のように、パラメーターの属性です。

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

キーワードを使用して参照型をエクスポートする場合は、 [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 割り当てセマンティクスを指定することもできます。 これは、データが漏洩しないようにするために必要です。

<a name="Binding_Properties"></a>

### <a name="binding-properties"></a>バインディングのプロパティ

メソッドと同様に、目的の C プロパティはを使用してバインドされます。 [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)
属性を C# のプロパティに直接マップします。 メソッドと同様に、プロパティは、 [`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute)
および [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
アトリビュート.

内部のプロパティで属性を使用する場合は、実際には、 [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) getter と setter の2つのメソッドをバインドします。 エクスポートするために指定する名前は、"set" という語を前に付加することによって計算されます。 set を使用して、 **ベース** の最初の文字を大文字に変換し、セレクターが引数を受け取るよう **にします** 。 つまり、 `[Export ("label")]` プロパティに適用されるは、"label" メソッドと "setLabel:" という目的で実際にバインドされます。

場合によっては、目的の C プロパティが上記のパターンに従わず、名前が手動で上書きされることがあります。 そのような場合は、を使用して、バインディングの生成方法を制御できます。 [`[Bind]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAttribute) 
get アクセス操作子または setter の属性。次に例を示します。

```csharp
[Export ("menuVisible")]
bool MenuVisible { [Bind ("isMenuVisible")] get; set; }
```

次に、"isMenuVisible" と "setMenuVisible:" をバインドします。 必要に応じて、次の構文を使用してプロパティをバインドできます。

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

Get アクセス操作子と setter は、上のバインドとバインドではとして明示的に定義され `name` `setName` ます。

を使用した静的プロパティのサポートに加えて、次のように、を使用して [`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) スレッド静的プロパティを装飾することもでき [`[IsThreadStatic]`](~/cross-platform/macios/binding/binding-types-reference.md#IsThreadStaticAttribute) ます。

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

メソッドと同様に、一部のパラメーターにフラグを付けること [`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) ができます。 [`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)
プロパティには、null がプロパティの有効な値であることを示します。次に例を示します。

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

[`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)パラメーターは、setter で直接指定することもできます。

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>カスタムコントロールのバインドに関する注意点

カスタムコントロールのバインドを設定するときは、次の点に注意する必要があります。

1. **バインドプロパティは静的でなければなりません** 。プロパティのバインドを定義するときは、 [`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) 属性を使用する必要があります。
2. **プロパティ名は正確に一致する必要があり** ます。プロパティのバインドに使用される名前は、カスタムコントロールのプロパティの名前と正確に一致している必要があります。
3. **プロパティの型は、厳密に一致する必要があり** ます。プロパティのバインドに使用される変数の型は、カスタムコントロールのプロパティの型と正確に一致する必要があります。
4. プロパティの getter メソッドまたは setter メソッドに配置された **ブレークポイントとゲッター/setter** -ブレークポイントはヒットしません。
5. **コールバックを観察** する-カスタムコントロールのプロパティ値の変更を通知するには、監視コールバックを使用する必要があります。

上記のいずれかの警告が表示されない場合は、バインディングが実行時にサイレントに失敗する可能性があります。

<a name="MutablePattern"></a>

#### <a name="objective-c-mutable-pattern-and-properties"></a>目的-C の変更可能なパターンとプロパティ

目的 C のフレームワークでは、一部のクラスが変更可能なサブクラスで不変である場合の表現形式を使用します。 たとえば、 `NSString` は変更できないバージョンですが、 `NSMutableString` は変異させるサブクラスです。

これらのクラスでは、変更できない基底クラスに、getter を持つプロパティが含まれていますが、setter は含まれていません。 変更可能なバージョンで setter を導入するために使用します。 これは C# では実際には不可能であるため、この表現表現を C# で使用できる表現にマップする必要がありました。

これを C# にマップする方法は、基本クラスに getter と setter の両方を追加することですが、set アクセス操作子のフラグを [`[NotImplemented]`](~/cross-platform/macios/binding/binding-types-reference.md#NotImplementedAttribute)
属性.

次に、mutable サブクラスで、 [`[Override]`](~/cross-platform/macios/binding/binding-types-reference.md#OverrideAttribute) 
プロパティが親の動作を実際にオーバーライドしていることを確認するための属性。

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

<a name="Binding_Constructors"></a>

### <a name="binding-constructors"></a>バインディングコンストラクター

ツールは、 `btouch-native` クラス内に4つのコンストラクターを自動的に生成します。クラスには、次のものが生成され `Foo` ます。

- `Foo ()`: 既定のコンストラクター (目標 C の "init" コンストラクターにマップ)
- `Foo (NSCoder)`: NIB files の逆シリアル化中に使用されるコンストラクター (目的の C の "initWithCoder:" コンストラクターにマップされます)。
- `Foo (IntPtr handle)`: ハンドルベースの作成のコンストラクター。ランタイムがアンマネージオブジェクトからマネージオブジェクトを公開する必要がある場合に、ランタイムによって呼び出されます。
- `Foo (NSEmptyFlag)`: これは、二重初期化を防ぐために派生クラスによって使用されます。

定義するコンストラクターについては、インターフェイス定義内で次のシグネチャを使用して宣言する必要があります。これらは、値を返す必要があり、 `IntPtr` メソッドの名前はコンストラクターである必要があります。 たとえば、コンストラクターをバインドするに `initWithFrame:` は、次のように使用します。

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

<a name="Binding_Protocols"></a>

### <a name="binding-protocols"></a>プロトコルのバインド

API の設計ドキュメントで説明されているように、 [モデルとプロトコルの説明](~/ios/internals/api-design/index.md#models)に関するセクションでは、Xamarin. iOS は、目的の C プロトコルを、 [`[Model]`](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute)
属性. これは通常、目的の C デリゲートクラスを実装するときに使用されます。

通常のバインドされたクラスとデリゲートクラスの大きな違いは、デリゲートクラスには1つ以上のオプションのメソッドがある場合があるということです。

たとえば、クラスについて考えてみましょう `UIKit` `UIAccelerometerDelegate` 。これは、Xamarin でのバインド方法です。

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

これは、の定義では省略可能なメソッドであるため、 `UIAccelerometerDelegate` 他に行うことはできません。 ただし、プロトコルに必要なメソッドがあった場合は、 [`[Abstract]`](~/cross-platform/macios/binding/binding-types-reference.md#AbstractAttribute)
属性をメソッドに対して行います。 これにより、実装のユーザーがメソッドの本体を実際に提供するようになります。

一般に、プロトコルは、メッセージに応答するクラスで使用されます。 これは、通常、プロトコルのメソッドに応答するオブジェクトのインスタンスを "delegate" プロパティに割り当てることによって行われます。

Xamarin の慣例では、の任意のインスタンスを `NSObject` デリゲートに割り当てることができ、厳密に型指定されたバージョンを公開する、目的 C の疎結合スタイルの両方をサポートします。 このため、通常は、 `Delegate` 厳密に型指定されたプロパティと、厳密に型指定されていないプロパティの両方を提供し `WeakDelegate` ます。 通常、疎型のバージョンをにバインド [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) し、属性を使用して [`[Wrap]`](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) 厳密に型指定されたバージョンを提供します。

これは、クラスをバインドした方法を示してい `UIAccelerometer` ます。

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

<a name="iOS7ProtocolSupport"></a>

**Monotouch.dialog 7.0 の新方法**

Monotouch.dialog 7.0 以降では、新たに強化されたプロトコルバインド機能が導入されました。  この新しいサポートにより、特定のクラスで1つ以上のプロトコルを採用するために、目的の C の表現を簡単に使用できるようになります。

目的が C のすべてのプロトコル定義に対して、すべての `MyProtocol` `IMyProtocol` 必要なメソッドをプロトコルから一覧表示するインターフェイスと、すべてのオプションのメソッドを提供する拡張クラスを使用できるようになりました。  上記のを Xamarin Studio エディターの新しいサポートと組み合わせることで、開発者は、前の抽象モデルクラスの個別のサブクラスを使用しなくても、プロトコルメソッドを実装できます。

属性を含む定義で [`[Protocol]`](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) は、次の3つのサポートクラスが生成され、プロトコルの使用方法が大幅に向上します。

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

**クラス実装** は、の個々のメソッドをオーバーライドし、完全なタイプセーフを取得できる、完全な抽象クラスを提供します。  ただし、C# では複数の継承がサポートされていないため、異なる基底クラスが必要になる場合がありますが、インターフェイスを実装する必要があります。

生成された **インターフェイス定義** はにあります。  これは、プロトコルから必要なすべてのメソッドを含むインターフェイスです。  これにより、開発者は、インターフェイスを実装するだけで、プロトコルを実装することができます。  ランタイムは、プロトコルを採用するときに型を自動的に登録します。

インターフェイスには、必要なメソッドが一覧表示されるだけで、オプションのメソッドが公開されることに注意してください。  これは、プロトコルを採用するクラスが、必要なメソッドの完全な型チェックを取得することを意味しますが、弱い型指定を行う必要があります (手動で [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 
省略可能なプロトコルメソッドの属性と一致します。

プロトコルを使用する API を簡単に使用できるようにするために、バインディングツールは、すべての省略可能なメソッドを公開する拡張メソッドクラスも生成します。  これは、API を使用している限り、すべてのメソッドを含むプロトコルとして扱うことができることを意味します。

API でプロトコル定義を使用する場合は、API 定義にスケルトン空のインターフェイスを記述する必要があります。  API で MyProtocol を使用する場合は、次の手順を実行する必要があります。

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

#### <a name="adopting-protocol-generated-interfaces"></a>プロトコルによって生成されたインターフェイスの導入

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


<a name="Binding_Class_Extensions"></a>

### <a name="binding-class-extensions"></a>バインディングクラスの拡張

目的 C では、C# の拡張メソッドと同様に、新しいメソッドでクラスを拡張することができます。 これらのメソッドのいずれかが存在する場合は、 [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 
目的の C メッセージの受信者としてメソッドにフラグを付ける属性。

たとえば、Xamarin で `NSString` `UIKit` は、がのメソッドとしてインポートされるときに、に定義されている拡張メソッドをバインドしました。次に例を `NSStringDrawingExtensions` 示します。

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

<a name="Binding_Objective-C_Argument_Lists"></a>

### <a name="binding-objective-c-argument-lists"></a>バインディング目標-C 引数リスト

可変個引数は、引数をサポートします。 次に例を示します。

```objc
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

C# からこのメソッドを呼び出すには、次のようなシグネチャを作成します。

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

これにより、メソッドが internal として宣言され、ユーザーから上記の API が非表示になりますが、ライブラリに公開されます。 その後、次のようなメソッドを記述できます。

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

<a name="Binding_Fields"></a>

### <a name="binding-fields"></a>バインド (フィールドを)

場合によっては、ライブラリで宣言されているパブリックフィールドにアクセスします。

通常、これらのフィールドには、参照する必要がある文字列または整数値が含まれます。 一般的に、特定の通知を表す文字列として使用され、ディクショナリのキーとして使用されます。

フィールドをバインドするには、インターフェイス定義ファイルにプロパティを追加し、属性を使用してプロパティを装飾し [`[Field]`](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) ます。 この属性は、1つのパラメーター (参照するシンボルの C 名) を受け取ります。 次に例を示します。

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

から派生していない静的クラス内のさまざまなフィールドをラップする場合 `NSObject` は、 [`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute_Class) 
クラスの属性: 次のようになります。

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

上のは、 `LonelyClass` から派生せず、 `NSObject` `NSSomeEventNotification` 
 `NSString` として公開されるへのバインディングを格納 `NSString` するを生成します。

属性は、 [`[Field]`](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) 次のデータ型に適用できます。

- `NSString` 参照 (読み取り専用プロパティのみ)
- `NSArray` 参照 (読み取り専用プロパティのみ)
- 32ビット整数 ( `System.Int32` )
- 64ビット整数 ( `System.Int64` )
- 32ビット浮動小数点 ( `System.Single` )
- 64ビット浮動小数点 ( `System.Double` )
- `System.Drawing.SizeF`
- `CGSize`

ネイティブフィールド名に加えて、ライブラリ名を渡すことによって、フィールドが配置されているライブラリ名を指定できます。

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

静的にリンクしている場合は、バインド先のライブラリがないため、という名前を使用する必要があり `__Internal` ます。

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums"></a>

### <a name="binding-enums"></a>列挙型のバインド

`enum`バインドファイルにを直接追加することで、(バインディングと最終的なプロジェクトの両方でコンパイルする必要がある) 別のソースファイルを使用しなくても、API 定義内で簡単に使用できるようになります。

例:

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

また、定数を置換する独自の列挙型を作成することもでき `NSString` ます。 この場合、ジェネレーターは **自動的に** 、列挙値と nsstring 定数を変換するメソッドを作成します。

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

上の例では、属性を使用して修飾することができ `void Perform (NSString mode);` [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) ます。 これにより、バインドコンシューマーからの定数ベースの API が **非表示** になります。

ただし、これにより、API の代替手段として属性が使用されるため、型のサブクラスが制限され [`[Wrap]`](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) ます。 これらのメソッドは生成されません `virtual` 。つまり、オーバーライドすることはできません。つまり、適切な選択である可能性があります。

別の方法として、元の、ベースの定義をとしてマークする方法も `NSString` `[Protected]` あります。 これにより、必要に応じてサブクラス化を行うことができ、wrap'ed バージョンは引き続き動作し、オーバーライドされたメソッドを呼び出します。

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>`NSValue`、 `NSNumber` 、およびを `NSString` より適切な型にバインドする

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute)属性を使用する `NSNumber` `NSValue` と、 `NSString` より正確な C# 型へのバインドと (列挙) が可能になります。 属性を使用すると、ネイティブ API でより正確で正確な .NET API を作成できます。

メソッド (戻り値)、パラメーター、およびプロパティは、を使用して装飾でき [`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) ます。 唯一の制限は、メンバーを内に配置することはでき **ません** 。 [`[Protocol]`](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) 
または [`[Model]`](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) インターフェイス。

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

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) また `NSNumber` `NSValue` 、はおよび (列挙型) の配列もサポートしてい `NSString` ます。

次に例を示します。

```csharp
[BindAs (typeof (CAScroll []))]
[Export ("supportedScrollModes")]
NSString [] SupportedScrollModes { get; set; }
```

出力:

```csharp
[Export ("supportedScrollModes")]
CAScroll [] SupportedScrollModes { get; set; }
```

`CAScroll` は、サポートされている `NSString` 列挙型です。適切な値を取得 `NSString` し、型変換を処理します。

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute)サポートされている変換の種類については、ドキュメントを参照してください。

<a name="Binding_Notifications"></a>

### <a name="binding-notifications"></a>バインド通知

通知とは、にポストされたメッセージであり、 `NSNotificationCenter.DefaultCenter` アプリケーションのある部分から別の部分にメッセージをブロードキャストするメカニズムとして使用されます。 開発者は、通常、 [Nsnotificationcenter](xref:Foundation.NSNotificationCenter)の [Addobserver](xref:Foundation.NSNotificationCenter.AddObserver(Foundation.NSString,System.Action{Foundation.NSNotification})) メソッドを使用して通知をサブスクライブします。 アプリケーションは、メッセージを通知センターにポストするときに、通常、 [Nsnotification. UserInfo](xref:Foundation.NSNotification.UserInfo) dictionary に格納されているペイロードを格納します。 このディクショナリは厳密に型指定されていません。ユーザーは通常、ディクショナリで使用できるキーとディクショナリに格納できる値の型をドキュメントに読み込む必要があるため、情報を取得することは間違いが起こりやすくなります。 キーの存在は、ブール値としても使用されることがあります。

Xamarin. iOS バインディングジェネレーターは、開発者が通知をバインドできるようにします。 これを行うには、 [`[Notification]`](~/cross-platform/macios/binding/binding-types-reference.md#NotificationAttribute)
でタグ付けされたプロパティの属性 [`[Field]`](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute)
プロパティ (パブリックまたはプライベートにすることができます)。

この属性は、ペイロードを含まない通知の引数なしで使用できます。また、API 定義で別のインターフェイスを参照するを指定することもでき `System.Type` ます。この場合、通常は "EventArgs" で終わる名前が使用されます。 ジェネレーターは、サブクラスを持つクラスにインターフェイスを変換し、そこに一覧表示されている `EventArgs` すべてのプロパティを含めます。 [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)属性を EventArgs クラスで使用して、値を取得する目的の C 辞書を検索するために使用するキーの名前を一覧表示する必要があります。

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
   }
}
```

コードのユーザーは、次のようなコードを使用して、 [Nsdefaultcenter](xref:Foundation.NSNotificationCenter.DefaultCenter) にポストされた通知を簡単にサブスクライブできます。

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
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

上の例では、プロパティとプロパティを持つクラスが生成されます。これらのプロパティは、 `MyScreenChangedEventArgs` `ScreenX` `ScreenY` キー "screenxkey" と "screenxkey" をそれぞれ使用して、適切な変換を適用します[。](xref:Foundation.NSNotification.UserInfo) 属性は、 `[ProbePresence]` 値を抽出するのではなく、でキーが設定されているかどうかを調べるジェネレーターに使用され `UserInfo` ます。 これは、キーの存在が値である場合 (通常はブール値の場合) に使用されます。

これにより、次のようなコードを記述できます。

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

<a name="Binding_Categories"></a>

### <a name="binding-categories"></a>バインドのカテゴリ

カテゴリは、クラスで使用可能なメソッドとプロパティのセットを拡張するために使用される、目的の C の機構です。   実際には、特定のフレームワークがリンクされている場合 (たとえば)、 `NSObject` 新しいフレームワークがにリンクされている場合にのみ、基本クラスの機能を拡張する (たとえば) ために使用され `UIKit` ます。   場合によっては、機能によってクラスの特徴を整理するために使用されます。   これらは、精神と C# の拡張メソッドに似ています。このカテゴリは、次のようになります。

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

上の例でライブラリに見つかった場合は、メソッドを使用してのインスタンスを拡張し `UIView` `makeBackgroundRed` ます。

これらをバインドするには、 [`[Category]`](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) インターフェイス定義で属性を使用します。  を使用する場合 [`[Category]`](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute)
属性。 [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 
属性の変更は、拡張する基底クラスを指定するために使用されます。これは、拡張する型になります。

`UIView`拡張機能がどのようにバインドされ、C# 拡張メソッドに変換されるかを次に示します。

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

上の例では、 `MyUIViewExtension` 拡張メソッドを含むクラスを作成し `MakeBackgroundRed` ます。  これは、任意のサブクラスで "MakeBackgroundRed" を呼び出すことができることを意味します。これにより、 `UIView` 目的の C でも同じ機能が得られます。 場合によっては、カテゴリを使用してシステムクラスを拡張するのではなく、装飾のために機能を整理します。  例:

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

を使用できますが、 [`[Category]`](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute)
属性この装飾スタイルの宣言に加えて、すべてをクラス定義に追加することもできます。  これらはどちらも同じことを実現します。

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

次のような場合には、カテゴリをマージするだけではありません。

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);

    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

<a name="Binding_Blocks"></a>

### <a name="binding-blocks"></a>バインドブロック

ブロックは Apple によって導入された新しい構造体であり、C# の匿名メソッドに相当するものを目的の C に持ち込むことができます。 たとえば、クラスは `NSSet` このメソッドを公開するようになりました。

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

上の説明は、という1つの引数を受け取るという名前のメソッドを宣言して `enumerateObjectsUsingBlock:` `block` います。 このブロックは、現在の環境 ("this" ポインター、ローカル変数とパラメーターへのアクセス) のキャプチャをサポートしているという点で、C# の匿名メソッドに似ています。 上の上のメソッドは、 `NSSet` 2 つのパラメーター `NSObject` ( `id obj` 部分) とブール値 () へのポインターを持つブロックを呼び出し `BOOL *stop` ます。

この種の API を btouch にバインドするには、最初にブロックの型シグネチャを C# デリゲートとして宣言し、次のように API エントリポイントから参照する必要があります。

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

これで、コードで C# から関数を呼び出すことができるようになりました。

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

また、必要に応じてラムダを使用することもできます。

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

<a name="GeneratingAsync"></a>

### <a name="asynchronous-methods"></a>非同期メソッド

バインディングジェネレーターは、特定のクラスのメソッドを、非同期のわかりやすいメソッド (タスクまたはタスク T を返すメソッド) に変換でき &lt; &gt; ます。

使用できるのは、 [`[Async]`](~/cross-platform/macios/binding/binding-types-reference.md#AsyncAttribute) 
void を返し、最後の引数がコールバックであるメソッドの属性。  メソッドにこのを適用すると、バインドジェネレーターは、サフィックスを使用して、そのメソッドのバージョンを生成し `Async` ます。  コールバックがパラメーターを受け取らない場合、戻り値はになり `Task` ます。コールバックがパラメーターを受け取ると、結果はになり `Task<T>` ます。  コールバックが複数のパラメーターを受け取る場合は、またはを設定して、すべてのプロパティを保持する、 `ResultType` `ResultTypeName` 生成される型の目的の名前を指定する必要があります。

例:

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

上記のコードでは、LoadFile メソッドと次の両方が生成されます。

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types"></a>

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>弱い NSDictionary パラメーターの厳密な型を提示しています

目標 C API の多くの場所では、パラメーターは特定のキーと値を持つ弱く型指定された api として渡されますが、これらはエラーが発生しやすくなります (無効なキーを渡し、警告を表示することはできません `NSDictionary` 。無効な値を渡すことができ、警告は表示されません)。また、

この解決策は、厳密に型指定されたバージョンの API を提供する厳密に型指定されたバージョンを提供することです。また、背後では、基になるさまざまなキーと値をマップします。

たとえば、目的の C API がを受け入れ、 `NSDictionary` `XyzVolumeKey` `NSNumber` 0.0 から1.0 のボリューム値と文字列を受け取るキーを取得するキーを取得した場合、ユーザーは次の `XyzCaptionKey` ような優れた API を使用する必要があります。

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

`Volume`このプロパティは、null 値を許容する float として定義されます。これは、目的の C では、これらのディクショナリに値を設定する必要がないため、値が設定されない場合があります。

これを行うには、いくつかの操作を行う必要があります。

- 厳密に型指定されたクラスを作成します。これは、 [Dictionarycontainer](xref:Foundation.DictionaryContainer) をサブクラスにし、各プロパティのさまざまな getter および setter を提供します。
- `NSDictionary`新しい厳密に型指定されたバージョンを取得するために使用するメソッドのオーバーロードを宣言します。

厳密に型指定されたクラスを手動で作成することも、ジェネレーターを使用して作業を行うこともできます。  まず、何が行われているかを理解し、次に自動アプローチについて理解するために、この方法を手動で試してみましょう。

このためのサポートファイルを作成する必要があります。コントラクト API にはありません。  XyzOptions クラスを作成するには、次のように記述する必要があります。

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

次に、低レベルの API の上に高レベルの API を表示するラッパーメソッドを提供する必要があります。

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

API を上書きする必要がない場合は、「」を使用して、NSDictionary ベースの API を安全に非表示にすることができます。 [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
属性.

ご覧のとおり、 [`[Wrap]`](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute)
属性を使用して新しい API エントリポイントを作成し、厳密に型指定されたクラスを使用し `XyzOptions` ます。  ラッパーメソッドでは、null を渡すこともできます。

ここでは、値がどこから来たかについて説明しませんでした `XyzOptionsKeys` 。  次のように、通常は API が静的クラスに表示するキーをグループ化し `XyzOptionsKeys` ます。

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

これらの厳密に型指定された辞書を作成するための自動サポートについて説明します。  これにより、多くの定型句が回避され、外部ファイルを使用する代わりに、API コントラクトで直接ディクショナリを定義できます。

厳密に型指定されたディクショナリを作成するには、API にインターフェイスを導入し、 [StrongDictionary](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) 属性を使用してそれを装飾します。  これにより、ジェネレーターは、から派生 `DictionaryContainer` し、厳密に型指定されたアクセサーを提供するインターフェイスと同じ名前を持つクラスを作成する必要があることをジェネレーターに指示します。

属性は、 [`[StrongDictionary]`](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) 1 つのパラメーターを受け取ります。これは、ディクショナリキーを含む静的クラスの名前です。  その後、インターフェイスの各プロパティは、厳密に型指定されたアクセサーになります。  既定では、コードは、静的クラスでサフィックスが "Key" のプロパティの名前を使用してアクセサーを作成します。

つまり、厳密に型指定されたアクセサーを作成する場合は、外部ファイルが不要になり、すべてのプロパティに対して手動で getter と setter を作成したり、手動でキーを参照したりする必要がなくなります。

バインド全体は次のようになります。

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

メンバー内で `XyzOption` 別のフィールド (サフィックスが付いたプロパティの名前ではない) を参照する必要がある場合は、 `Key` プロパティを [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 
使用する名前を持つ属性。

<a name="Type_mappings"></a>

## <a name="type-mappings"></a>型のマッピング

このセクションでは、C の型を C# 型にマップする方法について説明します。

<a name="Simple_Types"></a>

### <a name="simple-types"></a>単純型

次の表は、目的の C と CocoaTouch の型を Xamarin. iOS の世界にマップする方法を示しています。

|目的-C 型の名前|Xamarin Unified API の種類|
|---|---|
|`BOOL`, `GLboolean`|`bool`|
|`NSInteger`|`nint`|
|`NSUInteger`|`nuint`|
|`CFTimeInterval` / `NSTimeInterval`|`double`|
|`NSString` ([NSString のバインドに関する詳細](~/ios/internals/api-design/nsstring.md))|`string`|
|`char *`|`string` (関連項目: [`[PlainString]`](~/cross-platform/macios/binding/binding-types-reference.md#plainstring) )|
|`CGRect`|`CGRect`|
|`CGPoint`|`CGPoint`|
|`CGSize`|`CGSize`|
|`CGFloat`, `GLfloat`|`nfloat`|
|CoreFoundation types ( `CF*` )|`CoreFoundation.CF*`|
|`GLint`|`nint`|
|`GLfloat`|`nfloat`|
|Foundation 型 ( `NS*` )|`Foundation.NS*`|
|`id`|`Foundation`.`NSObject`|
|`NSGlyph`|`nint`|
|`NSSize`|`CGSize`|
|`NSTextAlignment`|`UITextAlignment`|
|`SEL`|`ObjCRuntime.Selector`|
|`dispatch_queue_t`|`CoreFoundation.DispatchQueue`|
|`CFTimeInterval`|`double`|
|`CFIndex`|`nint`|
|`NSGlyph`|`nuint`|

<a name="Arrays"></a>

### <a name="arrays"></a>配列

Xamarin の iOS ランタイムは、C# の配列をに変換して変換する処理を自動的に実行します `NSArrays` 。たとえば、のを返す架空の目的 C のメソッドなど `NSArray` `UIViews` です。

```csharp
// Get the peer views - untyped
- (NSArray *)getPeerViews ();

// Set the views for this container
- (void) setViews:(NSArray *) views
```

は次のようにバインドされます。

```csharp
[Export ("getPeerViews")]
UIView [] GetPeerViews ();

[Export ("setViews:")]
void SetViews (UIView [] views);
```

この方法では、厳密に型指定された C# 配列を使用します。これにより、IDE では、ユーザーに推測を強制することなく、実際の型で適切なコード補完を提供できます。また、ドキュメントを参照して、配列に含まれるオブジェクトの実際の型を調べることもできます。

配列に格納されている実際のほとんどの派生型を追跡できない場合は、を `NSObject []` 戻り値として使用できます。

<a name="Selectors"></a>

### <a name="selectors"></a>セレクター

セレクターは、特殊な型として目的の C API に表示され `SEL` ます。 セレクターをバインドする場合は、型をにマップし `ObjCRuntime.Selector` ます。  通常、セレクターは、オブジェクト、ターゲットオブジェクト、およびターゲットオブジェクトで呼び出すセレクターの両方を持つ API で公開されます。 これらの両方を基本的に C# デリゲートに対応させます。これは、呼び出すメソッドと、メソッドを呼び出すオブジェクトの両方をカプセル化するものです。

バインディングは次のようになります。

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

これは、通常、アプリケーションでメソッドがどのように使用されるかを示しています。

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

C# 開発者にとっては、バインディングを簡単にするために、通常はパラメーターを受け取るメソッドを提供し `NSAction` ます。これにより、C# のデリゲートとラムダをの代わりに使用できるようになり `Target+Selector` ます。 これを行うには、通常、次のように `SetTarget` フラグを付けてメソッドを非表示にします。 [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
次のように、新しいヘルパーメソッドを公開します。

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

これで、ユーザーコードは次のように記述できます。

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

<a name="Strings"></a>

### <a name="strings"></a>文字列

を受け取るメソッドをバインドする場合は、 `NSString` 戻り値の型とパラメーターの両方で、C# の文字列型に置き換えることができます。

を直接使用する場合 `NSString` は、文字列をトークンとして使用する必要があります。 文字列との詳細については `NSString` 、「 [Nsstring の API 設計](~/ios/internals/api-design/nsstring.md) 」ドキュメントを参照してください。

まれに、API が、 `char *` 目的の c 文字列 () ではなく、c に似た文字列 () を公開する場合があり `NSString *` ます。 そのような場合は、パラメーターに [`[PlainString]`](~/cross-platform/macios/binding/binding-types-reference.md#plainstring)
属性.

<a name="outref_parameters"></a>

### <a name="outref-parameters"></a>out/ref パラメーター

Api によっては、パラメーターの値が返されます。または、参照渡しでパラメーターを渡すこともできます。

通常、シグネチャは次のようになります。

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

最初の例では、共通の目的 C の表現を使用してエラーコードを返し、ポインターへのポインター `NSError` が渡され、戻り時に値が設定されています。   2番目のメソッドは、目的の C メソッドがオブジェクトを取得し、その内容を変更する方法を示しています。   これは、純粋な出力値ではなく、参照渡しになります。

バインドは次のようになります。

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

<a name="Memory_management_attributes"></a>

### <a name="memory-management-attributes"></a>メモリ管理属性

属性を使用し、呼び出された [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) メソッドによって保持されるデータを渡す場合は、次の例のように、2番目のパラメーターとして渡すことで引数のセマンティクスを指定できます。

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

上の例では、値に "Retain" セマンティクスがあるとしてフラグが付けられています。 使用できるセマンティクスは次のとおりです。

- 代入
- コピー
- 保持

<a name="Style_Guidelines"></a>

### <a name="style-guidelines"></a>スタイルのガイドライン

<a name="Using_[Internal]"></a>

#### <a name="using-internal"></a>使用 [Internal]

使用できるのは、 [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
パブリック API からメソッドを非表示にする属性です。 公開されている API が低レベルで、この方法に基づいて別のファイルに高レベルの実装を提供する場合に、この操作を行うことができます。

また、バインドジェネレーターで制限が発生した場合にも使用できます。たとえば、一部の高度なシナリオではバインドされていない型が公開され、独自の方法でバインドする場合があり、独自の方法で型をラップする必要があります。

<a name="Event_Handlers_and_Callbacks"></a>

## <a name="event-handlers-and-callbacks"></a>イベントハンドラーとコールバック

目標 C クラスは、通常、デリゲートクラス (目標 C デリゲート) でメッセージを送信することによって、通知または要求情報をブロードキャストします。

このモデルは、Xamarin によって完全にサポートされ、表示される場合があります。 iOS は煩雑な場合があります。 Xamarin では、C# のイベントパターンと、このような状況で使用できるクラスのメソッドコールバックシステムを公開しています。 これにより、次のようなコードを実行できます。

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

バインディングジェネレーターは、目的の C パターンを C# パターンにマップするために必要な入力の量を減らすことができます。

Xamarin. iOS 1.4 以降では、特定の目的 C デリゲートのバインドを生成するようジェネレーターに指示し、ホスト型の C# イベントとプロパティとしてデリゲートを公開することもできます。

このプロセスには2つのクラスが関係しています。これは、現在イベントを出力しているホストクラスであり、これらのクラスを `Delegate` または `WeakDelegate` および実際のデリゲートクラスに送信します。

次のセットアップを検討してください。

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

クラスをラップするには、次のことを行う必要があります。

- ホストクラスで、にを追加します。 [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute)  
   デリゲートとして機能する型と、公開した C# 名を宣言します。 上記の例では `typeof (MyClassDelegate)` 、とは `WeakDelegate` それぞれとです。
- デリゲートクラスで、3つ以上のパラメーターを持つ各メソッドで、自動的に生成された EventArgs クラスに使用する型を指定する必要があります。

バインディングジェネレーターは、1つのイベント変換先のみをラップするように制限されていません。一部の目的 C クラスは、複数のデリゲートにメッセージを出力することができます。そのため、このセットアップをサポートする配列を用意する必要があります。 ほとんどのセットアップでは必要ありませんが、ジェネレーターはそのようなケースをサポートする準備ができています。

結果のコードは次のようになります。

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

`EventArgs`は、生成するクラスの名前を指定するために使用され `EventArgs` ます。 署名ごとに1つを使用する必要があります (この例では、には `EventArgs` `With` nint 型のプロパティが含まれています)。

上記の定義では、ジェネレーターによって生成される MyClass に次のイベントが生成されます。

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

これで、次のようなコードを使用できるようになりました。

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

コールバックは、イベントの呼び出しと同じように、複数の潜在的なサブスクライバーを使用するのではなく (たとえば、複数のメソッドがイベントまたはイベントにフックできる `Clicked` `DownloadFinished` )、コールバックは1つのサブスクライバーだけを持つことができます。

プロセスは同じですが、唯一の違いは、生成されるクラスの名前を公開するのではなく、 `EventArgs` 実際には、結果として得られる C# のデリゲート名に名前を指定するために EventArgs を使用することです。

デリゲートクラスのメソッドが値を返す場合、バインディングジェネレーターはこれをイベントではなく親クラスのデリゲートメソッドにマップします。 このような場合は、ユーザーがデリゲートにフックしていない場合に、メソッドによって返される既定値を指定する必要があります。 これを行うには、 [`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute)
または [`[DefaultValueFromArgument]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) 属性。

[`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) は、戻り値をハードコーディングします。 [`[DefaultValueFromArgument]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute)
は、返される入力引数を指定するために使用されます。

<a name="Enumerations_and_Base_Types"></a>

## <a name="enumerations-and-base-types"></a>列挙型と基本型

Btouch インターフェイス定義システムによって直接サポートされていない列挙型または基本型を参照することもできます。 これを行うには、列挙型とコア型を別のファイルに配置し、btouch に渡す追加ファイルの1つとしてこれを含めます。

<a name="Linking_the_Dependencies"></a>

## <a name="linking-the-dependencies"></a>依存関係のリンク

アプリケーションに含まれていない Api をバインドする場合は、実行可能ファイルがこれらのライブラリにリンクされていることを確認する必要があります。

Xamarin に通知する必要があります。ライブラリをリンクするには、ビルド構成を変更して、次 `mtouch` のように、"-gcc_flags" オプションを使用して新しいライブラリとリンクする方法を指定するいくつかの追加のビルド引数を使用してコマンドを呼び出すようにします。

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

上の例では `libMyLibrary.a` 、 `libSystemLibrary.dylib` 最終的な `CFNetwork` 実行可能ファイルにフレームワークライブラリがリンクされています。

または、 [`[LinkWithAttribute]`](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) コントラクトファイル (など) に埋め込むことができるアセンブリレベルを利用することもでき `AssemblyInfo.cs` ます。
を使用する場合 [`[LinkWithAttribute]`](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) は、バインディングの作成時にネイティブライブラリを使用できるようにする必要があります。これにより、ネイティブライブラリがアプリケーションと共に埋め込まれます。 次に例を示します。

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

なぜコマンドが必要なのか、というのは、のコードをコンパイルするのに `-force_load` -ObjC フラグが使用されるためです。これは、Xamarin の実行時に必要なカテゴリをサポートするために必要なメタデータを保持しません (リンカーとコンパイラによって削除されます)。

<a name="Assisted_References"></a>

## <a name="assisted-references"></a>支援参照

アクションシートやアラートボックスなどの一部の一時的なオブジェクトは、開発者を追跡するのが面倒であり、バインドジェネレーターがここで少し役に立ちます。

たとえば、メッセージを表示した後にイベントを生成したクラスがある場合、従来の方法では次のように `Done` 処理されます。

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

上記のシナリオでは、開発者はオブジェクトへの参照を保持し、ユーザー自身の box の参照をリークまたはアクティブにクリアする必要があります。  コードのバインド時に、ジェネレーターは参照の追跡をサポートし、特別なメソッドが呼び出されるとクリアします。上記のコードは次のようになります。

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

インスタンスで変数を保持する必要がなくなったことに注意してください。変数はローカル変数で動作し、オブジェクトが死んだときに参照をクリアする必要はありません。

これを利用するには、クラスの宣言に Events プロパティを設定し、さらに、次 [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) `KeepUntilRef` のように、オブジェクトが処理を完了したときに呼び出されるメソッドの名前に設定された変数に設定する必要があります。

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

<a name="Inheriting_Protocols"></a>

## <a name="inheriting-protocols"></a>プロトコルの継承

Xamarin. iOS v2.0 では、プロパティでマークされたプロトコルからの継承がサポートされています。 [`[Model]`](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) これは、プロトコルがプロトコルから継承されるなど、特定の API パターンで役立ち、 `MapKit` `MKOverlay` `MKAnnotation` から継承される多数のクラスによって採用され `NSObject` ます。

これまでは、すべての実装にプロトコルをコピーする必要がありましたが、このような場合は、 `MKShape` クラスがプロトコルから継承 `MKOverlay` し、必要なすべてのメソッドが自動的に生成されるようになりました。

## <a name="related-links"></a>関連リンク

- [バインディングのサンプル](/samples/xamarin/ios-samples/bindingsample/)