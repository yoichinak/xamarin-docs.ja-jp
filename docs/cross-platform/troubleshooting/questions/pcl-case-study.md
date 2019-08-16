---
title: トレースおよび TPL データフローに関連する問題を解決する
description: 'PCL のケース スタディ: Microsoft TPL Dataflow NuGet パッケージの System.Diagnostics.Tracing に関連する問題の解決方法'
ms.prod: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.date: 04/17/2018
author: asb3993
ms.author: amburns
ms.openlocfilehash: 09aef14efdce93e28326deb78292da98f1969ea1
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69521566"
---
# <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-package"></a>PCL のケース スタディ: Microsoft TPL Dataflow NuGet パッケージの System.Diagnostics.Tracing に関連する問題の解決方法

> [!IMPORTANT]
> この特定の`System.Diagnostic.Tracing`例では、Xamarin の最新バージョンでは既定でエラーは生成されなくなりました。 推奨される回避策は引き続き機能しますが、「エラーのレイヤー」セクションで説明されているいくつかのバグが修正されていることに注意してください。
> さらに、.NET Standard は、クロスプラットフォームの .NET Api を実装するための推奨される方法であることに注意してください。

## <a name="summary"></a>Summary

Xamarin iOS と Xamarin Android は、参照として許可されているすべての PCL プロファイルの 100% を実装していません。 Visual Studio for Mac、Visual Studio、NuGet パッケージマネージャーでの実用的な利便性のために、Xamarin プロジェクトでは、実装が_不完全_な複数のプロファイルを使用できます。 たとえば、現在、Xamarin と Xamarin Android のどちらにも、"system.string" という PCL 名前空間の型の完全な実装が含まれていません。 この制限により、Microsoft TPL データフロー NuGet パッケージの既定`portable-net45+win8+wpa81`のバージョンを使用しようとすると、3つの層のエラーが発生します。

## <a name="workaround-switch-the-app-project-to-reference-the-portable-net45win8wp8wpa81-version-of-the-tpl-dataflow-library"></a>回避策:TPL データフローライブラリの`portable-net45+win8+wp8+wpa81`バージョンを参照するようにアプリプロジェクトを切り替える

(これにより、3つのエラーレイヤーすべてが回避され、すべての最新バージョンの Xamarin で動作します)。

1. アプリケーションプロジェクトの **.csproj**ファイルをテキストエディターで開きます。

2. 次のような行を探します。

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. を ( `portable-net45+win8+wp8+wpa81` `portable-net45+win8+wpa81` 追加`+wp8` ) に変更します。

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### <a name="explanation"></a>説明

ライブラリのバージョンでは、トレースがまったく参照されていないので、3つのすべての問題を完全に回避できます。 `portable-net45+win8+wp8+wpa81`

### <a name="limitations"></a>制限事項

- ライブラリ`portable-net45+win8+wp8+wpa81`のバージョンに、 `portable-net45+win8+wpa81`バージョンの機能のうち 100% が含まれていない可能性があります。

- 既定では、nuget パッケージ`portable-net45+win8+wpa81`マネージャーによって PCL NuGet パッケージのバージョンがインストールされるため、手動で参照を調整する必要があります。

## <a name="details-about-the-three-layers-of-errors"></a>3層のエラーの詳細

1. 現在 、XamarinVS ファサードアセンブリは、すべての Mac バージョンの Xamarin. Android (非パブリックバグ 34888) に含まれておらず、9.0 より低い (または Windows 上の3.11.1443 よりも低い) すべての Xamarin. iOS バージョンからは存在しません。[バグ 32388](https://bugzilla.xamarin.com/show_bug.cgi?id=32388))。 この問題が発生すると、配置ターゲットとリンカーの設定に応じて、次のいずれかのエラーが発生します。

    - Xamarin... targets:エラー :アセンブリの読み込み中に例外が発生しました:FileNotFoundException:アセンブリ ' 4.0.0.0, Version =, Culture = ニュートラル, PublicKeyToken = b03f5f7f11d50a3a ' を読み込むことができませんでした。 Mono for Android プロファイルには存在しない可能性がありますか。

    - ファイルまたはアセンブリ ' System. Diagnostics. Tracing ' またはその依存関係の1つを読み込めませんでした。 指定されたファイルが見つかりません。 (FileNotFoundException)

    - MTOUCH: エラー MT3001:アセンブリ '/Users/macuser/Projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mtouch-cache/64/Build/System.Threading.Tasks.Dataflow.dll ' を AOT にできませんでした

    - MTOUCH: エラー MT2002:アセンブリを解決できませんでした:' 4.0.0.0, Version =, Culture = ニュートラル, PublicKeyToken = b03f5f7f11d50a3a '

2. "27337 [" の型の現在の Mono 実装](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs)に、いくつかのメソッドオーバーロードがありません ([バグ](https://bugzilla.xamarin.com/show_bug.cgi?id=27337))。 この問題が発生すると、Xamarin アプリをビルドするときに、次のいずれかのリンカーエラーが発生します。

    - /Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: エラー:タスク LinkAssemblies の実行エラー: エラー XA2006:メタデータ項目 ' WriteEvent:: (system.string, System.object []) ' への参照。 (' 4.5.24.0, Version =, Culture = ニュートラル, PublicKeyToken = b03f5f7f11d50a3a ' で定義されています) です。 (' ')' 4.5.24.0, Version =, Culture = ニュートラル, PublicKeyToken = b03f5f7f11d50a3a ' から解決できませんでした。

    - MTOUCH: エラー MT2002:"WriteEvent, Version = 4.0.0.0, Culture = ニュートラル, PublicKeyToken = b03f5f7f11d50a3a" からの "System.void:: (system.string, System.object [])" の参照を解決できませんでした ("" というエラーが発生しました)

3. "34890 [" の型の現在の Mono 実装](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs)も、現在、_空_の "ダミー" の実装 ([バグ](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)) です。 このため、これらのメソッドを実行時に使用しようとすると、予期しない結果が生じる可能性があります。 Microsoft TPL データフローライブラリの_特定_のケースで`WriteEvent(System.Int32,System.Object[])`は、の呼び出しがほとんどのライブラリの動作にとって重要ではないように見えます。そのため、"レイヤー 2" ([バグ 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)、空の実装の追加) の修正は十分であると考えられます。ほとんどの Microsoft TPL データフローのユースケース。

## <a name="questions--answers"></a>質問 & 回答

### <a name="i-was-able-to-leave-linking-enabled-with-the-portable-net45win8wpa81-version-of-the-library-on-older-versions-of-xamarinios-or-on-xamarinandroid-how-did-that-work"></a>以前のバージョンの xamarin. iOS または`portable-net45+win8+wpa81` xamarin Android で、ライブラリのバージョンでリンクを有効にしたままにすることができました。 どのように動作するのでしょうか。

#### <a name="answer"></a>受信

以前のバージョンの xamarin または xamarin では、ビルドが "正常に完了" (リンクが有効になっている状態) になる_ことがあり_ます`System.Diagnostics.Tracing.dll` 。_参照アセンブリ_ \[\]への参照を含める場合は、MacのAndroidです。_ファサード_アセンブリ\[2] ではなく、残念ながら、これは "正しい" 回避策ではありません。 参照アセンブリは、アプリのようなプラットフォーム固有のコードではなく、_ポータブルライブラリ_をビルドする場合にのみ使用することを目的としています。 (単にビルドするのではなく) 参照アセンブリに含まれているコードを_実行_しようとすると、予期しない結果が生じる可能性があります。 適切な修正は、不足`WriteEvent(System.Int32,System.Object[])`しているオーバーロードを Mono チームが[`EventSource`](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs)型に追加することです ([バグ 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337))。 ここでは、前の「回避策」 `portable-net45+win8+wp8+wpa81`セクションで説明したように、Microsoft TPL データフローライブラリのバージョンに切り替えることをお勧めします。

(Stackoverflow (<https://stackoverflow.com/a/23591322/2561894>) からより簡単に関連する古い応答を確認した後に、この記事を読んでいる可能性がある人には、参照アセンブリとファサードアセンブリの区別が示されて_いない_ことに注意してください)。

**\[1\] "参照アセンブリ" の場所**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (Mono):`/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] "ファサードアセンブリ" の場所**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

Mac (Mono):`/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`


### <a name="will-it-help-if-i-manually-add-a-reference-to-the-systemdiagnosticstracing-facade-assembly"></a>"System.string" ファサードアセンブリへの参照を手動で追加した場合に役立ちますか?

_特に、この2つの手順を使用して問題を解決することはできますか。_

1. _次のいずれかの場所から、ファサードアセンブリをアプリケーションプロジェクトフォルダーにコピーします。`System.Diagnostics.Tracing.dll`_

    Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (Mono):`/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. _Xamarin または Xamarin. Android アプリケーションプロジェクトで、ファサードアセンブリへの参照を追加します。_

#### <a name="answer"></a>受信

いいえ、これは役に立ちません。

- Xamarin. iOS 9.0 または Windows 上の最新バージョンの Xamarin. Android では、この回避策は厳密には冗長であり、同じ id を持つ "アセンブリ ' system.string ' は既にインポートされています" と同様のコンパイルエラーが発生する可能性があります。

- Xamarin. iOS 8.10 またはそれ以降、または Mac 上の Xamarin Android の場合、この回避策は、"レイヤー 1" の見つからないアセンブリの問題に対して_のみ_役立ちます。 "レイヤー 2" リンカーエラーは解決され_ない_ため、完全な解決策ではありません。

### <a name="can-i-use-the-systemdiagnosticstracing-nuget-packagehttpswwwnugetorgpackagessystemdiagnosticstracing-to-solve-the-problem"></a>[トレース NuGet パッケージ](https://www.nuget.org/packages/System.Diagnostics.Tracing/)を使用して問題を解決することはできますか。

#### <a name="answer"></a>受信

いいえ。 NuGet 3.0 "DNXCore50" パッケージには、"" および "netcore50" のプラットフォーム固有の実装のみが含まれています。 これは、Xamarin android ("モノ Android") と Xamarin ("Monotouch.dialog" および "xamarinios") の実装を明示的に_省略_します。 つまり、パッケージをインストールしても、Xamarin および Xamarin の iOS プロジェクトには_影響しません_。 NuGet パッケージでは、これらの両方のプラットフォームが型の_独自_の実装を提供していることを前提としています。 この前提は、Mono には名前空間の実装があることを意味する "正しい" ことですが、 \#上記の\#3 つのエラー層の詳細については、この実装は現在、まま. そのため、Mono チームが[バグ 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)と[バグ 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)を解決するには、適切な修正が必要です。

## <a name="next-steps"></a>次の手順

詳細については、お問い合わせください。または、上記の情報を利用した後もこの問題が発生する場合は、「 [Xamarin で使用できるサポートオプション](~/cross-platform/troubleshooting/support-options.md)」を参照してください。連絡先オプション、提案、および必要に応じて新しいバグをファイルに登録する方法については、こちらを参照してください.
