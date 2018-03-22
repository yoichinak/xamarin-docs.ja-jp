---
title: "Xamarin.Mac のしくみ"
description: "このドキュメントでは、Xamarin.Mac の内部機能について説明します。 具体的には、コンス トラクター、事前にコンパイル、メモリ管理、および、レジストラーで検索します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: C2053ABB-6DBF-4233-AEEA-B72FC6A81FE1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/25/2017
ms.openlocfilehash: a1dbff32b113bd1c3a6b2058a34c73977c59c9e5
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="how-xamarinmac-works"></a>Xamarin.Mac のしくみ

ほとんどの開発者が、ただし、内部「マジック」Xamarin.Mac のについて心配する必要がなくなりますが、内部で処理動作は c# レンズで既存のドキュメントを解釈して問題のデバッグの両方で利用する方法の大まかな知識を持つ場合これらが発生します。

アプリケーションが 2 つの環境をつなぐ Xamarin.Mac で: ネイティブ クラスのインスタンスを含む、OBJECTIVE-C ベース ランタイムがある (`NSString`、`NSApplication`など)、マネージ クラスのインスタンスを含む、c ランタイム (`System.String`、 `HttpClient`など)。 これら 2 つの環境間 Xamarin.Mac 作成されるため、双方向ブリッジ アプリが Objective c (セレクター) のメソッドを呼び出すことができます (など`NSApplication.Init`) Objective C が戻る (メソッドと同様、アプリのデリゲートに対する) アプリの c# メソッドを呼び出すことができます。 使用して Objective C への呼び出しを透過的に処理は、一般に、 **P/invoke**と実行時のコードを Xamarin では提供します。

<a name="exposing-classes" />

## <a name="exposing-c-classes--methods-to-objective-c"></a>C# のクラスを公開する/OBJECTIVE-C メソッド

ただし、OBJECTIVE-C がアプリの c# オブジェクトにコールバックするには、OBJECTIVE-C が理解できる方法で公開する必要があります。 使用してこれを行う、`Register`と`Export`属性。 次の例を参照してください。

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

この例では、Objective C のランタイムと呼ばれるクラスについて認識できるよう`MyClass`セレクターと呼ばれる`init`と`run`です。

ほとんどの場合、これは、開発者に無視することができます、実装の詳細、アプリを受け取るほとんどのコールバックのいずれかによってオーバーライドされたメソッドになるよう`base`クラス (など`AppDelegate`、 `Delegates`、 `DataSources`) または**アクション**Api に渡されます。 すべてのような場合、`Export`属性は c# コードでは必要ありません。

## <a name="constructor-runthrough"></a>コンス トラクター runthrough

開発者は、インスタンス化できる場所からなど、アプリの c# クラス構造 API を使用して、Objective C のランタイムを公開する必要があります多くの場合、ストーリー ボードまたは XIB ファイルで呼び出されるとします。 Xamarin.Mac アプリで使用される 5 つの最も一般的なコンス トラクターを次に示します。

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

一般に、開発者がおきます、`IntPtr`と`NSCoder`カスタムなど一部の種類を作成するときに生成されるコンス トラクター`NSViews`だけです。 Xamarin.Mac を Objective C ランタイム要求への応答でこれらのコンス トラクターのいずれかを呼び出す必要があるし、それを削除して、アプリがクラッシュするネイティブ コード内と、問題では正確に把握するが困難である可能性があります。

## <a name="memory-management-and-cycles"></a>メモリ管理とサイクル

メモリ管理 Xamarin.Mac でいろいろな点で Xamarin.iOS とよく似ています。 このドキュメントの範囲外のいずれか、複雑なトピックを勧めします。 お読みください、[メモリおよびパフォーマンスのベスト プラクティス](~/cross-platform/deploy-test/memory-perf-best-practices.md)です。

## <a name="ahead-of-time-compilation"></a>コンパイルの事前

通常、.NET アプリケーションでは、構築されているときに、マシン語コードにコンパイルしないで、IL コードを取得するという、中間層にコンパイルする代わりに_ジャスト イン タイム_(JIT) は、アプリが起動されたときにマシン語コードにコンパイルします。

モノラル ランタイム JIT コンパイルに要する時間このマシン語コード低下する可能性が Xamarin.Mac アプリの起動が最大 20% を生成するために必要なマシン語コードの時間がかかるようです。

Apple iOS に課せられる制限のための IL コードの JIT コンパイルは Xamarin.iOS を使用できません。 その結果、すべての Xamarin.iOS アプリがいっぱいになる_Ahead 時間_(AOT) が、ビルド中のマシン語コードにコンパイルします。

新しい Xamarin.Mac にする機能です AOT IL コードは、アプリのビルド中、Xamarin.iOS ことと同じようにします。 Xamarin.Mac を使用して、_ハイブリッド_AOT、必要なマシン語コードの大部分をコンパイルするが、により、ランタイムと Reflection.Emit をサポートするために続行する柔軟性が必要な領域をコンパイルする (およびその他のケースを処理します。現在では動作 Xamarin.Mac)。

AOT が Xamarin.Mac アプリを支援できる 2 つの主要な領域があります。

- **「ネイティブ」クラッシュ ログのより強力な**- Cocoa Api に無効な呼び出しを行うときに頻繁に発生すると、ネイティブ コードで Xamarin.Mac アプリケーションがクラッシュする場合 (送信する場合など、`null`メソッドにその内容に同意しない)、ネイティブ JIT を使用してログがクラッシュします。フレームは、分析するが困難。 以降、JIT のフレームには、デバッグ情報がない、16 進数のオフセットを持つ複数の行およびがありますに何が起こったかを判断できません。 AOT が"real"に名前付きフレームを生成し、トレースは、読みやすくします。 つまり Xamarin.Mac アプリと通信できるよりネイティブ ツールなど**lldb**と**楽器**です。
- **時間のパフォーマンスをより深く起動**– を複数起動再度、JIT コンパイルのすべてのコードでの大規模な Xamarin.Mac アプリケーションかなりの時間がかかることができます。 AOT とは、この作業を前もってです。

### <a name="enabling-aot-compilation"></a>AOT コンパイルを有効にします。

ダブルクリックして Xamarin.Mac で AOT が有効になっている、**プロジェクト名**で、**ソリューション エクスプ ローラー**に間を移動する、 **Mac をビルド**と追加`--aot:[options]`に**追加の mmp 引数:**フィールド (場所`[options]`は、1 つ以上のオプションを制御する AOT 型の下を参照してください)。 例えば:

![追加の mmp 引数に AOT を追加する](how-it-works-images/aot01.png "追加 mmp 引数を追加する AOT")

> [!IMPORTANT]
> AOT を有効にすると、コンパイルは、数分までも、ビルド、時間を大幅に増加するが、20% の平均でアプリの起動時間を向上させることができます。 その結果、AOT コンパイルのみで有効にする**リリース**Xamarin.Mac アプリのビルドします。

### <a name="aot-compilation-options"></a>Aot コンパイル オプション

Xamarin.Mac アプリでのコンパイルに AOT を有効にする場合、調整できるいくつかの異なるオプションがあります。

- `none` -AOT コンパイルします。 これは、既定の設定です。
- `all` -AOT、MonoBundle 内のすべてのアセンブリをコンパイルします。
- `core` -AOT コンパイル、 `Xamarin.Mac`、`System`と`mscorlib`アセンブリ。
- `sdk` -AOT コンパイル、`Xamarin.Mac`と基本クラス ライブラリ (BCL) アセンブリ。
- `|hybrid` -ハイブリッド AOT IL の分解を使用できる上記のオプションのいずれかにこれによりがの結果になったコンパイルの時間を追加します。
- `+` -AOT コンパイルへの 1 つが含まれます。
- `-` -AOT コンパイルから 1 つのファイルを削除します。

たとえば、 `--aot:all,-MyAssembly.dll` 、MonoBundle 内のアセンブリのすべての AOT コンパイルを有効にするには_を除く_`MyAssembly.dll`と`--aot:core|hybrid,+MyOtherAssembly.dll,-mscorlib.dll`ハイブリッドが有効にするには、コード AOT、 `MyOtherAssembly.dll` を除外`mscorlib.dll`.

## <a name="partial-static-registrar"></a>静的な部分のレジストラー

Xamarin.Mac アプリを開発するときに、変更を完了して、テスト間の時間を最小限に抑える会議の開発の期限に重要になることができます。 戦略などのモジュール化コードベース単体テストに役立ちますコンパイルにかかる時間を削減するアプリで、高価な完全な再構築を必要とする回数が少なくします。

さらに、Xamarin.Mac に新しく_部分の静的レジストラー_ (ようになり Xamarin.iOS によって) 大幅に削減で Xamarin.Mac アプリの起動時間、**デバッグ**構成します。 縮小する部分の静的なレジストラーを使用する方法を理解することがほとんどでは、5 倍向上デバッグ起動レジストラーは、どのような違いは静的および動的、およびこの「部分的な静的な」バージョンの動作のバック グラウンドのビットになります。

### <a name="about-the-registrar"></a>レジストラーについて

任意の Xamarin.Mac の中身アプリケーション Apple および Objective C のランタイムから Cocoa フレームワークの範囲です。 Xamarin.Mac の主な役割は、この"ネイティブ world"と c# の"マネージ world"の間のブリッジを作成します。 このタスクの一部は、レジストラーでは、内部が実行されるによって処理される`NSApplication.Init ()`メソッドです。 これは、Xamarin.Mac Cocoa Api の使用を必要とする理由の 1 つ`NSApplication.Init`を最初に呼び出されます。

レジストラーのジョブが、アプリの c# などのクラスから派生するクラスの存在を Objective C のランタイムに通知する`NSApplicationDelegate`、 `NSView`、 `NSWindow`、および`NSObject`です。 これには、すべての種類のアプリを登録する必要がある新機能とレポートには、各種類では、どのような要素を確認するスキャンが必要です。

いずれかのスキャンを実行できます**動的に**、リフレクションを使用してアプリケーションの起動時または**静的に**、ビルド時間ステップとします。 登録の種類を選択するときに、開発者は、次の対応する必要があります。

- 静的登録は、起動時間を大幅に節約できますが、速度が低下するビルド時間が大幅に (2 倍以上の通常のデバッグ ビルド時)。 これは、既定値になります**リリース**構成をビルドします。
- 動的登録遅延この作業この追加作業が顕著な一時停止 (には、少なくとも 2 秒) を作成できますが、アプリケーションを起動し、コード生成をスキップするまでアプリケーションの起動でします。 これはデバッグ構成のビルドでは、特に顕著を既定に動的登録し、リフレクションの動作が遅くなります。

Xamarin.iOS 8.13 で初めて導入された一部の静的登録では、オプションの両方の最適な開発者を示します。 すべての要素の登録情報を事前計算して`Xamarin.Mac.dll`Xamarin.Mac (のみをビルド時にリンクできる必要があります) をスタティック ライブラリ内で、この情報を配布するには、Microsoft が取り外さほとんどの場合、動的のリフレクションとビルド時に影響を与えない中に登録します。

### <a name="enabling-the-partial-static-registrar"></a>部分の静的なレジストラーを有効にします。

ダブルクリックして Xamarin.Mac の部分的な静的レジストラーが有効になっている、**プロジェクト名**で、**ソリューション エクスプ ローラー**に間を移動する、 **Mac をビルド**を追加して`--registrar:static`を**追加 mmp 引数:**フィールドです。 例えば:

![追加の mmp 引数に、部分的な静的レジストラーを追加する](how-it-works-images/psr01.png "追加 mmp 引数に、一部の静的なレジストラーを追加します。")

## <a name="additional-resources"></a>その他の技術情報

内部的に作業の方法のいくつかのより詳細な説明を次に示します。

- [Objective C セレクター](~/ios/internals/objective-c-selectors.md)
- [レジストラー](~/ios/internals/registrar.md)
- [IOS および OS X 向け Xamarin Unified API](~/cross-platform/macios/unified/index.md)
- [Theading の基礎](~/ios/app-fundamentals/threading.md)
- [デリゲート、プロトコル、およびイベント](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [に関しては `newrefcount`](~/ios/internals/newrefcount.md)

