---
title: Xamarin.Mac のしくみ
description: このドキュメントでは、Xamarin. Mac の内部動作について説明します。 特に、コンストラクター、メモリ管理、前もってのコンパイル、およびレジストラーについて見ていきます。
ms.prod: xamarin
ms.assetid: C2053ABB-6DBF-4233-AEEA-B72FC6A81FE1
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 05/25/2017
ms.openlocfilehash: 05bb9a5552022ea1eb5cd92df90659f7ebacb7cc
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571650"
---
# <a name="how-xamarinmac-works"></a>Xamarin.Mac のしくみ

多くの場合、開発者は Xamarin. Mac の内部的な "マジック" について心配する必要はありませんが、内部での動作について大まかに理解することで、C# のレンズやデバッグの問題が発生したときに既存のドキュメントを解釈することができます。

Xamarin. Mac では、アプリケーションは2つのワールドを橋渡しします。これには、ネイティブクラス (、など) のインスタンスを含む、C ベースのランタイムがあり `NSString` `NSApplication` ます。また、C# ランタイムにはマネージクラスのインスタンス (、など) が含まれてい `System.String` `HttpClient` ます。 これら2つの環境間で、Xamarin は2つの方法でブリッジを作成します。これにより、アプリは目的の C (など) のメソッド (セレクター) を呼び出すことができ、 `NSApplication.Init` 目標 c はアプリの C# メソッドを呼び出すことができます (アプリデリゲートのメソッドと同様)。 一般に、目的 C への呼び出しは、 **P/invoke**を使用して透過的に処理され、一部のランタイムコードは Xamarin によって処理されます。

<a name="exposing-classes"></a>

## <a name="exposing-c-classes--methods-to-objective-c"></a>C# のクラス/メソッドを目的に公開する-C

ただし、目的 C がアプリの C# オブジェクトにコールバックする場合は、目的の C が理解できる方法で公開する必要があります。 これは、 `Register` 属性と属性を使用して行い `Export` ます。 次の例を参照してください。

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

この例では、目的の C ランタイムは、 `MyClass` およびというセレクターを持つという名前のクラスについて認識するようになりました `init` `run` 。

ほとんどの場合、これは開発者が無視できる実装の詳細です。アプリが受信するほとんどのコールバックは `base` 、クラス (、、) のオーバーライドされたメソッド `AppDelegate` 、 `Delegates` `DataSources` または api に渡される**アクション**によって行われます。 これらのすべてのケースで、 `Export` C# コードでは属性は必要ありません。

## <a name="constructor-runthrough"></a>コンストラクター runthrough

多くの場合、開発者は、アプリケーションの C# クラス構築 API を目的の C ランタイムに公開して、ストーリーボードまたは XIB ファイルで呼び出されたときなどの場所からインスタンス化できるようにする必要があります。 Xamarin. Mac アプリで使用される最も一般的な5つのコンストラクターを次に示します。

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

通常、開発者は、 `IntPtr` `NSCoder` カスタム単体などの型を作成するときに生成されるコンストラクターとコンストラクターを残しておく必要があり `NSViews` ます。 目的の C ランタイム要求への応答としてこれらのコンストラクターのいずれかを呼び出す必要があり、削除した場合、アプリはネイティブコード内でクラッシュし、問題を正確に把握することが困難になる可能性があります。

## <a name="memory-management-and-cycles"></a>メモリ管理とサイクル

Xamarin. Mac でのメモリ管理は、多くの点で Xamarin. iOS と非常によく似ています。 また、このドキュメントでは扱いません。 「[メモリとパフォーマンスのベストプラクティス](~/cross-platform/deploy-test/memory-perf-best-practices.md)」を参照してください。

## <a name="ahead-of-time-compilation"></a>事前にコンパイルする

通常、.NET アプリケーションは、ビルド時にコンピューターコードにコンパイルされません。その代わり、アプリの起動時にコンピューターコードにコンパイルされた_ジャストインタイム_(JIT) を取得する IL コードと呼ばれる中間層にコンパイルされます。

Mono ランタイムがこのマシンコードを JIT コンパイルするのに要する時間は、必要なマシンコードが生成されるまでに時間がかかるため、Xamarin. Mac アプリの起動が最大20% 遅くなる可能性があります。

IOS では、Apple によって制限されているため、IL コードの JIT コンパイルは Xamarin. iOS では使用できません。 その結果、すべての Xamarin iOS アプリがビルドサイクル中にコンピューターコードにコンパイルされ、完全に_事前_に (AOT) 実行されます。

Xamarin. Mac を初めて使用すると、Xamarin と同じように、アプリのビルドサイクル中に IL コードを AOT にすることができます。 Xamarin. Mac では、必要なマシンコードの大部分をコンパイルする_ハイブリッド_AOT アプローチを使用しますが、ランタイムは必要な trampolines をコンパイルし、柔軟にリフレクションをサポートすることができます。出力 (および、現在 Xamarin. Mac で動作するその他のユースケース)。

AOT が Xamarin. Mac アプリに役立つ2つの主要な領域があります。

- **"ネイティブ" クラッシュログの向上**-ネイティブコードで Xamarin アプリケーションがクラッシュした場合 (を受け入れないメソッドへのの送信など、Cocoa api に対して無効な呼び出しを行う場合 `null` )、JIT フレームでのネイティブクラッシュログを分析するのは困難です。 JIT フレームにはデバッグ情報がないため、16進数のオフセットを持つ複数の行が存在し、何が起こっているかがわかりません。 AOT では、"real" という名前付きフレームが生成され、トレースの方がはるかに読みやすくなります。 これはまた、Xamarin **db**や**楽器**などのネイティブツールと連携して、Xamarin アプリがより適切にやり取りすることも意味します。
- **起動時間のパフォーマンスの向上**-大規模な Xamarin. Mac アプリケーションで、2回の起動時間がある場合、すべてのコードの JIT コンパイルにかなりの時間がかかることがあります。 AOT は、事前に機能します。

### <a name="enabling-aot-compilation"></a>AOT コンパイルの有効化

Mmp で**プロジェクト****ソリューションエクスプローラー**名をダブルクリックし、 **mac ビルド**に移動して、追加の arguments: フィールドに追加することで、Xamarin. mac で aot が有効になります `--aot:[options]` (ここで**Additional mmp arguments:** `[options]` は AOT の種類を制御するための1つまたは複数のオプションについては、以下を参照してください)。 次に例を示します。

![追加の mmp 引数に AOT を追加する](how-it-works-images/aot01.png "追加の mmp 引数に AOT を追加する")

> [!IMPORTANT]
> AOT コンパイルを有効にすると、ビルド時間が大幅に長くなることがありますが、数分かかることがありますが、アプリの起動時間は平均で20% 向上します。 そのため、AOT のコンパイルは、Xamarin. Mac アプリの**リリース**ビルドでのみ有効にする必要があります。

### <a name="aot-compilation-options"></a>Aot コンパイルオプション

Xamarin. Mac アプリで AOT のコンパイルを有効にするときに調整できるオプションがいくつかあります。

- `none`-AOT のコンパイルがありません。 これが既定の設定です。
- `all`-AOT は、MonoBundle 内のすべてのアセンブリをコンパイルします。
- `core`-AOT は `Xamarin.Mac` 、、、およびの各アセンブリをコンパイル `System` `mscorlib` します。
- `sdk`-AOT は、 `Xamarin.Mac` および基本クラスライブラリ (BCL) アセンブリをコンパイルします。
- `|hybrid`-これを上記のオプションのいずれかに追加すると、IL の削除を可能にするハイブリッド AOT が有効になりますが、コンパイル時間が長くなります。
- `+`-AOT コンパイル用のファイルが1つ含まれています。
- `-`-AOT コンパイルから1つのファイルを削除します。

たとえば、は、 `--aot:all,-MyAssembly.dll` MonoBundle 内のすべてのアセンブリに対して AOT コンパイル_except_を有効にし、ハイブリッドを `MyAssembly.dll` 有効にし `--aot:core|hybrid,+MyOtherAssembly.dll,-mscorlib.dll` ます。コード AOT にはが含まれ、は除外され `MyOtherAssembly.dll` `mscorlib.dll` ます。

## <a name="partial-static-registrar"></a>部分的な静的レジスタ

Xamarin. Mac アプリの開発時に、変更を完了してテストするまでの時間を最小限に抑えることは、開発の期限を満たすために重要になる可能性があります。 コードベースと単体テストのモジュール化などの戦略を使用すると、アプリが負荷の大きい完全な再構築を必要とする回数を減らすことができるため、コンパイル時間が短縮されます。

さらに、Xamarin. Mac を初めて使用すると、_部分的な静的レジストラー_ (開拓によって実行される) によって、**デバッグ**構成での xamarin. mac アプリの起動時間が大幅に短縮されます。 部分的な静的レジスタの使用方法を理解することで、デバッグの開始における約5倍の改善を圧迫ことができます。レジスタの内容、静的と動的の相違点、およびこの "部分的な静的" バージョンの動作について少し説明します。

### <a name="about-the-registrar"></a>レジストラーについて

Xamarin. Mac アプリケーションの内部では、Apple と目的 C ランタイムの Cocoa フレームワークがあります。 C# の主要な役割は、この "ネイティブワールド" と "マネージ環境" の間にブリッジを構築することです。 このタスクの一部はレジストラーによって処理され、メソッドの内部で実行され `NSApplication.Init ()` ます。 この理由の1つは、Xamarin. Mac で Cocoa Api を使用する場合は、 `NSApplication.Init` 最初にを呼び出す必要があるためです。

レジストラーの仕事は、、、、などのクラスから派生するアプリの C# クラスが存在することを目的とする C ランタイムに通知することです `NSApplicationDelegate` `NSView` `NSWindow` `NSObject` 。 これを行うには、アプリ内のすべての種類のスキャンを行い、登録する必要がある項目と、レポートする各種類の要素を決定する必要があります。

このスキャンは、ビルド時の手順として、リフレクションを使用したアプリケーションの起動時に**動的**に実行するか、または**静的**に行うことができます。 登録の種類を選択する場合、開発者は次の点に注意する必要があります。

- 静的登録では起動時間を大幅に短縮できますが、ビルド時間が大幅に低下する可能性があります (通常は、デバッグビルド時間が2倍を超えます)。 これは、**リリース**構成ビルドの既定値になります。
- 動的登録では、アプリケーションが起動されてコード生成がスキップされるまで、この作業は遅延しますが、この追加作業によってアプリケーションの起動に顕著な一時停止 (少なくとも2秒) が発生する可能性があります。 これは、デバッグ構成のビルドで特に顕著です。既定では、動的登録が適用され、リフレクションの速度が低下します。

最初に Xamarin. iOS 8.13 で導入された部分的な静的登録では、開発者は両方のオプションを使用できます。 内のすべての要素の登録情報を事前に計算 `Xamarin.Mac.dll` し、その情報を (ビルド時にのみリンクする必要がある) スタティックライブラリの Xamarin. Mac に発送することにより、Microsoft は、ビルド時間に影響を与えずに、動的レジストラーのリフレクション時間の大半を削除しました。

### <a name="enabling-the-partial-static-registrar"></a>部分的な静的レジストラーを有効にする

**ソリューションエクスプローラー**で**プロジェクト名**をダブルクリックし、[ **mac ビルド**] に移動して `--registrar:static` 、[**追加の mmp arguments:** ] フィールドに追加することで、部分的な静的レジスタが Xamarin. Mac で有効になります。 次に例を示します。

![部分静的レジスタを追加の mmp 引数に追加する](how-it-works-images/psr01.png "部分静的レジスタを追加の mmp 引数に追加する")

## <a name="additional-resources"></a>その他の技術情報

ここでは、これらの機能が内部でどのように動作するかについて、さらに詳しく説明します。

- [目標-C セレクター](~/ios/internals/objective-c-selectors.md)
- [レジストラー](~/ios/internals/registrar.md)
- [IOS および OS X 用 Xamarin Unified API](~/cross-platform/macios/unified/index.md)
- [Theading の基礎](~/ios/app-fundamentals/threading.md)
- [デリゲート、プロトコル、およびイベント](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [に関しては`newrefcount`](~/ios/internals/newrefcount.md)
