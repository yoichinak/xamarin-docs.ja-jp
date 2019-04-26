---
title: Xamarin.Mac のしくみ
description: このドキュメントでは、Xamarin.Mac の内部動作を説明します。 具体的には、コンス トラクター、時間のコンパイル前のメモリ管理、およびレジストラーに見えます。
ms.prod: xamarin
ms.assetid: C2053ABB-6DBF-4233-AEEA-B72FC6A81FE1
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 05/25/2017
ms.openlocfilehash: 0635e110cb2aa7bc00234d3d06df57e0fd6f966e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61033846"
---
# <a name="how-xamarinmac-works"></a>Xamarin.Mac のしくみ

ほとんどの開発者は、ただし、内部「マジック」、Xamarin.Mac の気にする必要ことはありませんが両方の解釈既存ドキュメントでの内部でモ ノの動作の利用方法の大まかな理解をC#レンズとデバッグ問題が発生したとき。

Xamarin.Mac では、アプリケーションは、2 つの世界をブリッジします。ネイティブ クラスのインスタンスを含む、Objective C ベースのランタイムがある (`NSString`、`NSApplication`など) があると、C#ランタイムのインスタンスを含むマネージ クラス (`System.String`、`HttpClient`など)。 アプリは objective-c で (セレクター) のメソッドを呼び出すことができますのでこれら 2 つの環境間 Xamarin.Mac が双方向ブリッジを作成 (など`NSApplication.Init`)、OBJECTIVE-C では、アプリを呼び出すことがC#メソッド (など、アプリケーション デリゲートのメソッド) バックアップを作成します。 使用して OBJECTIVE-C への呼び出しを透過的に処理が一般に、 **P/invoke**とランタイムのコードを Xamarin を提供します。

<a name="exposing-classes" />

## <a name="exposing-c-classes--methods-to-objective-c"></a>公開するC#クラス/objective-c メソッド

ただし、OBJECTIVE-C にアプリのコールバックのC#Objective C が理解できる方法で公開する必要がありますが、オブジェクトします。 使用してこれには、`Register`と`Export`属性。 次の例を参照してください。

```csharp
[Register ("MyClass")]
public class MyClass : NSObject
{
   [Export ("init")]
   public MyClass ()
   {
   }

   [Export ("run")]
   public void Run ()
   {
   }
}
```

この例では、OBJECTIVE-C ランタイムと呼ばれるクラスについて認識できるよう`MyClass`セレクターと呼ばれる`init`と`run`します。

ほとんどの場合、これは、開発者は無視できますが、実装の詳細アプリを受け取るほとんどのコールバックはでオーバーライドされたメソッドを使用していずれかをする`base`クラス (など`AppDelegate`、 `Delegates`、 `DataSources`) か、または**アクション**Api に渡されます。 すべてのような場合、`Export`属性は不要で、C#コード。

## <a name="constructor-runthrough"></a>コンス トラクター runthrough

開発者は、アプリの公開する必要があります多くの場合、C#クラス構造 API を使用して場合などの場所からインスタンス化するため、OBJECTIVE-C ランタイムでストーリー ボードまたは XIB ファイルと呼ばれます。 Xamarin.Mac アプリで使用される 5 つの最も一般的なコンス トラクターを次に示します。

```csharp
// Called when created from unmanaged code
public CustomView (IntPtr handle) : base (handle)
{
   Initialize ();
}

// Called when created directly from a XIB file
[Export ("initWithCoder:")]
public CustomView (NSCoder coder) : base (coder)
{
   Initialize ();
}

// Called from C# to instance NSView with a Frame (initWithFrame)
public CustomView (CGRect frame) : base (frame)
{
}

// Called from C# to instance NSView without setting the frame (init)
public CustomView () : base ()
{
}

// This is a special case constructor that you call on a derived class when the derived called has an [Export] constructor.
// For example, if you call init on NSString then you don’t want to call init on NSObject.
public CustomView () : base (NSObjectFlag.Empty)
{
}
```

開発者が一般に、ままにする必要があります、`IntPtr`と`NSCoder`カスタムなど一部の種類を作成するときに生成されるコンス トラクター`NSViews`だけです。 Xamarin.Mac は、OBJECTIVE-C ランタイム要求への応答でこれらのコンス トラクターのいずれかを呼び出す必要がある、削除した場合は、ネイティブ コード内で、アプリがクラッシュし、問題では正確に把握が難しい場合があります。

## <a name="memory-management-and-cycles"></a>メモリ管理とサイクル

Xamarin.Mac でのメモリ管理は、xamarin.ios のような非常に多くの点でです。 このドキュメントの範囲を超えるいずれかの複雑なトピックにもなります。 参照してください、[メモリとパフォーマンスのベスト プラクティス](~/cross-platform/deploy-test/memory-perf-best-practices.md)します。

## <a name="ahead-of-time-compilation"></a>事前 of time コンパイル

通常、.NET アプリケーションでは、構築すると、マシン コードにコンパイルしないで、IL コードを取得すると呼ばれる中間のレイヤーにコンパイルする代わりに_ジャスト イン タイム_(JIT) は、アプリを起動するときをマシン コードにコンパイルします。

Mono ランタイム JIT コンパイルに必要な時間このマシン語コード速度が低下 Xamarin.Mac アプリの起動を最大 20% を生成するために必要なマシン語コードの時間がかかる。

Ios Apple によって課される制限のため、IL コードの JIT コンパイルの対象は Xamarin.iOS には使用できません。 その結果、すべての Xamarin.iOS アプリは完全な_Ahead Of Time_ (AOT) がビルド サイクル中にマシン コードにコンパイルします。

新しい Xamarin.Mac には AOT する機能、IL コード、アプリのビルド サイクル中に Xamarin.iOS できますと同様です。 Xamarin.Mac を使用して、_ハイブリッド_、必要なマシン語コードの大部分をコンパイルする AOT アプローチが必要な領域と引き続き Reflection.Emit をサポートする柔軟性をコンパイルするランタイム (でき、その他のユース ケースを現在では機能 Xamarin.Mac)。

これには AOT が Xamarin.Mac アプリを支援できる 2 つの主要な領域があります。

- **「ネイティブ」のクラッシュ ログを向上**- Cocoa Api に不正な呼び出しを行うときに頻繁に発生すると、ネイティブ コードで、Xamarin.Mac アプリケーションがクラッシュする場合 (送信するなど、`null`同意しないメソッドに)、ネイティブ クラッシュ ログと JITフレームは、分析に困難です。 JIT のフレームはデバッグ情報を持っていないためとがあります 16 進数のオフセットを持つ複数の行に何が起こったかを判断できません。 AOT は「実際の」名前付きのフレームを生成し、トレースがはるかに読みやすくします。 Xamarin.Mac アプリと対話するよりネイティブ ツールなども意味**lldb**と**Instruments**します。
- **時のパフォーマンスをより適切に起動**- 複数スタートアップ再度 JIT コンパイルのすべてのコードでの大規模な Xamarin.Mac アプリケーションかなりの時間がかかることができます。 AOT はこの作業を前もってです。

### <a name="enabling-aot-compilation"></a>AOT コンパイルを有効にします。

AOT がダブルクリックで Xamarin.Mac で有効になって、**プロジェクト名**で、**ソリューション エクスプ ローラー**に移動する、 **Mac ビルド**追加`--aot:[options]`に**追加の mmp 引数:** フィールド (場所`[options]`AOT の種類を制御、以下を参照してください。 1 つまたは複数のオプション)。 例えば:

![AOT を追加するのには、追加の mmp 引数](how-it-works-images/aot01.png "追加の mmp 引数に AOT の追加")

> [!IMPORTANT]
> AOT を有効にする、コンパイルは、数分までも、ビルド、時間を大幅に増加しますが、20% が平均でアプリの起動時間を向上させることができます。 そのため、AOT コンパイルする必要がありますのみで有効にする**リリース**Xamarin.Mac アプリのビルドします。

### <a name="aot-compilation-options"></a>Aot コンパイル オプション

Xamarin.Mac アプリで AOT コンパイルを有効にする場合に調整できるいくつかの異なるオプションがあります。

- `none` -AOT コンパイルします。 これは、既定の設定です。
- `all` -AOT、MonoBundle ですべてのアセンブリをコンパイルします。
- `core` -AOT コンパイル、 `Xamarin.Mac`、`System`と`mscorlib`アセンブリ。
- `sdk` -AOT コンパイル、`Xamarin.Mac`と基本クラス ライブラリ (BCL) のアセンブリ。
- `|hybrid` -上記のオプションのいずれかにこれにより、ハイブリッド AOT IL ストリッピングを使用できるがの結果になったコンパイルの時間を追加します。
- `+` -AOT コンパイルの 1 つのファイルが含まれています。
- `-` -AOT コンパイルから 1 つのファイルを削除します。

たとえば、 `--aot:all,-MyAssembly.dll` 、MonoBundle 内のアセンブリのすべての AOT コンパイルを有効にすると_を除く_`MyAssembly.dll`と`--aot:core|hybrid,+MyOtherAssembly.dll,-mscorlib.dll`ハイブリッドを有効にすると、AOT のコードを含める、 `MyOtherAssembly.dll` を除外`mscorlib.dll`.

## <a name="partial-static-registrar"></a>部分的な静的レジストラー

Xamarin.Mac アプリを開発する場合、変更を完了し、テストまでの時間を最小限に抑える開発期限を守ることに重要になることができます。 戦略などのモジュール化のコードベースし、アプリで高価な完全な再構築を必要とする回数が少なく、コンパイルにかかる時間を短縮する単体テストが役立つことができます。

さらに、Xamarin.Mac に新しく_部分の静的なレジストラー_ (Xamarin.iOS で初めて考案したもの) に大幅に時間を短縮できます、起動に Xamarin.Mac アプリの**デバッグ**構成します。 縮小する部分の静的なレジストラーを使用する方法を理解すること、デバッグ起動では、5 倍向上ほぼレジストラーは、どのようなは、静的と動的の間の違いと、この「部分的な静的な」バージョンが何でバック グラウンドのビットになります。

### <a name="about-the-registrar"></a>レジストラーについて

内部的には、Xamarin.Mac のアプリケーションには Apple と OBJECTIVE-C ランタイムから Cocoa フレームワークによってもたらされます。 この"ネイティブ world"と"マネージ world"の間のブリッジの構築C#Xamarin.Mac の主な役割です。 このタスクの一部は、レジストラーでは、内部で実行される、によって処理される`NSApplication.Init ()`メソッド。 これは、Xamarin.Mac での Cocoa Api の使用を必要とする理由の 1 つ`NSApplication.Init`を最初に呼び出されます。

レジストラーの仕事は、OBJECTIVE-C のアプリの存在を通知するC#などのクラスから派生するクラス`NSApplicationDelegate`、 `NSView`、 `NSWindow`、および`NSObject`します。 これには、アプリ内のすべての型を登録する必要があるものとレポートには、各種類では、どのような要素を特定のスキャンが必要です。

いずれかのスキャンを実行できます**動的に**、リフレクションを使用してアプリケーションの起動時または**静的に**、ビルド時間ステップとして。 登録の種類を選択するときに、開発者は、次の注意にあります。

- 静的登録は、起動時間を大幅に短縮できますが、できるビルド時間が大幅に低下 (2 倍以上の通常のデバッグ ビルド時)。 既定値になりますこの**リリース**構成をビルドします。
- 動的登録この作業完了するまで遅延この追加作業は、著しい一時停止 (最低 2 秒) を作成できますが、アプリケーションを起動し、コード生成をスキップします。 アプリケーションの起動でします。 動的登録がリフレクションは低速を既定では、デバッグ構成のビルドで特に顕著です。

Xamarin.iOS の 8.13 で初めて導入された一部の静的登録は両方のオプションの長所を開発者に提供します。 すべての要素の登録情報を事前に計算して`Xamarin.Mac.dll`とスタティック ライブラリ (つまりのみをビルド時にリンクできる必要があります) で Xamarin.Mac を使ってこの情報を配布するには、マイクロソフトはほとんどのリフレクションの動的を削除しましたビルド時に影響しない中に登録します。

### <a name="enabling-the-partial-static-registrar"></a>一部の静的なレジストラーを有効にします。

ダブルクリックして Xamarin.Mac で部分的な静的レジストラーが有効になっている、**プロジェクト名**で、**ソリューション エクスプ ローラー**に移動する、 **Mac ビルド**の追加`--registrar:static`を**追加の mmp 引数:** フィールド。 例えば:

![追加の mmp 引数への部分的な静的レジストラーの追加](how-it-works-images/psr01.png "部分の静的なレジストラーを追加の mmp 引数に追加します。")

## <a name="additional-resources"></a>その他の技術情報

モ ノの内部で動作のいくつかのより詳細な説明を次に示します。

- [OBJECTIVE-C セレクター](~/ios/internals/objective-c-selectors.md)
- [レジストラー](~/ios/internals/registrar.md)
- [IOS および OS X 用 Xamarin Unified API](~/cross-platform/macios/unified/index.md)
- [Theading の基礎](~/ios/app-fundamentals/threading.md)
- [デリゲート、プロトコル、およびイベント](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [に関しては `newrefcount`](~/ios/internals/newrefcount.md)

