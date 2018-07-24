---
title: System.Diagnostics.Tracing と TPL データ フローに関連する問題を解決するには
description: 'PCL ケース スタディ: Microsoft TPL データ フローの NuGet パッケージの System.Diagnostics.Tracing に関連する問題を解決する方法ですか?'
ms.prod: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.date: 04/17/2018
author: asb3993
ms.author: amburns
ms.openlocfilehash: 1acc9ccc78ad14198a59e74d1fae845790d66b16
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
ms.locfileid: "33918527"
---
# <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-package"></a>PCL ケース スタディ: Microsoft TPL データ フローの NuGet パッケージの System.Diagnostics.Tracing に関連する問題を解決する方法ですか?

> [!IMPORTANT]
> この例の`System.Diagnostic.Tracing`Xamarin の最新のバージョンで既定では不要になったすべてのエラーが生成されます。 推奨される回避策は引き続き動作すると、中に「エラーのレイヤー」セクションに記載されたバグの一部が修正されましたに注意してください。
> さらに、.NET 標準が設定されるクロスプラット フォームの .NET Api を実装することをお勧めに注意してください。

## <a name="summary"></a>まとめ

Xamarin.iOS および Xamarin.Android には、参照として、すべての PCL プロファイルの 100% が実装していません。 Xamarin プロジェクトがのみを持つ複数のプロファイルの使用を許可する Mac、Visual Studio、および、NuGet package manager 用 Visual Studio で実用的な便宜上_不完全な_実装します。 たとえば、Xamarin.iOS と Xamarin.Android のどちらも現在が含まれています"System.Diagnostics.Tracing"PCL 内の型の完全な実装には名前空間。 既定値を使用しようとしています。 この制限のため、エラーの 3 つのレイヤーに`portable-net45+win8+wpa81`Microsoft TPL データ フローの NuGet パッケージのバージョン。

## <a name="workaround-switch-the-app-project-to-reference-the-portable-net45win8wp8wpa81-version-of-the-tpl-dataflow-library"></a>回避策: スイッチ、アプリ プロジェクトを参照する、 `portable-net45+win8+wp8+wpa81` TPL データフロー ライブラリのバージョン

(これは回避エラーおよびすべての最新バージョンの Xamarin の機能の 3 つすべてのレイヤーです)。

1. アプリケーション プロジェクトを開く **.csproj**ファイルをテキスト エディターでします。

2. 次のように行を探します。

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. 変更`portable-net45+win8+wpa81`に`portable-net45+win8+wp8+wpa81`(`+wp8`が追加される)。

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### <a name="explanation"></a>説明

`portable-net45+win8+wp8+wpa81`ライブラリのバージョンを参照していません**System.Diagnostics.Tracing.dll** _まったく_問題のすべての 3 つの層を完全に回避できます。

### <a name="limitations"></a>制限事項

- `portable-net45+win8+wp8+wpa81`ライブラリのバージョンでは 100% の機能が用意されていない可能性があります、`portable-net45+win8+wpa81`バージョン。

- NuGet package manager をインストール、`portable-net45+win8+wpa81`既定では、参照を手動で調整する必要がありますので PCL NuGet パッケージのバージョン。

## <a name="details-about-the-three-layers-of-errors"></a>次の 3 つの層のエラーの詳細

1. **System.Diagnostics.Tracing.dll**ファサード アセンブリは現在 Xamarin.Android (パブリックでないバグ 34888) のすべての Mac バージョンから消え、不在がち、すべての Xamarin.iOS バージョン 9.0 より低い (または XamarinVS 3.11.1443 より低いからWindows) に (固定[バグ 32388](https://bugzilla.xamarin.com/show_bug.cgi?id=32388))。 この問題は発生してリンカーの配置ターゲットに応じて次のエラーのいずれかの設定。

    - Xamarin.Android.Common.targets: エラー: アセンブリの読み込み中に例外: System.IO.FileNotFoundException: アセンブリを読み込むことができませんでした ' System.Diagnostics.Tracing、バージョン 4.0.0.0、Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a を =' です。 おそらく Android プロファイルの Mono に存在しないか。

    - ファイルまたはアセンブリ 'System.Diagnostics.Tracing' またはその依存関係の 1 つを読み込めませんでした。 指定されたファイルが見つかりません。 (System.IO.FileNotFoundException)

    - MTOUCH: エラー MT3001: AOT アセンブリではない可能性があります '/Users/macuser/Projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mtouch-cache/64/Build/System.Threading.Tasks.Dataflow.dll '

    - MTOUCH: エラー MT2002: アセンブリを解決できませんでした: ' System.Diagnostics.Tracing, Version 4.0.0.0、Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a を ='

2. 現在["System.Diagnostics.Tracing"内の型の Mono 実装](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs)一部のメソッド オーバー ロードがありません ([バグ 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337))。 この問題は、次のリンカー エラーのいずれかの Xamarin アプリを構築するときになります。

    - /Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: error : Error executing task LinkAssemblies: error XA2006: Reference to metadata item 'System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])' (defined in 'System.Threading.Tasks.Dataflow, Version=4.5.24.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a') from 'System.Threading.Tasks.Dataflow, Version=4.5.24.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' could not be resolved.

    - MTOUCH: エラー MT2002: から"System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])"の参照を解決できませんでした"System.Diagnostics.Tracing、バージョン = 4.0.0.0、Culture = neutral, PublicKeyToken =b03f5f7f11d50a3a"

3. 現在["System.Diagnostics.Tracing"内の型の Mono 実装](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs)もは現在、_空_「ダミー」の実装 ([バグ 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890))。 実行時にこれらのメソッドを使用するあらゆる試みにそのため、予期しない結果が生成可能性があります。 _特定_ケースへの呼び出しを Microsoft TPL データフロー ライブラリの思えます`WriteEvent(System.Int32,System.Object[])`ライブラリの動作のほとんどの必須ではないため、「レイヤー 2」の修正プログラム ([バグ 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)、空の追加実装) が Microsoft TPL データ フローのほとんどの用途のための十分な場合があります。

## <a name="questions--answers"></a>質問と回答

### <a name="i-was-able-to-leave-linking-enabled-with-the-codeportable-net45win8wpa81code-version-of-the-library-on-older-versions-of-xamarinios-or-on-xamarinandroid-how-did-that-work"></a>有効にするリンクのままにすることができました、 <code>portable-net45+win8+wpa81</code> Xamarin.iOS の古いバージョンや Xamarin.Android 上のライブラリのバージョン。 その動作をどのようにしましたか。

#### <a name="answer"></a>応答

_考えられる_、ビルドの完全な「正常」(有効になっているリンク) の Xamarin.iOS の旧バージョンでまたは取得 Xamarin.Android Mac 上でへの参照を含める場合、 `System.Diagnostics.Tracing.dll` _のアセンブリを参照_\[1\]ではなく、_ファサード アセンブリ_ \[2]、残念なこととは「適切」問題を回避します。 参照アセンブリにのみ作成するときに使用するもの_ポータブル ライブラリ_アプリのようにプラットフォーム固有ではないコードです。 しようとして_実行_(に対して構築ではなく) の参照をアセンブリに含まれるコードは予期しない結果を生成する可能性があります。 モノラルをチームに欠落している追加の修正プログラムになります`WriteEvent(System.Int32,System.Object[])`をオーバー ロード、 [ `EventSource` ](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs)型 ([バグ 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337))。 ようになりました、最適なオプションに切り替えるには、`portable-net45+win8+wp8+wpa81`回避策に説明したように、Microsoft TPL データフロー ライブラリのバージョン。

(StackOverflow から関連する古いより簡単に取得応答の確認後にこの記事の内容を読み取り中する他のユーザー (<http://stackoverflow.com/a/23591322/2561894>)、参照アセンブリとファサード アセンブリの違いが注_いない_触れられています)。

**\[1\] 「アセンブリ参照」の場所**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (モノラル): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] 「ファサード アセンブリ」の場所**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

Mac (モノラル): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`


### <a name="will-it-help-if-i-manually-add-a-reference-to-the-systemdiagnosticstracing-facade-assembly"></a>その場合に役立ちます"System.Diagnostics.Tracing"ファサード アセンブリへの参照を手動で追加しますか。

_具体的にはこれら 2 つの手順を使用して問題を解決するのですか。_

1. _コピー、`System.Diagnostics.Tracing.dll`ファサード フォルダーにアセンブリ、アプリケーション プロジェクトから、次の場所のいずれか。_

    Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (モノラル): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. _Xamarin.iOS または Xamarin.Android アプリケーション プロジェクトでは、ファサード アセンブリへの参照を追加します。_

#### <a name="answer"></a>応答

いいえ、このことができます。

- Xamarin.iOS 9.0 または Xamarin.Android Windows 上の任意の最新バージョンでは、この回避策は厳密に冗長性があり「と同じ id でアセンブリ 'System.Diagnostics.Tracing' が既にインポートされています。」のようなコンパイル エラーが発生する可能性があります。

- この回避策が役立つ Xamarin.iOS 8.10 または低いまたは Xamarin.Android Mac 上が_のみ_「レイヤー 1」不足しているアセンブリの問題。 _いない_のため完全なソリューションではありません「レイヤー 2」のリンカー エラーを解決します。

### <a name="can-i-use-the-systemdiagnosticstracing-nuget-packagehttpswwwnugetorgpackagessystemdiagnosticstracing-to-solve-the-problem"></a>使用することができます、 [System.Diagnostics.Tracing NuGet パッケージ](https://www.nuget.org/packages/System.Diagnostics.Tracing/)問題を解決しますか?

#### <a name="answer"></a>応答

いいえ、NuGet 3.0"System.Diagnostics.Tracing"パッケージには、"DNXCore50"と"netcore50"のプラットフォーム固有の実装にはのみが含まれます。 明示的に_省略_Xamarin.Android ("MonoAndroid") と Xamarin.iOS ("MonoTouch"および"xamarinios") の実装です。 つまり、パッケージをインストールする必要は_影響しない_Xamarin.Android および Xamarin.iOS プロジェクト。 NuGet パッケージは、これらのプラットフォームの両方を提供する前提としています。 その_独自_型の実装です。 この想定はモノラルがあるという意味では「適切」 _、_ 実装は、名前空間の下で説明したポイントとして\#2 と\#詳細」エラーの 3 つのレイヤーについて、上記の 3、実装が完全ではありません。 適切な修正されるため、解決するのには、Mono チームの[バグ 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)と[バグ 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)です。

## <a name="next-steps"></a>次の手順

詳細については、問い合わせ先、または、上記の情報を使用した後もこの問題が残っている場合を参照してください[どのようなサポート オプションは、Xamarin を使用しますか?](~/cross-platform/troubleshooting/support-options.md)推奨事項は、の連絡先のオプションについて方法だけでなく必要な場合は、新しいバグをファイルです。
