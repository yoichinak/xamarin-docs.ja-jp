---
title: Xamarin.iOS エラー
description: このドキュメントでは、mtouch、Xamarin.iOS アプリケーション バンドルを使用するツールによって生成されるさまざまなエラーについて説明します。 エラーはコードによって一覧表示され、完全な説明を指定します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9F76162B-D622-45DA-996B-2FBF8017E208
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: e9332ba34f113f56859065c74c24c116a331eceb
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789448"
---
# <a name="xamarinios-errors"></a>Xamarin.iOS エラー

## <a name="mt0xxx-mtouch-error-messages"></a>MT0xxx: mtouch エラー メッセージ

たとえば、 パラメーター、環境、ツールがありません。

<!--
 MT0xxx mtouch itself, e.g. parameters, environment (e.g. missing tools)
 https://github.com/xamarin/xamarin-macios/blob/master/tools/mtouch/error.cs
    -->

<a name="MT0000" />

### <a name="mt0000-unexpected-error---please-fill-a-bug-report-at-httpbugzillaxamarincom"></a>MT0000: 予期しないエラーでバグ報告を入力してください。 http://bugzilla.xamarin.com

予期しないエラーが発生しました。 ください[バグ レポートの提出](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)できるだけ多くの情報を含みます。

* 冗長性を最大にビルド ログが完全 (例:`-v -v -v -v`で、**追加 mtouch 引数**) です。
* エラーを再現最小限テスト_ケースそして
* すべてのバージョン情報

正確なバージョン情報を取得する最も簡単な方法を使用する、 **Visual Studio for Mac** ] メニューの [**に関する Visual Studio for Mac**項目、**詳細の表示**ボタンをクリックし、コピー/貼り付け、バージョン情報 (使用することができます、**コピー情報**ボタン)。

<a name="MT0001" />

### <a name="mt0001--devname-was-provided-without-any-device-specific-action"></a>MT0001: '-devname' 任意のデバイスに固有の操作なしに指定されました

これは、ときに何もデバイスに固有の mtouch - devname が渡された場合に生成される警告 (-logdev/installdev/killdev/launchdev/-listapps) が要求されました。

<a name="MT0002" />

### <a name="mt0002-could-not-parse-the-environment-variable-"></a>MT0002: 環境変数を解析できませんでした * です。

このエラーは、無効な環境キーを設定しようとする場合は発生変数の値のペアを = です。 正しい形式です。 `mtouch --setenv=VARIABLE=VALUE`

<a name="MT0003" />

### <a name="mt0003-application-name-exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MT0003: アプリケーション名 '* .exe' SDK または製品アセンブリ (.dll) 名と競合しています。

実行可能アセンブリの名前と、アプリケーションの名前は、アプリで作成された dll の名前と一致ことはできません。 実行可能ファイルの名前を変更してください。

<a name="MT0004" />

### <a name="mt0004-new-refcounting-logic-requires-sgen-to-be-enabled-too"></a>MT0004: 新しいカウント ロジック SGen も有効にする必要があります。

カウントの拡張機能を有効にした場合も、プロジェクトのビルド オプション (詳細設定 タブ)、iOS で SGen ガベージ コレクターを有効にする必要があります。

Xamarin.iOS 7.2.1 始まるこの要件が解除されて、新しいカウント ロジックは Boehm および SGen ガベージ コレクターの両方で有効にすることができます。

<a name="MT0005" />

### <a name="mt0005-the-output-directory--does-not-exist"></a>MT0005: 出力ディレクトリ * 存在しません。

ディレクトリを作成してください。

今後このエラーは生成されません、mtouch が自動的に作成、ディレクトリが存在しない場合。

<a name="MT0006" />

### <a name="mt0006-there-is-no-devel-platform-at--use---platformplat-to-specify-the-sdk"></a>MT0006: devel プラットフォームではありません *、--プラットフォームを使用して、SDK を指定する PLAT を = です。

Xamarin.iOS には、エラー メッセージに示された場所に SDK ディレクトリを見つけることができません。 パスが正しいことを確認してください。

<a name="MT0007" />

### <a name="mt0007-the-root-assembly--does-not-exist"></a>MT0007: ルート アセンブリ * 存在しません。

Xamarin.iOS には、エラー メッセージに示されている場所でアセンブリを見つけることができません。 パスが正しいことを確認してください。

<a name="MT0008" />

### <a name="mt0008-you-should-provide-one-root-assembly-only-found--assemblies-"></a>MT0008: する必要がありますいずれかのルート アセンブリのみ、見つかった # のアセンブリを指定します。 * です。

1 つ以上のルート アセンブリは、1 つしかルート アセンブリがあっても、mtouch に渡されました。

<a name="MT0009" />

### <a name="mt0009-error-while-loading-assemblies-"></a>MT0009: アセンブリの読み込み中にエラーが: * です。

ルート アセンブリの参照アセンブリの読み込み中にエラーが発生しました。 詳細については、ビルド出力で指定することがあります。

<a name="MT0010" />

### <a name="mt0010-could-not-parse-the-command-line-arguments-"></a>MT0010: コマンドライン引数を解析できませんでした: * です。

コマンドライン引数の解析中にエラーが発生しました。 これらがすべて正しいことを確認してください。

<a name="MT0011" />

### <a name="mt0011--was-built-against-a-more-recent-runtime--than-monotouch-supports"></a>MT0011: * MonoTouch をサポートしているより新しいランタイム (*) に対してビルドされました。

この警告は通常、プロジェクトが構築されていませんでした Xamarin.iOS BCL を使用してクラス ライブラリを参照しているために報告されます。

のみ、.NET 2.0 をサポートするシステムで .NET 4.0 SDK を使用して、アプリが機能しない場合と同様では、Xamarin.iOS で .NET 4.0 を使用して作成されたライブラリが機能しない、Xamarin.iOS で API が存在しないを使用、可能性があります。

一般的なソリューションでは、Xamarin.iOS クラス ライブラリとして、ライブラリを作成します。 これは新しい Xamarin.iOS クラス ライブラリ プロジェクトを作成することで実現でき、すべてのソース ファイルを追加します。 ライブラリのソース コードがない、おりする場合は、ベンダーに問い合わせて、Xamarin.iOS と互換性のあるバージョンのライブラリを提供することを要求します。

<a name="MT0012" />

### <a name="mt0012-incomplete-data-is-provided-to-complete-"></a>MT0012: 不完全なデータが指定されて完了 * です。

このエラーは、Xamarin.iOS の現在のバージョンでもはや報告されていません。

<a name="MT0013" />

### <a name="mt0013-profiling-support-requires-sgen-to-be-enabled-too"></a>MT0013: プロファイルのサポート、sgen も有効にする必要があります。

SGen (--sgen) プロファイリングする場合に有効にする必要があります (--プロファイリング) を有効にします。

<a name="MT0014" />

### <a name="mt0014-the-ios--sdk-does-not-support-building-applications-targeting-"></a>MT0014: iOS * SDK が対象とするアプリケーションの構築をサポートしていません * です。

これは、次のような状況で発生することができます。

*  ARMv6 が有効になっているし、Xcode 4.5 以降がインストールされています。
*  ARMv7s が有効になっているし、Xcode 4.4 以降がインストールされています。

Xcode のインストールされたバージョンが選択されているアーキテクチャをサポートしていることを確認してください。

<a name="MT0015" />

### <a name="mt0015-invalid-abi--supported-abis-are-i386-x8664--armv7-armv7llvm-armv7llvmthumb2-armv7s-armv7sllvm-armv7sllvmthumb2-arm64-and-arm64llvm"></a>MT0015: 無効な ABI: * です。 サポートされている ABIs されます: i386、x86_64、常に armv7、常に armv7 + ある llvm、常に armv7 ある llvm + thumb2、armv7s、armv7s + ある llvm、armv7s + ある llvm + thumb2、arm64 と arm64 + ある llvm です。

無効な ABI は、mtouch に渡されました。 有効な ABI を指定してください。

<a name="MT0016" />

### <a name="mt0016-the-option--has-been-deprecated"></a>MT0016: オプション * は廃止されました。

説明したように mtouch オプションは廃止されておりは無視されます。

<a name="MT0017" />

### <a name="mt0017-you-should-provide-a-root-assembly"></a>MT0017: ルート アセンブリを指定する必要があります。

ルート アセンブリ (通常、メインの実行可能ファイル) を指定する必要がアプリを構築するときにします。

<a name="MT0018" />

### <a name="mt0018-unknown-command-line-argument-"></a>MT0018: 不明なコマンドライン引数: * です。

Mtouch では、エラー メッセージに記載されているコマンドラインの引数は認識されません。

<a name="MT0019" />

### <a name="mt0019-only-one---loginstallkilllaunchdev-or---launchdebugsim-option-can-be-used"></a>MT0019: 1 つのみ--[ログ | インストール | kill | 起動] デベロッパーまたは--[起動 | デバッグ] sim オプションを使用できます。

同時に使用することはできません mtouch のいくつかのオプションがあります。

-  --logdev
-  --installdev
-  --killdev
-  --launchdev
-  --launchdebug
-  --launchsim

<a name="MT0020" />

### <a name="mt0020-the-valid-options-for--are-"></a>MT0020: に対して有効なオプション '\*'は'\*' です。

<a name="MT0021" />

### <a name="mt0021-cannot-compile-using-gccg---use-gcc-when-using-the-static-registrar-this-is-the-default-when-compiling-for-device-either-remove-the---use-gcc-flag-or-use-the-dynamic-registrar---registrardynamic"></a>MT0021: は、gcc を使用してコンパイルできません/g++ (--使用 gcc) (これは既定のデバイス用にコンパイルするときに) 静的なレジストラーを使用する場合。 削除するか、--使用 gcc フラグまたは動的なレジストラーを使用して (--レジストラー: ダイナミック)。

<a name="MT0022" />

### <a name="mt0022-the-options---unsupported--enable-generics-in-registrar-and---registrar-are-not-compatible"></a>MT0022: オプション '- サポートされていない--有効にする-ジェネリック-で-レジストラー' および '--レジストラー' は互換性がありません。

両方のオプションを削除する`--unsupported--enable-generics-in-registrar`と`--registrar`です。 既定レジストラー Xamarin.iOS 7.2.1 に開始すると、ジェネリックがサポートされています。

このエラーは表示されなくなります (コマンドライン引数`--unsupported--enable-generics-in-registrar`mtouch から削除されました)。

<a name="MT0023" />

### <a name="mt0023-application-name-exe-conflicts-with-another-user-assembly"></a>MT0023: アプリケーション名 '* .exe' 別のユーザー アセンブリと競合しています。

実行可能アセンブリの名前と、アプリケーションの名前は、アプリで作成された dll の名前と一致ことはできません。 実行可能ファイルの名前を変更してください。

<a name="MT0024" />

### <a name="mt0024-could-not-find-required-file-"></a>MT0024: 必要なファイルが見つかりませんでした ' *'。

<a name="MT0025" />

### <a name="mt0025-no-sdk-version-was-provided-please-add---sdkxy-to-specify-which-ios-sdk-should-be-used-to-build-your-application"></a>MT0025: SDK バージョンは指定されませんでした。 追加してください。`--sdk=X.Y`を指定する iOS アプリケーションをビルドする SDK を使用する必要があります。

<a name="MT0026" />

### <a name="mt0026-could-not-parse-the-command-line-argument--"></a>MT0026: コマンドライン引数を解析できませんでした ' *': *

<a name="MT0027" />

### <a name="mt0027-the-options--and--are-not-compatible"></a>MT0027: オプション\*'および'\*' は互換性がありません。

<a name="MT0028" />

### <a name="mt0028-cannot-enable-pie--pie-when-targeting-ios-41-or-earlier-please-disable-pie--piefalse-or-set-the-deployment-target-to-at-least-ios-42"></a>MT0028:、円グラフが有効にすることはできません (の円) iOS 4.1 またはそれ以前を対象とする場合。 円グラフを無効にしてください (-円: false) に、少なくとも、配置ターゲットを設定または iOS 4.2

<a name="MT0029" />

### <a name="mt0029-repl---enable-repl-is-only-supported-in-the-simulator---sim"></a>MT0029: REPL (--有効 repl) は、シミュレーターでのみサポート (--sim)。

REPL は、シミュレーターの場合をビルドする場合にのみサポートされます。 つまり、渡した場合`--enable-repl`mtouch にも渡す必要があります`--sim`です。

<a name="MT0030" />

### <a name="mt0030-the-executable-name--and-the-app-name--are-different-this-may-prevent-crash-logs-from-getting-symbolicated-properly"></a>MT0030: 実行可能ファイル名 (\*) とアプリ名 (\*) が異なる、これもされなくなるクラッシュ ログ正しく symbolicated を取得します。

Xcode の symbolicates とき (関数名とファイルと行番号へのメモリ アドレスを変換します)、実行可能ファイルとアプリ (拡張子なし) は異なる名前を付ける場合に、処理が失敗する可能性があります。

解決するには、プロジェクトのビルド/iOS アプリケーションのオプション、またはプロジェクトのビルド/出力 オプションの変更 'アセンブリ名' で 'アプリケーション名' を変更このいずれか。

<a name="MT0031" />

### <a name="mt0031-the-command-line-arguments---enable-background-fetch-and---launch-for-background-fetch-require---launchsim-too"></a>MT0031: コマンドライン引数 '--有効にする に background fetch' および'--起動用に background fetch' が必要 '--launchsim' すぎます。

<a name="MT0032" />

### <a name="mt0032-the-option---debugtrack-is-ignored-unless---debug-is-also-specified"></a>MT0032: オプション '--debugtrack' 場合を除き、無視されます'--デバッグ ' も指定されています。

<a name="MT0033" />

### <a name="mt0033-a-xamarinios-project-must-reference-either-monotouchdll-or-xamariniosdll"></a>MT0033: monotouch.dll または Xamarin.iOS.dll、Xamarin.iOS プロジェクトを参照する必要があります。

<a name="MT0034" />

### <a name="mt0034-cannot-include-both-monotouchdll-and-xamariniosdll-in-the-same-xamarinios-project----is-referenced-explicitly-while--is-referenced-by-"></a>MT0034: 同じ Xamarin.iOS プロジェクトでの 'monotouch.dll' と 'Xamarin.iOS.dll' の両方を含めることはできません '\*' は、明示的に参照中に '\*' によって参照される ' *'。

<!-- MT0035 unused -->

<a name="MT0036" />

### <a name="mt0036-cannot-launch-a--simulator-for-a--app-please-enable-the-correct-architectures-in-your-projects-ios-build-options-advanced-page"></a>MT0036: を起動できません、* 用のシミュレーターで、* アプリ。 プロジェクトの iOS ビルド オプション (詳細設定 ページ) で正しいミラーサイトを有効にしてください。

<a name="MT0037" />

### <a name="mt0037-monotouchdll-is-not-64-bit-compatible-either-reference-xamariniosdll-or-do-not-build-for-a-64-bit-architecture-arm64-andor-x8664"></a>MT0037: monotouch.dll には、64 ビット互換性です。 Xamarin.iOS.dll を参照するか (ARM64 や x86_64) は、64 ビット アーキテクチャを構築しないでください。

<a name="MT0038" />

### <a name="mt0038-the-old-registrars---registraroldstaticolddynamic-are-not-supported-when-referencing-xamariniosdll"></a>MT0038: 古いレジストラー (--レジストラー: oldstatic | olddynamic) Xamarin.iOS.dll を参照するときにサポートされていません。

<a name="MT0039" />

### <a name="mt0039-applications-targeting-armv6-cannot-reference-xamariniosdll"></a>MT0039: ARMv6 を対象とするアプリケーションは Xamarin.iOS.dll を参照することはできません。

<a name="MT0040" />

### <a name="mt0040-could-not-find-the-assembly--referenced-by-"></a>MT0040: アセンブリが見つかりませんでした '\*', によって参照される'\*' です。

<a name="MT0041" />

### <a name="mt0041-cannot-reference-both-monotouchdll-and-xamariniosdll"></a>MT0041:、'monotouch.dll' と 'Xamarin.iOS.dll' の両方を参照できません。

<a name="MT0042" />

### <a name="mt0042-no-reference-to-either-monotouchdll-or-xamariniosdll-was-found-a-reference-to-monotouchdll-will-be-added"></a>MT0042: monotouch.dll または Xamarin.iOS.dll への参照が見つかりませんでした。 Monotouch.dll への参照が追加されます。

<a name="MT0043" />

### <a name="mt0043-the-boehm-garbage-collector-is-currently-not-supported-when-referencing-xamariniosdll-the-sgen-garbage-collector-has-been-selected-instead"></a>MT0043: Boehm ガベージ コレクターは現在サポートされていません 'Xamarin.iOS.dll' を参照するときにします。 SGen ガベージ コレクターは、代わりに選択されています。

統合プロジェクト SGen ガベージ コレクターのみがサポートされます。 ガベージ コレクターとして Boehm を指定する追加 mtouch フラグがないことを確認します。

<a name="MT0044" />

### <a name="mt0044---listsim-is-only-supported-with-xcode-60-or-later"></a>MT0044:--listsim は Xcode 6.0 またはそれ以降にのみサポートされます。

新しい Xcode バージョンをインストールします。

<a name="MT0045" />

### <a name="mt0045---extension-is-only-supported-when-using-the-ios-80-or-later-sdk"></a>MT0045:--iOS を使用する場合にのみ拡張機能はサポートされて 8.0 (またはそれ以降) の SDK です。

<!-- MT0046 is not reported anymore -->

<a name="MT0047" />

### <a name="mt0047-the-minimum-deployment-target-for-unified-applications-is-511-the-current-deployment-target-is--please-select-a-newer-deployment-target-in-your-projects-ios-application-options"></a>MT0047: 統合アプリケーションの最小の配置のターゲットは 5.1.1、現在の配置ターゲットは ' *'。 プロジェクトの iOS アプリケーションのオプションで、新しい配置ターゲットを選択してください。

<!-- MT0048 is not reported anymore -->

<a name="MT0049" />

### <a name="mt0049-framework-is-supported-only-if-deployment-target-is-80-or-later--features-might-not-work-correctly"></a>MT0049: 配置ターゲットが 8.0 以降である場合にのみ、*.framework はサポートされています。 * 機能が正しく動作しない可能性があります。

指定されたフレームワークは、配置ターゲットを指す iOS のバージョンでサポートされていません。 新しい iOS のバージョンでは、配置ターゲットを更新するか、アプリからの指定されたフレームワークを使用して削除します。

<!-- MT0050 is not reported anymore -->

<a name="MT0051" />

### <a name="mt0051-xamarinios--requires-xcode-50-or-later-the-current-xcode-version-found-in--is-"></a>MT0051: Xamarin.iOS * Xcode 5.0 以降が必要です。 現在の Xcode のバージョン (で見つかった *) は、* です。

新しい Xcode をインストールします。

<a name="MT0052" />

### <a name="mt0052-no-command-specified"></a>MT0052: 指定されたコマンドがありません。

Mtouch のアクションが指定されていません。

<!-- 0053 is used by mmp -->

<a name="MT0054" />

### <a name="mt0054-unable-to-canonicalize-the-path--"></a>MT0054: パスの正規化にできません ' *': *。

これは、内部エラーです。 バグを報告してください。 このエラーが発生する場合、 [ http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT0055" />

### <a name="mt0055-the-xcode-path--does-not-exist"></a>MT0055: Xcode パス ' *' がありません。

使用して、Xcode パスが渡される`--sdkroot`存在しません。 有効なパスを指定してください。

<a name="MT0056" />

### <a name="mt0056-cannot-find-xcode-in-the-default-location-applicationsxcodeapp-please-install-xcode-or-pass-a-custom-path-using---sdkroot-path"></a>MT0056: は、既定の場所で Xcode を見つけることができません (/Applications/Xcode.app)。 Xcode をインストールしてください--sdkroot を使用してカスタム パスを渡す<path>です。

<a name="MT0057" />

### <a name="mt0057-cannot-determine-the-path-to-xcodeapp-from-the-sdk-root--please-specify-the-full-path-to-the-xcodeapp-bundle"></a>MT0057: は、sdk のルートから Xcode.app にパスを決定することはできません ' *'。 Xcode.app バンドルへの完全パスを指定してください。

使用して渡されるパス`--sdkroot`Xcode の有効なアプリケーションが指定されていません。 Xcode アプリへのパスを指定してください。

<a name="MT0058" />

### <a name="mt0058-the-xcodeapp--is-invalid-the-file--does-not-exist"></a>MT0058: Xcode.app '\*' が正しくありません (ファイル '\*' がありません)。

使用して渡されるパス`--sdkroot`Xcode の有効なアプリケーションが指定されていません。 Xcode アプリへのパスを指定してください。

<a name="MT0059" />

### <a name="mt0059-could-not-find-the-currently-selected-xcode-on-the-system-"></a>MT0059: で、システム上の現在選択されている Xcode を見つけることができませんでした *。

<a name="MT0060" />

### <a name="mt0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned--but-that-directory-does-not-exist"></a>MT0060: は、システムで現在選択されている Xcode を見つけられませんでした。 'xcode 選択--印刷パス' が返されます ' *' が、そのディレクトリは存在しません。

<a name="MT0061" />

### <a name="mt0061-no-xcodeapp-specified-using---sdkroot-using-the-system-xcode-as-reported-by-xcode-select---print-path-"></a>MT0061: Xcode.app (--sdkroot を使用)、指定された 'xcode 選択--印刷パス' によって報告されたシステム Xcode を使用します *。

これとなる Xcode を説明する情報の警告、none が指定されているために使用します。

<a name="MT0062" />

### <a name="mt0062-no-xcodeapp-specified-using---sdkroot-or-xcode-select---print-path-using-the-default-xcode-instead-"></a>MT0062: Xcode.app (を使用して指定--sdkroot または ' xcode 選択--印刷パス ')、既定の Xcode を代わりに使用します *。

これとなる Xcode を説明する情報の警告、none が指定されているために使用します。

<a name="MT0063" />

### <a name="mt0063-cannot-find-the-executable-in-the-extension--no-cfbundleexecutable-entry-in-its-infoplist"></a>: MT0063 拡張機能で実行可能ファイルを見つけることができません * (Info.plist で CFBundleExecutable エントリがありません)

すべての Info.plist では、エントリは、ビルド時に自動的に生成する必要がありますが、(CFBundleExecutable エントリを使用して) 実行可能ファイル、なることが必要です。

これは通常 Xamarin.iOS; のバグを示しますバグ報告を送信してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)とテスト_ケースをします。

<a name="MT0064" />

### <a name="mt0064-xamarinios-only-supports-embedded-frameworks-with-unified-projects"></a>MT0064: Xamarin.iOS には、埋め込まれたフレームワーク統合プロジェクトでのみサポートしています。

Xamarin.iOS のみをサポートしている組み込みフレームワークを Unified API を使用する場合Unified API を使用して、プロジェクトを更新してください。

<a name="MT0065" />

### <a name="mt0065-xamarinios-only-supports-embedded-frameworks-when-deployment-target-is-at-least-80-current-deployment-target--embedded-frameworks-"></a>MT0065: Xamarin.iOS のみをサポートする埋め込みのフレームワークの配置先が 8.0 以上である場合 (現在の配置ターゲット: * フレームワークを埋め込み: *)

Xamarin.iOS のみをサポートしている組み込みフレームワークがしたとき、配置ターゲットには、少なくとも 8.0 (以前のバージョンの iOS は、埋め込みのフレームワークをサポートしていません)。

8.0 以降に、プロジェクトの Info.plist で配置ターゲットを更新してください。

<a name="MT0066" />

### <a name="mt0066-invalid-build-registrar-assembly-"></a>MT0066: 無効なビルド レジストラー アセンブリ: *

これは通常 Xamarin.iOS; のバグを示しますバグ報告を送信してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)とテスト_ケースをします。

<a name="MT0067" />

### <a name="mt0067-invalid-registrar-"></a>MT0067: 無効なレジストラー: *

これは通常 Xamarin.iOS; のバグを示しますバグ報告を送信してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)とテスト_ケースをします。

<a name="MT0068" />

### <a name="mt0068-invalid-value-for-target-framework-"></a>MT0068: ターゲット フレームワークに無効な値: * です。

使用して無効なターゲット フレームワークが渡されました: ターゲット フレームワークの引数。 有効なターゲット フレームワークを指定してください。

<a name="MT0069" />

<!--### MT0069: currently unused -->

<a name="MT0070" />

### <a name="mt0070-invalid-target-framework--valid-target-frameworks-are-"></a>MT0070: 無効なターゲット フレームワーク: * です。 有効なターゲット フレームワークが: * です。

使用して無効なターゲット フレームワークが渡されました: ターゲット フレームワークの引数。 有効なターゲット フレームワークを指定してください。

<a name="MT0071" />

### <a name="mt0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinios-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT0071: 不明なプラットフォーム: * です。 これは通常 Xamarin.iOS; のバグを示しますバグ報告を送信してください http://bugzilla.xamarin.com とテスト_ケースをします。

これは通常 Xamarin.iOS; のバグを示しますバグ報告を送信してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)とテスト_ケースをします。

<a name="MT0072" />

### <a name="mt0072-extensions-are-not-supported-for-the-platform-"></a>MT0072: プラットフォームの拡張はサポートされていない ' *'。

これは通常 Xamarin.iOS; のバグを示しますバグ報告を送信してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)とテスト_ケースをします。

<a name="MT0073" />

### <a name="mt0073-xamarinios--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a>MT0073: Xamarin.iOS * の配置ターゲットをサポートしていません * (最小値は *)。 プロジェクトの Info.plist で新しい配置ターゲットを選択してください。

エラー メッセージに指定されている最小の配置ターゲットプロジェクトの Info.plist で新しい配置ターゲットを選択してください。

配置ターゲットの更新が可能でない場合は、Xamarin.iOS の古いバージョンを使用ししてください。

<a name="MT0074" />

### <a name="mt0074-xamarinios--does-not-support-a-minimum-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinios"></a>MT0074: Xamarin.iOS * の最小の配置ターゲットをサポートしていません * (最大値は *)。 プロジェクトの Info.plist で以前の配置ターゲットを選択するか、Xamarin.iOS の新しいバージョンにアップグレードしてください。

Xamarin.iOS では、Xamarin.iOS のこの特定のバージョン用にビルドされたバージョンより新しいバージョンに、最小の配置ターゲットを設定することはできません。

プロジェクトの Info.plist で古いの最小の配置ターゲットを選択するか、Xamarin.iOS の新しいバージョンにアップグレードしてください。

<a name="MT0075" />

### <a name="mt0075-invalid-architecture--for--projects-valid-architectures-are-"></a>MT0075: 無効なアーキテクチャ ' *' の * プロジェクト。 有効なアーキテクチャは、: *

無効なアーキテクチャが指定されました。 アーキテクチャが有効であることを確認してください。

<a name="MT0076" />

### <a name="mt0075-no-architecture-specified-using-the---abi-argument-an-architecture-is-required-for--projects"></a>MT0075: (--abi 引数を使用して) 指定アーキテクチャがありません。 アーキテクチャが必要 * プロジェクト。

これは通常 Xamarin.iOS; のバグを示しますバグ報告を送信してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)とテスト_ケースをします。

<a name="MT0077" />

### <a name="mt0076-watchos-projects-must-be-extensions"></a>MT0076: WatchOS プロジェクトは、拡張機能である必要があります。

これは通常 Xamarin.iOS; のバグを示しますバグ報告を送信してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)とテスト_ケースをします。

<a name="MT0078" />

### <a name="mt0077-incremental-builds-are-enabled-with-a-deployment-target--80-currently--this-is-not-supported-the-resulting-application-will-not-launch-on-ios-9-so-the-deployment-target-will-be-set-to-80"></a>MT0077: インクリメンタル ビルドが配置対象 < 8.0 で有効になって (現在 *)。 これはサポートされていません (結果として得られるアプリケーションは起動せずに iOS 9)、配置ターゲットを 8.0 に設定されます。

これは、インクリメンタルに作業を正しくビルドされるように、配置ターゲットがこのビルドの 8.0 に設定されていることを通知する警告です。

インクリメンタル ビルドには、(結果として得られるアプリケーションは、それ以外の場合 iOS 9 では起動しない) ため、配置ターゲットが少なくとも 8.0 のみサポートされます。

<a name="MT0079" />

### <a name="mt0078-the-recommended-xcode-version-for-xamarinios--is-xcode--or-later-the-current-xcode-version-found-in--is-"></a>MT0078: 推奨 Xcode のバージョン Xamarin.iOS * は、Xcode * またはそれ以降。 現在の Xcode のバージョン (で見つかった *) は、* です。

これは、Xcode の現在のバージョンは、Xamarin.iOS のこのバージョンの Xcode の推奨されるバージョンではないことを通知する警告です。

最適な動作を確実に Xcode をアップグレードしてください。

<a name="MT0080" />

### <a name="mt0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MT0080: NewRefCount 無効にすると、--新しい-refcount:false は推奨されなくなりました。

これは、通知する警告を無効に新しい要求 refcount (--新しい - refcount:false) は無視されました。

参照カウントの新機能すべてのプロジェクトに対して必須ともはやを無効にするにはそのため。

<a name="MT0081" />

### <a name="mt0081-the-command-line-argument---download-crash-report-also-requires---download-crash-report-to"></a>MT0081: コマンドライン引数--ダウンロード クラッシュ レポートも必要です--ダウンロードでクラッシュ レポート-をします。

<a name="MT0082" />

### <a name="mt0082-repl---enable-repl-is-only-supported-when-linking-is-not-used---nolink"></a>MT0082: REPL (--有効 repl) はリンクを使用しない場合にのみサポート (--nolink)。

<a name="MT0083" />

### <a name="mt0083-asm-only-bitcode-is-not-supported-on-watchos-use-either---bitcodemarker-or---bitcodefull"></a>MT0083: Asm 専用 bitcode は watchOS ではサポートされていません。 いずれかの--bitcode を使用して: マーカーまたは--bitcode: 完全です。

<a name="MT0084" />

### <a name="mt0084-bitcode-is-not-supported-in-the-simulator-do-not-pass---bitcode-when-building-for-the-simulator"></a>MT0084: Bitcode は、シミュレーターではサポートされていません。 シミュレーターを作成するときに、--bitcode を渡さないでください。

<a name="MT0085" />

### <a name="mt0085-no-reference-to--was-found-it-will-be-added-automatically"></a>MT0085: の参照なしに ' *' が見つかりませんでした。 自動的に追加されます。

<a name="MT0086" />

### <a name="mt0086-a-target-framework---target-framework-must-be-specified-when-building-for-tvos-or-watchos"></a>MT0086: ターゲット フレームワーク (--ターゲット フレームワーク) TVOS または WatchOS をビルドするときに指定する必要があります。

これは通常 Xamarin.iOS; のバグを示しますバグ報告を送信してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)とテスト_ケースをします。

<a name="MT0087" />

### <a name="mt0087-incremental-builds---fastdev-is-not-supported-with-the-boehm-gc-incremental-builds-will-be-disabled"></a>MT0087: インクリメンタル ビルド (--fastdev) Boehm GC ではサポートされません。 インクリメンタル ビルドは無効になります。

<a name="MT0088" />

### <a name="mt0088-the-gc-must-be-in-cooperative-mode-for-watchos-apps-please-remove-the---coopfalse-argument-to-mtouch"></a>MT0088: GC は、watchOS アプリの協調モードである必要があります。 削除してください - coop: false 引数 mtouch にします。

<a name="MT0089" />

### <a name="mt0089-the-option--cannot-take-the-value--when-cooperative-mode-is-enabled-for-the-gc"></a>MT0089: オプション '\*'、値を取ることはできません'\*' GC の協調動作モードが有効な場合です。

<a name="MT0091" />

### <a name="mt0091-this-version-of-xamarinios-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-set-the-managed-linker-behaviour-to-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MT0091: Xamarin.iOS のこのバージョンので、* SDK (Xcode に同梱されて *)。 か、必要なヘッダー ファイルを取得またはリンク フレームワーク Sdk には、のみ (新しい Api を回避しようとしてください) を管理されたリンカー動作を設定する Xcode をアップグレードします。

Xamarin.iOS には、アプリケーションのビルドに、エラー メッセージで指定された SDK のバージョンからのヘッダー ファイルが必要です。 このエラーを解決するための推奨方法は、必要な SDK を取得する Xcode にアップグレードする、これは、すべての必要なヘッダー ファイルが含まれます。 インストールされている、Xcode のバージョンが複数ある場合や既定以外の場所に、Xcode を使用する場合は場合、は、IDE の設定 で Xcode の正しい場所を設定することを確認してください。

代替ソリューションがマネージ リンカーを有効にするのには、潜在的なです。 これにより、使用されていない API を含む、ほとんどの場合、ヘッダー ファイルが不足している (または未完了) の新しい API が削除されます。 ただしこの場合は機能しません、プロジェクトは、Xcode 1 よりも新しい SDK で導入された API を使用して提供します。

最終 straw ソリューションは、Xamarin.iOS の古いバージョンを使用することは、SDK プロジェクトをサポートするいると、次の必要があります。

<!-- MT0092 used by mlaunch -->

<a name="MT0093" />

### <a name="mt0093-could-not-find-mlaunch"></a>MT0093: 'mlaunch' が見つかりませんでした。

<!-- MT0094 is not reported anymore -->

<a name="MT0095" />

### <a name="mt0095-aot-files-could-not-be-copied-to-the-destination-directory-dest-error"></a>MT0095: Aot ファイル コピーできませんでした {dest} インポート先ディレクトリ: {エラー}

<a name="MT0096" />

### <a name="mt0096-no-reference-to-xamariniosdll-was-found"></a>MT0096: Xamarin.iOS.dll への参照が見つかりませんでした。

<!-- MT0097: used by mmp -->
<!-- MT0098: used by mmp -->

<a name="MT0099" />

### <a name="mt0099-internal-error--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MT0099: 内部エラー * です。 テスト_ケースとバグのレポートを送信してください (http://bugzilla.xamarin.com)です。

Xamarin.iOS で内部整合性チェックが失敗したときに、このエラー メッセージが報告されます。

Xamarin.iOS; のバグを示しますバグ報告を送信してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)とテスト_ケースをします。

<a name="MT0100" />

### <a name="mt0100-invalid-assembly-build-target--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MT0100: 無効なアセンブリのビルド ターゲット: ' *'。 テスト_ケースとバグのレポートを送信してください (http://bugzilla.xamarin.com)です。

Xamarin.iOS で内部整合性チェックが失敗したときに、このエラー メッセージが報告されます。

これは、常に Xamarin.iOS; のバグバグ報告を送信してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)とテスト_ケースをします。

<a name="MT0101" />

### <a name="mt0101-the-assembly--is-specified-multiple-times-in---assembly-build-target-arguments"></a>MT0101: アセンブリ ' *'--アセンブリ build ターゲットの引数で複数回指定されています。

エラー メッセージに示されているアセンブリには、--アセンブリ build ターゲットの引数で複数回を指定します。 各アセンブリが 1 回記載されているだけを確認してください。

<a name="MT0102" />

### <a name="mt0102-the-assemblies--and--have-the-same-target-name--but-different-targets--and-"></a>MT0102: アセンブリ\*'および'\*'ターゲット名前が同じ ('\*') が異なるターゲット ('\*' および ' *')。

エラー メッセージに示されているアセンブリでは、競合するビルド ターゲットがあります。

例えば:

    --assembly-build-target:Assembly1.dll=framework=MyBinary --assembly-build-target:Assembly2.dll=dynamiclibrary=MyBinary

この例が同じため、動的なライブラリと、フレームワークの両方を作成しようとしています (`MyBinary`)。

<a name="MT0103" />

### <a name="mt0103-the-static-object--contains-more-than-one-assembly--but-each-static-object-must-correspond-with-exactly-one-assembly"></a>MT0103: 静的オブジェクト '\*' 複数のアセンブリが含まれています ('\*')、各静的オブジェクトは、正確に 1 つのアセンブリに対応する必要がありますが、します。

単一の静的オブジェクトには、エラー メッセージに示されているアセンブリのすべてがコンパイルされます。 これは許可されていません、別の静的オブジェクトにすべてのアセンブリをコンパイルする必要があります。

例えば:

    --assembly-build-target:Assembly1.dll=staticobject=MyBinary --assembly-build-target:Assembly2.dll=staticobject=MyBinary

この例は、静的オブジェクトを構築しようとしています (`MyBinary`) の 2 つのアセンブリで構成されています (`Assembly1.dll`と`Assembly2.dll`)、これは許可されません。

<a name="MT0105" />

### <a name="mt0105-no-assembly-build-target-was-specified-for-"></a>MT0105: アセンブリのビルド ターゲットが指定されていません ' *'。

ビルド時に、アセンブリの指定を使用してターゲット`--assembly-build-target`、すべてのアセンブリはアプリで割り当てられているビルド ターゲットが存在する必要があります。

このエラーは、エラー メッセージに示されているアセンブリがビルド ターゲットが割り当てられているアセンブリを持っていない場合に報告されます。

ドキュメントを参照してください`--assembly-build-target`についてさらにします。

<a name="MT0106" />

### <a name="mt0106-the-assembly-build-target-name--is-invalid-the-character--is-not-allowed"></a>MT0106: アセンブリのビルド ターゲット名 '\*' が正しくありません。 文字 '\*' は許可されていません。

アセンブリのビルド ターゲットの名前は、有効なファイル名である必要があります。

これらの値がこのエラーをトリガーする例。

    --assembly-build-target:Assembly1.dll=staticobject=my/path.o

`my/path.o`ディレクトリの区切り記号のための有効なファイル名ではありません。

<a name="MT0107" />

### <a name="mt0107-the-assemblies--have-different-custom-llvm-optimizations--which-is-not-allowed-when-they-are-all-compiled-to-a-single-binary"></a>MT0107: アセンブリ\*' がある別のカスタムのある LLVM 最適化 (\*) はすべて、コンパイル時に 1 つのバイナリにはできません。

<a name="MT0108" />

### <a name="mt0108-the-assembly-build-target--did-not-match-any-assemblies"></a>MT0108: アセンブリのビルドのターゲットは ' *' すべてのアセンブリが一致しませんでした。

<a name="MT0109" />

### <a name="mt0109-the-assembly-0-was-loaded-from-a-different-path-than-the-provided-path-provided-path-1-actual-path-2"></a>MT0109: アセンブリ '{0}' は、指定したパスとは異なるパスから読み込まれました (パスを指定: {1}、実際のパス: {2})。

これは、アプリケーションによって参照されるアセンブリが要求したよりも、別の場所から読み込まれたことを示す警告です。

アプリが (最初のアセンブリのみが使用されます)、予期しない結果になる可能性がありますが、異なる場所からでは、同じ名前を持つ複数のアセンブリを参照している可能性があります。

<a name="MT0110" />

### <a name="mt0110-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-third-party-binding-libraries-and-that-compiles-to-bitcode"></a>MT0110: インクリメンタル ビルドが無効になって Xamarin.iOS のこのバージョンでは、インクリメンタル ビルド サード パーティのバインドのライブラリが含まれていると、bitcode にコンパイルされるプロジェクトではサポートされていません。

このバージョンの Xamarin.iOS がサード パーティのバインドのライブラリが含まれていると、bitcode (tvOS と watchOS プロジェクト) にコンパイルされるプロジェクトではインクリメンタル ビルドをサポートしていないために、インクリメンタル ビルドを無効にされています。

ユーザーによる操作は必要ありません、このメッセージは単なる情報。

詳細については、次を参照してください。 bug #[51710](https://bugzilla.xamarin.com/show_bug.cgi?id=51710)です。

この警告は今後報告されていません。

<a name="MT0111" />

### <a name="mt0111-bitcode-has-been-enabled-because-this-version-of-xamarinios-does-not-support-building-watchos-projects-using-llvm-without-enabling-bitcode"></a>MT0111: Bitcode が有効になって Xamarin.iOS のこのバージョンでは、構築 watchOS はサポートされていないためにプロジェクト bitcode を有効にしなくてもある LLVM を使用します。

Bitcode が有効になって自動的にこのバージョンの Xamarin.iOS が構築 watchOS をサポートしていないためにプロジェクト bitcode を有効にしなくてもある LLVM を使用します。

ユーザーによる操作は必要ありません、このメッセージは単なる情報。

詳細については、次を参照してください。 bug #[51634](https://bugzilla.xamarin.com/show_bug.cgi?id=51634)です。

<a name="MT0112" />

### <a name="mt0112-native-code-sharing-has-been-disabled-because-"></a>MT0112: ネイティブ コードの共有は無効化されて *

複数の理由コードの共有を無効にすることができます。

* コンテナー アプリケーションの配置ターゲットが iOS 8.0 よりも古いため (が *))。

ネイティブ コードの共有が必要です iOS 8.0 ネイティブ コードの共有が実装されているため、ユーザーのフレームワークを使用して iOS 8.0 に導入されました。

* コンテナーのアプリには、I18N アセンブリ (*) が含まれています。

ネイティブ コードの共有は現在サポートされていません、コンテナーのアプリに I18N アセンブリが含まれている場合。

* コンテナーのアプリがあるためマネージ リンカー (*) のカスタムの xml の定義です。

ネイティブ コードを共有する場合は、マネージ リンカーのカスタムの xml 定義を使用するプロジェクトではサポートされていませんする必要があります。

<a name="MT0113" />

### <a name="mt0113-native-code-sharing-has-been-disabled-for-the-extension--because-"></a>MT0113: ネイティブ コードの共有が無効になっている拡張機能の ' *' ため * です。

* bitcode オプションは、コンテナー アプリ間のみが異なるため (\*) と拡張機能 (\*)。

  ネイティブ コードを共有するには、コードを共有するプロジェクト間で bitcode オプションが一致する必要があります。

* -アセンブリのビルドのターゲットは、コンテナーのアプリ間で別のオプション (\*) と拡張機能 (\*)。

  ネイティブ コードの共有が必要です-アセンブリのビルドのターゲットのオプションは同じコードを共有するプロジェクトです。

  この条件は、いずれかのない有効または無効にすべてのプロジェクトでの増分をビルドする場合に発生します。

* I18N アセンブリは、コンテナーのアプリ間で異なるため (\*) と拡張機能 (\*)。

  ネイティブ コードの共有は現在サポートされていません I18N アセンブリを組み込んで拡張機能です。

* AOT コンパイラに対する引数は、コンテナーのアプリ間で異なるため (\*) と拡張機能 (\*)。

  ネイティブ コードを共有するには、AOT コンパイラに引数が、コードを共有するプロジェクト間で違いはないことが必要です。

* AOT コンパイラに他の引数は、コンテナーのアプリ間で異なるため (\*) と拡張機能 (\*)。

  ネイティブ コードを共有するには、AOT コンパイラに引数が、コードを共有するプロジェクト間で違いはないことが必要です。

  この条件は、プロジェクト間で異なる、' あらゆる操作を行う 32 ビット浮動小 64 ビット浮動小数点として' 場合に発生します。

* ある LLVM が有効になっても行われないためコンテナー アプリの両方で無効になっています (\*) と拡張機能 (\*)。

  ネイティブ コードを共有するには、ある LLVM が有効になっているなどのコードを共有するすべてのプロジェクトに対して無効になっている必要があります。

* マネージ リンカー設定がコンテナー アプリ間で異なるため (\*) と拡張機能 (\*)。

  ネイティブ コードを共有するには、マネージ リンカー設定がコードを共有するすべてのプロジェクトと同じである必要があります。

* マネージ リンカーのスキップされたアセンブリは、コンテナーのアプリ間で異なるため (\*) と拡張機能 (\*)。

  ネイティブ コードを共有するには、マネージ リンカー設定がコードを共有するすべてのプロジェクトと同じである必要があります。

* 拡張機能があるためマネージ リンカー (*) のカスタムの xml の定義です。

  ネイティブ コードを共有する場合は、マネージ リンカーのカスタムの xml 定義を使用するプロジェクトではサポートされていませんする必要があります。

* ABI のコンテナーのアプリを構築していないため * (作業中の拡張機能はこの ABI の構築) します。

  ネイティブ コードを共有するには、任意のアプリの拡張機能のビルドのすべてのアーキテクチャのコンテナーのアプリがビルドされる必要があります。

  インスタンス: この状態 ARM64 + ARMv7、構築、拡張機能が、for ARM64 だけコンテナー アプリがビルドされる場合に発生します。

* ABI のコンテナーのアプリを構築ため\*、拡張機能の ABI と互換性がありません (\*)。

  ネイティブ コードを共有するには、すべてのプロジェクトが、正確な同じ API を構築することが必要です。

  インスタンス: この状態の拡張機能が常に ARMv7 + ある llvm thumb2 のビルドしますが、常に ARMv7 + ある llvm だけコンテナー アプリをビルドする場合に発生します。

* コンテナー アプリケーションでアセンブリを参照しているため '\*'from'\*' 拡張機能は異なるバージョンの参照中に、' *'。

  ネイティブ コードを共有するには、すべてのアセンブリのコードを共有するすべてのプロジェクトが同じバージョンを使用する必要があります。

<!-- MT0114: used by mmp -->

<a name="MT0115" />

### <a name="mt0115-it-is-recommended-to-reference-dynamic-symbols-using-code---dynamic-symbol-modecode-when-bitcode-is-enabled"></a>MT0115: 推奨のコードを使用して動的なシンボルを参照する (--ダイナミック記号モード コード) bitcode が有効な場合です。

Xamarin.iOS プロジェクトは多くの場合、参照ネイティブ シンボルが動的につまりネイティブ リンカーがネイティブのリンクの処理中にこのようなネイティブ シンボルを削除可能性がありますネイティブ リンカーでこれらのシンボルを使用することが表示されないためです。

Xamarin.iOS がこのようなシンボルを保持するネイティブ リンカーを要求する通常 (を使用して、`-u symbol`リンカー フラグ)、ときに、ネイティブ リンカー bitcode 用にコンパイルするは受け付けられません、`-u`フラグ。

Xamarin.iOS が代替ソリューションを実装します。 これらのシンボルを参照する余分なネイティブ コードを生成し、したがってネイティブ リンカーがわかります。 これらのシンボルが使用されています。 これは、bitcode をコンパイルするときに自動的に行います。

場合`--dynamic-symbol-mode=linker`mtouch、ソリューションは無効にする、および Xamarin.iOS を渡そうとすると、この代わりに渡される`-u`ネイティブ リンカーにします。 これは、がネイティブ リンカー エラーになります可能性があります。

解決策は、削除、`--dynamic-symbol-mode=linker`追加 mtouch 引数は、プロジェクトのビルド オプションの引数。

<!-- 0116 - 0124: free to use -->

<a name="MT0116" />

### <a name="mt0116-invalid-architecture-arch-32-bit-architectures-are-not-supported-when-deployment-target-is-11-or-later-make-sure-the-project-does-not-build-for-a-32-bit-architecture"></a>MT0116: 無効なアーキテクチャ: {arch}。 配置ターゲットが 11 以降の場合は、32 ビット アーキテクチャはサポートされていません。 32 ビット アーキテクチャ用のプロジェクトがビルドできないことを確認してください。

iOS 11 では、配置ターゲットが iOS 11 以降である場合、32 ビット アプリケーションをビルドすることはできませんので、32 ビット アプリケーションのサポートは含まれません。

Arm64 をプロジェクトの iOS のビルド オプションで、ターゲット アーキテクチャを変更するか、以前の iOS バージョンに、プロジェクトの Info.plist で配置ターゲットを変更します。

<a name="MT0117" />

### <a name="mt0117-cant-launch-a-32-bit-app-on-a-simulator-that-only-supports-64-bit"></a>MT0117:、64 ビットのみをサポートするシミュレーター上の 32 ビット アプリケーションを起動できません。

<a name="MT0118" />

### <a name="mt0118-aot-files-could-not-be-found-at-the-expected-directory-msymdir"></a>MT0118: Aot ファイル見つかりませんでした '{msymdir}' の予期されるディレクトリにします。

<!-- 0119 - 0123: free to use -->

<a name="MT0123" />

### <a name="mt0123-the-executable-assembly--does-not-reference-"></a>MT0123: 実行可能アセンブリ * を参照していません * です。

プラットフォーム アセンブリへの参照が見つかりませんでした (Xamarin.iOS.dll/Xamarin.TVOS.dll/Xamarin.WatchOS.dll)、実行可能アセンブリにします。

これは通常発生プラットフォーム アセンブリから何かを使用する実行可能プロジェクトでコードがないです。インスタンスのこのエラーは、空の Main メソッド (およびその他のコードはなし) には表示。

```csharp
class Program {
    void Main (string[] args)
    {
    }
}
```

プラットフォーム アセンブリの API を使用すると、エラーを解決します。

```csharp
class Program {
    void Main (string[] args)
    {
        System.Console.WriteLine (typeof (UIKit.UIWindow));
    }
}
```

<a name="MT0124" />

### <a name="mt0124-could-not-set-the-current-language-to-lang-according-to-langlang-exception"></a>MT0124: '{lang}' を現在の言語を設定できませんでした (LANG に従って = {LANG}): {例外}

これは、警告、エラー メッセージの言語に現在の言語を設定できなかったことを示すです。

現在の言語は、システムの言語に設定されます。

<a name="MT0125" />

### <a name="mt0125-the---assembly-build-target-command-line-argument-is-ignored-in-the-simulator"></a>MT0125:-アセンブリのビルド ターゲットをシミュレーターでコマンドライン引数は無視されます。

操作は必要ありません、このメッセージは単なる情報。

<a name="MT0126" />

### <a name="mt0126-incremental-builds-have-been-disabled-because-incremental-builds-are-not-supported-in-the-simulator"></a>MT0126: インクリメンタル ビルドが無効になっていますインクリメンタル ビルドは、シミュレーターではサポートされていないためです。

操作は必要ありません、このメッセージは単なる情報。

<a name="MT0127" />

### <a name="mt0127-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-more-than-one-third-party-binding-libraries"></a>MT0127: インクリメンタル ビルドが無効になって Xamarin.iOS のこのバージョンでは、インクリメンタル ビルドを複数のいずれかのサード パーティのバインドのライブラリを含むプロジェクトでサポートされていません。

Xamarin.iOS のこのバージョンは常に正常にビルドされないサードパーティのバインドの複数のライブラリ プロジェクトであるために、自動的にインクリメンタル ビルドを無効にされています。

操作は必要ありません、このメッセージは単なる情報。

詳細については、次を参照してください。 bug #[52727](https://bugzilla.xamarin.com/show_bug.cgi?id=52727)です。

<a name="MT0128" />

### <a name="mt0128-could-not-touch-the-file--"></a>MT0128:、ファイルでした触れないで ' *': *

(これは部分的なビルドが正しく行われることを確認する) ファイルに触れたときにエラーが発生しました。

この警告は無視できます可能性があります。問題が発生した場合に、バグをファイル (https://bugzilla.xamarin.com] (https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS))し、それが調査されます。

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx: プロジェクトに関連するエラー メッセージ

### <a name="mt10xx-installer--mtouch"></a>MT10xx: インストーラー/mtouch

<!--
 MT1xxx file copy / symlinks (project related)
  MT10xx installer.cs / mtouch.cs
  -->

<a name="MT1001" />

### <a name="mt1001-could-not-find-an-application-at-the-specified-directory"></a>MT1001: で、指定されたディレクトリでのアプリケーションを見つけることができませんでした。

<a name="MT1002" />

### <a name="mt1002-could-not-create-symlinks-files-were-copied"></a>MT1002: シンボリック リンクを作成できませんでした、ファイルがコピーされました。

<a name="MT1003" />

### <a name="mt1003-could-not-kill-the-application--you-may-have-to-kill-the-application-manually"></a>MT1003: に、アプリケーションが強制終了しませんでした ' *'。 アプリケーションを手動で強制終了する必要があります。

<a name="MT1004" />

### <a name="mt1004-could-not-get-the-list-of-installed-applications"></a>MT1004: は、インストールされているアプリケーションの一覧を取得できませんでした。

<a name="MT1005" />

### <a name="mt1005-could-not-kill-the-application--on-the-device----you-may-have-to-kill-the-application-manually"></a>MT1005: に、アプリケーションが強制終了しませんでした '\*'on 'デバイス\*': *-アプリケーションを手動で強制終了する必要があります。

<a name="MT1006" />

### <a name="mt1006-could-not-install-the-application--on-the-device--"></a>MT1006: アプリケーションをインストールできませんでした '\*'on 'デバイス\*': * です。

<a name="MT1007" />

### <a name="mt1007-failed-to-launch-the-application--on-the-device---you-can-still-launch-the-application-manually-by-tapping-on-it"></a>MT1007: アプリケーションの起動に失敗しました '\*'on 'デバイス\*': * です。 タップして手動でアプリケーションを起動することができますも。

<a name="MT1008" />

### <a name="mt1008-failed-to-launch-the-simulator"></a>MT1008: シミュレーターを起動できませんでした。

Mtouch がシミュレーターの起動に失敗した場合、このエラーは報告されます。   これ場合もありますシミュレーターが古い場合または停止しているプロセスの実行が既に存在します。

Unix のコマンドラインで発行される次のコマンドは、スタック シミュレーター プロセスを強制終了を使用できます。

```bash
$ launchctl list|grep UIKitApplication|awk '{print $3}'|xargs launchctl remove
```

<a name="MT1009" />

### <a name="mt1009-could-not-copy-the-assembly--to--"></a>MT1009: アセンブリをコピーできませんでした '\*'to'\*': *

これは、Xamarin.iOS の特定のバージョンの既知の問題です。

このような場合は、次の回避策を試してください。

```bash
sudo chmod 0644 /Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/*/*.mdb
```

ただし、Xamarin.iOS の最新のバージョンでこの問題が解決されているためくださいファイルで新しいバグ[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)完全なバージョン情報とビルドのログ出力を使用します。

<a name="MT1010" />

### <a name="mt1010-could-not-load-the-assembly--"></a>MT1010: アセンブリを読み込めませんでした ' *': *

<a name="MT1011" />

### <a name="mt1011-could-not-add-missing-resource-file-"></a>MT1011: 不足しているリソース ファイルを追加できませんでした: ' *'

<a name="MT1012" />

### <a name="mt1012-failed-to-list-the-apps-on-the-device--"></a>: MT1012 デバイスでアプリを一覧表示できませんでした ' *': *

<a name="MT1013" />

### <a name="mt1013-dependency-tracking-error-no-files-to-compare-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT1013: 依存関係のエラーを追跡します。 比較するファイルがありません。 バグ報告を送信してください http://bugzilla.xamarin.com とテスト_ケースをします。

これは、Xamarin.iOS のバグを示します。 バグを送信してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)テスト caes とします。

<a name="MT1014" />

### <a name="mt1014-failed-to-re-use-cached-version-of--"></a>: MT1014 キャッシュされたバージョンを再利用できませんでした ' *': * です。

<a name="MT1015" />

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015: 実行可能ファイルを作成できませんでした ' *': *

<a name="MT1015" />

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015: 実行可能ファイルを作成できませんでした ' *': *

<a name="MT1016" />

### <a name="mt1016-failed-to-create-the-notice-file-because-a-directory-already-exists-with-the-same-name"></a>MT1016: 同じ名前のディレクトリが既に存在するために、通知ファイルを作成できませんでした。

ディレクトリを削除`NOTICE`プロジェクトからです。

<a name="MT1017" />

### <a name="mt1017-failed-to-create-the-notice-file-"></a>MT1017: 通知ファイルの作成に失敗しました: * です。

<a name="MT1018" />

### <a name="mt1018-your-application-failed-code-signing-checks-and-could-not-be-installed-on-the-device--check-your-certificates-provisioning-profiles-and-bundle-ids-probably-your-device-is-not-part-of-the-selected-provisioning-profile-error-0xe8008015"></a>MT1018: アプリケーションはコード署名の確認に失敗しましたし、デバイスにインストールできませんでした ' *'。 プロビジョニング プロファイル、証明書を確認し、バンドル id。 おそらく、デバイスは、選択したプロビジョニング プロファイルの一部 (エラー: 0xe8008015)。

<a name="MT1019" />

### <a name="mt1019-your-application-has-entitlements-not-supported-by-your-current-provisioning-profile-and-could-not-be-installed-on-the-device--please-check-the-ios-device-log-for-more-detailed-information-error-0xe8008016"></a>MT1019: アプリケーションは、現在のプロビジョニング プロファイルでサポートされていない権利を持つし、デバイスにインストールできませんでした ' *'。 詳細については、iOS デバイスのログを確認してください (エラー: 0xe8008016)。

これは、場合に発生することができます。

* アプリケーションでは、現在のプロビジョニング プロファイルをサポートしない権利を持ちます。
  考えられる解決策:
  - 権利をサポートする別のプロビジョニング プロファイルを指定して、アプリケーションのニーズ。
  - 現在のプロビジョニング プロファイルでサポートされていません権利を削除します。
* 使用しているプロビジョニング プロファイルに含まれていないに配置しようとしているデバイスです。
  考えられる解決策:
  - Xcode でのテンプレートから新しいアプリを作成、同じプロビジョニング プロファイルを選択して同じデバイスに展開します。、 場合もあります Xcode が (それ以外の場合は、Xcode するを依頼を行うには新機能) に新しいデバイスにプロビジョニング プロファイルを自動的に更新することができます。
  -IOS デベロッパー センターに移動します。 新しいデバイスが、プロビジョニング プロファイルを更新し、コンピューターに更新されたプロビジョニング プロファイルをダウンロードします。

エラーの詳細については、iOS デバイスのログを印刷するほとんどの場合、問題の診断することができます。

<a name="MT1020" />

### <a name="mt1020-failed-to-launch-the-application--on-the-device--"></a>MT1020: アプリケーションの起動に失敗しました '\*'on 'デバイス\*': *

<a name="MT1021" />

### <a name="mt1021-could-not-copy-the-file--to--2"></a>MT1021: は、ファイルをコピーできませんでした '\*'to'\*'。 {2}

ファイルをコピーできませんでした。 コピー操作からのエラー メッセージは、エラーの詳細についてがします。

<a name="MT1022" />

### <a name="mt1022-could-not-copy-the-directory--to--2"></a>MT1022: ディレクトリにコピーできませんでした '\*'to'\*'。 {2}

ディレクトリをコピーできませんでした。 コピー操作からのエラー メッセージは、エラーの詳細についてがします。

<a name="MT1023" />

### <a name="mt1023-could-not-communicate-with-the-device-to-find-the-application---"></a>MT1023: は、アプリケーションを検索するデバイスと通信できませんでした ' *': *

デバイス上のアプリケーションを検索しようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。

<a name="MT1024" />

### <a name="mt1024-the-application-signature-could-not-be-verified-on-device--please-make-sure-that-the-provisioning-profile-is-installed-and-not-expired-error-0xe8008017"></a>MT1024: アプリケーションの署名を検証できませんでしたデバイスに ' *'。 プロビジョニング プロファイルがインストールされ、有効期限が切れていないことを確認してください (エラー: 0xe8008017)。

デバイスは、署名を検証できなかったために、アプリケーションのインストールを拒否しました。

プロビジョニング プロファイルがインストールされ、有効期限が切れていないことを確認してください。

<a name="MT1025" />

### <a name="mt1025-could-not-list-the-crash-reports-on-the-device-"></a>MT1025:、デバイスでクラッシュ レポートが一覧表示できませんでした * です。

デバイスでクラッシュ レポートを表示しようとしているときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes とデバイスを同期します。

<a name="MT1026" />

### <a name="mt1026-could-not-download-the-crash-report--from-the-device-"></a>MT1026: クラッシュ レポートをダウンロードできませんでした * デバイスから * です。

デバイスからクラッシュ レポートをダウンロードしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes とデバイスを同期します。

<a name="MT1027" />

### <a name="mt1027-cant-use-xcode-7-to-launch-applications-on-devices-with-ios--xcode-7-only-supports-ios-6"></a>MT1027: は、Xcode 7 以降を使用して ios デバイスでアプリケーションを起動することはできません * (Xcode 7 のみをサポートしている iOS 6 以降)。

6.0 以降のバージョンの iOS デバイスにアプリケーションを起動する Xcode 7 以降を使用することはできません。

Xcode の古いバージョンを使用するか、それを起動するには、手動でアプリをタップしてください。

<a name="MT1028" />

### <a name="mt1028-invalid-device-specification--expected-ios-watchos-or-all"></a>MT1028: 無効なデバイス仕様: ' *'。 予期された 'ios'、'watchos' または 'all' です。

--を使用して、デバイスの仕様が渡されるデバイスが有効ではありません。 有効な値: 'ios'、'watchos' または 'all' です。

<a name="MT1029" />

### <a name="mt1029-could-not-find-an-application-at-the-specified-directory-"></a>MT1029: で、指定されたディレクトリでのアプリケーションを見つけることができませんでした *。

--Launchdev に渡されるアプリケーションのパスが存在しません。 有効なアプリ バンドルを指定してください。

<a name="MT1030" />

### <a name="mt1030-launching-applications-on-device-using-a-bundle-identifier-is-deprecated-please-pass-the-full-path-to-the-bundle-to-launch"></a>MT1030: バンドル id を使用してデバイス上のアプリケーションの起動は推奨されません。 起動するバンドルの完全パスを渡してください。

アプリ バンドル id だけではなくデバイスの起動にパスを渡すことをお勧めします。

<a name="MT1031" />

### <a name="mt1031-could-not-launch-the-app--on-the-device--because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1031: アプリを起動できませんでした '\*'on 'デバイス\*' デバイスがロックされているためです。 デバイスのロックを解除してから、再試行してください。

デバイスのロックを解除してから、再試行してください。

<a name="MT1032" />

### <a name="mt1032-this-application-executable-might-be-too-large--mb-to-execute-on-device-if-bitcode-was-enabled-you-might-want-to-disable-it-for-development-it-is-only-required-to-submit-applications-to-apple"></a>MT1032: このアプリケーションの実行可能ファイルが大きすぎる可能性があります (* MB) デバイス上でを実行します。 Bitcode が有効になっている場合は、開発に対しては無効にすることができますは Apple へのアプリケーションを送信するのみ必要です。

<a name="MT1033" />

### <a name="mt1033-could-not-uninstall-the-application--from-the-device--"></a>MT1033: アプリケーションをアンインストールできませんでした '\*'' デバイスから\*': *

<!-- 1034 used by mmp -->

<a name="MT1035" />

### <a name="mt1035-cannot-include-different-versions-of-the-framework-name"></a>MT1035: は、異なるバージョンの framework '{name}' を含めることはできません。

同じフレームワークのさまざまなバージョンとリンクすることはできません。

これは通常、拡張機能は、(サードパーティのバインド アセンブリ) を通じて可能性のあるコンテナー アプリよりも framework の別のバージョンを参照する場合に発生します。

次のこのエラーがある複数[MT1036](#MT1036)エラーがそれぞれ異なるフレームワークのパスの一覧表示します。

<a name="MT1036" />

### <a name="mt1036-framework-name-included-from-path-related-to-previous-error"></a>MT1036: Framework '{name}' に含まれる: {パス} (以前のエラーに関連)

と共にこのエラーが報告された[MT1036](#MT1036)です。 参照してください[MT1036](#MT1036)詳細についてはします。

### <a name="mt11xx-debug-service"></a>MT11xx: サービスをデバッグします。

<!--
  MT11xx DebugService.cs
  -->

<a name="MT1101" />

### <a name="mt1101-could-not-start-app"></a>MT1101: アプリを開始できませんでした。

<a name="MT1102" />

### <a name="mt1102-could-not-attach-to-the-app-to-kill-it-"></a>MT1102: は (強制終了) をアプリにアタッチできませんでした *。

<a name="MT1103" />

### <a name="mt1103-could-not-detach"></a>MT1103: はデタッチできませんでした。

<a name="MT1104" />

### <a name="mt1104-failed-to-send-packet-"></a>MT1104: パケットの送信に失敗しました: *

<a name="MT1105" />

### <a name="mt1105-unexpected-response-type"></a>MT1105: 予期しない応答の種類

<a name="MT1106" />

### <a name="mt1106-could-not-get-list-of-applications-on-the-device-request-timed-out"></a>MT1106: デバイスにアプリケーションの一覧を取得できませんでした。 要求がタイムアウトしました。

<a name="MT1107" />

### <a name="mt1107-application-failed-to-launch-"></a>MT1107: アプリケーションを起動できませんでした *。

デバイスがロックされていることを確認してください。

信頼開発者の場合は、エンタープライズ アプリケーションを配置するか、無料のプロビジョニング プロファイルを使用する必要があります (詳細については <a href="http://stackoverflow.com/a/30726375/183422">ここ</a> )。

<a name="MT1108" />

### <a name="mt1108-could-not-find-developer-tools-for-this-xx-yy-device"></a>MT1108: は、この XX (YY) デバイスの開発者向けのツールを見つけられませんでした。

Mtouch からいくつかの操作が必要、 <tt>DeveloperDiskImage.dmg</tt>存在するファイル。   このファイルは Xcode の一部でありで、に対してビルドを使用している SDK の基準とした通常のある、 <tt>Xcode.app/Contents/Developer/iPhoneOS.platform/DeviceSupport/VERSION/DeveloperDiskImage.dmg</tt>です。

このエラーは、接続されているデバイスに一致する DeveloperDiskImage.dmg があるないため、発生します。

<a name="MT1109" />

### <a name="mt1109-application-failed-to-launch-because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1109: アプリケーションは、デバイスがロックされているため、起動に失敗しました。 デバイスのロックを解除してから、再試行してください。

デバイスがロックされていることを確認してください。

<a name="MT1110" />

### <a name="mt1110-application-failed-to-launch-because-of-ios-security-restrictions-please-ensure-the-developer-is-trusted"></a>MT1110: アプリケーションは iOS セキュリティ制限があるため起動できませんでした。 開発者が信頼されていることを確認してください。

信頼開発者の場合は、エンタープライズ アプリケーションを配置するか、無料のプロビジョニング プロファイルを使用する必要があります (詳細については <a href="http://stackoverflow.com/a/30726375/183422">ここ</a> )。

<a name="MT1111" />

### <a name="mt1111-application-launched-successfully-but-its-not-possible-to-wait-for-the-app-to-exit-as-requested-because-its-not-possible-to-detect-app-termination-when-launching-using-gdbserver"></a>MT1111: アプリケーションが正常に起動されるが、アプリを gdbserver を使用してを起動するときに、アプリの終了を検出することはできないために要求を終了するまで待機することはできません。

### <a name="mt12xx-simulator"></a>MT12xx: シミュレーター

<!--
  MT12xx simcontroller.cs
  -->

<a name="MT1201" />

### <a name="mt1201-could-not-load-the-simulator-"></a>MT1201: は、シミュレーターを読み込むことができません: *

<a name="MT1202" />

### <a name="mt1202-invalid-simulator-configuration-"></a>MT1202: 無効なシミュレーターの構成: *

<a name="MT1203" />

### <a name="mt1203-invalid-simulator-specification-"></a>MT1203: 無効なシミュレーター仕様: *

<a name="MT1204" />

### <a name="mt1204-invalid-simulator-specification--runtime-not-specified"></a>MT1204: 無効なシミュレーター仕様 ' *': ランタイムが指定されていません。

<a name="MT1205" />

### <a name="mt1205-invalid-simulator-specification--device-type-not-specified"></a>MT1205: 無効なシミュレーター仕様 ' *': デバイスの種類が指定されていません。

<a name="MT1206" />

### <a name="mt1206-could-not-find-the-simulator-runtime-"></a>MT1206: シミュレーター ランタイムが見つかりませんでした ' *'。

<a name="MT1207" />

### <a name="mt1207-could-not-find-the-simulator-device-type-"></a>MT1207: シミュレーター デバイスの種類が見つかりませんでした ' *'。

<a name="MT1208" />

### <a name="mt1208-could-not-find-the-simulator-runtime-"></a>MT1208: シミュレーター ランタイムが見つかりませんでした ' *'。

<a name="MT1209" />

### <a name="mt1209-could-not-find-the-simulator-device-type-"></a>MT1209: シミュレーター デバイスの種類が見つかりませんでした ' *'。

<a name="MT1210" />

### <a name="mt1210-invalid-simulator-specification--unknown-key-"></a>MT1210: 無効なシミュレーター仕様: \*、不明なキー '\*'

<a name="MT1211" />

### <a name="mt1211-the-simulator-version--does-not-support-the-simulator-type-"></a>MT1211: シミュレーターのバージョン '\*'は、シミュレーターの種類をサポートしていません'\*'

<a name="MT1212" />

### <a name="mt1212-failed-to-create-a-simulator-version-where-type---and-runtime--"></a>MT1212: シミュレーターのバージョンを作成できませんでしたここに入力 = * とランタイム = * です。

<a name="MT1213" />

### <a name="mt1213-invalid-simulator-specification-for-xcode-4-"></a>MT1213: Xcode 4 用無効なシミュレーター仕様: *

<a name="MT1214" />

### <a name="mt1214-invalid-simulator-specification-for-xcode-5-"></a>MT1214: Xcode 5 無効なシミュレーター仕様: *

<a name="MT1215" />

### <a name="mt1215-invalid-sdk-specified-"></a>MT1215: 指定された無効な SDK: *

<a name="MT1216" />

### <a name="mt1216-could-not-find-the-simulator-udid-"></a>MT1216: 見つかりませんでした。 シミュレーター UDID ' *'。

<a name="MT1217" />

### <a name="mt1217-could-not-load-the-app-bundle-at-"></a>MT1217: でのアプリ バンドルをロードできませんでした ' *'。

<a name="MT1218" />

### <a name="mt1218-no-bundle-identifier-found-in-the-app-at-"></a>MT1218: バンドル id では見つかりませんでした、アプリに ' *'。

<a name="MT1219" />

### <a name="mt1219-could-not-find-the-simulator-for-"></a>MT1219: のシミュレーターが見つかりませんでした ' *'。

<a name="MT1220" />

### <a name="mt1220-could-not-find-the-latest-simulator-runtime-for-device-"></a>MT1220: で、デバイス用のシミュレーターの最新のランタイムを見つけることができませんでした ' *'。

通常、これは、Xcode で問題を示します。

この問題を解決しようとするもの:

* Xcode で 1 回、シミュレーターを使用します。
* -Sdk を使用して、明示的な SDK バージョンを渡す<version>です。
* Xcode を再インストールします。

<a name="MT1221" />

### <a name="mt1221-could-not-find-the-paired-iphone-simulator-for-the-watchos-simulator-"></a>MT1221: 見つかりませんでした。 1 対の iPhone シミュレーター WatchOS シミュレーターの場合は ' *'。

WatchOS シミュレーター WatchOS アプリを起動するときに、対になった iOS シミュレーターにも必要があります。

シミュレーターは、ios シミュレーター Xcode のデバイスの UI を使用してペアリングできますを見る (ウィンドウ メニューには、デバイスが ->)。

### <a name="mt13xx-linkwith"></a>MT13xx: [ソースコードファ]

<!--
  MT13xx [LinkWith]
  -->

<a name="MT1301" />

### <a name="mt1301-native-library---was-ignored-since-it-does-not-match-the-current-build-architectures-"></a>MT1301: ネイティブ ライブラリ`*`(\*) 現在のビルド ミラーサイトが一致しないので無視されました (\*)

<a name="MT1302" />

### <a name="mt1302-could-not-extract-the-native-library--from--please-ensure-the-native-library-was-properly-embedded-in-the-managed-assembly-if-the-assembly-was-built-using-a-binding-project-the-native-library-must-be-included-in-the-project-and-its-build-action-must-be-objcbindingnativelibrary"></a>MT1302: ネイティブ ライブラリを抽出できませんでした ' *' から '+' です。 ネイティブ ライブラリが適切に埋め込まれているマネージ アセンブリを (場合バインド プロジェクトを使用して、アセンブリがビルドされたネイティブ ライブラリをプロジェクトに含める必要がある、そのビルド アクションは 'ObjcBindingNativeLibrary' である必要があります) を確認してください。

<a name="MT1303" />

### <a name="mt1303-could-not-decompress-the-native-framework--from--please-review-the-build-log-for-more-information-from-the-native-zip-command"></a>MT1303: ネイティブのフレームワークを解凍できませんでした '\*'from'\*' です。 詳細については、ネイティブ 'zip' コマンドからのビルド ログを確認してください。

バインドのライブラリから指定されたネイティブ フレームワークを解凍できませんでした。

ネイティブ 'zip' のコマンドは、このエラーの詳細については、bulid ログを確認してください。

<a name="MT1304" />

### <a name="mt1304-the-embedded-framework--in--is-invalid-it-does-not-contain-an-infoplist"></a>MT1304: 埋め込み framework ' *' で * が正しくありません: Info.plist が含まれていません。

指定された埋め込みフレームワークは、Info.plist が含まれていないであるためできません有効なフレームワークです。

フレームワークが有効であることを確認してください。

<a name="MT1305" />

### <a name="mt1305-the-binding-library--contains-a-user-framework--but-embedded-user-frameworks-require-ios-80-the-current-deployment-target-is--please-set-the-deployment-target-in-the-infoplist-file-to-at-least-80"></a>MT1305: バインド ライブラリ '\*' ユーザー フレームワークが含まれています (\*) が埋め込まれたユーザー フレームワークには、iOS 8.0 が必要があります (現在の配置ターゲットは、*)。 少なくとも 8.0 Info.plist ファイルで、配置ターゲットを設定してください。

指定したバインディング ライブラリには、埋め込みのフレームワークが含まれていますが、Xamarin.iOS では、iOS 8.0 以降に埋め込まれたフレームワークのみがサポートしています。

ください。 このエラーを解決するには、少なくとも 8.0 Info.plist ファイル内に配置ターゲットを設定 (または埋め込みフレームワークを使用しない)。

### <a name="mt14xx-crash-reports"></a>MT14xx: クラッシュ レポート

<!--
  MT14xx    CrashReports.cs
  -->

<a name="MT1400" />

### <a name="mt1400-could-not-open-crash-report-service-afcconnectionopen-returned-"></a>MT1400: クラッシュ レポート サービスを開けませんでした返された AFCConnectionOpen *。

クラッシュ レポートには、デバイスからアクセスしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes とデバイスを同期します。

<a name="MT1401" />

### <a name="mt1401-could-not-close-crash-report-service-afcconnectionclose-returned-"></a>MT1401: クラッシュ レポート サービスを閉じることができません返された AFCConnectionClose *。

クラッシュ レポートには、デバイスからアクセスしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes とデバイスを同期します。

<a name="MT1402" />

### <a name="mt1402-could-not-read-file-info-for--afcfileinfoopen-returned-"></a>MT1402: でのファイル情報を読み取れませんでした *: 返される AFCFileInfoOpen *

クラッシュ レポートには、デバイスからアクセスしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes とデバイスを同期します。

<a name="MT1403" />

### <a name="mt1403-could-not-read-crash-report-afcdirectoryopen--returned-"></a>MT1403: クラッシュ レポートを読み取れませんでした: AFCDirectoryOpen (*) が返されます *。

クラッシュ レポートには、デバイスからアクセスしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes とデバイスを同期します。

<a name="MT1404" />

### <a name="mt1404-could-not-read-crash-report-afcfilerefopen--returned-"></a>MT1404: クラッシュ レポートを読み取れませんでした: AFCFileRefOpen (*) が返されます *。

クラッシュ レポートには、デバイスからアクセスしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes とデバイスを同期します。

<a name="MT1405" />

### <a name="mt1405-could-not-read-crash-report-afcfilerefread--returned-"></a>MT1405: クラッシュ レポートを読み取れませんでした: AFCFileRefRead (*) が返されます *。

クラッシュ レポートには、デバイスからアクセスしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes とデバイスを同期します。

<a name="MT1406" />

### <a name="mt1406-could-not-list-crash-reports-afcdirectoryopen--returned-"></a>MT1406:、クラッシュ レポートが一覧表示できませんでした: AFCDirectoryOpen (*) が返されます *。

クラッシュ レポートには、デバイスからアクセスしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes とデバイスを同期します。

<!--- 1407 used by mmp -->

### <a name="mt16xx-macho"></a>MT16xx: MachO

<!--
  MT16xx    MachO.cs
  -->

<a name="MT1600" />

### <a name="mt1600-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1600: 一致する O ダイナミック ライブラリではない (不明なヘッダー ' 0 x *')。 * です。

対象の動的なライブラリの処理中にエラーが発生しました。

ダイナミック ライブラリが有効な一致する O ダイナミック ライブラリであることを確認してください。

使って、ライブラリの形式を確認することができます、`file`端末からコマンド。

    file -arch all -l /path/to/library.dylib

<a name="MT1601" />

### <a name="mt1601-not-a-static-library-unknown-header--"></a>MT1601: スタティック ライブラリではない (不明なヘッダー ' *')。 * です。

問題のスタティック ライブラリの処理中にエラーが発生しました。

スタティック ライブラリが有効な一致する O スタティック ライブラリであることを確認してください。

使って、ライブラリの形式を確認することができます、`file`端末からコマンド。

    file -arch all -l /path/to/library.a

<a name="MT1602" />

### <a name="mt1602-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1602: 一致する O ダイナミック ライブラリではない (不明なヘッダー ' 0 x *')。 * です。

対象の動的なライブラリの処理中にエラーが発生しました。

ダイナミック ライブラリが有効な一致する O ダイナミック ライブラリであることを確認してください。

使って、ライブラリの形式を確認することができます、`file`端末からコマンド。

    file -arch all -l /path/to/library.dylib

<a name="MT1603" />

### <a name="mt1603-unknown-format-for-fat-entry-at-position--in-"></a>MT1603: 位置にあるエントリを fat 形式が不明な * の * です。

問題の fat アーカイブの処理中にエラーが発生しました。

Fat アーカイブが有効であることを確認してください。

使って fat アーカイブの形式を確認することができます、`file`端末からコマンド。

    file -arch all -l /path/to/file

<a name="MT1604" />

### <a name="mt1604-file-of-type--is-not-a-macho-file-"></a>MT1604: ファイルの種類 * MachO ファイル (*) ではありません。

指定した MachO ファイルの処理中にエラーが発生しました。

ファイルが有効な一致する O ダイナミック ライブラリであることを確認してください。

使用してファイルの形式を検証することができます、`file`端末からコマンド。

    file -arch all -l /path/to/file

## <a name="mt2xxx-linker-error-messages"></a>MT2xxx: リンカーのエラー メッセージ

<!--
 MT2xxx Linker
  MT20xx Linker (general) errors
  -->

<a name="MT2001" />

### <a name="mt2001-could-not-link-assemblies"></a>MT2001: アセンブリをリンクできませんでした。

このエラー マネージ リンカーが、例外など、予期しないエラーが発生したことを意味し、完了またはできなかった処理されているアセンブリを保存します。 正確なエラーの詳細については、ビルド ログの一部をなどになります

```
error MT2001: Could not link assemblies.
    Method: `System.Void Todo.TodoListPageCS/<<-ctor>b__1_0>d::SetStateMachine(System.Runtime.CompilerServices.IAsyncStateMachine)`
    Assembly: `QuickTodo, Version=1.0.6297.28241, Culture=neutral, PublicKeyToken=null`
Reason: Value cannot be null.
Parameter name: instruction
```

このような問題のバグのレポートをファイルに重要です。 ほとんどの場合は、回避策を適切な修正プログラムが公開されるまでに指定できます。 上記の情報が、問題を解決する (テスト ケースやアセンブリ binairy) と共に重要です。

<a name="MT2002" />

### <a name="mt2002-can-not-resolve-reference-"></a>MT2002: 参照を解決できません: *

<a name="MT2003" />

### <a name="mt2003-option--will-be-ignored-since-linking-is-disabled"></a>MT2003: オプション ' *' はリンクが無効になるので無視されます

<a name="MT2004" />

### <a name="mt2004-extra-linker-definitions-file--could-not-be-located"></a>MT2004: Extra リンカー定義ファイル ' *' に見つかりませんでした。

<a name="MT2005" />

### <a name="mt2005-definitions-from--could-not-be-parsed"></a>MT2005: 定義 ' *' を解析できませんでした。

<a name="MT2006" />

### <a name="mt2006-can-not-load-mscorlibdll-from--please-reinstall-xamarinios"></a>MT2006: から mscorlib.dll を読み込むことはできません: * です。 Xamarin.iOS を再インストールしてください。

これは、通常、Xamarin.iOS インストールに問題があることを示します。 Xamarin.iOS を再インストールしてください。

<!--- 2007 used by mmp -->
<!--- 2009 used by mmp -->

<a name="MT2010" />

### <a name="mt2010-unknown-httpmessagehandler--valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MT2010: 不明な HttpMessageHandler`*`です。 有効な値は HttpClientHandler (既定値)、CFNetworkHandler または NSUrlSessionHandler です。

<a name="MT2011" />

### <a name="mt2011-unknown-tlsprovider---valid-values-are-default-or-appletls"></a>MT2011: 不明な TlsProvider`*`です。  有効な値は、既定値または appletls はします。

渡された値`tls-provider=`TLS (Transport Layer Security)、有効なプロバイダーではありません。

`default`と`appletls`が唯一の有効な値は、両方を表す、同じオプション、ネイティブの Apple TLS API を使用して SSL や TLS をサポートを提供することです。

<!--- 2012 used by mmp -->
<!--- 2013 used by mmp -->
<!--- 2014 used by mmp -->

<a name="MT2015" />

### <a name="mt2015-invalid-httpmessagehandler--for-watchos-the-only-valid-value-is-nsurlsessionhandler"></a>MT2015: 無効な HttpMessageHandler `*` watchOS 用です。 唯一の有効な値は、NSUrlSessionHandler です。

これは、プロジェクト ファイルが無効な HttpMessageHandler を指定するために発生する警告です。

既定では生成されたプレビュー ツールの以前のバージョンはプロジェクト ファイルで値が無効です。

この警告を解決するには、をテキスト エディターで、プロジェクト ファイルを開くし、XML から HttpMessageHandler のすべてのノードを削除します。

<a name="MT2016" />

### <a name="mt2016-invalid-tlsprovider-legacy-option-the-only-valid-value-appletls-will-be-used"></a>MT2016: 無効な TlsProvider`legacy`オプション。 唯一の有効値`appletls`使用されます。

`legacy`プロバイダーが完全に管理された SSLv3/TLSv1 唯一のプロバイダーに付属していない Xamarin.iOS されなくなります。 この古いプロバイダーを使用していたし、新しいで今すぐビルドするプロジェクト`appletls`いずれか。

この警告を修正するには、テキスト エディターで、プロジェクト ファイルを開くすべてを削除 'MtouchTlsProvider' XML からのノードです。

<a name="MT2017" />

### <a name="mt2017-could-not-process-xml-description"></a>MT2017: は、XML の説明を処理できませんでした。

つまりにエラーがある、 [XML リンカーの構成ファイルをカスタム](https://developer.xamarin.com/guides/cross-platform/advanced/custom_linking/)ファイルを確認してください。 指定しました。

<a name="MT2018" />

### <a name="mt2018-the-assembly--is-referenced-from-two-different-locations--and-"></a>MT2018: アセンブリ '\*' は 2 つの場所から参照: '\*' および ' *'。

エラー メッセージに示されているアセンブリは、複数の場所から読み込まれます。 常に同じバージョンのアセンブリを使用することを確認してください。

<a name="MT2019" />

### <a name="mt2019-can-not-load-the-root-assembly-"></a>MT2019:、ルート アセンブリを読み込めません ' *'。

ルート アセンブリをロードできませんでした。 エラー メッセージにパスが既存のファイルを指すことと、有効な .NET アセンブリであることを確認してください。

<a name="MT202x" />

### <a name="mt202x-binding-optimizer-failed-processing-"></a>MT202x: は、バインディング オプティマイザーが処理に失敗しました。`...`です。

予期しない問題は、最適化するためにバインド コードを生成しようとしたときに発生しました。 問題の原因で、要素は、エラー メッセージにという名前です。 という名前のアセンブリ (または型またはという名前のメソッドを含む) は、この問題を解決する必要がありますで提供される、[バグ レポート](http://bugzilla.xamarin.com)有効になっている詳細度で完全なビルド ログと共に (つまり`-v -v -v -v`で、**追加 mtouch引数**)。

最後の桁`x`になります。
* `0` アセンブリの名前。
* `1` 型の名前です。
* `3` メソッドの名前です。

<a name="MT2030" />

### <a name="mt2030-remove-user-resources-failed-processing-"></a>MT2030: 失敗した処理を削除するユーザー リソース`...`です。

予期しない問題は、ユーザー リソースを削除しようとしているときに発生しました。 問題の原因で、アセンブリは、エラー メッセージにという名前です。 アセンブリにこの問題を解決する必要がありますで提供される、[バグ レポート](http://bugzilla.xamarin.com)有効になっている詳細度で完全なビルド ログと共に (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

ユーザー リソースとは、アプリケーション バンドルを作成する、ビルド時に、抽出する必要があります (リソース) としてのアセンブリ内に含まれるファイルです。 バインディングには、以下の項目が含まれます。

* `__monotouch_content_*` および`__monotouch_pages_*`; のリソースと
* バインド アセンブリ; 内に埋め込まれたネイティブ ライブラリ

<a name="MT2040" />

### <a name="mt2040-default-httpmessagehandler-setter-failed-processing-"></a>MT2040: 既定 HttpMessageHandler set アクセス操作子が失敗しました処理`...`です。

既定値を設定しようとするときに予期しない問題が発生しました`HttpMessageHandler`アプリケーションです。 送信してください、[バグ レポート](http://bugzilla.xamarin.com)有効になっている詳細度で完全なビルド ログと共に (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2050" />

### <a name="mt2050-code-remover-failed-processing-"></a>MT2050: は、コードを削除する操作子が処理に失敗しました。`...`です。

予期しない問題が発生しました、アプリケーションと共に配布 BCL からコードを削除しようとしています。 送信してください、[バグ レポート](http://bugzilla.xamarin.com)有効になっている詳細度で完全なビルド ログと共に (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2060" />

### <a name="mt2060-sealer-failed-processing-"></a>MT2060: Sealer 失敗処理`...`です。

予期しない問題には、(最終的な) 型またはメソッドをシールするしようとしているとき、または一部のメソッドを devirtualizing が発生しました。 問題の原因で、アセンブリは、エラー メッセージにという名前です。 アセンブリにこの問題を解決する必要がありますで提供される、[バグ レポート](http://bugzilla.xamarin.com)有効になっている詳細度で完全なビルド ログと共に (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2070" />

### <a name="mt2070-metadata-reducer-failed-processing-"></a>MT2070: メタデータ Reducer 失敗処理`...`です。

予期しない問題が発生しました、アプリケーションからのメタデータを低減しようとしています。 問題の原因で、アセンブリは、エラー メッセージにという名前です。 アセンブリにこの問題を解決する必要がありますで提供される、[バグ レポート](http://bugzilla.xamarin.com)有効になっている詳細度で完全なビルド ログと共に (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2080" />

### <a name="mt2080-marknsobjects-failed-processing-"></a>MT2080: MarkNSObjects 失敗処理`...`です。

マークするときに予期しない問題が発生しました`NSObject`アプリケーションからサブクラスです。 問題の原因で、アセンブリは、エラー メッセージにという名前です。 アセンブリにこの問題を解決する必要がありますで提供される、[バグ レポート](http://bugzilla.xamarin.com)有効になっている詳細度で完全なビルド ログと共に (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2090" />

### <a name="mt2090-inliner-failed-processing-"></a>MT2090: このインライナは処理が失敗しました`...`です。

予期しない問題には、インライン コードをアプリケーションからときが発生しました。 問題の原因で、アセンブリは、エラー メッセージにという名前です。 アセンブリにこの問題を修正するのにはで指定する必要があります、[バグ レポート](https://bugzilla.xamarin.com)有効になっている詳細度で完全なビルド ログと共に (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<!-- MT21xx: more linker errors -->

<!--- 2100 used by mmp -->

<a name="MT2100" />

### <a name="mt2100-smart-enum-conversion-preserver-failed-processing-"></a>MT2100: スマート列挙型の変換の保持に失敗しました処理`...`です。

予期しない問題には、アプリケーションからスマート列挙型の変換メソッドをマークするときが発生しました。 問題の原因で、アセンブリは、エラー メッセージにという名前です。 アセンブリにこの問題を修正するのにはで指定する必要があります、[バグ レポート](https://bugzilla.xamarin.com)有効になっている詳細度で完全なビルド ログと共に (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2101" />

### <a name="mt2101-cant-resolve-the-reference--referenced-from-the-method--in-"></a>MT2101: は、参照を解決できない '\*'、メソッドから参照されている'\*' に ' *'。

エラー メッセージに記載されているメソッドを処理するときに、無効なアセンブリ参照が発生しました。

問題の原因で、アセンブリは、エラー メッセージにという名前です。 アセンブリにこの問題を解決する必要がありますで提供される、[バグ レポート](https://bugzilla.xamarin.com)有効になっている詳細度で完全なビルド ログと共に (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2102" />

### <a name="mt2102-error-processing-the-method--in-the-assembly--"></a>MT2102: メソッドを処理中にエラー '\*'assembly' で\*': *

予期しない問題が発生しました、エラー メッセージに記載されているメソッドをマークするしようとしています。

問題の原因で、アセンブリは、エラー メッセージにという名前です。 アセンブリにこの問題を解決する必要がありますで提供される、[バグ レポート](https://bugzilla.xamarin.com)有効になっている詳細度で完全なビルド ログと共に (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2103" />

### <a name="mt2103-error-processing-assembly--"></a>MT2103: エラー処理のアセンブリ '\*': *

アセンブリを処理するときに、予期しないエラーが発生しました。

問題の原因で、アセンブリは、エラー メッセージにという名前です。 アセンブリにこの問題を修正するのにはで指定する必要があります、[バグ レポート](https://bugzilla.xamarin.com)有効になっている詳細度で完全なビルド ログと共に (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2104" />

### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: アセンブリをリンクできません '{0}' 混在モードであるとします。

混合モード アセンブリは、リンカーによっては処理できません。

参照してください https://msdn.microsoft.com/library/x0w2664k.aspx 混合モード アセンブリの詳細についてはします。

## <a name="mt3xxx-aot-error-messages"></a>MT3xxx: AOT エラー メッセージ

<!--
 MT3xxx AOT
  MT30xx AOT (general) errors
  -->

<a name="MT3001" />

### <a name="mt3001-could-not-aot-the-assembly-"></a>MT3001: でした AOT アセンブリではない ' *'

これにより、AOT コンパイラのバグが通常を示します。 バグを送信してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)で、プロジェクトをエラーを再現するために使用できます。

回避するためにこのプロジェクトの iOS ビルド オプションで、インクリメンタル ビルドを無効にすると考えられる場合があります (が、バグ、したがっても報告してください)。

<a name="MT3002" />

### <a name="mt3002-aot-restriction-method--must-be-static-since-it-is-decorated-with-monopinvokecallback-see-httpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbackshttpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbacks"></a>MT3002: AOT 制限: メソッド ' *' [MonoPInvokeCallback] で装飾されているために、静的なをする必要があります。 参照してください。 [https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks](https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks)

このエラー メッセージ、AOT コンパイラに由来します。

<a name="MT3003" />

### <a name="mt3003-conflicting---debug-and---llvm-options-soft-debugging-is-disabled"></a>MT3003: 競合している--デバッグおよび--ある llvm オプション。 ソフト デバッグは無効です。

ある LLVM が有効にすると、デバッグはサポートされていません。 アプリをデバッグする必要がある場合は、まずある LLVM を無効にします。

<a name="MT3004" />

### <a name="mt3004-could-not-aot-the-assembly--because-it-doesnt-exist"></a>MT3004: でした AOT アセンブリではない ' *' が存在しないためです。

<a name="MT3005" />

### <a name="mt3005-the-dependency--of-the-assembly--was-not-found-please-review-the-projects-references"></a>MT3005: 依存関係 '\*'assembly' の\*' が見つかりませんでした。 プロジェクトの参照を確認してください。

これは通常、アセンブリが別のバージョンのプラットフォーム アセンブリ (通常、mscorlib.dll の .NET 4 バージョン) を参照する場合に発生します。

これはサポートされていませんし、可能性がありますいないビルドまたは正常に実行 (アセンブリと同じ .NET 4 のバージョン Xamarin.iOS バージョンがありません mscorlib.dll の API に使用できます)。

<a name="MT3006" />

### <a name="mt3006-could-not-compute-a-complete-dependency-map-for-the-project-this-will-result-in-slower-build-times-because-xamarinios-cant-properly-detect-what-needs-to-be-rebuilt-and-what-does-not-need-to-be-rebuilt-please-review-previous-warnings-for-more-details"></a>MT3006: は、プロジェクトの完全な依存関係のマッピングを計算できませんでした。 これは、結果、低速ビルド時間 Xamarin.iOS を再構築する必要のある (および、どのような再構築する必要はありません) で正しく検出できないためです。 詳細については、以前の警告を確認してください。

 ビルドまたは正常に実行 (アセンブリと同じ .NET 4 のバージョン Xamarin.iOS バージョンがありません mscorlib.dll の API に使用できます)。

<a name="MT3007" />

### <a name="mt3007-debug-info-files-mdb-will-not-be-loaded-when-llvm-is-enabled"></a>MT3007: デバッグ情報ファイル (*.mdb) は読み込まれませんある llvm が有効にするとします。

<a name="MT3008" />

### <a name="mt3008-bitcode-support-requires-the-use-of-the-llvm-aot-backend---llvm"></a>MT3008: Bitcode サポートのある LLVM AOT バックエンドの使用が必要です (--ある llvm)

Bitcode サポートがある LLVM AOT バックエンドの使用が必要です (--ある llvm)。

Bitcode サポートを無効にするか、ある LLVM を有効にします。

<!--- 3009 used by mmp -->
<!--- 3010 used by mmp -->

## <a name="mt4xxx-code-generation-error-messages"></a>MT4xxx: コードの生成エラー メッセージ

### <a name="mt40xx-main"></a>MT40xx: メイン

<!--
 MT4xxx code generation
  MT40xx main.m
  -->

<a name="MT4001" />

### <a name="mt4001-the-main-template-could-not-be-expanded-to-"></a>MT4001: メインのテンプレートを展開できませんを`*`です。

Main.m を生成するときにエラーが発生しました。 バグを送信してください[ http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4002" />

### <a name="mt4002-failed-to-compile-the-generated-code-for-pinvoke-methods-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4002: P/invoke メソッドの生成されたコードをコンパイルできませんでした。 バグ報告を送信してください。 http://bugzilla.xamarin.com

P/invoke メソッドの生成されたコードをコンパイルできませんでした。 バグ報告を送信してください[ http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

### <a name="mt41xx-registrar"></a>MT41xx: レジストラー

<!--
  MT41xx registrar.m
  -->

<a name="MT4101" />

### <a name="mt4101-the-registrar-cannot-build-a-signature-for-type-"></a>MT4101: レジストラーは型のシグネチャを作成できない`*`です。

ランタイムは、目標 C. との間のマーシャ リングする方法を認識しませんエクスポートの API で、種類が見つかりました

Xamarin.iOS が該当するタイプをサポートすると思われる場合に、拡張機能の要求を送信してください[ http://bugzilla.xamarin.com](http://bugzilla.xamarin.com)です。

<a name="MT4102" />

### <a name="mt4102-the-registrar-found-an-invalid-type--in-signature-for-method--use--instead"></a>MT4102: レジストラーに無効な型が見つかった`*`メソッドのシグネチャで`*`です。 代わりに、`*` を使用してください。

これは、現在にのみ行われる 1 つの型と: System.DateTime です。 Objective C と同じ (NSDate) を使用してください。

<a name="MT4103" />

### <a name="mt4103-the-registrar-found-an-invalid-type--in-signature-for-method--the-type-implements-inativeobject-but-does-not-have-a-constructor-that-takes-two-intptr-bool-arguments"></a>MT4103: レジストラーに無効な型が見つかった`*`メソッドのシグネチャで`*`: 型は INativeObject を実装しますが、2 つを受け取るコンス トラクターがありません (IntPtr、bool) の引数

これは、レジストラーが上記の特性を持つシグネチャ内の型を検出するとします。 レジストラーが、型の新しいインスタンスを作成する必要があります、ここでは、(IntPtr、bool) を持つコンス トラクターを必要と署名 - 最初の引数 (IntPtr) 管理対象のハンドルを指定、呼び出し元がネイティブの所有権を渡す場合に、2 番目(場合、この値は false を保持」は、オブジェクトで呼び出されます) を処理します。

<a name="MT4104" />

### <a name="mt4104-the-registrar-cannot-marshal-the-return-value-for-type--in-signature-for-method-"></a>MT4104: レジストラーをマーシャ リングできない型の戻り値`*`メソッドのシグネチャで`*`です。

ランタイムは、目標 C. との間のマーシャ リングする方法を認識しませんエクスポートの API で、種類が見つかりました

Xamarin.iOS が該当するタイプをサポートすると思われる場合に、拡張機能の要求を送信してください[ http://bugzilla.xamarin.com](http://bugzilla.xamarin.com)です。

<a name="MT4105" />

### <a name="mt4105-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4105: レジストラーをマーシャ リングできない型のパラメーター`*`メソッドのシグネチャで`*`です。

Xamarin.iOS が該当するタイプをサポートすると思われる場合に、拡張機能の要求を送信してください[ http://bugzilla.xamarin.com](http://bugzilla.xamarin.com)です。

<a name="MT4106" />

### <a name="mt4106-the-registrar-cannot-marshal-the-return-value-for-structure--in-signature-for-method-"></a>MT4106: レジストラーをマーシャ リングできない構造体の戻り値`*`メソッドのシグネチャで`*`です。

ランタイムは、目標 C. との間のマーシャ リングする方法を認識しませんエクスポートの API で、種類が見つかりました

Xamarin.iOS が該当するタイプをサポートすると思われる場合に、拡張機能の要求を送信してください[ http://bugzilla.xamarin.com](http://bugzilla.xamarin.com)です。

<a name="MT4107" />

### <a name="mt4107-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4107: レジストラーをマーシャ リングできない型のパラメーター`*`メソッドのシグネチャで`+`です。

ランタイムは、目標 C. との間のマーシャ リングする方法を認識しませんエクスポートの API で、種類が見つかりました

Xamarin.iOS が該当するタイプをサポートすると思われる場合に、拡張機能の要求を送信してください[ http://bugzilla.xamarin.com](http://bugzilla.xamarin.com)です。

<a name="MT4108" />

### <a name="mt4108-the-registrar-cannot-get-the-objectivec-type-for-managed-type-"></a>MT4108: レジストラーはマネージ型にも種類を取得できません`*`です。

ランタイムは、目標 C. との間のマーシャ リングする方法を認識しませんエクスポートの API で、種類が見つかりました

Xamarin.iOS が該当するタイプをサポートすると思われる場合に、拡張機能の要求を送信してください[ http://bugzilla.xamarin.com](http://bugzilla.xamarin.com)です。

<a name="MT4109" />

### <a name="mt4109-failed-to-compile-the-generated-registrar-code-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4109: は、レジストラーが生成されたコードのコンパイルに失敗しました。 バグ報告を送信してください。 http://bugzilla.xamarin.com

レジストラーの生成されたコードをコンパイルできませんでした。 ビルド ログは、コードがコンパイルされていない理由を説明する、ネイティブ コンパイラからの出力が含まれます。

これは、常に Xamarin.iOS; のバグバグ報告を送信してください[ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com)プロジェクトとテスト ケースです。

<a name="MT4110" />

### <a name="mt4110-the-registrar-cannot-marshal-the-out-parameter-of-type--in-signature-for-method-"></a>MT4110: レジストラーをマーシャ リングできない型の out パラメーター`*`メソッドのシグネチャで`*`です。

<a name="MT4111" />

### <a name="mt4111-the-registrar-cannot-build-a-signature-for-type--in-method-"></a>MT4111: レジストラーは型のシグネチャを作成できない`*`メソッドで`*`です。

<a name="MT4112" />

### <a name="mt4112-the-registrar-found-an-invalid-type--registering-generic-types-with-objective-c-is-not-supported-and-may-lead-to-random-behavior-andor-crashes-for-backwards-compatibility-with-older-versions-of-xamarinios-it-is-possible-to-ignore-this-error-by-passing---unsupported--enable-generics-in-registrar-as-an-additional-mtouch-argument-in-the-projects-ios-build-options-page-see-developerxamarincomguidesiosadvancedtopicsregistrarhttpsdeveloperxamarincomguidesiosadvancedtopicsregistrar-for-more-information"></a>MT4112: レジストラーに無効な型が見つかった`*`です。 Objective C にジェネリック型を登録することはサポートされていませんし、ランダムな動作やクラッシュする可能性があります (の後方 Xamarin.iOS の古いバージョンとの互換性を可能であればを渡すことによってこのエラーを無視する`--unsupported--enable-generics-in-registrar`追加 mtouch としてプロジェクトの iOS ビルド オプション ページに渡す引数。 参照してください[developer.xamarin.com/guides/ios/advanced_topics/registrar](https://developer.xamarin.com/guides/ios/advanced_topics/registrar)詳細については)。

<a name="MT4113" />

### <a name="mt4113-the-registrar-found-a-generic-method--exporting-generic-methods-is-not-supported-and-will-lead-to-random-behavior-andor-crashes"></a>MT4113: レジストラーにジェネリック メソッドが見つかりました: '\*.\*' です。 ジェネリック メソッドのエクスポートはサポートされていませんし、ランダムな動作やクラッシュにつながります。

<a name="MT4114" />

### <a name="mt4114-unexpected-error-in-the-registrar-for-the-method----please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4114: メソッドのレジストラーで予期しないエラー '\*.\*'-でバグ報告を送信してください http://bugzilla.xamarin.com

<a name="MT4116" />

### <a name="mt4116-could-not-register-the-assembly--"></a>MT4116: アセンブリを登録できませんでした ' *': *

<a name="MT4117" />

### <a name="mt4117-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4117: レジストラーで、メソッドの署名の不一致が検出された '*.*'-メソッドは、セレクターを示します * パラメーター、マネージ メソッドがあるときに * パラメーター。

<a name="MT4118" />

### <a name="mt4118-cannot-register-two-managed-types--and--with-the-same-native-name-"></a>MT4118: 2 つのマネージ型を登録できません ('\*'および'\*') と同じネイティブ名前 ('* ')。

<a name="MT4119" />

### <a name="mt4119-could-not-register-the-selector--of-the-member--because-the-selector-is-already-registered-on-a-different-member"></a>MT4119: セレクターを登録できませんでした '\*'のメンバー'\*。 *'、セレクターが既に別のメンバーに登録されているためです。

<a name="MT4120" />

### <a name="mt4120-the-registrar-found-an-unknown-field-type--in-field--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4120: レジストラーが検出された不明なフィールド型 '\*'field' in\*。 *'。 バグ報告を送信してください。 http://bugzilla.xamarin.com

このエラーは、Xamarin.iOS でバグを示します。 バグ報告を送信してください[ http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4121" />

### <a name="mt4121-cannot-use-gccg-to-compile-the-generated-code-from-the-static-registrar-when-using-the-accounts-framework-the-header-files-provided-by-apple-used-during-the-compilation-require-clang-either-use-clang---compilerclang-or-the-dynamic-registrar---registrardynamic"></a>MT4121: GCC は使用できません/g++ (コンパイル時に使用する Apple によって提供されるヘッダー ファイルで必要な Clang) アカウント フレームワークを使用するときに、静的レジストラーによって生成されたコードをコンパイルします。 Clang を使用するか (--コンパイラ: clang) または動的レジストラー (--レジストラー: ダイナミック)。

<a name="MT4122" />

### <a name="mt4122-cannot-use-the-clang-compiler-provided-in-the--sdk-to-compile-the-generated-code-from-the-static-registrar-when-non-ascii-type-names--are-present-in-the-application-either-use-gccg---compilergccg-the-dynamic-registrar---registrardynamic-or-a-newer-sdk"></a>MT4122: で提供される Clang コンパイラを使用できません、*です。* ASCII 以外の場合に、静的レジストラーから生成されたコードをコンパイルする SDK の名前を入力 ('* ') が、アプリケーション内に存在します。 GCC を使用するか/g++ (--コンパイラ: gcc | g++)、動的レジストラー (--レジストラー: 動的) または新しい SDK。

<a name="MT4123" />

### <a name="mt4123-the-type-of-the-variadic-parameter-in-the-variadic-function--must-be-systemintptr"></a>MT4123: 可変個引数関数の可変個引数パラメーターの型 ' *' System.IntPtr をする必要があります。

<a name="MT4124" />

### <a name="mt4124-invalid--found-on--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4124: 無効な * で見つかった ' *'。 バグ報告を送信してください。 http://bugzilla.xamarin.com

このエラーは、Xamarin.iOS でバグを示します。 バグ報告を送信してください[ http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4125" />

### <a name="mt4125-the-registrar-found-an-invalid-type--in-signature-for-method--the-interface-must-have-a-protocol-attribute-specifying-its-wrapper-type"></a>MT4125: レジストラーが検出された無効な型 '\*'メソッドのシグネチャは' in\*': インターフェイスは、そのラッパー型を指定するプロトコル属性を持つ必要があります。

<a name="MT4126" />

### <a name="mt4126-cannot-register-two-managed-protocols--and--with-the-same-native-name-"></a>MT4126: 2 つの管理されているプロトコルを登録できません ('\*'および'\*') と同じネイティブ名前 ('* ')。

<a name="MT4127" />

### <a name="mt4127-cannot-register-more-than-one-interface-method-for-the-method--which-is-implementing-"></a>MT4127: メソッドの 1 つ以上のインターフェイス メソッドを登録できません '\*' (これは、実装する '\*')。

<a name="MT4128" />

### <a name="mt4128-the-registrar-found-an-invalid-generic-parameter-type--in-the-method--the-generic-parameter-must-have-an-nsobject-constraint"></a>MT4128: レジストラーが検出された無効なジェネリック パラメーター型 '\*'method' の\*' です。 ジェネリック パラメーター 'NSObject' 制約があります。

<a name="MT4129" />

### <a name="mt4129-the-registrar-found-an-invalid-generic-return-type--in-the-method--the-generic-return-type-must-have-an-nsobject-constraint"></a>MT4129: レジストラーが検出された無効なジェネリック戻り値の型 '\*'method' の\*' です。 ジェネリック戻り値の型 'NSObject' 制約があります。

<a name="MT4130" />

### <a name="mt4130-the-registrar-cannot-export-static-methods-in-generic-classes-"></a>MT4130: レジストラーはジェネリック クラスの静的メソッドをエクスポートできません ('* ')。

<a name="MT4131" />

### <a name="mt4131-the-registrar-cannot-export-static-properties-in-generic-classes-"></a>MT4131: レジストラーはジェネリック クラスの静的なプロパティをエクスポートできません ('\*.\*')。

<a name="MT4132" />

### <a name="mt4132-the-registrar-found-an-invalid-generic-return-type--in-the-property--the-return-type-must-have-an-nsobject-constraint"></a>MT4132: レジストラーが検出された無効なジェネリック戻り値の型 '\*'property' in\*' です。 戻り値の型 'NSObject' 制約があります。

<a name="MT4133" />

### <a name="mt4133-cannot-construct-an-instance-of-the-type--from-objective-c-because-the-type-is-generic-runtime-exception"></a>: MT4133 型のインスタンスを構築することはできません ' *' Objective C から型がジェネリックであるためです。 [ランタイム例外]

<a name="MT4134" />

### <a name="mt4134-your-application-is-using-the--framework-which-isnt-included-in-the-ios-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-ios--while-youre-building-with-the-ios--sdk-please-select-a-newer-sdk-in-your-apps-ios-build-options"></a>MT4134: アプリケーションを使用して、' *' フレームワークで、iOS アプリのビルドを使用する SDK に含まれていない (このフレームワークは、iOS で導入された * ios をビルドするときに、* SDK)。アプリの iOS のビルド オプションで新しい SDK を選択してください。

<a name="MT4135" />

### <a name="mt4135-the-member--has-an-export-attribute-that-doesnt-specify-a-selector-a-selector-is-required"></a>MT4135: メンバー '\*.\*' セレクターを指定していないエクスポート属性があります。 セレクターが必要です。

<a name="MT4136" />

### <a name="mt4136-the-registrar-cannot-marshal-the-parameter-type--of-the-parameter--in-the-method-"></a>MT4136: レジストラーをマーシャ リングできないパラメーター型 '\*'parameter' の\*'method' の\**'。

<!-- MT4137 is unused -->

<a name="MT4138" />

### <a name="mt4138-the-registrar-cannot-marshal-the-property-type--of-the-property-"></a>MT4138: レジストラーをマーシャ リングできないプロパティの型 '\*'property' の\*。 *'。

<a name="MT4139" />

### <a name="mt4139-the-registrar-cannot-marshal-the-property-type--of-the-property--properties-with-the-connect-attribute-must-have-a-property-type-of-nsobject-or-a-subclass-of-nsobject"></a>MT4139: レジストラーをマーシャ リングできないプロパティの型 '\*'property' の\*。 *'。 [接続] 属性を持つプロパティは、NSObject のプロパティの型 (または NSObject のサブクラス) が必要です。

<a name="MT4140" />

### <a name="mt4140-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-variadic-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4140: レジストラーで、メソッドの署名の不一致が検出された '*.*'-セレクターは、可変個引数メソッドを示します * パラメーター、マネージ メソッドがあるときに * パラメーター。

<a name="MT4141" />

### <a name="mt4141-cannot-register-the-selector--on-the-member--because-xamarinios-implicitly-registers-this-selector"></a>MT4141: セレクターを登録できません。 '\*'member' on\*' Xamarin.iOS がこのセレクターを暗黙的に登録されるためです。

これは、framework の型をサブクラス化し、'' を保持を実装しようとしています。 'release' または 'dealloc' メソッド。

```csharp
class MyNSObject : NSObject
{
    [Export ("retain")]
    new void Retain () {}

    [Export ("release")]
    new void Release () {}

    [Export ("dealloc")]
    new void Dealloc () {}
}
```

ただし、クラスが、framework の最初のサブクラスでない場合は、これらのメソッドをオーバーライドすることが可能型をお勧めします。

```csharp
class MyNSObject : NSObject
{
}

class MyCustomNSObject : MyNSObject
{
    [Export ("retain")]
    new void Retain () {}

    [Export ("release")]
    new void Release () {}

    [Export ("dealloc")]
    new void Dealloc () {}
}
```

Xamarin.iOS がここではよりも優先されます`retain`、`release`と`dealloc`上、`MyNSObject`クラス、競合はありません。

<a name="MT4142" />

### <a name="mt4142-failed-to-register-the-type-"></a>MT4142: 型を登録できませんでした ' *'。

<a name="MT4143" />

### <a name="mt4143-the-objectivec-class--could-not-be-registered-it-does-not-seem-to-derive-from-any-known-objectivec-class-including-nsobject"></a>MT4143: もクラス ' *' できなかった登録されると、これはいない (NSObject を含む) の既知のもクラスから派生します。

<a name="MT4144" />

### <a name="mt4144-cannot-register-the-method--since-it-does-not-have-an-associated-trampoline-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>: MT4144 メソッドを登録できません ' *' 関連付けられているトランポリンがないためです。 バグ報告を送信してください http://bugzilla.xamarin.com です。

これは、Xamarin.iOS のバグを示します。 バグを送信してください[ http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4145" />

### <a name="mt4145-invalid-enum--enums-with-the-native-attribute-must-have-a-underlying-enum-type-of-either-long-or-ulong"></a>MT4145: 無効な列挙 ' *': [ネイティブ] 属性を持つ列挙型は 'long' または 'ulong' のいずれかの基になる列挙型である必要があります。

<a name="MT4146" />

### <a name="mt4146-the-name-parameter-of-the-registrar-attribute-on-the-class---contains-an-invalid-character--"></a>MT4146: クラスのレジストラー属性の Name パラメーター\*'('\*') に無効な文字が含まれています:'\*' (\*)。

Objectice C クラスの名前は、つまり空白を含めることはできません、`Register`マネージ クラスの対応する属性を持つことはできません、`Name`パラメーターは、空白文字を含めることはできません。

確認してください、`Register`エラー メッセージに記載されているマネージ クラスの属性に空白が含まれていません。

<a name="MT4147" />

### <a name="mt4147-detected-a-protocol-inheriting-from-the-jsexport-protocol-while-using-the-dynamic-registrar-it-is-not-possible-to-export-protocols-to-javascriptcore-dynamically-the-static-registrar-must-be-used-add---registrarstatic-to-the-additional-mtouch-arguments-in-the-projects-ios-build-options-to-select-the-static-registrar"></a>MT4147:、動的なレジストラーを使用しているときに、JSExport プロトコルから継承するプロトコルが検出されました。 JavaScriptCore にプロトコルを動的にエクスポートすることはできません。静的なレジストラーを使用する必要があります (追加 '--追加 mtouch 引数は、プロジェクトの iOS ビルド オプションを静的なレジストラーを選択するには、レジストラー: 静的)。

<a name="MT4148" />

### <a name="mt4148-the-registrar-found-a-generic-protocol--exporting-generic-protocols-is-not-supported"></a>MT4148: レジストラーに一般的なプロトコルが見つかりました: ' *'。 一般的なプロトコルのエクスポートはサポートされていません。

<a name="MT4149" />

### <a name="mt4149-cannot-register-the-method--because-the-type-of-the-first-parameter--does-not-match-the-category-type-"></a>MT4149: は、メソッドを登録できません '\*.\*' ため、最初のパラメーターの型 ('\*') カテゴリの種類と一致しません ('\*')。

<a name="MT4150" />

### <a name="mt4150-cannot-register-the-type--because-the-type-property--in-its-category-attribute-does-not-inherit-from-nsobject"></a>MT4150: は、種類を登録できません '\*' ため、Type プロパティ ('\*') のカテゴリには、属性は NSObject から継承できません。

<a name="MT4151" />

### <a name="mt4151-cannot-register-the-type--because-the-type-property-in-its-category-attribute-isnt-set"></a>MT4151: は、種類を登録できません ' *'、カテゴリ属性で、Type プロパティが設定されていないためです。

<a name="MT4152" />

### <a name="mt4152-cannot-register-the-type--as-a-category-because-it-implements-inativeobject-or-subclasses-nsobject"></a>MT4152: は、種類を登録できません ' *'、カテゴリとして INativeObject クラスまたはサブクラス NSObject を実装するためです。

<a name="MT4153" />

### <a name="mt4153-cannot-register-the-type--as-a-category-because-its-generic"></a>MT4153: は、種類を登録できません '\*' カテゴリとしてジェネリックであるためです。

<a name="MT4154" />

### <a name="mt4154-cannot-register-the-method--as-a-category-method-because-its-generic"></a>: MT4154 メソッドを登録できません '\*' カテゴリ メソッドとしてジェネリックであるためです。

<a name="MT4155" />

### <a name="mt4155-cannot-register-the-method--with-the-selector--as-a-category-method-on--because-the-objective-c-already-has-an-implementation-for-this-selector"></a>MT4155: は、メソッドを登録できません '\*'with 'セレクター\*' カテゴリのメソッドとして ' *'、OBJECTIVE-C 既にためこのセレクターの実装です。

<a name="MT4156" />

### <a name="mt4156-cannot-register-two-categories--and--with-the-same-native-name-"></a>MT4156: 2 つのカテゴリを登録できません ('\*'および'\*') と同じネイティブ名前 ('* ')。

<a name="MT4157" />

### <a name="mt4157-cannot-register-the-category-method--because-at-least-one-parameter-is-required-and-its-type-must-match-the-category-type-"></a>MT4157: カテゴリのメソッドを登録できません '\*' には、少なくとも 1 つのパラメーターが必要なため (その型がカテゴリの種類と一致する必要があります '\*')

<a name="MT4158" />

### <a name="mt4158-cannot-register-the-constructor--in-the-category--because-constructors-in-categories-are-not-supported"></a>MT4158: コンス トラクターを登録できません。 * カテゴリで * カテゴリに属するコンス トラクターはサポートされていません。

<a name="MT4159" />

### <a name="mt4159-cannot-register-the-method--as-a-category-method-because-category-methods-must-be-static"></a>MT4159: は、メソッドを登録できません ' *' カテゴリ メソッドとしてカテゴリ メソッドは静的である必要があります。

<a name="MT4160" />

### <a name="mt4160-invalid-character---found-in-selector--for-"></a>MT4160: 無効な文字 '\*' (\*) セレクターで見つかった '\*'for'\*' です。

<a name="MT4161" />

### <a name="mt4161-the-registrar-found-an-unsupported-structure--all-fields-in-a-structure-must-also-be-structures-field--with-type-2-is-not-a-structure"></a>MT4161: レジストラーに対してサポートされていない構造が検出された '\*': 構造体のすべてのフィールドは構造体にもあります (フィールド'\*'type' with{2}' は構造体ではありません)。

レジストラーではサポートされていないフィールドを持つ構造体が見つかりませんでした。

Objective C に公開されている構造体のすべてのフィールドには、構造 (クラスではなく) 必要があります。

<a name="MT4162" />

### <a name="mt4162-the-type--used-as--2-is-not-available-in---it-was-introduced-in---please-build-with-a-newer--sdk-usually-done-by-using-the-most-recent-version-of-xcode"></a>MT4162: 型 '\*' (として使用される * {2}) では使用できません * * (で導入された * *)\*新しいをビルドしてください * SDK (通常は Xcode の最新バージョンを使用して行われます。

レジストラーでは、現在の SDK に含まれていない型が見つかりました。

Xcode をアップグレードしてください。

<a name="MT4163" />

### <a name="mt4163-internal-error-in-the-registrar--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4163: (*)、レジストラーでの内部エラーです。 バグ報告を送信してください。 http://bugzilla.xamarin.com

このエラーは、Xamarin.iOS でバグを示します。 バグ報告を送信してください[ http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4164" />

### <a name="mt4164-cannot-export-the-property--because-its-selector--is-an-objective-c-keyword-please-use-a-different-name"></a>MT4164: プロパティをエクスポートできません '\*' ため、セレクター '\*' Objective C キーワードします。 別の名前を使用してください。

対象のプロパティのセレクターが有効な Objective C 識別子です。

セレクターとして Objective C の有効な識別子を使用してください。

<a name="MT4165" />

### <a name="mt4165-the-registrar-couldnt-find-the-type-systemvoid-in-any-of-the-referenced-assemblies"></a>MT4165: ときに、レジストラーによって型 'system.void' が参照されるアセンブリのいずれかに見つかりませんでした。

このエラーの最も可能性の高いは、Xamarin.iOS のバグを示します。 バグ報告を送信してください[ http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4166" />

### <a name="mt4166-cannot-register-the-method--because-the-signature-contains-a-type--that-isnt-a-reference-type"></a>: MT4166 メソッドを登録できません '\*' シグネチャに型が含まれているため (\*) は参照型ではないです。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[ http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4167" />

### <a name="mt4167-cannot-register-the-method--because-the-signature-contains-a-generic-type--with-a-generic-argument-type-that-isnt-an-nsobject-subclass-"></a>MT4167: は、メソッドを登録できません '\*' シグネチャにジェネリック型が含まれているため (\*)、NSObject サブクラス (*) を汎用引数の型にします。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[ http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4168" />

### <a name="mt4168-cannot-register-the-type-managedname-because-its-objective-c-name-exportedname-is-an-objective-c-keyword-please-use-a-different-name"></a>MT4168: は、種類を登録できません '{マネージ\_名前}' ためその Objective C の名前' {エクスポート\_名前}' Objective C キーワードします。 別の名前を使用してください。

該当するタイプの Objective C の名前は、Objective C の有効な識別子ではありません。

Objective C の有効な識別子を使用してください。

<a name="MT4169" />

### <a name="mt4169-failed-to-generate-a-pinvoke-wrapper-for-method-message"></a>MT4169: {メソッド} の P/invoke ラッパーを生成できませんでした {message}。

Xamarin.iOS した P/invoke のラッパー関数を生成できませんでした。
基になる原因の報告されたエラー メッセージを確認してください。

<a name="MT4170" />

### <a name="mt4170-the-registrar-cant-convert-from-managed-type-to-native-type-for-the-return-value-in-the-method-method"></a>MT4170: レジストラーに変換できません '{マネージ型}' から 'ネイティブ {type}' {メソッド} メソッドの戻り値の。

エラーの説明を参照してください<a href="#MT4172">MT4172</a>です。

<a name="MT4171" />

### <a name="mt4171-the-bindas-attribute-on-the-member-member-is-invalid-the-bindas-type-type-is-different-from-the-property-type-type"></a>MT4171: メンバー {member} で BindAs 属性が正しくありません: BindAs 型 {type} は {type} プロパティ型と異なります。

BindAs 属性内の型にアタッチされているメンバーの種類に対応を確認してください。

<a name="MT4172" />

### <a name="mt4172-the-registrar-cant-convert-from-native-type-to-managed-type-for-the-parameter-parameter-name-in-the-method-method"></a>MT4172: レジストラーに変換できません '{ネイティブ型}' から '{マネージ型}' {メソッド} 内のパラメーター '{parameter name}' にします。

レジストラーでは、上記の種類間の変換はサポートされていません。

これは、対象の API は、Xamarin.iOS; によって提供される場合の Xamarin.iOS のバグバグを送信してください [http://bugzilla.xamarin.com][1]です。

ネイティブ ライブラリ用のバインド プロジェクトの開発中に、これを実行する場合があれば型の新しい組み合わせのサポートを追加します。 大文字と小文字の場合は、拡張機能により要求を送信してください ([http://bugzilla.xamarin.com][2]) テストでケース テーブルとおを評価します。

[1]: https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS
[2]: https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS&component=General&bug_severity=enhancement

## <a name="mt5xxx-gcc-and-toolchain-error-messages"></a>MT5xxx: GCC ツール チェーンのエラー メッセージ

### <a name="mt51xx-compilation"></a>MT51xx: コンパイル

<!--
 MT5xxx GCC and toolchain
  MT51xx compilation
  -->

<a name="MT5101" />

### <a name="mt5101-missing--compiler-please-install-xcode-command-line-tools-component"></a>MT5101: 見つからない ' *' コンパイラです。 Xcode コマンド ライン ツール コンポーネントをインストールしてください。

<a name="MT5102" />

### <a name="mt5102-failed-to-assemble-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5102: ファイルの作成に失敗しました ' *'。 バグ報告を送信してください。 http://bugzilla.xamarin.com

<a name="MT5103" />

### <a name="mt5103-failed-to-compile-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5103: ファイルをコンパイルできませんでした ' *'。 バグ報告を送信してください。 http://bugzilla.xamarin.com

<a name="MT5104" />

### <a name="mt5104-could-not-find-neither-the--nor-the--compiler-please-install-xcode-command-line-tools-component"></a>MT5104: 見つかりませんでした。 どちらも、'\*'も'\*' コンパイラです。 Xcode コマンド ライン ツール コンポーネントをインストールしてください。

<!-- 5105 is used by mmp -->

<a name="MT5106" />

### <a name="mt5106-could-not-compile-the-files--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5106: ファイルをコンパイルできませんでした ' *'。 バグ報告を送信してください。 http://bugzilla.xamarin.com

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

### <a name="mt52xx-linking"></a>MT52xx: リンク

<!--
  MT52xx linking
  -->

<a name="MT5201" />

### <a name="mt5201-native-linking-failed-please-review-the-build-log-and-the-user-flags-provided-to-gcc-"></a>MT5201: ネイティブ リンクできませんでした。 ビルド ログは、gcc に提供されるユーザー フラグを確認してください *。

<a name="MT5202" />

### <a name="mt5202-native-linking-failed-please-review-the-build-log"></a>MT5202: ネイティブ リンクできませんでした。 ビルド ログを確認してください。

<a name="MT5203" />

### <a name="mt5203-native-linking-warning-"></a>MT5203: 警告をリンク ネイティブ: *

<!--- 5204-5208 are not used -->

<a name="MT5209" />

### <a name="mt5209-native-linking-error-"></a>MT5209: ネイティブのエラーをリンクします *。

<a name="MT5210" />

### <a name="mt5210-native-linking-failed-undefined-symbol--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-are-properly-linked-in"></a>MT5210: ネイティブ リンクに失敗しました、未定義のシンボル: * です。 必要なすべてのフレームワークが参照されているし、ネイティブ ライブラリを正しくリンクすることを確認してください。

これは、ネイティブのリンカーには、任意の場所で参照されているシンボルが見つけられない場合に発生します。 これが発生するいくつかの理由があります。

* サード パーティのバインドには、フレームワークが必要ですが、これには、バインディングを指定しない、`[LinkWith]`属性。 ソリューション:
  - サード パーティ製バインディングの作成者やソースにアクセス権を持つ場合、変更、バインディングの`[LinkWith]`属性が必要なフレームワークを含めます。

            [LinkWith ("mylib.a", Frameworks = "SystemConfiguration")]

  - サード パーティのバインドを変更することはできません場合、手動でリンクできますフレームワークを渡すことによって`-gcc_flags '-framework SystemFramework'`に`mtouch`(これは、プロジェクトの iOS のビルド オプション ページの追加 mtouch 引数を変更することによって行います。 注意してくださいこれ必要がありますすべてのプロジェクト構成に対して実行すること)。
* 場合によっては、管理されているバインディングがいくつかのネイティブ ライブラリので構成され、すべてをバインドに含める必要があります。 解決するには、バインディングのプロジェクトに必要なすべてのネイティブ ライブラリを追加するために、バインディングの各プロジェクトで 1 つ以上のネイティブ ライブラリを設定することができます。</li>
* 管理対象のバインディングは、ネイティブ ライブラリに存在しないネイティブのシンボルを参照します。
    これは通常、バインディングは、しばらくの間、存在しており、ネイティブ コードが変更されてその期間中にいるため、特定のネイティブ クラスが、削除または名前変更、バインディングが更新されていないときに場合に発生します。
* P/invoke は、存在しないネイティブ シンボルを参照します。 Xamarin.iOS 7.4 以降、 <a href="#MT5214">MT5214</a>この場合のエラーが報告されます (詳細については、MT5214 を参照してください)。
* サード パーティ製バインド/ライブラリは、C++ を使用して作成されましたが、これには、バインディングを指定しない、`[LinkWith]`属性。 これは非常に簡単に認識、通常、完全修飾の C++ シンボルがシンボルがあるために、(1 つの一般的な例は`__ZNKSt9exception4whatEv`)。
  - サード パーティ製バインディングの作成者やソースにアクセス権を持つ場合、変更、バインディングの`[LinkWith]`を設定する属性、`IsCxx`フラグ。

            [LinkWith ("mylib.a", IsCxx = true)]

  - 渡すことによって同等のフラグを設定するには、サードパーティのバインドを変更することはできません、またはサード パーティ製ライブラリを手動でリンクしている場合、 <code>-cxx</code> mtouch (これは、プロジェクトの iOS のビルド オプション ページの追加 mtouch 引数を変更するには. 注意してくださいこれ必要がありますすべてのプロジェクト構成に対して実行すること)。

<a name="MT5211" />

### <a name="mt5211-native-linking-failed-undefined-objective-c-class--the-symbol--could-not-be-found-in-any-of-the-libraries-or-frameworks-linked-with-your-application"></a>MT5211: ネイティブ リンクに失敗しました、未定義の OBJECTIVE-C クラス:\*です。 シンボル '\*' ライブラリまたはアプリケーションを使用してリンクされているフレームワークのいずれかで見つかりませんでした。

これは、ネイティブのリンカーには、任意の場所が参照する OBJECTIVE-C クラスが見つけられない場合に発生します。 これが発生するいくつかの理由がある: 同じである[MT5210](#MT5210)し、さらに。

* サード パーティのバインド、OBJECTIVE-C プロトコルのバインドがしなかったに注釈を付けないことで、 <code>[Protocol]</code> api 定義内の属性です。 ソリューション:
  - 不足している追加`[Protocol]`属性。

              [BaseType (typeof (NSObject))]
              [Protocol] // Add this
              public interface MyProtocol
              {
              }

<a name="MT5212" />

### <a name="mt5212-native-linking-failed-duplicate-symbol-"></a>MT5212: ネイティブ リンクに失敗しました、重複するシンボル: * です。

これは、ネイティブのリンカーには、すべてのネイティブ ライブラリ間で重複したシンボルが発生した場合に発生します。 次のこのエラーがあります 1 つまたは複数[MT5213](#MT5213)エラーが発生するたびに、シンボルの場所を使用します。 このエラーの考えられる理由:

* 2 回は、同じネイティブ ライブラリです。
* 同じシンボルを定義する 2 つの個別のネイティブ ライブラリに発生します。
* ネイティブ ライブラリは、正しく組み込まれていないと、2 回以上同じシンボルが含まれています。
  これは、ターミナルからのコマンドは、次のセットを使用して確認できます (i386 は x86_64/armv7/armv7s/arm64 アーキテクチャに従ってをビルドするために置き換えてください)。

        # Native libraries are usually fat libraries, containing binary code for
        # several architectures in the same file. First we extract the binary
        # code for the architecture we're interested in.
        lipo libNative.a -thin i386 -output libNative.i386.a

        # Now query the native library for the duplicated symbol.
        nm libNative.i386.a | fgrep 'SYMBOL'

        # You can also list the object files inside the native library.
        # In most cases this will reveal duplicated object files.
        ar -t libNative.i386.a

  これにはこれを解決する、いくつかの可能な方法があります。

  - ネイティブ ライブラリのプロバイダーが問題を修正し、更新されたバージョンの提供を要求します。
  - (この場合にのみ機能、問題が重複しているオブジェクト ファイルでは実際には) 余分なオブジェクト ファイルを削除して自分で解決すること

            # Find out if the library is a fat library, and which
            # architectures it contains.
            lipo -info libNative.a

            # Extract each architecture (i386/x86_64/armv7/armv7s/arm64) to a separate file
            lipo libNative.a -thin ARCH -output libNative.ARCH.a

            # Extract the object files for the offending architecture
            # This will remove the duplicates by overwriting them
            # (since they have the same filename)
            mkdir -p ARCH
            cd ARCH
            ar -x ../libNative.ARCH.a

            # Reassemble the object files in an .a
            ar -r ../libNative.ARCH.a *.o
            cd ..

            # Reassemble the fat library
            lipo *.a -create -output libNative.a

  - 未使用のコードを削除するようにリンカーを依頼します。 Xamarin.iOS は、この処理に自動的に次の条件をすべて満たしている場合。
    - すべてのサードパーティのバインドの`[LinkWith]`SmartLink 属性を有効にします。

            [assembly: LinkWith ("libNative.a", SmartLink = true)]

    - いいえ`-gcc_flags`mtouch でプロジェクトの iOS のビルド オプションの追加 mtouch 引数フィールド) に渡されます。
    - 追加することで未使用のコードを削除するには、直接リンカーを依頼することも`-gcc_flags -dead_strip`追加 mtouch 引数は、プロジェクトの iOS にビルド オプション。

<a name="MT5213" />

### <a name="mt5213-duplicate-symbol-in--location-related-to-previous-error"></a>MT5213: 内のシンボルを複製: * (場所は、前のエラーに関連する)

と共にこのエラーが報告された[MT5212](#MT5212)です。 参照してください[MT5212](#MT5212)詳細についてはします。

<a name="MT5214" />

### <a name="mt5214-native-linking-failed-undefined-symbol--this-symbol-was-referenced-the-managed-member--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-linked"></a>MT5214: ネイティブ リンクに失敗しました、未定義のシンボル: * です。 このシンボルへの参照がマネージ メンバー * です。 必要なすべてのフレームワークがリンクされている参照とネイティブ ライブラリをされていることを確認してください。

このエラーは、マネージ コードには、存在しないネイティブ メソッドを P/invoke が含まれている場合に報告されます。 例えば:

```csharp
using System.Runtime.InteropServices;
class MyImports {
   [DllImport ("__Internal")]
   public static extern void MyInexistentFunc ();
}
```

これには、いくつかの考えられる解決策があります。

  -  ソース コードから問題の P/invoke を削除します。
  -  (これは、プロジェクトの ios のビルド オプション、「すべてのアセンブリ」に「リンカー動作」を設定して) すべてのアセンブリのマネージ リンカーを有効にします。 アプリから効果的に使用しないすべての P/invoke を削除します (自動的の代わりに手動でなどの以前の時点)。 欠点は、シミュレーターのビルドをある程度低下しますが、これを行うし、リフレクションを使用している場合、リンカーの詳細についてを参照して、アプリを壊す可能性があることを[ここ](~/ios/deploy-test/linker.md))
  -  不足しているネイティブ シンボルを含む 2 つ目のネイティブ ライブラリを作成します。 問題を回避するだけであることに注意してください (これらの関数の呼び出しを試行した場合は、アプリがクラッシュ)。

<a name="MT5215" />

### <a name="mt5215-references-to--might-require-additional--frameworkxxx-or--lxxx-instructions-to-the-native-linker"></a>MT5215: 参照の ' *' その他の必要があります framework ネイティブ リンカーに指示を XXX または - lXXX を =

これは、P/invoke が、対象のライブラリを参照することが検出されましたが、アプリがそれにリンクされませんを示す警告が表示されます。

<a name="MT5216" />

### <a name="mt5216-native-linking-failed-for--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5216: ネイティブ リンクに失敗しました * です。 バグ報告を送信してください。 http://bugzilla.xamarin.com

AOT コンパイラからの出力をリンクするときに、このエラーは報告されます。

このエラーの最も可能性の高いは、Xamarin.iOS のバグを示します。 バグ報告を送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT5217" />

### <a name="mt5217-native-linking-possibly-failed-because-the-linker-command-line-was-too-long--characters"></a>MT5217: リンカーのコマンドラインが長すぎるので、可能性のあるネイティブ リンクできませんでした (* 文字)。

ネイティブのリンクに失敗し、これは、リンカーのコマンドが長すぎるのでが発生しました。 可能であれば。

Xamarin.iOS プロジェクトは多くの場合、参照ネイティブ シンボルが動的につまりネイティブ リンカーがネイティブのリンクの処理中にこのようなネイティブ シンボルを削除可能性がありますネイティブ リンカーでこれらのシンボルを使用することが表示されないためです。

Xamarin.iOS を使用してこのようなシンボルを保持するネイティブ リンカーを要求する通常、`-u symbol`多くこのような記号、コマンド ライン全体がある場合は、リンカー フラグ可能性があります、最大長を超えてコマンド ライン オペレーティング システムで指定したとおりです。

このような動的シンボルのいくつかの可能なソースがあります。

* P/invoke を静的にリンクされたライブラリ内のメソッド (ここで、dll の名前は`__Internal`DllImport 属性で`[DllImport ("__Internal")]`)。
* 静的にリンクされたライブラリでのメモリ位置への参照プロジェクトのバインドからのフィールド (`[Field]`属性)。
* Objective C のプロジェクトのバインディング (インクリメンタル ビルドを使用する場合や、静的なレジストラーを使用していない場合など) から静的にリンクされたライブラリで参照されているクラス。

考えられる解決策:

* (唯一の SDK アセンブリではなく、すべてのアセンブリに対して可能である) 場合は、マネージ リンカーを有効にします。 これは、可能性がありますが削除ソースの十分な動的シンボルできるように、リンカーのコマンドラインが最大数を超えています。
* P/invoke、フィールドへの参照や OBJECTIVE-C クラスの数を減らします。
* 短い名前を持つ動的シンボルを書き直してください。
* 渡す`-dlsym:false`プロジェクトの iOS の他の mtouch の引数としてビルド オプション。 このオプションは、Xamarin.iOS AOT コンパイルのコードで、ネイティブ参照が生成され、このシンボルを保持するようにリンカーを依頼する必要はありません。 ただし、しか動作しないデバイスは、次のビルド、および、スタティック ライブラリに存在しない関数に P/invoke がある場合は、リンカー エラーが発生します。
* 渡す`--dynamic-symbol-mode=code`プロジェクトの iOS での追加 mtouch 引数としてビルド オプション。 このオプションでは、Xamarin.iOS はネイティブ リンカー コマンドラインの引数を使用してこれらのシンボルを保持するかを確認するのではなく、これらのシンボルを参照するその他のネイティブ コードが生成されます。 このアプローチの欠点は、それが増大する実行可能ファイルのサイズ多少です。
* 渡すことによって、静的なレジストラーを有効にする`--registrar:static`プロジェクトの iOS の他の mtouch の引数としてビルドのオプションは (静的レジストラーは既に既定のデバイスのビルドのため、シミュレーターを作成) します。 静的レジストラーは、OBJECTIVE-C クラスを静的に参照するコードを生成するので、このようなクラスを保持するネイティブ リンカーを依頼する必要はありません。
* デバイスのビルド) (インクリメンタル ビルドを無効にします。 インクリメンタル ビルドが有効な場合は、静的レジストラーによって生成されたコードは Xamarin.iOS が保持するようにリンカーをもらう必要がありますもことを意味 Objective C のクラスを参照するネイティブのリンカーによってと見なされません。 インクリメンタル ビルドを無効化させると、このニーズができなくなります。

<a name="MT5218" />

### <a name="mt5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MT5218: は動的なシンボル {シンボル} を無視することはできません (--無視ダイナミック シンボル = {シンボル})、動的なシンボルが検出されなかったためです。

コマンドライン引数`--ignore-dynamic-symbol=symbol`が渡されましたが、このシンボルは手動で保存する必要がある動的シンボルとして認識したシンボルではありません。

この 2 つの主な理由があります。

* シンボル名が正しくありません。
    * シンボル名にアンダー スコアを付加しません。
    * Objective C のクラスの記号が`OBJC_CLASS_$_<classname>`です。
* シンボルが正しいことが、(いくつかビルド オプションの原因を変更するための動的のシンボルの正確な一覧) 通常の方法で既に保存されているシンボル。

### <a name="mt53xx-other-tools"></a>MT53xx: その他のツール

<!--
  MT53xx other tools
  -->

<a name="MT5301" />

### <a name="mt5301-missing-strip-tool-please-install-xcode-command-line-tools-component"></a>MT5301:、'削除' のツールが不足しています。 Xcode コマンド ライン ツール コンポーネントをインストールしてください。

<a name="MT5302" />

### <a name="mt5302-missing-dsymutil-tool-please-install-xcode-command-line-tools-component"></a>MT5302: 不足している 'dsymutil' するツールです。 Xcode コマンド ライン ツール コンポーネントをインストールしてください。

<a name="MT5303" />

### <a name="mt5303-failed-to-generate-the-debug-symbols-dsym-directory-please-review-the-build-log"></a>MT5303: (dSYM ディレクトリ) のデバッグ シンボルを生成できませんでした。 ビルド ログを確認してください。

デバッグ シンボルを作成する最終的な .app ディレクトリで dsymutil を実行しているときにエラーが発生しました。 Dsymutil の出力が表示および修正方法を確認するためのビルド ログを確認してください。

<a name="MT5304" />

### <a name="mt5304-failed-to-strip-the-final-binary-please-review-the-build-log"></a>MT5304: 最終的なバイナリを削除できませんでした。 ビルド ログを確認してください。

アプリケーションからデバッグ情報を削除する 'ストリップ' ツールの実行中にエラーが発生しました。

<a name="MT5305" />

### <a name="mt5305-missing-lipo-tool-please-install-xcode-command-line-tools-component"></a>MT5305: 不足している 'lipo' するツールです。 Xcode コマンド ライン ツール コンポーネントをインストールしてください。

<a name="MT5306" />

### <a name="mt5306-failed-to-create-the-a-fat-library-please-review-the-build-log"></a>: MT5306 作成できませんでした、fat ライブラリです。 ビルド ログを確認してください。

'Lipo' ツールの実行中にエラーが発生しました。 'Lipo' によって報告されたエラーを確認するビルド ログを確認してください。

<a name="MT5307" />

### <a name="mt5307-failed-to-sign-the-executable-please-review-the-build-log"></a>MT5307: は、実行可能ファイルの署名に失敗しました。 ビルド ログを確認してください。

アプリケーションに署名するときにエラーが発生しました。 'Codesign' によって報告されたエラーを確認するビルド ログを確認してください。

<!-- 5308 is used by mmp -->
<!-- 5309 is used by mmp -->
<!-- 5310 is used by mmp -->

## <a name="mt6xxx-mtouch-internal-tools-error-messages"></a>MT6xxx: mtouch 内部ツールのエラー メッセージ

### <a name="mt600x-stripper"></a>MT600x: ストリッパ

<!--
 MT6xxx mtouch internal tools
  MT600x Stripper
  -->

<a name="MT6001" />

### <a name="mt6001-running-version-of-cecil-doesnt-support-assembly-stripping"></a>MT6001: セシルの実行中のバージョンをサポートしていないアセンブリの削除

<a name="MT6002" />

### <a name="mt6002-could-not-strip-assembly-"></a>MT6002: アセンブリを削除できませんでした`*`です。

アプリケーションでアセンブリからマネージ コード (IL コードを削除する) を削除する場合、エラーが発生しました。

<a name="MT6003" />

### <a name="mt6003-unauthorizedaccessexception-message"></a>MT6003: [UnauthorizedAccessException メッセージ]

セキュリティ エラーが発生しましたデバッグ アプリケーションからシンボルを削除します。

## <a name="mt7xxx-msbuild-error-messages"></a>MT7xxx: MSBuild のエラー メッセージ

<!--
 MT7xxx msbuild errors
  -->

<a name="MT7001" />

### <a name="mt7001-could-not-resolve-host-ips-for-wifi-debugger-settings"></a>MT7001: WiFi デバッガーの設定のホストの ip アドレスを解決できませんでした。

*MSBuild タスク: DetectDebugNetworkConfigurationTaskBase*

トラブルシューティングの手順。

- 実行しようとしています。 `csharp -e 'System.Net.Dns.GetHostEntry (System.Net.Dns.GetHostName ()).AddressList'` (をようにする、IP アドレスおよびエラーではなく言うまでもなく)。
- 実行しようとしています"ping \`hostname\`"可能性がありますを提供する詳細については、like: `cannot resolve MyHost.local: Unknown host`

場合によっては、「ローカル ネットワーク」の問題であるし、不明なホストを追加することによって対処できます`127.0.0.1   MyHost.local`で`/etc/hosts`です。

<a name="MT7002" />

### <a name="mt7002-this-machine-does-not-have-any-network-adapters-this-is-required-when-debugging-or-profiling-on-device-over-wifi"></a>MT7002: このコンピューターには任意のネットワーク アダプターがありません。 これは、機能は、デバッグや WiFi にデバイスでプロファイリングを実行時に必要です。

*MSBuild タスク: DetectDebugNetworkConfigurationTaskBase*

<a name="MT7003" />

### <a name="mt7003-the-app-extension--does-not-contain-an-infoplist"></a>MT7003: アプリの拡張機能、' *'、Info.plist が含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7004" />

### <a name="mt7004-the-app-extension--does-not-specify-a-cfbundleidentifier"></a>MT7004: アプリの拡張機能、' *'、CFBundleIdentifier が指定されていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7005" />

### <a name="mt7005-the-app-extension--does-not-specify-a-cfbundleexecutable"></a>MT7005: アプリの拡張機能、' *'、CFBundleExecutable が指定されていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7006" />

### <a name="mt7006-the-app-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>アプリ拡張機能の MT7006: '\*' が無効な CFBundleIdentifier (\*)、メインのアプリ バンドルの CFBundleIdentifier (*) で始まらないです。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7007" />

### <a name="mt7007-the-app-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>アプリ拡張機能の MT7007: '\*' が、CFBundleIdentifier (\*) 無効なサフィックス".key"で終わること。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7008" />

### <a name="mt7008-the-app-extension--does-not-specify-a-cfbundleshortversionstring"></a>MT7008: アプリの拡張機能、' *'、CFBundleShortVersionString が指定されていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7009" />

### <a name="mt7009-the-app-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>MT7009: アプリの拡張機能、' *' が無効な Info.plist: NSExtension ディクショナリが含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7010" />

### <a name="mt7010-the-app-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionpointidentifier-value"></a>MT7010: アプリの拡張機能、' *' が無効な Info.plist: NSExtension ディクショナリに NSExtensionPointIdentifier 値が含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7011" />

### <a name="mt7011-the-watchkit-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionattributes-dictionary"></a>MT7011: WatchKit 拡張子 ' *' が無効な Info.plist: NSExtension ディクショナリに NSExtensionAttributes ディクショナリが含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7012" />

### <a name="mt7012-the-watchkit-extension--does-not-have-exactly-one-watch-app"></a>MT7012: WatchKit 拡張子 ' *' が 1 つだけの watch アプリはありません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7013" />

### <a name="mt7013-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-must-contain-the-watch-companion-capability"></a>MT7013: WatchKit 拡張子 '*' が無効な Info.plist: UIRequiredDeviceCapabilities' ウォッチ コンパニオン ' 機能を含める必要があります。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7014" />

### <a name="mt7014-the-watch-app--does-not-contain-an-infoplist"></a>Watch アプリの MT7014: ' *'、Info.plist が含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7015" />

### <a name="mt7015-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>Watch アプリの MT7015: ' *'、CFBundleIdentifier が指定されていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7016" />

### <a name="mt7016-the-watch-app--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>Watch アプリの MT7016: '\*' が無効な CFBundleIdentifier (\*)、メインのアプリ バンドルの CFBundleIdentifier (*) で始まらないです。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7017" />

### <a name="mt7017-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected-watch-4-but-found--"></a>Watch アプリの MT7017: '\*' は有効な UIDeviceFamily 値がありません。 必要な監視」(4)' が見つかりましたが、'\* (*)' です。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7018" />

### <a name="mt7018-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>Watch アプリの MT7018: ' *'、CFBundleExecutable が指定されていません

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7019" />

### <a name="mt7019-the-watch-app--has-an-invalid-wkcompanionappbundleidentifier-value--it-does-not-match-the-main-app-bundles-cfbundleidentifier-"></a>MT7019: Watch アプリ '\*' が無効な WKCompanionAppBundleIdentifier 値 ('\*')、メインのアプリ バンドルの CFBundleIdentifier と一致しません ('* ')。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7020" />

### <a name="mt7020-the-watch-app--has-an-invalid-infoplist-the-wkwatchkitapp-key-must-be-present-and-have-a-value-of-true"></a>Watch アプリの MT7020: ' *' が無効な Info.plist: WKWatchKitApp キーが存在して 'true' の値を持つ必要があります。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7021" />

### <a name="mt7021-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>Watch アプリの MT7021: ' *' が無効な Info.plist: LSRequiresIPhoneOS キーを表示することはできません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7022" />

### <a name="mt7022-the-watch-app--does-not-contain-a-watch-extension"></a>Watch アプリの MT7022: ' *' ウォッチ拡張機能が含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7023" />

### <a name="mt7023-the-watch-extension--does-not-contain-an-infoplist"></a>ウォッチ拡張機能の MT7023: ' *'、Info.plist が含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7024" />

### <a name="mt7024-the-watch-extension--does-not-specify-a-cfbundleidentifier"></a>ウォッチ拡張機能の MT7024: ' *'、CFBundleIdentifier が指定されていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7025" />

### <a name="mt7025-the-watch-extension--does-not-specify-a-cfbundleexecutable"></a>ウォッチ拡張機能の MT7025: ' *'、CFBundleExecutable が指定されていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7026" />

### <a name="mt7026-the-watch-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>ウォッチ拡張機能の MT7026: '\*' が無効な CFBundleIdentifier (\*)、メインのアプリ バンドルの CFBundleIdentifier (*) で始まらないです。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7027" />

### <a name="mt7027-the-watch-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>ウォッチ拡張機能の MT7027: '\*' が、CFBundleIdentifier (\*) 無効なサフィックス".key"で終わることです。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7028" />

### <a name="mt7028-the-watch-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>ウォッチ拡張機能の MT7028: ' *' が無効な Info.plist: NSExtension ディクショナリが含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7029" />

### <a name="mt7029-the-watch-extension--has-an-invalid-infoplist-the-nsextensionpointidentifier-must-be-comapplewatchkit"></a>ウォッチ拡張機能の MT7029: ' *' が無効な Info.plist: NSExtensionPointIdentifier は"com.apple.watchkit"である必要があります。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7030" />

### <a name="mt7030-the-watch-extension--has-an-invalid-infoplist-the-nsextension-dictionary-must-contain-nsextensionattributes"></a>ウォッチ拡張機能の MT7030: ' *' が無効な Info.plist: NSExtension ディクショナリ NSExtensionAttributes を含める必要があります。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7031" />

### <a name="mt7031-the-watch-extension--has-an-invalid-infoplist-the-nsextensionattributes-dictionary-must-contain-a-wkappbundleidentifier"></a>ウォッチ拡張機能の MT7031: ' *' が無効な Info.plist: NSExtensionAttributes ディクショナリは、WKAppBundleIdentifier を含める必要があります。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7032" />

### <a name="mt7032-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-should-not-contain-the-watch-companion-capability"></a>MT7032: WatchKit 拡張子 '*' が無効な Info.plist: UIRequiredDeviceCapabilities には' ウォッチ コンパニオン ' 機能が含まれていない必要があります。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7033" />

### <a name="mt7033-the-watch-app--does-not-contain-an-infoplist"></a>Watch アプリの MT7033: ' *'、Info.plist が含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7034" />

### <a name="mt7034-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>Watch アプリの MT7034: ' *'、CFBundleIdentifier が指定されていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7035" />

### <a name="mt7035-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected--but-found--"></a>Watch アプリの MT7035: '\*' は有効な UIDeviceFamily 値がありません。 予想 '\*'が見つかりました'\* (\*)' です。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7036" />

### <a name="mt7036-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>Watch アプリの MT7036: ' *'、CFBundleExecutable が指定されていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7037" />

### <a name="mt7037-the-watchkit-extension-extensionname-has-an-invalid-wkappbundleidentifier-value--it-does-not-match-the-watch-apps-cfbundleidentifier-"></a>MT7037: WatchKit 拡張機能 '{extensionName}' に無効な WKAppBundleIdentifier 値 ('\*')、Watch アプリの CFBundleIdentifier と一致しません ('\*')。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7038" />

### <a name="mt7038-the-watch-app--has-an-invalid-infoplist-the-wkcompanionappbundleidentifier-must-exist-and-must-match-the-main-app-bundles-cfbundleidentifier"></a>Watch アプリの MT7038: ' *' が無効な Info.plist: WKCompanionAppBundleIdentifier が存在し、メインのアプリ バンドルの CFBundleIdentifier に一致する必要があります。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7039" />

### <a name="mt7039-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>Watch アプリの MT7039: ' *' が無効な Info.plist: LSRequiresIPhoneOS キーを表示することはできません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7040" />

### <a name="mt7040-the-app-bundle-appbundlepath-does-not-contain-an-infoplist"></a>MT7040: アプリ バンドル {AppBundlePath} では、Info.plist は含まれません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7041" />

### <a name="mt7041-main-infoplist-path-does-not-specify-a-cfbundleidentifier"></a>MT7041: メイン Info.plist パスでは、CFBundleIdentifier は指定しません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7042" />

### <a name="mt7042-main-infoplist-path-does-not-specify-a-cfbundleexecutable"></a>MT7042: メイン Info.plist パスでは、CFBundleExecutable は指定しません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7043" />

### <a name="mt7043-main-infoplist-path-does-not-specify-a-cfbundlesupportedplatforms"></a>MT7043: メイン Info.plist パスでは、CFBundleSupportedPlatforms は指定しません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7044" />

### <a name="mt7044-main-infoplist-path-does-not-specify-a-uidevicefamily"></a>MT7044: メイン Info.plist パスでは、UIDeviceFamily は指定しません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7045" />

### <a name="mt7045-unrecognized-format-"></a>MT7045: 認識されない形式: * です。

*MSBuild タスク: PropertyListEditorTaskBase*

ここで * を指定できます。

- string
- array
- dict
- bool
- 実数
- 整数
- date
- [データ]

<a name="MT7046" />

### <a name="mt7046-add-entry--incorrectly-specified"></a>MT7046: を追加します。 エントリ、*、正しく指定されていません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7047" />

### <a name="mt7047-add-entry--contains-invalid-array-index"></a>MT7047: 追加: エントリ、*、無効な配列インデックスが含まれています。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7048" />

### <a name="mt7048-add--entry-already-exists"></a>MT7048: を追加します。 * エントリが既に存在します。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7049" />

### <a name="mt7049-add-cant-add-entry--to-parent"></a>MT7049: を追加します。 エントリを追加することはできません *、親にします。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7050" />

### <a name="mt7050-delete-cant-delete-entry--from-parent"></a>MT7050: 削除: のエントリを削除することはできません *、親からです。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7051" />

### <a name="mt7051-delete-entry--contains-invalid-array-index"></a>MT7051: 削除: エントリ、*、無効な配列インデックスが含まれています。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7052" />

### <a name="mt7052-delete-entry--does-not-exist"></a>MT7052: 削除: エントリ、*、存在しません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7053" />

### <a name="mt7053-import-entry--incorrectly-specified"></a>MT7053: インポート: エントリ、*、正しく指定されていません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7054" />

### <a name="mt7054-import-entry--contains-invalid-array-index"></a>MT7054: インポート: エントリ、*、無効な配列インデックスが含まれています。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7055" />

### <a name="mt7055-import-error-reading-file-"></a>MT7055: インポート: ファイルの読み取りエラー: * です。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7056" />

### <a name="mt7056-import-cant-add-entry--to-parent"></a>MT7056: インポート: エントリを追加することはできません *、親にします。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7057" />

### <a name="mt7057-merge-cant-add-array-entries-to-dict"></a>MT7057 マージ: dict. に配列のエントリを追加することはできません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7058" />

### <a name="mt7058-merge-specified-entry-must-be-a-container"></a>MT7058。 マージ: 指定されたエントリは、コンテナーである必要があります。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7059" />

### <a name="mt7059-merge-entry--contains-invalid-array-index"></a>MT7059。 マージ: エントリ、*、無効な配列インデックスが含まれています。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7060" />

### <a name="mt7060-merge-entry--does-not-exist"></a>MT7060。 マージ: エントリ、*、存在しません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7061" />

### <a name="mt7061-merge-error-reading-file-"></a>MT7061。 マージ: ファイルの読み取りエラー: * です。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7062" />

### <a name="mt7062-set-entry--incorrectly-specified"></a>MT7062: 設定: エントリ、*、正しく指定されていません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7063" />

### <a name="mt7063-set-entry--contains-invalid-array-index"></a>MT7063: 設定: エントリ、*、無効な配列インデックスが含まれています。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7064" />

### <a name="mt7064-set-entry--does-not-exist"></a>MT7064: 設定: エントリ、*、存在しません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7065" />

### <a name="mt7065-unknown-propertylist-editor-action-"></a>MT7065: 不明な PropertyList エディター アクション: * です。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7066" />

### <a name="mt7066-error-loading--"></a>MT7066: 読み込みエラー ' *': * です。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7067" />

### <a name="mt7067-error-saving--"></a>MT7067: エラー保存 ' *': * です。

*MSBuild タスク: PropertyListEditorTaskBase*

## <a name="mt8xxx-runtime-error-messages"></a>MT8xxx: 実行時のエラー メッセージ

<!--
 MT8xxx runtime
  MT800x misc
  -->

<a name="MT8001" />

### <a name="mt8001-version-mismatch-between-the-native-xamarinios-runtime-and-monotouchdll-please-reinstall-xamarinios"></a>ネイティブ Xamarin.iOS ランタイムと monotouch.dll 間 MT8001: バージョンが一致していません。 Xamarin.iOS を再インストールしてください。

<a name="MT8002" />

### <a name="mt8002-could-not-find-the-method--in-the-type-"></a>MT8002: メソッドが見つかりませんでした '\*'in '型\*' です。

<a name="MT8003" />

### <a name="mt8003-failed-to-find-the-closed-generic-method--on-the-type-"></a>: MT8003 クローズ ジェネリック メソッドが見つかりませんでした '\*'type' で\*' です。

<a name="MT8004" />

### <a name="mt8004-cannot-create-an-instance-of--for-the-native-object-0x-of-type--because-another-instance-already-exists-for-this-native-object-of-type-"></a>MT8004: がのインスタンスを作成することはできません * のネイティブ オブジェクト 0 x * (型の ' *') このネイティブ オブジェクトの別のインスタンスが既に存在するため、(型の *)。

<a name="MT8005" />

### <a name="mt8005-wrapper-type--is-missing-its-native-objectivec-class-"></a>MT8005: ラッパー型 '\*'が不足しているネイティブもクラス'\*' です。

<a name="MT8006" />

### <a name="mt8006-failed-to-find-the-selector--on-the-type-"></a>MT8006: セレクターが見つかりませんでした '\*'type' で\*'。

<a name="MT8007" />

### <a name="mt8007-cannot-get-the-method-descriptor-for-the-selector--on-the-type--because-the-selector-does-not-correspond-to-a-method"></a>MT8007: は、セレクターのメソッドの記述子を取得できません '\*'type' で\*' セレクターがメソッドに対応していないため、

<a name="MT8008" />

### <a name="mt8008-the-loaded-version-of-xamariniosdll-was-compiled-for--bits-while-the-process-is--bits-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8008: Xamarin.iOS.dll の読み込まれているバージョンは用にコンパイルされた * 処理中に、ビット * ビットです。 バグを送信してください http://bugzilla.xamarin.com です。

これは、ビルド処理で問題があることを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8009" />

### <a name="mt8009-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-s-parameter--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8009: メソッドの変換メソッドを委任するブロックが見つかりません *.*'s パラメーター # * です。 バグを送信してください http://bugzilla.xamarin.com です。

これは、API が正しくバインドされていないを示します。 Xamarin によって公開される API の場合は、当社 bugzilla でバグを送信してください ([http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)) 場合は、サード パーティ製のバインディングでは、製造元に問い合わせてください。

<a name="MT8010" />

### <a name="mt8010-native-type-size-mismatch-between-xamariniosmacdll-and-the-executing-architecture-xamariniosmacdll-was-built-for--bit-while-the-current-process-is--bit"></a>Xamarin 間 MT8010: ネイティブ型のサイズの不一致です。[iOS |Mac] .dll と実行中のアーキテクチャです。 Xamarin。[iOS |Mac] .dll が構築された *-bit で、現在のプロセスは *-ビットです。

これは、ビルド処理で問題があることを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8011" />

### <a name="mt8011-unable-to-locate-the-delegate-to-block-conversion-attribute-delegateproxy-for-the-return-value-for-the-method--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8011: メソッドの戻り値のブロック変換属性 ([DelegateProxy]) にデリゲートが見つかりません *.* です。 バグを送信してください http://bugzilla.xamarin.com です。

Xamarin.iOS は、(ブロックをデリゲートに変換) を実行時に、必要なメソッドを検索できませんでした。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8012" />

### <a name="mt8012-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8012: メソッドの戻り値の無効な DelegateProxyAttribute *.*: DelegateType が null です。 バグを送信してください http://bugzilla.xamarin.com です。

該当するメソッドの DelegateProxy 属性が正しくありません。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8013" />

### <a name="mt8013-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-2-specifies-a-type-without-a-handler-field-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8013: メソッドの戻り値の無効な DelegateProxyAttribute *.*: DelegateType ({2}) 'Handler' フィールドがない型を指定します。 バグを送信してください http://bugzilla.xamarin.com です。

該当するメソッドの DelegateProxy 属性が正しくありません。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8014" />

### <a name="mt8014-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8014: メソッドの戻り値の無効な DelegateProxyAttribute *.*:「DelegateType ({2}) 'Handler' フィールド null です。 バグを送信してください http://bugzilla.xamarin.com です。

該当するメソッドの DelegateProxy 属性が正しくありません。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8015" />

### <a name="mt8015-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-not-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8015: メソッドの戻り値の無効な DelegateProxyAttribute *.*:「DelegateType ({2}) 'Handler' フィールドは、デリゲートではないのは、* です。 バグを送信してください http://bugzilla.xamarin.com です。

該当するメソッドの DelegateProxy 属性が正しくありません。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8016" />

### <a name="mt8016-unable-to-convert-delegate-to-block-for-the-return-value-for-the-method--because-the-input-isnt-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8016: メソッドの戻り値のブロックをデリゲートに変換できません *.* 入力には、委任が存在しないためは、* です。 バグを送信してください http://bugzilla.xamarin.com です。

該当するメソッドの DelegateProxy 属性が正しくありません。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<!-- 8017 is used by mmp -->

<a name="MT8018" />

### <a name="mt8018-internal-consistency-error-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8018: 内部の一貫性エラーがあります。 バグ報告を送信してください http://bugzilla.xamarin.com です。

これは、Xamarin.iOS のバグを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8019" />

### <a name="mt8019-could-not-find-the-assembly--in-the-loaded-assemblies"></a>MT8019: 見つかりませんでした。 アセンブリ * 読み込まれたアセンブリからです。

これは、Xamarin.iOS のバグを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8020" />

### <a name="mt8020-could-not-find-the-module-with-metadatatoken--in-the-assembly-"></a>MT8020: MetadataToken でモジュールが見つかりませんでした *、アセンブリ内で * です。

これは、Xamarin.iOS のバグを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8021" />

### <a name="mt8021-unknown-implicit-token-type-"></a>MT8021: 不明な暗黙的なトークンの種類: * です。

これは、Xamarin.iOS のバグを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8022" />

### <a name="mt8022-expected-the-token-reference--to-be-a--but-its-a--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8022: 予想トークンへの参照 * である、* は、* です。 バグ報告を送信してください http://bugzilla.xamarin.com です。

これは、Xamarin.iOS のバグを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8023" />

### <a name="mt8023-an-instance-object-is-required-to-construct-a-closed-generic-method-for-the-open-generic-method--token-reference--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8023: インスタンス オブジェクトがオープン ジェネリック メソッドのクローズ ジェネリック メソッドを構築するために必要な: * (トークンの参照: *)。 バグ報告を送信してください http://bugzilla.xamarin.com です。

これは、Xamarin.iOS のバグを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8024" />

### <a name="mt8024-could-not-find-a-valid-extension-type-for-the-smart-enum-smarttype-please-file-a-bug-at-httpsbugzillaxamarincom"></a>MT8024: は、スマート enum '{smart_type}' の拡張機能の有効な型を見つけられませんでした。 バグを送信してください https://bugzilla.xamarin.com です。

これは、Xamarin.iOS のバグを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。
