---
title: System.Diagnostics.Tracing と TPL データ フローに関連する問題を解決するには
description: 'PCL のケース スタディ: Microsoft TPL Dataflow NuGet パッケージの System.Diagnostics.Tracing に関連する問題の解決方法'
ms.prod: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.date: 04/17/2018
author: asb3993
ms.author: amburns
ms.openlocfilehash: d9aa85b946f20addb7d69c559bff68c6b1f75429
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669844"
---
# <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-package"></a>PCL のケース スタディ: Microsoft TPL Dataflow NuGet パッケージの System.Diagnostics.Tracing に関連する問題の解決方法

> [!IMPORTANT]
> この例の`System.Diagnostic.Tracing`Xamarin の最新バージョンでは既定ですべてのエラーが生成されなくなった。 推奨される回避策は機能しますが、中には、いくつかの「層のエラー」セクションで説明されているバグが修正されましたに注意してください。
> さらに、.NET Standard がクロスプラット フォーム対応 .NET Api を実装する方法に注意してください。

## <a name="summary"></a>まとめ

Xamarin.iOS および Xamarin.Android では、参照として、すべての PCL プロファイルの 100% は実装されていません。 Xamarin プロジェクトを Visual Studio for Mac、Visual Studio、および NuGet パッケージ マネージャー、実用的なしやすくするためには、のみを持つ複数のプロファイルの使用を許可する_不完全な_実装します。 たとえば、Xamarin.iOS と Xamarin.Android のどちらも現在が含まれています"System.Diagnostics.Tracing"PCL 内の型の完全な実装には名前空間。 既定値を使用しようとしています。 この制限のため、エラーの 3 つの層に`portable-net45+win8+wpa81`Microsoft TPL Dataflow NuGet パッケージのバージョン。

## <a name="workaround-switch-the-app-project-to-reference-the-portable-net45win8wp8wpa81-version-of-the-tpl-dataflow-library"></a>回避策:参照するアプリのプロジェクトを切り替え、 `portable-net45+win8+wp8+wpa81` TPL データフロー ライブラリのバージョン

(これにより、エラーと Xamarin のすべての最新バージョンの動作のすべての 3 つの層です)。

1. アプリケーション プロジェクトを開く **.csproj**テキスト エディターでファイル。

2. 次のように行を見つけます。

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. 変更`portable-net45+win8+wpa81`に`portable-net45+win8+wp8+wpa81`(`+wp8`が追加されます)。

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### <a name="explanation"></a>説明

`portable-net45+win8+wp8+wpa81`ライブラリのバージョンを参照しない**System.Diagnostics.Tracing.dll** _まったく_問題の 3 つすべての層を完全に回避できます。

### <a name="limitations"></a>制限事項

- `portable-net45+win8+wp8+wpa81`ライブラリのバージョンに 100% 機能にはが含まれていない可能性があります、`portable-net45+win8+wpa81`バージョン。

- NuGet パッケージ マネージャーをインストール、 `portable-net45+win8+wpa81` PCL NuGet パッケージのバージョン、既定で、参照を手動で調整する必要があります。

## <a name="details-about-the-three-layers-of-errors"></a>次の 3 つの層のエラーの詳細

1. **System.Diagnostics.Tracing.dll**ファサード アセンブリは、現在 Xamarin.Android (パブリックでないバグ 34888) のすべての Mac バージョンから存在し、存在しないすべての Xamarin.iOS のバージョン 9.0 よりも低い (を上回るか下回る XamarinVS 3.11.1443 からWindows) に (固定[バグ 32388](https://bugzilla.xamarin.com/show_bug.cgi?id=32388))。 この問題と、配置ターゲットとリンカーによっては、次のエラーのいずれかの設定。

    - Xamarin.Android.Common.targets:エラー :アセンブリの読み込み中に例外:System.IO.FileNotFoundException:アセンブリを読み込むことができません ' System.Diagnostics.Tracing、バージョン = 4.0.0.0、Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a'。 おそらく Mono for Android のプロファイルに存在しないか。

    - ファイルまたはアセンブリ 'System.Diagnostics.Tracing' またはその依存関係の 1 つを読み込めませんでした。 指定されたファイルが見つかりません。 (System.IO.FileNotFoundException)

    - MTOUCH: エラー MT3001:AOT アセンブリではない可能性があります '/Users/macuser/Projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mtouch-cache/64/Build/System.Threading.Tasks.Dataflow.dll '

    - MTOUCH: エラー MT2002:アセンブリを解決できませんでした。'System.Diagnostics.Tracing, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'

2. 現在["System.Diagnostics.Tracing"内の型の Mono 実装](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs)一部のメソッド オーバー ロードがありません ([バグ 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337))。 この問題が Xamarin アプリを構築するときに、次のリンカー エラーのいずれかです。

    - /Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: エラー。LinkAssemblies タスクの実行エラー: エラー XA2006:メタデータへの参照を 'System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])' の項目 (で定義されている' System.Threading.Tasks.Dataflow、バージョン 4.5.24.0、カルチャを = = neutral, PublicKeyToken = b03f5f7f11d50a3a')' System.Threading.Tasks.Dataflow、バージョン = 4.5.24.0、Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a' を解決できませんでした。

    - MTOUCH: エラー MT2002:"System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])"参照から解決できませんでした"System.Diagnostics.Tracing、バージョン = 4.0.0.0、Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a"

3. 現在["System.Diagnostics.Tracing"内の型の Mono 実装](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs)もは現在、_空_「ダミー」の実装 ([バグ 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890))。 実行時にこれらのメソッドを使用するとは、予期しない結果を生成ため可能性があります。 _特定_ケースの Microsoft TPL データフロー ライブラリへの呼び出しに見えます`WriteEvent(System.Int32,System.Object[])`、ライブラリの動作のほとんどは必須ではないため、「レイヤー 2」の修正プログラム ([バグ 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)、空の追加実装) は、ほとんどの Microsoft TPL Dataflow ユース ケースのための十分ながあります。

## <a name="questions--answers"></a>質問と回答

### <a name="i-was-able-to-leave-linking-enabled-with-the-codeportable-net45win8wpa81code-version-of-the-library-on-older-versions-of-xamarinios-or-on-xamarinandroid-how-did-that-work"></a>リンクでは有効のままにすることができました、<code>portable-net45+win8+wpa81</code>古いバージョンの Xamarin.iOS または Xamarin.Android ライブラリのバージョン。 その作業をどのようにしましたか。

#### <a name="answer"></a>応答

_可能_を取得するビルドを完全な「正常」(有効になっているリンクの) の古いバージョンの Xamarin.iOS または Xamarin.Android Mac 上でへの参照を含める場合、 `System.Diagnostics.Tracing.dll` _アセンブリを参照_\[1\]なく_ファサード アセンブリ_ \[2]、残念ながらこれは「正しい」問題を回避します。 参照アセンブリが構築するときに使用するもののみ_ポータブル ライブラリ_アプリのようなプラットフォーム固有ではないコードです。 しようとして_実行_(それに対して構築ではなく) 参照アセンブリに含まれるコードが予期しない結果を生成する可能性があります。 修正プログラムが不足している追加の Mono チームのなります`WriteEvent(System.Int32,System.Object[])`をオーバー ロード、 [ `EventSource` ](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs)型 ([バグ 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337))。 最適なオプションに切り替えるには今すぐ、`portable-net45+win8+wp8+wpa81`回避策に説明したように、Microsoft TPL データフロー ライブラリのバージョン。

(任意のユーザー、StackOverflow の関連する場合、古い回答の確認後にこの記事を読むことがあります (<https://stackoverflow.com/a/23591322/2561894>)、参照アセンブリとファサード アセンブリが区別されたに注意してください_いない_説明があります)。

**\[1\] 「参照アセンブリ」の場所**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (Mono) の場合: `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] 「ファサード アセンブリ」の場所**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

Mac (Mono) の場合: `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`


### <a name="will-it-help-if-i-manually-add-a-reference-to-the-systemdiagnosticstracing-facade-assembly"></a>役に立つ"System.Diagnostics.Tracing"ファサード アセンブリへの参照を手動で追加しますか。

_これら 2 つの手順を使用して問題を解決しました具体的にはか。_

1. _コピー、`System.Diagnostics.Tracing.dll`ファサード アセンブリを次の場所のいずれかからアプリケーションのプロジェクト フォルダーに。_

    Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (Mono) の場合: `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. _Xamarin.iOS または Xamarin.Android アプリケーションのプロジェクトでは、ファサード アセンブリへの参照を追加します。_

#### <a name="answer"></a>応答

いいえ、これが役に立ちません。

- Xamarin.iOS 9.0 または任意の最新バージョンの Windows で Xamarin.Android では、この回避策は厳密に冗長性があり「同じ id を持つアセンブリ 'System.Diagnostics.Tracing' が既にインポートされています。」のようなコンパイル エラーが発生する可能性があります。

- Xamarin.iOS 8.10 以下または Mac 上で Xamarin.Android では、この回避策は役立ちますが、_のみ_「レイヤー 1」不足しているアセンブリの問題をします。 _いない_完全なソリューションではないため、「レイヤー 2」リンカー エラーを解決します。

### <a name="can-i-use-the-systemdiagnosticstracing-nuget-packagehttpswwwnugetorgpackagessystemdiagnosticstracing-to-solve-the-problem"></a>使用することができます、 [System.Diagnostics.Tracing NuGet パッケージ](https://www.nuget.org/packages/System.Diagnostics.Tracing/)問題を解決するでしょうか。

#### <a name="answer"></a>応答

いいえ、NuGet 3.0"System.Diagnostics.Tracing"パッケージには、プラットフォーム固有の実装"に DNXCore50"と"netcore50"のみが含まれます。 明示的に_省略_("MonoAndroid") の Xamarin.Android と Xamarin.iOS ("MonoTouch"および"xamarinios") を実装します。 つまり、パッケージをインストールする必要がある_影響しない_Xamarin.Android と Xamarin.iOS プロジェクト用。 NuGet パッケージは、これらのプラットフォームの両方が提供している前提としています。 その_独自_型の実装。 この想定は Mono がという意味で「正しい」 _、_ 実装が下に説明したポイントとして、名前空間の\#2 と\#上、「3 つの層のエラーの詳細」の 3 つ、実装は完全ではありません。 解決するのには Mono チームの適切な修正になるよう[バグ 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)と[バグ 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)します。

## <a name="next-steps"></a>次の手順

問い合わせ、または上記の情報を使用した後でもこの問題が残っている場合を参照してください、詳細については[Xamarin のどのようなサポート オプションを使用しますか?](~/cross-platform/troubleshooting/support-options.md)連絡先オプション、推奨事項は、についてする方法についても必要な場合は、新しいバグをファイルします。
