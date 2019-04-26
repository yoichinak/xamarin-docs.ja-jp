---
title: Xamarin.iOS のエラー
description: このドキュメントでは、Xamarin.iOS アプリケーションをバンドルするために使用するツールの mtouch によって生成されたさまざまなエラーについて説明します。 エラーがコードによって一覧表示されて、完全な説明を指定します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9F76162B-D622-45DA-996B-2FBF8017E208
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/06/2018
ms.openlocfilehash: e6e3a989db922dc2941cca4c888c862ffe159241
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61422053"
---
# <a name="xamarinios-errors"></a>Xamarin.iOS のエラー

## <a name="mt0xxx-mtouch-error-messages"></a>MT0xxx: mtouch エラー メッセージ

たとえば、 パラメーター、環境、ツールがありません。

<!--
 MT0xxx mtouch itself, e.g. parameters, environment (e.g. missing tools)
 https://github.com/xamarin/xamarin-macios/blob/master/tools/mtouch/error.cs
    -->

<a name="MT0000" />

### <a name="mt0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcomxamarinxamarin-maciosissuesnew"></a>MT0000:予期しないエラーでバグ報告を入力してください https://github.com/xamarin/xamarin-macios/issues/new

予期しないエラーが発生しました。 ください[バグ レポートを](https://github.com/xamarin/xamarin-macios/issues/new)できるだけ多くの情報を含みます。

* 完全な詳細レベルでログをビルド (例:`-v -v -v -v`で、**追加 mtouch 引数**)。
* エラーを再現する最小のテスト_ケースそして
* すべてのバージョン情報

正確なバージョン情報を取得する最も簡単な方法が使用するには、 **Visual Studio for Mac** ] メニューの [**について Visual Studio for Mac**項目、**詳細の表示**ボタンをクリックし、コピー/貼り付け、バージョン情報 (使用することができます、**コピー情報**ボタン)。

<a name="MT0001" />

### <a name="mt0001--devname-was-provided-without-any-device-specific-action"></a>MT0001: '-devname' のデバイスに固有の操作なしに指定されました

これは - devname がときに何もデバイスに固有の mtouch に渡された場合に生成される警告 (-logdev/installdev/killdev/launchdev/-listapps) が要求されました。

<a name="MT0002" />

### <a name="mt0002-could-not-parse-the-environment-variable-"></a>MT0002:環境変数を解析できませんでした *。

このエラーは、無効な環境キーを設定しようとする場合は発生変数の値のペアを = です。 正しい形式です。 `mtouch --setenv=VARIABLE=VALUE`

<a name="MT0003" />

### <a name="mt0003-application-name-exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MT0003:アプリケーション名 '* .exe' SDK またはプロダクト アセンブリ (.dll) 名と競合します。

実行可能アセンブリの名前と、アプリケーションの名前は、アプリの任意の dll の名前と一致ことはできません。 実行可能ファイルの名前を変更してください。

<a name="MT0004" />

### <a name="mt0004-new-refcounting-logic-requires-sgen-to-be-enabled-too"></a>MT0004:新しいカウント ロジックでは、SGen が有効にする必要があります。

カウントの拡張機能を有効にした場合、SGen ガベージ コレクター、プロジェクトの iOS ビルド オプション (詳細設定 タブ) でも有効にする必要があります。

Xamarin.iOS 7.2.1 以降この要件が解除されて、新しい refcounting ロジックは Boehm と SGen ガベージ コレクターの両方で有効にすることができます。

<a name="MT0005" />

### <a name="mt0005-the-output-directory--does-not-exist"></a>MT0005:出力ディレクトリ * が存在しません。

ディレクトリを作成してください。

このエラーはもはや生成されません、mtouch は自動的に作成、ディレクトリがない場合。

<a name="MT0006" />

### <a name="mt0006-there-is-no-devel-platform-at--use---platformplat-to-specify-the-sdk"></a>MT0006:Devel プラットフォームではありません *、- プラットフォームを使用して、SDK を指定するフォームを = です。

Xamarin.iOS には、エラー メッセージに記載されている場所に SDK のディレクトリを見つけることができません。 パスが正しいことを確認してください。

<a name="MT0007" />

### <a name="mt0007-the-root-assembly--does-not-exist"></a>MT0007:ルート アセンブリ * が存在しません。

Xamarin.iOS には、エラー メッセージに記載されている場所にアセンブリを見つけることはできません。 パスが正しいことを確認してください。

<a name="MT0008" />

### <a name="mt0008-you-should-provide-one-root-assembly-only-found--assemblies-"></a>MT0008:ルート アセンブリのみ、見つかった # 提供する必要があります。 *。

1 つ以上のルート アセンブリは、1 つだけルート アセンブリがあっても、mtouch に渡されました。

<a name="MT0009" />

### <a name="mt0009-error-while-loading-assemblies-"></a>MT0009:アセンブリの読み込み中にエラー: *。

ルート アセンブリの参照アセンブリの読み込み中にエラーが発生しました。 詳細については、ビルド出力で指定する可能性があります。

<a name="MT0010" />

### <a name="mt0010-could-not-parse-the-command-line-arguments-"></a>MT0010:コマンドライン引数を解析できませんでした: *。

コマンドライン引数の解析中にエラーが発生しました。 これらがすべて正しいことを確認してください。

<a name="MT0011" />

### <a name="mt0011--was-built-against-a-more-recent-runtime--than-monotouch-supports"></a>MT0011: * MonoTouch サポートより新しいランタイム (*) がビルドされます。

Xamarin.iOS の BCL を使用して構築されたいないクラス ライブラリへの参照がプロジェクトに含まれるため、この警告は通常報告されます。

のみ、.NET 2.0 をサポートするシステムでは、.NET 4.0 の SDK を使用して、アプリが機能しないのと同じ方法では、.NET 4.0 を使用して構築されたライブラリが Xamarin.iOS で動作しない可能性があります、Xamarin.iOS で存在しない API を使用します。

全般的なソリューションでは、Xamarin.iOS クラス ライブラリとしてライブラリをビルドします。 これは、新しい Xamarin.iOS クラス ライブラリ プロジェクトを作成して実行でき、すべてのソース ファイルを追加します。 場合は、ライブラリのソース コードがない、ベンダーに問い合わせて、ライブラリの Xamarin.iOS と互換性のあるバージョンを提供することを要求してください。

<a name="MT0012" />

### <a name="mt0012-incomplete-data-is-provided-to-complete-"></a>MT0012:完了する不完全なデータが提供される *。

このエラーは、Xamarin.iOS の現在のバージョンでは今後報告されません。

<a name="MT0013" />

### <a name="mt0013-profiling-support-requires-sgen-to-be-enabled-too"></a>MT0013:プロファイルのサポートには、sgen が有効にする必要があります。

SGen (--sgen) プロファイリングする場合に有効にする必要があります (--プロファイル) を有効にします。

<a name="MT0014" />

### <a name="mt0014-the-ios--sdk-does-not-support-building-applications-targeting-"></a>MT0014:IOS * SDK が対象とするアプリケーションの構築をサポートしていません *。

これは、次の状況で発生します。

*  ARMv6 が有効になっているし、Xcode 4.5 またはそれ以降がインストールされています。
*  ARMv7s が有効になっているし、Xcode 4.4 以降がインストールされています。

Xcode のインストールされているバージョンが選択されているアーキテクチャをサポートしていることを確認してください。

<a name="MT0015" />

### <a name="mt0015-invalid-abi--supported-abis-are-i386-x8664--armv7-armv7llvm-armv7llvmthumb2-armv7s-armv7sllvm-armv7sllvmthumb2-arm64-and-arm64llvm"></a>MT0015:ABI が無効です: *。 サポートされる Abi が: i386、x86_64、armv7、armv7 + llvm、armv7 と llvm + thumb2、armv7s、armv7s + llvm、armv7s + llvm + thumb2、arm64 および arm64 と llvm します。

無効な ABI mtouch が渡されました。 有効な ABI を指定してください。

<a name="MT0016" />

### <a name="mt0016-the-option--has-been-deprecated"></a>MT0016:オプション * 非推奨とされました。

Mtouch に説明したオプションは非推奨とされました、無視されます。

<a name="MT0017" />

### <a name="mt0017-you-should-provide-a-root-assembly"></a>MT0017:ルート アセンブリを指定する必要があります。

ルート アセンブリ (通常、メインの実行可能ファイル) を指定する必要があります、アプリを構築するときにします。

<a name="MT0018" />

### <a name="mt0018-unknown-command-line-argument-"></a>MT0018:不明なコマンドライン引数: *。

Mtouch は、エラー メッセージに記載されているコマンドライン引数を認識しません。

<a name="MT0019" />

### <a name="mt0019-only-one---loginstallkilllaunchdev-or---launchdebugsim-option-can-be-used"></a>MT0019:1 つだけ--[ログ | インストール | kill | 起動] 開発または -[起動 | デバッグ] sim オプションを使用できます。

同時に使用することはできません mtouch のいくつかのオプションがあります。

-  --logdev
-  --installdev
-  --killdev
-  --launchdev
-  --launchdebug
-  --launchsim

<a name="MT0020" />

### <a name="mt0020-the-valid-options-for--are-"></a>MT0020:有効なオプション '\*'は'\*'。

<a name="MT0021" />

### <a name="mt0021-cannot-compile-using-gccg---use-gcc-when-using-the-static-registrar-this-is-the-default-when-compiling-for-device-either-remove-the---use-gcc-flag-or-use-the-dynamic-registrar---registrardynamic"></a>MT0021:Gcc を使用してコンパイルできません/g++ (--使用 gcc) (これは、既定のデバイスのコンパイル時に) 静的なレジストラーを使用する場合。 削除するか-使用 gcc フラグまたは動的なレジストラーを使用して (--レジストラー: 動的)。

<a name="MT0022" />

### <a name="mt0022-the-options---unsupported--enable-generics-in-registrar-and---registrar-are-not-compatible"></a>MT0022:オプション '- サポートされていない - 有効にする-ジェネリック-で-レジストラー' と '--レジストラー' は互換性がありません。

どちらのオプションを削除`--unsupported--enable-generics-in-registrar`と`--registrar`します。 既定のレジストラーに Xamarin.iOS 7.2.1 以降、ジェネリックがサポートされています。

このエラーは表示されなくなります (コマンドライン引数`--unsupported--enable-generics-in-registrar`mtouch から削除されました)。

<a name="MT0023" />

### <a name="mt0023-application-name-exe-conflicts-with-another-user-assembly"></a>MT0023:アプリケーション名 '* .exe' 別のユーザーのアセンブリと競合します。

実行可能アセンブリの名前と、アプリケーションの名前は、アプリの任意の dll の名前と一致ことはできません。 実行可能ファイルの名前を変更してください。

<a name="MT0024" />

### <a name="mt0024-could-not-find-required-file-"></a>MT0024:必要なファイルが見つかりませんでした。 ' *'。

<a name="MT0025" />

### <a name="mt0025-no-sdk-version-was-provided-please-add---sdkxy-to-specify-which-ios-sdk-should-be-used-to-build-your-application"></a>MT0025:SDK のバージョンが指定されていません。 追加してください。`--sdk=X.Y`を指定する iOS アプリケーションのビルドに SDK を使用する必要があります。

<a name="MT0026" />

### <a name="mt0026-could-not-parse-the-command-line-argument--"></a>MT0026:コマンドライン引数を解析できませんでした ' *': *

<a name="MT0027" />

### <a name="mt0027-the-options--and--are-not-compatible"></a>MT0027:オプション\*'と'\*' は互換性がありません。

<a name="MT0028" />

### <a name="mt0028-cannot-enable-pie--pie-when-targeting-ios-41-or-earlier-please-disable-pie--piefalse-or-set-the-deployment-target-to-at-least-ios-42"></a>MT0028:円グラフを有効にすることはできません (の円) 4.1 またはそれ以前の iOS を対象とする場合。 円グラフを無効にしてください (-円: false) 以上に、配置ターゲットを設定または iOS 4.2

<a name="MT0029" />

### <a name="mt0029-repl---enable-repl-is-only-supported-in-the-simulator---sim"></a>MT0029:REPL (--有効にする repl) は、シミュレーターでのみサポート (--sim)。

REPL は、シミュレーターのビルドしている場合にのみサポートされます。 これは、ため、渡した場合`--enable-repl`mtouch、する必要がありますも合格する`--sim`。

<a name="MT0030" />

### <a name="mt0030-the-executable-name--and-the-app-name--are-different-this-may-prevent-crash-logs-from-getting-symbolicated-properly"></a>MT0030:実行可能ファイル名 (\*) とアプリ名 (\*) が異なる可能性がありますこうクラッシュ ログが正しくシンボルを取得します。

Xcode の symbolicates とき (関数名、ファイル/行番号をメモリ アドレスに変換)、実行可能ファイルとアプリの別の名前 (拡張子なし) 場合に、処理が失敗する可能性があります。

解決するには、プロジェクトのビルド/iOS アプリケーションのオプション、またはプロジェクトのビルド/出力オプションの変更 'アセンブリ名' で 'アプリケーション名' を変更このいずれか。

<a name="MT0031" />

### <a name="mt0031-the-command-line-arguments---enable-background-fetch-and---launch-for-background-fetch-require---launchsim-too"></a>MT0031:コマンドライン引数 '--有効にする-バック グラウンド フェッチ' と '-起動のバック グラウンド フェッチ' が必要です '--launchsim' すぎます。

<a name="MT0032" />

### <a name="mt0032-the-option---debugtrack-is-ignored-unless---debug-is-also-specified"></a>MT0032:オプション '-debugtrack' しない限りは無視されます'--デバッグ ' も指定します。

<a name="MT0033" />

### <a name="mt0033-a-xamarinios-project-must-reference-either-monotouchdll-or-xamariniosdll"></a>MT0033:Monotouch.dll または Xamarin.iOS.dll Xamarin.iOS プロジェクトを参照する必要があります。

<a name="MT0034" />

### <a name="mt0034-cannot-include-both-monotouchdll-and-xamariniosdll-in-the-same-xamarinios-project----is-referenced-explicitly-while--is-referenced-by-"></a>MT0034:-同じ Xamarin.iOS プロジェクトで 'monotouch.dll' と 'Xamarin.iOS.dll' の両方を含めることはできません '\*' を明示的に参照中に '\*' によって参照される ' *'。

<!-- MT0035 unused -->

<a name="MT0036" />

### <a name="mt0036-cannot-launch-a--simulator-for-a--app-please-enable-the-correct-architectures-in-your-projects-ios-build-options-advanced-page"></a>MT0036:起動できません、* 用のシミュレーターは、* アプリ。 プロジェクトの iOS ビルド オプション (詳細設定 ページ) で正しいミラーサイトを有効にしてください。

<a name="MT0037" />

### <a name="mt0037-monotouchdll-is-not-64-bit-compatible-either-reference-xamariniosdll-or-do-not-build-for-a-64-bit-architecture-arm64-andor-x8664"></a>MT0037: monotouch.dll は、64 ビット互換性です。 Xamarin.iOS.dll を参照するか (ARM64 や x86_64) は、64 ビット アーキテクチャ用に構築しないでください。

<a name="MT0038" />

### <a name="mt0038-the-old-registrars---registraroldstaticolddynamic-are-not-supported-when-referencing-xamariniosdll"></a>MT0038:古いレジストラー (--レジストラー: oldstatic | olddynamic) Xamarin.iOS.dll を参照するときにサポートされていません。

<a name="MT0039" />

### <a name="mt0039-applications-targeting-armv6-cannot-reference-xamariniosdll"></a>MT0039:ARMv6 を対象とするアプリケーションでは、Xamarin.iOS.dll を参照できません。

<a name="MT0040" />

### <a name="mt0040-could-not-find-the-assembly--referenced-by-"></a>MT0040:アセンブリが見つかりませんでした '\*', によって参照される'\*'。

<a name="MT0041" />

### <a name="mt0041-cannot-reference-both-monotouchdll-and-xamariniosdll"></a>MT0041:'Monotouch.dll' と 'Xamarin.iOS.dll' の両方を参照することはできません。

<a name="MT0042" />

### <a name="mt0042-no-reference-to-either-monotouchdll-or-xamariniosdll-was-found-a-reference-to-monotouchdll-will-be-added"></a>MT0042:Monotouch.dll または Xamarin.iOS.dll への参照が見つかりませんでした。 Monotouch.dll への参照が追加されます。

<a name="MT0043" />

### <a name="mt0043-the-boehm-garbage-collector-is-currently-not-supported-when-referencing-xamariniosdll-the-sgen-garbage-collector-has-been-selected-instead"></a>MT0043:Boehm ガベージ コレクターは現在サポートされていません 'Xamarin.iOS.dll' を参照するときにします。 SGen ガベージ コレクターが代わりに選択されています。

統合プロジェクトでは、SGen ガベージ コレクターだけがサポートされます。 ガベージ コレクターと Boehm を指定するその他の mtouch フラグがないことを確認します。

<a name="MT0044" />

### <a name="mt0044---listsim-is-only-supported-with-xcode-60-or-later"></a>MT0044:--listsim は Xcode 6.0 以降にのみサポートされます。

新しい Xcode バージョンをインストールします。

<a name="MT0045" />

### <a name="mt0045---extension-is-only-supported-when-using-the-ios-80-or-later-sdk"></a>MT0045:--iOS を使用する場合、拡張機能がサポートされてのみ SDK の 8.0 (またはそれ以降)。

<!-- MT0046 is not reported anymore -->

<a name="MT0047" />

### <a name="mt0047-the-minimum-deployment-target-for-unified-applications-is-511-the-current-deployment-target-is--please-select-a-newer-deployment-target-in-your-projects-ios-application-options"></a>MT0047:統合アプリケーションの最小展開ターゲットは、5.1.1 は、現在の配置ターゲットは ' *'。 プロジェクトの iOS アプリケーションのオプションでは、新しい配置ターゲットを選択してください。

<!-- MT0048 is not reported anymore -->

<a name="MT0049" />

### <a name="mt0049-framework-is-supported-only-if-deployment-target-is-80-or-later--features-might-not-work-correctly"></a>MT0049: デプロイ ターゲットが 8.0 以降である場合にのみ、*.framework はサポートされています。 * 機能が正しく動作しない可能性があります。

配置ターゲットを指す iOS のバージョンでは、指定のフレームワークがサポートされていません。 新しい iOS のバージョンでは、配置ターゲットを更新するか、指定のフレームワークの使用状況をアプリから削除します。

<!-- MT0050 is not reported anymore -->

<a name="MT0051" />

### <a name="mt0051-xamarinios--requires-xcode-50-or-later-the-current-xcode-version-found-in--is-"></a>MT0051:Xamarin.iOS * Xcode 5.0 以降が必要です。 現在のバージョンの Xcode (で見つかった *) は * です。

新しい Xcode をインストールします。

<a name="MT0052" />

### <a name="mt0052-no-command-specified"></a>MT0052:指定されたコマンドはありません。

Mtouch のアクションが指定されていません。

<!-- 0053 is used by mmp -->

<a name="MT0054" />

### <a name="mt0054-unable-to-canonicalize-the-path--"></a>MT0054:パスを正規化することができません ' *': *。

これは、内部エラーです。 このエラーが発生した場合は、バグを提出してください[ http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)します。

<a name="MT0055" />

### <a name="mt0055-the-xcode-path--does-not-exist"></a>MT0055:Xcode パス ' *' が存在しません。

使用して Xcode パスが渡される`--sdkroot`存在しません。 有効なパスを指定してください。

<a name="MT0056" />

### <a name="mt0056-cannot-find-xcode-in-the-default-location-applicationsxcodeapp-please-install-xcode-or-pass-a-custom-path-using---sdkroot-path"></a>MT0056:既定の場所に Xcode を見つけることができません (/'/applications/xcode.app')。 Xcode をインストールするか、--sdkroot を使用して、カスタム パスを渡すください<path>します。

<a name="MT0057" />

### <a name="mt0057-cannot-determine-the-path-to-xcodeapp-from-the-sdk-root--please-specify-the-full-path-to-the-xcodeapp-bundle"></a>MT0057:Sdk のルートから Xcode.app にパスを決定できません ' *'。 Xcode.app バンドルへの完全パスを指定してください。

使用して渡されるパス`--sdkroot`有効な Xcode アプリを指定しません。 Xcode のアプリへのパスを指定してください。

<a name="MT0058" />

### <a name="mt0058-the-xcodeapp--is-invalid-the-file--does-not-exist"></a>MT0058:Xcode.app '\*' が無効です (ファイル '\*' が存在しない)。

使用して渡されるパス`--sdkroot`有効な Xcode アプリを指定しません。 Xcode のアプリへのパスを指定してください。

<a name="MT0059" />

### <a name="mt0059-could-not-find-the-currently-selected-xcode-on-the-system-"></a>MT0059:システムで現在選択されている Xcode が見つかりませんでした *。

<a name="MT0060" />

### <a name="mt0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned--but-that-directory-does-not-exist"></a>MT0060:システムで現在選択されている Xcode が見つかりませんでした。 'xcode 選択--印刷パス' 返される ' *' が、そのディレクトリが存在しません。

<a name="MT0061" />

### <a name="mt0061-no-xcodeapp-specified-using---sdkroot-using-the-system-xcode-as-reported-by-xcode-select---print-path-"></a>MT0061:(--Sdkroot を使用)、指定されていない Xcode.app 'xcode の選択--印刷パス' によって報告されたシステム Xcode を使用します *。

これは、Xcode がされますを説明する情報の警告、指定されていないために使用します。

<a name="MT0062" />

### <a name="mt0062-no-xcodeapp-specified-using---sdkroot-or-xcode-select---print-path-using-the-default-xcode-instead-"></a>MT0062:指定 (--sdkroot または 'xcode の選択--印刷パス' を使用)、代わりに既定の Xcode を使用していない Xcode.app: *

これは、Xcode がされますを説明する情報の警告、指定されていないために使用します。

<a name="MT0063" />

### <a name="mt0063-cannot-find-the-executable-in-the-extension--no-cfbundleexecutable-entry-in-its-infoplist"></a>MT0063:拡張機能で、実行可能ファイルを検索することはできません * (Info.plist で CFBundleExecutable エントリがありません)

すべての Info.plist では、エントリは、ビルド時に自動的に生成する必要がありますが、実行可能ファイル (CFBundleExecutable エントリを使用)、なることが必要です。

これは通常; Xamarin.iOS のバグを示しますバグ報告を提出してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)テスト_ケースを使用します。

<a name="MT0064" />

### <a name="mt0064-xamarinios-only-supports-embedded-frameworks-with-unified-projects"></a>MT0064:Xamarin.iOS には、埋め込みフレームワーク統合プロジェクトでのみサポートされます。

Xamarin.iOS は、Unified API を使用する場合のみ埋め込みフレームワークをサポートします。Unified API を使用して、プロジェクトを更新してください。

<a name="MT0065" />

### <a name="mt0065-xamarinios-only-supports-embedded-frameworks-when-deployment-target-is-at-least-80-current-deployment-target--embedded-frameworks-"></a>MT0065:Xamarin.iOS は、デプロイ ターゲットが 8.0 以上である場合のみ埋め込みフレームワークをサポート (現在の配置ターゲット: * 埋め込みフレームワーク: *)

Xamarin.iOS は、(以前のバージョンの iOS では、埋め込みのフレームワークをサポートしていない) ため、配置ターゲットが 8.0 以上でのみ埋め込みフレームワークをサポートします。

8.0 以上のプロジェクトの Info.plist で配置ターゲットを更新してください。

<a name="MT0066" />

### <a name="mt0066-invalid-build-registrar-assembly-"></a>MT0066:無効なビルド レジストラー アセンブリ: *

これは通常; Xamarin.iOS のバグを示しますバグ報告を提出してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)テスト_ケースを使用します。

<a name="MT0067" />

### <a name="mt0067-invalid-registrar-"></a>MT0067:レジストラーが無効です: *

これは通常; Xamarin.iOS のバグを示しますバグ報告を提出してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)テスト_ケースを使用します。

<a name="MT0068" />

### <a name="mt0068-invalid-value-for-target-framework-"></a>MT0068:ターゲット フレームワークに無効な値: *。

使用して、無効なターゲット フレームワークが渡された-ターゲット フレームワークの引数。 有効なターゲット フレームワークを指定してください。

<a name="MT0069" />

<!--### MT0069: currently unused -->

<a name="MT0070" />

### <a name="mt0070-invalid-target-framework--valid-target-frameworks-are-"></a>MT0070:無効なターゲット フレームワーク: *。 有効なターゲット フレームワークは、: *。

使用して、無効なターゲット フレームワークが渡された-ターゲット フレームワークの引数。 有効なターゲット フレームワークを指定してください。

<a name="MT0071" />

### <a name="mt0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinios-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT0071:不明なプラットフォーム: *。 これは通常 Xamarin.iOS; のバグを示しますバグ報告を送信してください http://bugzilla.xamarin.com とテスト_ケースをします。

これは通常; Xamarin.iOS のバグを示しますバグ報告を提出してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)テスト_ケースを使用します。

<a name="MT0072" />

### <a name="mt0072-extensions-are-not-supported-for-the-platform-"></a>MT0072:プラットフォームの拡張機能はサポートされていません ' *'。

これは通常; Xamarin.iOS のバグを示しますバグ報告を提出してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)テスト_ケースを使用します。

<a name="MT0073" />

### <a name="mt0073-xamarinios--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a>MT0073:Xamarin.iOS * の配置ターゲットをサポートしていません * (最小値は *)。 プロジェクトの Info.plist で新しい配置ターゲットを選択してください。

エラー メッセージに指定されている最小展開ターゲットプロジェクトの Info.plist で新しい配置ターゲットを選択してください。

配置ターゲットの更新ができない場合は、以前のバージョンの Xamarin.iOS を使用ししてください。

<a name="MT0074" />

### <a name="mt0074-xamarinios--does-not-support-a-minimum-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinios"></a>MT0074:Xamarin.iOS * の最小展開ターゲットをサポートしていません * (最大値は *)。 プロジェクトの Info.plist で以前の配置ターゲットを選択するか、Xamarin.iOS の新しいバージョンにアップグレードしてください。

Xamarin.iOS では、この特定のバージョンの Xamarin.iOS 用にビルドされたバージョンより新しいバージョンに最小展開ターゲットを設定することはできません。

プロジェクトの info.plist の古い最小展開ターゲットを選択するか、Xamarin.iOS の新しいバージョンにアップグレードしてください。

<a name="MT0075" />

### <a name="mt0075-invalid-architecture--for--projects-valid-architectures-are-"></a>MT0075:無効なアーキテクチャ ' *' の * プロジェクト。 有効なアーキテクチャは、: *

無効なアーキテクチャが指定されました。 アーキテクチャが有効であることを確認してください。

<a name="MT0076" />

### <a name="mt0075-no-architecture-specified-using-the---abi-argument-an-architecture-is-required-for--projects"></a>MT0075:(--Abi 引数を使用して) 指定アーキテクチャはありません。 アーキテクチャが必要です。 * プロジェクト。

これは通常; Xamarin.iOS のバグを示しますバグ報告を提出してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)テスト_ケースを使用します。

<a name="MT0077" />

### <a name="mt0076-watchos-projects-must-be-extensions"></a>MT0076:WatchOS プロジェクトは、拡張機能である必要があります。

これは通常; Xamarin.iOS のバグを示しますバグ報告を提出してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)テスト_ケースを使用します。

<a name="MT0078" />

### <a name="mt0077-incremental-builds-are-enabled-with-a-deployment-target--80-currently--this-is-not-supported-the-resulting-application-will-not-launch-on-ios-9-so-the-deployment-target-will-be-set-to-80"></a>MT0077:配置ターゲット < 8.0 でインクリメンタル ビルドが有効になっている (現在 *)。 これはサポートされていません (結果として得られるアプリケーションは起動せずに iOS 9)、ため、配置ターゲットを 8.0 に設定されます。

これは、インクリメンタルに作業を正しくビルドされるように、デプロイ ターゲットがこのビルドの 8.0 へ設定されていることを通知する警告です。

インクリメンタル ビルドには、(結果として得られるアプリケーションは、それ以外の場合 iOS 9 では起動しない) ため、配置ターゲットが 8.0 以上でのみサポートされます。

<a name="MT0079" />

### <a name="mt0078-the-recommended-xcode-version-for-xamarinios--is-xcode--or-later-the-current-xcode-version-found-in--is-"></a>MT0078:Xamarin.iOS の推奨される Xcode バージョン * は Xcode * またはそれ以降。 現在のバージョンの Xcode (で見つかった *) は * です。

これは、Xcode の現在のバージョンは、Xamarin.iOS のこのバージョンの Xcode の推奨されるバージョンではないことを通知する警告です。

最適な動作を確保する Xcode をアップグレードしてください。

<a name="MT0080" />

### <a name="mt0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MT0080:NewRefCount を無効にすると、- 新規-refcount:false が非推奨とされます。

これは、警告を通知する、新しいを無効にする要求 refcount (--新しい - refcount:false) は無視されました。

新しい refcount 機能が、すべてのプロジェクトは必須と、もはや無効にすることにないためです。

<a name="MT0081" />

### <a name="mt0081-the-command-line-argument---download-crash-report-also-requires---download-crash-report-to"></a>MT0081:コマンドライン引数--ダウンロード-クラッシュ レポートも必要です - ダウンロード-クラッシュ-レポート。

<a name="MT0082" />

### <a name="mt0082-repl---enable-repl-is-only-supported-when-linking-is-not-used---nolink"></a>MT0082:REPL (--有効にする repl) はリンクが使用されない場合にのみ (--nolink)。

<a name="MT0083" />

### <a name="mt0083-asm-only-bitcode-is-not-supported-on-watchos-use-either---bitcodemarker-or---bitcodefull"></a>MT0083:Asm 専用 bitcode は watchOS ではサポートされていません。 いずれかの--ビットコードを使用して: マーカーまたは--bitcode: 完全な。

<a name="MT0084" />

### <a name="mt0084-bitcode-is-not-supported-in-the-simulator-do-not-pass---bitcode-when-building-for-the-simulator"></a>MT0084:Bitcode はシミュレーターでサポートされていません。 シミュレーターのビルド時に、--ビットコードを渡さないでください。

<a name="MT0085" />

### <a name="mt0085-no-reference-to--was-found-it-will-be-added-automatically"></a>MT0085:参照が ' *' が見つかりました。 自動的に追加されます。

<a name="MT0086" />

### <a name="mt0086-a-target-framework---target-framework-must-be-specified-when-building-for-tvos-or-watchos"></a>MT0086:ターゲット フレームワーク (--ターゲット フレームワーク) TVOS、WatchOS ビルドするときに指定する必要があります。

これは通常; Xamarin.iOS のバグを示しますバグ報告を提出してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)テスト_ケースを使用します。

<a name="MT0087" />

### <a name="mt0087-incremental-builds---fastdev-is-not-supported-with-the-boehm-gc-incremental-builds-will-be-disabled"></a>MT0087:インクリメンタル ビルド (--fastdev) Boehm GC ではサポートされません。 インクリメンタル ビルドが無効になります。

<a name="MT0088" />

### <a name="mt0088-the-gc-must-be-in-cooperative-mode-for-watchos-apps-please-remove-the---coopfalse-argument-to-mtouch"></a>MT0088:GC は、watchOS アプリの共同のモードである必要があります。 削除してください-coop: false の mtouch 引数。

<a name="MT0089" />

### <a name="mt0089-the-option--cannot-take-the-value--when-cooperative-mode-is-enabled-for-the-gc"></a>MT0089:オプション '\*'値を取得できません'\*' GC の協調モードが有効な場合。

<a name="MT0091" />

### <a name="mt0091-this-version-of-xamarinios-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-set-the-managed-linker-behaviour-to-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MT0091:Xamarin.iOS のこのバージョンで、* SDK (Xcode に同梱されて *)。 いずれか、必要なヘッダー ファイルを取得またはリンク フレームワーク Sdk のみ (新しい Api を回避しようとしてください) にする管理対象のリンカーの動作を設定する Xcode をアップグレードします。

Xamarin.iOS では、アプリケーションの開発に、エラー メッセージで指定された SDK バージョンから、ヘッダー ファイルが必要です。 このエラーを解決するには、必要な SDK を取得する Xcode のアップグレードをお勧めしますが、これは、すべての必須のヘッダー ファイルが含まれます、です。 インストールされている場合、Xcode のバージョンが複数ある場合または既定以外の場所で、Xcode を使用する場合は、場合は、IDE の基本設定で正しい Xcode の場所を設定することを確認してください。

潜在的な代替ソリューションをマネージ リンカーを有効にするには。 これにより、使用されていない API を含む、ほとんどの場合、ヘッダー ファイルが不足している (または未完了) を新しい API が削除されます。 ただしこれは機能しません、プロジェクトは、Xcode 1 よりも新しい SDK で導入された API を使用している場合を提供します。

限界ソリューションについては、以前のバージョンの Xamarin.iOS を使用すること、SDK、プロジェクトをサポートするいると、次の必要があります。

<!-- MT0092 used by mlaunch -->

<a name="MT0093" />

### <a name="mt0093-could-not-find-mlaunch"></a>MT0093:'Mlaunch' が見つかりませんでした。

<!-- MT0094 is not reported anymore -->

<a name="MT0095" />

### <a name="mt0095-aot-files-could-not-be-copied-to-the-destination-directory-dest-error"></a>MT0095:Aot ファイルを {dest} インストール先ディレクトリにコピーできませんでした: {error}

<a name="MT0096" />

### <a name="mt0096-no-reference-to-xamariniosdll-was-found"></a>MT0096:Xamarin.iOS.dll への参照が見つかりませんでした。

<!-- MT0097: used by mmp -->
<!-- MT0098: used by mmp -->

<a name="MT0099" />

### <a name="mt0099-internal-error--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MT0099:内部エラー *。 テスト_ケースとバグの報告を提出してください (http://bugzilla.xamarin.com)します。

Xamarin.iOS で内部整合性チェックが失敗した場合、このエラー メッセージが報告されます。

これは Xamarin.iOS; のバグを示しますバグ報告を提出してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)テスト_ケースを使用します。

<a name="MT0100" />

### <a name="mt0100-invalid-assembly-build-target--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MT0100:無効なアセンブリのビルド ターゲット: ' *'。 テスト_ケースとバグの報告を提出してください (http://bugzilla.xamarin.com)します。

Xamarin.iOS で内部整合性チェックが失敗した場合、このエラー メッセージが報告されます。

これは、常に Xamarin.iOS; のバグバグ報告を提出してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)テスト_ケースを使用します。

<a name="MT0101" />

### <a name="mt0101-the-assembly--is-specified-multiple-times-in---assembly-build-target-arguments"></a>MT0101:アセンブリ ' *' が複数回を--アセンブリ ビルド ターゲットの引数で指定します。

エラー メッセージに記載されているアセンブリには、--アセンブリ ビルド ターゲットの引数を複数回が指定されています。 各アセンブリは 1 回しか説明を確認してください。

<a name="MT0102" />

### <a name="mt0102-the-assemblies--and--have-the-same-target-name--but-different-targets--and-"></a>MT0102:アセンブリの\*'と'\*'同じターゲットの名前を付ける ('\*')、異なるターゲット ('\*' と ' *')。

エラー メッセージに記載されているアセンブリでは、競合しているビルド ターゲットがあります。

例えば:

    --assembly-build-target:Assembly1.dll=framework=MyBinary --assembly-build-target:Assembly2.dll=dynamiclibrary=MyBinary

この例が同じであるとダイナミック ライブラリとフレームワークの両方を作成しようとしています (`MyBinary`)。

<a name="MT0103" />

### <a name="mt0103-the-static-object--contains-more-than-one-assembly--but-each-static-object-must-correspond-with-exactly-one-assembly"></a>MT0103:静的オブジェクト '\*' は、複数のアセンブリが含まれています ('\*')、各静的オブジェクトは、正確に 1 つのアセンブリに対応する必要がありますが、します。

エラー メッセージに記載されているアセンブリは、すべて 1 つの静的オブジェクトにコンパイルされます。 これは許可されていません、別の静的オブジェクトにすべてのアセンブリをコンパイルする必要があります。

例:

    --assembly-build-target:Assembly1.dll=staticobject=MyBinary --assembly-build-target:Assembly2.dll=staticobject=MyBinary

この例は、静的オブジェクトを構築しようとしています (`MyBinary`) の 2 つのアセンブリで構成されています (`Assembly1.dll`と`Assembly2.dll`)、これは許可されません。

<a name="MT0105" />

### <a name="mt0105-no-assembly-build-target-was-specified-for-"></a>MT0105:アセンブリのビルド ターゲットが指定されていません ' *'。

使用してターゲットがビルド アセンブリを指定`--assembly-build-target`アプリですべてのアセンブリのビルド ターゲットが割り当てられている必要があります。

エラー メッセージに記載されているアセンブリにビルド ターゲットが割り当てられているアセンブリがあるない場合にこのエラーが報告されます。

のドキュメントを参照して`--assembly-build-target`についてさらにします。

<a name="MT0106" />

### <a name="mt0106-the-assembly-build-target-name--is-invalid-the-character--is-not-allowed"></a>MT0106:アセンブリのビルド ターゲット名 '\*' が無効です。 文字 '\*' は許可されていません。

アセンブリのビルド ターゲットの名前は、有効なファイル名である必要があります。

これらの値がこのエラーをトリガーする例。

    --assembly-build-target:Assembly1.dll=staticobject=my/path.o

`my/path.o`ディレクトリの区切り記号により有効なファイル名ではありません。

<a name="MT0107" />

### <a name="mt0107-the-assemblies--have-different-custom-llvm-optimizations--which-is-not-allowed-when-they-are-all-compiled-to-a-single-binary"></a>MT0107:アセンブリの\*' 別のカスタム LLVM 最適化がある (\*) はすべて、コンパイル時に 1 つのバイナリには使用できません。

<a name="MT0108" />

### <a name="mt0108-the-assembly-build-target--did-not-match-any-assemblies"></a>MT0108:アセンブリのビルドのターゲットは ' *' すべてのアセンブリが一致しませんでした。

<a name="MT0109" />

### <a name="mt0109-the-assembly-0-was-loaded-from-a-different-path-than-the-provided-path-provided-path-1-actual-path-2"></a>MT0109:アセンブリ '{0}' が指定されたパスは異なるパスから読み込まれた (パスを指定: {1}、実際のパス: {2})。

これは、アプリケーションによって参照されるアセンブリが要求したより別の場所から読み込まれたことを示す警告です。

アプリが、同じ名前が、(最初のアセンブリのみが使用されます)、予期しない結果が生じるさまざまな場所から複数のアセンブリを参照している可能性があります。

<a name="MT0110" />

### <a name="mt0110-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-third-party-binding-libraries-and-that-compiles-to-bitcode"></a>MT0110:このバージョンの Xamarin.iOS がサード パーティのバインドのライブラリが含まれるビットコードをコンパイルするプロジェクトのインクリメンタル ビルドをサポートしていないために、インクリメンタル ビルドを無効にされています。

このバージョンの Xamarin.iOS がサード パーティのバインドのライブラリが含まれるビットコード (tvOS と watchOS プロジェクト) にコンパイルされるプロジェクトのインクリメンタル ビルドをサポートしていないために、インクリメンタル ビルドを無効にされています。

ユーザー側で操作は必要ありません、このメッセージは、純粋な情報。

詳細については、次を参照してください。 バグの #[51710](https://bugzilla.xamarin.com/show_bug.cgi?id=51710)します。

今後この警告は報告されません。

<a name="MT0111" />

### <a name="mt0111-bitcode-has-been-enabled-because-this-version-of-xamarinios-does-not-support-building-watchos-projects-using-llvm-without-enabling-bitcode"></a>MT0111:このバージョンの Xamarin.iOS でビルド watchOS がサポートされていないために、ビットコードを有効になっているビットコードを有効にせず、LLVM を使用するプロジェクトします。

Bitcode を有効に自動的にこのバージョンの Xamarin.iOS でビルド watchOS がサポートされていないためビットコードを有効にせず、LLVM を使用するプロジェクトします。

ユーザー側で操作は必要ありません、このメッセージは、純粋な情報。

詳細については、次を参照してください。 バグの #[51634](https://bugzilla.xamarin.com/show_bug.cgi?id=51634)します。

<a name="MT0112" />

### <a name="mt0112-native-code-sharing-has-been-disabled-because-"></a>MT0112:ネイティブ コードの共有が無効になっているため、*

コードの共有を無効にすることが複数の理由があります。

* コンテナー アプリのデプロイ ターゲットは iOS 8.0 より前であるため (が *))。

ネイティブ コードの共有ために必要な iOS 8.0 ネイティブ コードの共有は、ユーザーのフレームワークを使用して iOS 8.0 に導入されました。

* コンテナー アプリには、I18N アセンブリ (*) が含まれています。

ネイティブ コードの共有は現在サポートされていません、コンテナー アプリに I18N アセンブリが含まれている場合。

* コンテナー アプリのマネージ リンカー (*) のカスタムの xml の定義であるためです。

ネイティブ コードの共有には、カスタムの xml 定義を管理対象のリンカーを使用するプロジェクトはサポートされていませんが必要です。

<a name="MT0113" />

### <a name="mt0113-native-code-sharing-has-been-disabled-for-the-extension--because-"></a>MT0113:ネイティブ コードの共有が無効になっている拡張機能 ' *' ため *。

* bitcode オプションは、コンテナー アプリの間で異なるため、(\*) と拡張機能 (\*)。

  ネイティブ コードを共有するには、コードを共有するプロジェクト間でビットコード オプションが一致する必要があります。

* -アセンブリ-ビルド-ターゲットのオプションは、コンテナー アプリの間で異なる (\*) と拡張機能 (\*)。

  ネイティブ コードの共有が必要です-アセンブリ-ビルド-ターゲットのオプションは同じコードを共有するプロジェクトです。

  この条件は、いずれかのない有効または無効にすべてのプロジェクトでのインクリメンタル ビルドの場合に発生します。

* I18N アセンブリは、コンテナー アプリの間で異なるため (\*) と拡張機能 (\*)。

  ネイティブ コードの共有は現在サポートされていません I18N アセンブリを含む拡張機能。

* AOT コンパイラ引数は、コンテナー アプリの間で異なるため、(\*) と拡張機能 (\*)。

  ネイティブ コードを共有するには、AOT コンパイラに引数が、コードを共有するプロジェクト間で違いはないことが必要です。

* AOT コンパイラに他の引数が、コンテナー アプリの間で異なるためです (\*) と拡張機能 (\*)。

  ネイティブ コードを共有するには、AOT コンパイラに引数が、コードを共有するプロジェクト間で違いはないことが必要です。

  この条件は、プロジェクト間で、'すべて 32 ビット浮動小数点演算として実行 64 ビットの float' が異なる場合に発生します。

* LLVM が有効か、コンテナー アプリの両方で無効になっていないため、(\*) と拡張機能 (\*)。

  ネイティブ コードを共有するには、ある LLVM は有効または無効コードを共有するすべてのプロジェクトが必要です。

* 管理対象のリンカーの設定がコンテナー アプリの間で異なるためです (\*) と拡張機能 (\*)。

  ネイティブ コードを共有するには、マネージ リンカー設定がコードを共有するすべてのプロジェクトと同じである必要があります。

* 管理対象のリンカーのスキップされたアセンブリは、コンテナー アプリの間で異なるため (\*) と拡張機能 (\*)。

  ネイティブ コードを共有するには、マネージ リンカー設定がコードを共有するすべてのプロジェクトと同じである必要があります。

* 拡張機能がマネージ リンカー (*) のカスタムの xml の定義がします。

  ネイティブ コードの共有には、カスタムの xml 定義を管理対象のリンカーを使用するプロジェクトはサポートされていませんが必要です。

* ABI のコンテナー アプリを構築していないため、* (この ABI の拡張機能の構築) 時にします。

  ネイティブ コードを共有するには、任意のアプリの拡張機能のビルドをすべてのアーキテクチャ用のコンテナー アプリを構築することが必要です。

  インスタンス: この条件は、ARM64 + ARMv7 の拡張機能をビルド ARM64 用だけコンテナー アプリがビルドされる場合に発生します。

* ABI のコンテナー アプリを構築ため\*、これは、拡張機能の ABI と互換性がありません (\*)。

  ネイティブ コードを共有するには、まったく同一の API のすべてのプロジェクトを構築することが必要です。

  インスタンス: この条件は、拡張機能のビルドの ARMv7 + llvm thumb2、armv 7 + llvm のだけコンテナー アプリをビルドする場合に発生します。

* コンテナー アプリが、アセンブリを参照しているため、'\*'from'\*' 拡張機能から別のバージョンを参照するときに、' *'。

  ネイティブ コードを共有するには、同じバージョンのコードを共有するすべてのプロジェクトはすべてのアセンブリを使用する必要があります。

<!-- MT0114: used by mmp -->

<a name="MT0115" />

### <a name="mt0115-it-is-recommended-to-reference-dynamic-symbols-using-code---dynamic-symbol-modecode-when-bitcode-is-enabled"></a>MT0115:コードを使用して動的なシンボルを参照することを推奨 (--動的-記号モード = コード) ビットコードを有効にするとします。

Xamarin.iOS プロジェクトは多くの場合、ネイティブ シンボル参照を動的にネイティブ リンカーでこれらのシンボルを使用することが表示されないため、ネイティブ リンカーの動作がネイティブのリンク プロセス中にこのようなネイティブのシンボルを削除可能性があることを意味します。

通常は Xamarin.iOS がこのようなシンボルを保持するネイティブ リンカーの動作を求められます (を使用して、`-u symbol`リンカー フラグ) とネイティブ リンカー ビットコードのコンパイルを受け入れませんが、`-u`フラグ。

Xamarin.iOS が別のソリューションを実装します。 これらのシンボルを参照する余分なネイティブ コードを生成とネイティブ リンカーの動作を確認これらのシンボルが使用されているためです。 これは、ビットコードにコンパイルするときに自動的に実行されます。

場合`--dynamic-symbol-mode=linker`mtouch、ソリューションが無効にして、Xamarin.iOS を渡そうとすると、この代替に渡される`-u`ネイティブ リンカーの動作をします。 これがほとんどの場合、ネイティブ リンカー エラーの発生します。

解決策は、削除、`--dynamic-symbol-mode=linker`追加 mtouch 引数に、プロジェクトのビルド オプションから引数。

<!-- 0116 - 0124: free to use -->

<a name="MT0116" />

### <a name="mt0116-invalid-architecture-arch-32-bit-architectures-are-not-supported-when-deployment-target-is-11-or-later-make-sure-the-project-does-not-build-for-a-32-bit-architecture"></a>MT0116:無効なアーキテクチャ: {arch}。 配置ターゲットは、11 以降と 32 ビット アーキテクチャがサポートされていません。 プロジェクトが 32 ビット アーキテクチャ向けにビルドできないことを確認します。

iOS 11 では、展開対象が iOS 11 以降には、32 ビット アプリケーションを構築することはできませんので、32 ビット アプリケーションのサポートは含まれません。

Arm64 に、プロジェクトの iOS ビルド オプションで、ターゲット アーキテクチャを変更するか、以前の iOS バージョンにプロジェクトの Info.plist で配置ターゲットを変更します。

<a name="MT0117" />

### <a name="mt0117-cant-launch-a-32-bit-app-on-a-simulator-that-only-supports-64-bit"></a>MT0117:64 ビットのみをサポートするシミュレーター上の 32 ビット アプリケーションを起動することはできません。

<a name="MT0118" />

### <a name="mt0118-aot-files-could-not-be-found-at-the-expected-directory-msymdir"></a>MT0118:Aot ファイルが予期されるディレクトリ '{msymdir}' に見つかりませんでした。

<!-- 0119 - 0123: free to use -->

<a name="MT0123" />

### <a name="mt0123-the-executable-assembly--does-not-reference-"></a>MT0123:実行可能アセンブリ * を参照しない *。

プラットフォーム アセンブリへの参照が見つかりませんでした (Xamarin.iOS.dll/Xamarin.TVOS.dll/Xamarin.WatchOS.dll) で、実行可能アセンブリ。

これは通常発生プラットフォーム アセンブリからのものを使用する実行可能プロジェクトでコードがないです。たとえば、空のメイン メソッド (およびその他のコードはありません) は、このエラーを表示は。

```csharp
class Program {
    void Main (string[] args)
    {
    }
}
```

プラットフォーム アセンブリからの API を使用すると、エラーが解決されます。

```csharp
class Program {
    void Main (string[] args)
    {
        System.Console.WriteLine (typeof (UIKit.UIWindow));
    }
}
```

<a name="MT0124" />

### <a name="mt0124-could-not-set-the-current-language-to-lang-according-to-langlang-exception"></a>MT0124:'{Lang}' を現在の言語を設定できませんでした (LANG に従って = {LANG}): {例外}

これは、警告、エラー メッセージの言語に現在の言語を設定できなかったことを示すです。

現在の言語は、システムの言語に設定されます。

<a name="MT0125" />

### <a name="mt0125-the---assembly-build-target-command-line-argument-is-ignored-in-the-simulator"></a>MT0125:-アセンブリのビルドのターゲットは、シミュレーターでコマンドライン引数は無視されます。

操作は必要ありません、このメッセージは、純粋な情報。

<a name="MT0126" />

### <a name="mt0126-incremental-builds-have-been-disabled-because-incremental-builds-are-not-supported-in-the-simulator"></a>MT0126:インクリメンタル ビルドは、シミュレーターではサポートされていないために、インクリメンタル ビルドを無効にされています。

操作は必要ありません、このメッセージは、純粋な情報。

<a name="MT0127" />

### <a name="mt0127-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-more-than-one-third-party-binding-libraries"></a>MT0127:このバージョンの Xamarin.iOS が特定のサードパーティ製の複数のバインド ライブラリを含むプロジェクトのインクリメンタル ビルドをサポートしていないために、インクリメンタル ビルドを無効にされています。

Xamarin.iOS のこのバージョンは常にプロジェクトをビルド複数のサード パーティのバインド ライブラリを正しくために、自動的にインクリメンタル ビルドを無効にされています。

操作は必要ありません、このメッセージは、純粋な情報。

詳細については、次を参照してください。 バグの #[52727](https://bugzilla.xamarin.com/show_bug.cgi?id=52727)します。

<a name="MT0128" />

### <a name="mt0128-could-not-touch-the-file--"></a>MT0128:ファイルをタッチされませんでした ' *': *

(これは部分的なビルドが正しく行われることを確認するには、実行されます) ファイルに触れたときにエラーが発生しました。

この警告は無視できます可能性があります。問題があった場合に、バグをファイル (https://bugzilla.xamarin.com] (https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS))および調査されます。

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx:プロジェクトの関連するエラー メッセージ

### <a name="mt10xx-installer--mtouch"></a>MT10xx:インストーラー/mtouch

<!--
 MT1xxx file copy / symlinks (project related)
  MT10xx installer.cs / mtouch.cs
  -->

<a name="MT1001" />

### <a name="mt1001-could-not-find-an-application-at-the-specified-directory"></a>MT1001:指定されたディレクトリにアプリケーションを見つけることができませんでした。

<a name="MT1002" />

### <a name="mt1002-could-not-create-symlinks-files-were-copied"></a>MT1002:シンボリック リンクを作成できませんでした、コピーされたファイル

<a name="MT1003" />

### <a name="mt1003-could-not-kill-the-application--you-may-have-to-kill-the-application-manually"></a>MT1003:アプリケーションを強制終了できませんでした ' *'。 アプリケーションを手動で強制終了する必要があります。

<a name="MT1004" />

### <a name="mt1004-could-not-get-the-list-of-installed-applications"></a>MT1004:インストールされているアプリケーションの一覧を取得できませんでした。

<a name="MT1005" />

### <a name="mt1005-could-not-kill-the-application--on-the-device----you-may-have-to-kill-the-application-manually"></a>MT1005:アプリケーションを強制終了できませんでした '\*'on 'デバイス\*': *-アプリケーションを手動で強制終了する必要があります。

<a name="MT1006" />

### <a name="mt1006-could-not-install-the-application--on-the-device--"></a>MT1006:アプリケーションをインストールできませんでした '\*'on 'デバイス\*': *。

<a name="MT1007" />

### <a name="mt1007-failed-to-launch-the-application--on-the-device---you-can-still-launch-the-application-manually-by-tapping-on-it"></a>MT1007:アプリケーションの起動に失敗しました '\*'on 'デバイス\*': *。 タップして手動で引き続きアプリケーションを起動できます。

<a name="MT1008" />

### <a name="mt1008-failed-to-launch-the-simulator"></a>MT1008:シミュレーターを起動できませんでした。

Mtouch にシミュレーターの起動に失敗した場合、このエラーが報告されます。   これにも古い場合または配信不能シミュレーター プロセスの実行が既に存在します。

シミュレーターがスタックしているプロセスを強制終了には、Unix のコマンドラインで発行された、次のコマンドを使用できます。

```bash
$ launchctl list|grep UIKitApplication|awk '{print $3}'|xargs launchctl remove
```

<a name="MT1009" />

### <a name="mt1009-could-not-copy-the-assembly--to--"></a>MT1009:アセンブリをコピーできませんでした '\*'to'\*': *

これは、特定のバージョンの Xamarin.iOS の既知の問題です。

このような場合は、次の回避策を試してください。

```bash
sudo chmod 0644 /Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/*/*.mdb
```

ただし、Xamarin.iOS の最新バージョンでこの問題が解決されているためくださいファイルで新しいバグ[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)完全なバージョン情報とビルドのログ出力を使用します。

<a name="MT1010" />

### <a name="mt1010-could-not-load-the-assembly--"></a>MT1010:アセンブリを読み込むことができませんでした ' *': *

<a name="MT1011" />

### <a name="mt1011-could-not-add-missing-resource-file-"></a>MT1011:不足しているリソース ファイルを追加できませんでした: ' *'

<a name="MT1012" />

### <a name="mt1012-failed-to-list-the-apps-on-the-device--"></a>MT1012:デバイスでアプリを一覧表示できませんでした ' *': *。

<a name="MT1013" />

### <a name="mt1013-dependency-tracking-error-no-files-to-compare-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT1013:依存関係のエラーを追跡します。 比較するファイルがありません。 バグ報告を送信してください http://bugzilla.xamarin.com とテスト_ケースをします。

これは、Xamarin.iOS のバグを示します。 バグを提出してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)テスト caes とします。

<a name="MT1014" />

### <a name="mt1014-failed-to-re-use-cached-version-of--"></a>MT1014:キャッシュされたバージョンを再利用できませんでした。 ' *': *。

<a name="MT1015" />

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015:実行可能ファイルを作成できませんでした ' *': *

<a name="MT1015" />

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015:実行可能ファイルを作成できませんでした ' *': *

<a name="MT1016" />

### <a name="mt1016-failed-to-create-the-notice-file-because-a-directory-already-exists-with-the-same-name"></a>MT1016:同じ名前のディレクトリが既に存在するために通知ファイルを作成できませんでした。

ディレクトリを削除`NOTICE`プロジェクトから。

<a name="MT1017" />

### <a name="mt1017-failed-to-create-the-notice-file-"></a>MT1017:NOTICE ファイルの作成に失敗しました: *。

<a name="MT1018" />

### <a name="mt1018-your-application-failed-code-signing-checks-and-could-not-be-installed-on-the-device--check-your-certificates-provisioning-profiles-and-bundle-ids-probably-your-device-is-not-part-of-the-selected-provisioning-profile-error-0xe8008015"></a>MT1018:アプリケーションのコード署名の確認に失敗しましたし、デバイスにインストールできませんでした ' *'。 プロビジョニング プロファイル、証明書を確認し、バンドル id。 おそらく、デバイスが含まれていない、選択したプロビジョニング プロファイル (エラー。0xe8008015)。

<a name="MT1019" />

### <a name="mt1019-your-application-has-entitlements-not-supported-by-your-current-provisioning-profile-and-could-not-be-installed-on-the-device--please-check-the-ios-device-log-for-more-detailed-information-error-0xe8008016"></a>MT1019:アプリケーションが、現在のプロビジョニング プロファイルでサポートされていない権利と、デバイスにインストールできませんでした ' *'。 詳細については、iOS デバイスのログを確認してください (エラー。0xe8008016)。

これは、場合に発生します。

* アプリケーションでは、現在のプロビジョニング プロファイルがサポートされていない権利を持ちます。
  考えられる解決策:
  - 別の権利をサポートするプロビジョニング プロファイルを指定するアプリケーションのニーズ。
  - 現在のプロビジョニング プロファイルではサポートされていない権利を削除します。
* プロビジョニング プロファイルを使用するには含まれませんを配置しようとしているデバイス。
  考えられる解決策:
  - Xcode でテンプレートから新しいアプリを作成に同じプロビジョニング プロファイルを選択し、同じデバイスに展開します。 場合によって Xcode が (それ以外の場合は、Xcode は直接対処方法) で新しいデバイスでプロビジョニング プロファイルを自動的に更新することができます。
  -IOS デベロッパー センターに移動します。 新しいデバイスが、プロビジョニング プロファイルを更新し、コンピューターに更新されたプロビジョニング プロファイルをダウンロードします。

エラーの詳細については、iOS デバイスのログに出力されるほとんどの場合、問題の診断に役立ちます。

<a name="MT1020" />

### <a name="mt1020-failed-to-launch-the-application--on-the-device--"></a>MT1020:アプリケーションの起動に失敗しました '\*'on 'デバイス\*': *

<a name="MT1021" />

### <a name="mt1021-could-not-copy-the-file--to--2"></a>MT1021:ファイルをコピーできませんでした '\*'to'\*'。 {2}

ファイルをコピーできませんでした。 コピー操作からのエラー メッセージが、エラーの詳細について。

<a name="MT1022" />

### <a name="mt1022-could-not-copy-the-directory--to--2"></a>MT1022:ディレクトリをコピーできませんでした '\*'to'\*'。 {2}

ディレクトリをコピーできませんでした。 コピー操作からのエラー メッセージが、エラーの詳細について。

<a name="MT1023" />

### <a name="mt1023-could-not-communicate-with-the-device-to-find-the-application---"></a>MT1023:アプリケーションを検索するデバイスと通信できませんでした ' *': *

デバイス上のアプリケーションを検索しようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。

<a name="MT1024" />

### <a name="mt1024-the-application-signature-could-not-be-verified-on-device--please-make-sure-that-the-provisioning-profile-is-installed-and-not-expired-error-0xe8008017"></a>MT1024:デバイスでアプリケーションの署名を検証できませんでした ' *'。 プロビジョニング プロファイルがインストールされ、期限が切れていないことを確認してください (エラー。0xe8008017)。

デバイスでは、署名を検証しないために、アプリケーションのインストールが拒否されました。

プロビジョニング プロファイルがインストールされ、期限が切れていないことを確認してください。

<a name="MT1025" />

### <a name="mt1025-could-not-list-the-crash-reports-on-the-device-"></a>MT1025:デバイスでクラッシュ レポートの一覧を表示できませんでした *。

デバイスでクラッシュ レポートの一覧を表示しようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes でデバイスを同期します。

<a name="MT1026" />

### <a name="mt1026-could-not-download-the-crash-report--from-the-device-"></a>MT1026:クラッシュ レポートをダウンロードできませんでした *、デバイスから *。

デバイスからクラッシュ レポートをダウンロードしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes でデバイスを同期します。

<a name="MT1027" />

### <a name="mt1027-cant-use-xcode-7-to-launch-applications-on-devices-with-ios--xcode-7-only-supports-ios-6"></a>MT1027:Xcode 7 以降を使用して iOS デバイスでアプリケーションを起動することはできません * (Xcode 7 は、iOS 6 以降をのみサポート)。

Xcode 7 以降を使用して、iOS バージョン 6.0 の下のデバイス上のアプリケーションを起動することはできません。

Xcode の以前のバージョンを使用するか、それを起動するには、手動でアプリをタップしてください。

<a name="MT1028" />

### <a name="mt1028-invalid-device-specification--expected-ios-watchos-or-all"></a>MT1028:無効なデバイスの仕様: ' *'。 予想される 'ios'、'watchos' または 'all' です。

-を使用して、デバイスの仕様が渡されるデバイスが無効です。 有効な値: 'ios'、'watchos' または 'all' です。

<a name="MT1029" />

### <a name="mt1029-could-not-find-an-application-at-the-specified-directory-"></a>MT1029:指定されたディレクトリにアプリケーションを見つけることができませんでした *。

--Launchdev に渡されたアプリケーション パスが存在しません。 有効なアプリ バンドルを指定してください。

<a name="MT1030" />

### <a name="mt1030-launching-applications-on-device-using-a-bundle-identifier-is-deprecated-please-pass-the-full-path-to-the-bundle-to-launch"></a>MT1030:バンドル識別子を使用してデバイス上のアプリケーションの起動が非推奨とされます。 起動するバンドルには、完全なパスを渡してください。

バンドル id だけではなくデバイスに起動するアプリにパスを渡すことをお勧めします。

<a name="MT1031" />

### <a name="mt1031-could-not-launch-the-app--on-the-device--because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1031:アプリを起動できませんでした '\*'on 'デバイス\*'、デバイスがロックされているためです。 デバイスのロックを解除してから、やり直してください。

デバイスのロックを解除してから、やり直してください。

<a name="MT1032" />

### <a name="mt1032-this-application-executable-might-be-too-large--mb-to-execute-on-device-if-bitcode-was-enabled-you-might-want-to-disable-it-for-development-it-is-only-required-to-submit-applications-to-apple"></a>MT1032:このアプリケーションの実行可能ファイルが大きすぎる可能性があります (* MB) デバイス上で実行します。 ビットコードを有効にした場合は、開発を無効にしたい場合がありますは、アプリケーションを Apple に送信するのみ必要です。

<a name="MT1033" />

### <a name="mt1033-could-not-uninstall-the-application--from-the-device--"></a>MT1033:アプリケーションをアンインストールできませんでした '\*'' デバイスから\*': *

<!-- 1034 used by mmp -->

<a name="MT1035" />

### <a name="mt1035-cannot-include-different-versions-of-the-framework-name"></a>MT1035:異なるバージョンの framework '{name}' を含めることはできません。

同じフレームワークのさまざまなバージョンとリンクすることはできません。

これは通常、拡張機能は、(場合によって、サード パーティのバインドのアセンブリ) を使用してコンテナー アプリよりも、フレームワークのさまざまなバージョンを参照する場合に発生します。

次のこのエラーは複数存在する[MT1036](#MT1036)エラーそれぞれ別のフレームワークのパスを一覧表示します。

<a name="MT1036" />

### <a name="mt1036-framework-name-included-from-path-related-to-previous-error"></a>MT1036:Framework '{name}' に含まれる: {path} (以前のエラーに関連する)

このエラーが報告と共に[MT1036](#MT1036)します。 参照してください[MT1036](#MT1036)詳細についてはします。

### <a name="mt11xx-debug-service"></a>MT11xx:サービスをデバッグします。

<!--
  MT11xx DebugService.cs
  -->

<a name="MT1101" />

### <a name="mt1101-could-not-start-app"></a>MT1101:アプリを開始できませんでした。

<a name="MT1102" />

### <a name="mt1102-could-not-attach-to-the-app-to-kill-it-"></a>MT1102:(強制終了) をアプリにアタッチできませんでした *。

<a name="MT1103" />

### <a name="mt1103-could-not-detach"></a>MT1103:デタッチできませんでした。

<a name="MT1104" />

### <a name="mt1104-failed-to-send-packet-"></a>MT1104:パケットの送信に失敗しました: *

<a name="MT1105" />

### <a name="mt1105-unexpected-response-type"></a>MT1105:予期しない応答の種類

<a name="MT1106" />

### <a name="mt1106-could-not-get-list-of-applications-on-the-device-request-timed-out"></a>MT1106:デバイスのアプリケーションの一覧を取得できませんでした。要求がタイムアウトしました。

<a name="MT1107" />

### <a name="mt1107-application-failed-to-launch-"></a>MT1107:アプリケーションを起動できませんでした *。

デバイスがロックされていることを確認してください。

信頼開発者の場合は、エンタープライズ アプリケーションを配置するか、無料のプロビジョニング プロファイルを使用する必要があります (詳細については <a href="https://stackoverflow.com/a/30726375/183422">ここ</a> )。

<a name="MT1108" />

### <a name="mt1108-could-not-find-developer-tools-for-this-xx-yy-device"></a>MT1108:この XX (YY) デバイスの開発者ツールが見つかりませんでした。

Mtouch からいくつかの操作が必要な<tt>DeveloperDiskImage.dmg</tt>存在するファイル。   このファイルは Xcode の一部で、通常 ナェィェ ・のに対して、ビルドを使用している SDK は、 <tt>Xcode.app/Contents/Developer/iPhoneOS.platform/DeviceSupport/VERSION/DeveloperDiskImage.dmg</tt>します。

このエラーは、接続しているデバイスに一致する DeveloperDiskImage.dmg があるないため、いずれか発生します。

<a name="MT1109" />

### <a name="mt1109-application-failed-to-launch-because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1109:アプリケーションは、デバイスがロックされているため、起動に失敗しました。 デバイスのロックを解除してから、やり直してください。

デバイスがロックされていることを確認してください。

<a name="MT1110" />

### <a name="mt1110-application-failed-to-launch-because-of-ios-security-restrictions-please-ensure-the-developer-is-trusted"></a>MT1110:アプリケーションは、iOS のセキュリティ制限があるため起動できませんでした。 開発者は、信頼されていることを確認してください。

信頼開発者の場合は、エンタープライズ アプリケーションを配置するか、無料のプロビジョニング プロファイルを使用する必要があります (詳細については <a href="https://stackoverflow.com/a/30726375/183422">ここ</a> )。

<a name="MT1111" />

### <a name="mt1111-application-launched-successfully-but-its-not-possible-to-wait-for-the-app-to-exit-as-requested-because-its-not-possible-to-detect-app-termination-when-launching-using-gdbserver"></a>MT1111:正常に起動するアプリケーションのアプリ gdbserver を使用してを起動するときに、アプリの終了を検出することはできませんので、要求を終了するを待機することです。

### <a name="mt12xx-simulator"></a>MT12xx:シミュレーター

<!--
  MT12xx simcontroller.cs
  -->

<a name="MT1201" />

### <a name="mt1201-could-not-load-the-simulator-"></a>MT1201:シミュレーターを読み込むことができません: *

<a name="MT1202" />

### <a name="mt1202-invalid-simulator-configuration-"></a>MT1202:無効なシミュレーターの構成: *

<a name="MT1203" />

### <a name="mt1203-invalid-simulator-specification-"></a>MT1203:無効なシミュレーターの仕様: *

<a name="MT1204" />

### <a name="mt1204-invalid-simulator-specification--runtime-not-specified"></a>MT1204:無効なシミュレーターの仕様 ' *': ランタイムが指定されていません。

<a name="MT1205" />

### <a name="mt1205-invalid-simulator-specification--device-type-not-specified"></a>MT1205:無効なシミュレーターの仕様 ' *': デバイスの種類が指定されていません。

<a name="MT1206" />

### <a name="mt1206-could-not-find-the-simulator-runtime-"></a>MT1206:シミュレーターのランタイムが見つかりませんでした。 ' *'。

<a name="MT1207" />

### <a name="mt1207-could-not-find-the-simulator-device-type-"></a>MT1207:シミュレーターのデバイスの種類が見つかりませんでした。 ' *'。

<a name="MT1208" />

### <a name="mt1208-could-not-find-the-simulator-runtime-"></a>MT1208:シミュレーターのランタイムが見つかりませんでした。 ' *'。

<a name="MT1209" />

### <a name="mt1209-could-not-find-the-simulator-device-type-"></a>MT1209:シミュレーターのデバイスの種類が見つかりませんでした。 ' *'。

<a name="MT1210" />

### <a name="mt1210-invalid-simulator-specification--unknown-key-"></a>MT1210:無効なシミュレーターの仕様: \*、不明なキー '\*'

<a name="MT1211" />

### <a name="mt1211-the-simulator-version--does-not-support-the-simulator-type-"></a>MT1211:シミュレーターのバージョン '\*'は、シミュレーターの種類をサポートしていません'\*'

<a name="MT1212" />

### <a name="mt1212-failed-to-create-a-simulator-version-where-type---and-runtime--"></a>MT1212:シミュレーターのバージョンを作成できませんでした。 場所を入力 = * とランタイム = *。

<a name="MT1213" />

### <a name="mt1213-invalid-simulator-specification-for-xcode-4-"></a>MT1213:Xcode 4 の無効なシミュレーターの仕様: *

<a name="MT1214" />

### <a name="mt1214-invalid-simulator-specification-for-xcode-5-"></a>MT1214:Xcode 5 の仕様をシミュレーターが無効です: *

<a name="MT1215" />

### <a name="mt1215-invalid-sdk-specified-"></a>MT1215:指定された SDK が無効です: *

<a name="MT1216" />

### <a name="mt1216-could-not-find-the-simulator-udid-"></a>MT1216:シミュレーターの UDID が見つかりませんでした。 ' *'。

<a name="MT1217" />

### <a name="mt1217-could-not-load-the-app-bundle-at-"></a>MT1217:アプリ バンドルをロードできませんでした ' *'。

<a name="MT1218" />

### <a name="mt1218-no-bundle-identifier-found-in-the-app-at-"></a>MT1218:時にアプリでバンドル識別子が見つかりません ' *'。

<a name="MT1219" />

### <a name="mt1219-could-not-find-the-simulator-for-"></a>MT1219:用のシミュレーターが見つかりませんでした。 ' *'。

<a name="MT1220" />

### <a name="mt1220-could-not-find-the-latest-simulator-runtime-for-device-"></a>MT1220:デバイス シミュレーターの最新のランタイムが見つかりませんでした ' *'。

通常、これは、Xcode と問題を示します。

この問題を解決しようとするもの:

* Xcode で 1 回、シミュレーターを使用します。
* -Sdk を使用して、明示的な SDK バージョンを渡す<version>します。
* Xcode を再インストールします。

<a name="MT1221" />

### <a name="mt1221-could-not-find-the-paired-iphone-simulator-for-the-watchos-simulator-"></a>MT1221:WatchOS シミュレーターのペアになっている iPhone シミュレーターが見つかりませんでした ' *'。

WatchOS シミュレーターで WatchOS アプリを起動するときに、ペアになっている iOS シミュレーターにも必要があります。

シミュレーターと iOS シミュレーターの Xcode のデバイスの UI を使用してペアリングを見る (メニュー ウィンドウでは、デバイスを ->)。

### <a name="mt13xx-linkwith"></a>MT13xx: [ソースコードファ]

<!--
  MT13xx [LinkWith]
  -->

<a name="MT1301" />

### <a name="mt1301-native-library---was-ignored-since-it-does-not-match-the-current-build-architectures-"></a>MT1301:ネイティブ ライブラリ`*`(\*) 現在のビルド ミラーサイトと一致しませんので無視されました (\*)

<a name="MT1302" />

### <a name="mt1302-could-not-extract-the-native-library--from--please-ensure-the-native-library-was-properly-embedded-in-the-managed-assembly-if-the-assembly-was-built-using-a-binding-project-the-native-library-must-be-included-in-the-project-and-its-build-action-must-be-objcbindingnativelibrary"></a>MT1302:ネイティブ ライブラリを抽出できませんでした ' *' から '+'。 (バインド プロジェクトを使用してアセンブリがビルドされたプロジェクトで、ネイティブ ライブラリを含める必要がある、ビルド アクションは 'ObjcBindingNativeLibrary' である必要があります) 場合に、ネイティブ ライブラリがマネージ アセンブリに埋め込まれた適切かを確認してください。

<a name="MT1303" />

### <a name="mt1303-could-not-decompress-the-native-framework--from--please-review-the-build-log-for-more-information-from-the-native-zip-command"></a>MT1303:ネイティブ フレームワークの圧縮を解除できなかった '\*'from'\*'。 詳細については、ネイティブ 'zip' コマンドからのビルド ログを確認してください。

バインディング ライブラリから指定されたネイティブ フレームワークを解凍できませんでした。

このエラー ネイティブ 'zip' コマンドからの詳細については、bulid ログを確認してください。

<a name="MT1304" />

### <a name="mt1304-the-embedded-framework--in--is-invalid-it-does-not-contain-an-infoplist"></a>MT1304:埋め込みフレームワーク ' *' で * が無効です: Info.plist が含まれていません。

埋め込まれた指定のフレームワークは、Info.plist が含まれていないと、有効なフレームワークがそのため。

フレームワークが有効であることを確認してください。

<a name="MT1305" />

### <a name="mt1305-the-binding-library--contains-a-user-framework--but-embedded-user-frameworks-require-ios-80-the-current-deployment-target-is--please-set-the-deployment-target-in-the-infoplist-file-to-at-least-80"></a>MT1305:バインディング ライブラリ '\*' ユーザー フレームワークが含まれています (\*)、埋め込みユーザー フレームワークには、iOS 8.0 が必要がありますが、(現在の配置ターゲットは *)。 8.0 以上を Info.plist ファイルの配置ターゲットを設定してください。

指定したバインディング ライブラリには、埋め込みのフレームワークが含まれていますが、Xamarin.iOS では、iOS 8.0 以降で埋め込みフレームワークのみがサポートしています。

くださいこのエラーを解決するには少なくとも 8.0 Info.plist ファイルで、配置ターゲットを設定 (または埋め込みフレームワークを使用しないでください)。

### <a name="mt14xx-crash-reports"></a>MT14xx:クラッシュ レポート

<!--
  MT14xx    CrashReports.cs
  -->

<a name="MT1400" />

### <a name="mt1400-could-not-open-crash-report-service-afcconnectionopen-returned-"></a>MT1400:クラッシュ レポート サービスを開くことができませんでした。返される AFCConnectionOpen *

デバイスからクラッシュ レポートにアクセスしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes でデバイスを同期します。

<a name="MT1401" />

### <a name="mt1401-could-not-close-crash-report-service-afcconnectionclose-returned-"></a>MT1401:クラッシュ レポート サービスを閉じることができません。返される AFCConnectionClose *

デバイスからクラッシュ レポートにアクセスしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes でデバイスを同期します。

<a name="MT1402" />

### <a name="mt1402-could-not-read-file-info-for--afcfileinfoopen-returned-"></a>MT1402:ファイル情報を読み込めませんでした。 *。返される AFCFileInfoOpen *

デバイスからクラッシュ レポートにアクセスしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes でデバイスを同期します。

<a name="MT1403" />

### <a name="mt1403-could-not-read-crash-report-afcdirectoryopen--returned-"></a>MT1403:クラッシュ レポートを読み取れませんでした。AFCDirectoryOpen (*) が返されます *。

デバイスからクラッシュ レポートにアクセスしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes でデバイスを同期します。

<a name="MT1404" />

### <a name="mt1404-could-not-read-crash-report-afcfilerefopen--returned-"></a>MT1404:クラッシュ レポートを読み取れませんでした。AFCFileRefOpen (*) が返されます *。

デバイスからクラッシュ レポートにアクセスしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes でデバイスを同期します。

<a name="MT1405" />

### <a name="mt1405-could-not-read-crash-report-afcfilerefread--returned-"></a>MT1405:クラッシュ レポートを読み取れませんでした。AFCFileRefRead (*) が返されます *。

デバイスからクラッシュ レポートにアクセスしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes でデバイスを同期します。

<a name="MT1406" />

### <a name="mt1406-could-not-list-crash-reports-afcdirectoryopen--returned-"></a>MT1406:クラッシュ レポートの一覧を表示できませんでした。AFCDirectoryOpen (*) が返されます *。

デバイスからクラッシュ レポートにアクセスしようとするときにエラーが発生しました。

この問題を解決しようとするもの:

* デバイスからアプリケーションを削除してからやり直してください。
* デバイスを切断して再接続します。
* デバイスを再起動します。
* Mac を再起動します。
* (これは、クラッシュ レポート デバイスから削除) iTunes でデバイスを同期します。

<!--- 1407 used by mmp -->

### <a name="mt16xx-macho"></a>MT16xx:MachO

<!--
  MT16xx    MachO.cs
  -->

<a name="MT1600" />

### <a name="mt1600-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1600:MACH-O ダイナミック ライブラリではない (不明なヘッダー ' 0 x *')。 *。

ダイナミック ライブラリの問題の処理中にエラーが発生しました。

ダイナミック ライブラリが有効な MACH-O ダイナミック ライブラリであることを確認してください。

使用して、ライブラリの形式を検証できる、`file`ターミナルからコマンド。

    file -arch all -l /path/to/library.dylib

<a name="MT1601" />

### <a name="mt1601-not-a-static-library-unknown-header--"></a>MT1601:スタティック ライブラリではありません (不明なヘッダー ' *')。 *。

問題のスタティック ライブラリの処理中にエラーが発生しました。

スタティック ライブラリが有効な MACH-O スタティック ライブラリであることを確認してください。

使用して、ライブラリの形式を検証できる、`file`ターミナルからコマンド。

    file -arch all -l /path/to/library.a

<a name="MT1602" />

### <a name="mt1602-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1602:MACH-O ダイナミック ライブラリではない (不明なヘッダー ' 0 x *')。 *。

ダイナミック ライブラリの問題の処理中にエラーが発生しました。

ダイナミック ライブラリが有効な MACH-O ダイナミック ライブラリであることを確認してください。

使用して、ライブラリの形式を検証できる、`file`ターミナルからコマンド。

    file -arch all -l /path/to/library.dylib

<a name="MT1603" />

### <a name="mt1603-unknown-format-for-fat-entry-at-position--in-"></a>MT1603:不明な位置にあるエントリを fat 形式 * の *。

Fat アーカイブ対象の処理中にエラーが発生しました。

Fat アーカイブが有効であることを確認してください。

Fat アーカイブの形式を使用して検証できる、`file`ターミナルからコマンド。

    file -arch all -l /path/to/file

<a name="MT1604" />

### <a name="mt1604-file-of-type--is-not-a-macho-file-"></a>MT1604:ファイルの種類 * MachO ファイル (*) ではありません。

該当する MachO ファイルの処理中にエラーが発生しました。

ファイルが有効な MACH-O ダイナミック ライブラリであることを確認してください。

使用してファイルの形式を検証することができます、`file`ターミナルからコマンド。

    file -arch all -l /path/to/file

## <a name="mt2xxx-linker-error-messages"></a>MT2xxx:リンカーのエラー メッセージ

<!--
 MT2xxx Linker
  MT20xx Linker (general) errors
  -->

<a name="MT2001" />

### <a name="mt2001-could-not-link-assemblies"></a>MT2001:アセンブリをリンクできませんでした。

このエラー マネージ リンカーに、例外など、予期しないエラーが発生したことを意味し、でしたを完了するか処理されているアセンブリを保存します。 正確なエラーの詳細については、ビルド ログの一部を例になります

```
error MT2001: Could not link assemblies.
    Method: `System.Void Todo.TodoListPageCS/<<-ctor>b__1_0>d::SetStateMachine(System.Runtime.CompilerServices.IAsyncStateMachine)`
    Assembly: `QuickTodo, Version=1.0.6297.28241, Culture=neutral, PublicKeyToken=null`
Reason: Value cannot be null.
Parameter name: instruction
```

このような問題のバグを報告する重要です。 多くの場合は適切な修正プログラムが公開されるまで回避策を提供できます。 上記の情報は、問題を解決する (テスト ケースやアセンブリ binairy) も重要です。

<a name="MT2002" />

### <a name="mt2002-can-not-resolve-reference-"></a>MT2002:参照を解決できません: *

<a name="MT2003" />

### <a name="mt2003-option--will-be-ignored-since-linking-is-disabled"></a>MT2003:オプション ' *' はリンクが無効になっているために無視されます

<a name="MT2004" />

### <a name="mt2004-extra-linker-definitions-file--could-not-be-located"></a>MT2004:追加のリンカーの定義ファイル ' *' が見つかりませんでした。

<a name="MT2005" />

### <a name="mt2005-definitions-from--could-not-be-parsed"></a>MT2005:定義 ' *' を解析できませんでした。

<a name="MT2006" />

### <a name="mt2006-can-not-load-mscorlibdll-from--please-reinstall-xamarinios"></a>MT2006:Mscorlib.dll を読み込むことはできません: *。 Xamarin.iOS をインストールし直してください。

これは、通常、Xamarin.iOS のインストールに問題があることを示します。 Xamarin.iOS を再インストールしてください。

<!--- 2007 used by mmp -->
<!--- 2009 used by mmp -->

<a name="MT2010" />

### <a name="mt2010-unknown-httpmessagehandler--valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MT2010:不明な HttpMessageHandler`*`します。 有効な値は HttpClientHandler (既定値)、CFNetworkHandler または NSUrlSessionHandler です。

<a name="MT2011" />

### <a name="mt2011-unknown-tlsprovider---valid-values-are-default-or-appletls"></a>MT2011:不明な TlsProvider`*`します。  有効な値は、既定または appletls です。

渡された値`tls-provider=`有効な TLS (Transport Layer Security) プロバイダーではありません。

`default`と`appletls`が唯一の有効な値は、どちらも表す同じのオプションは、ネイティブ Apple TLS API を使用して、SSL や TLS のサポートを提供することです。

<!--- 2012 used by mmp -->
<!--- 2013 used by mmp -->
<!--- 2014 used by mmp -->

<a name="MT2015" />

### <a name="mt2015-invalid-httpmessagehandler--for-watchos-the-only-valid-value-is-nsurlsessionhandler"></a>MT2015:無効な HttpMessageHandler `*` watchOS 向けです。 唯一の有効な値は、NSUrlSessionHandler です。

これは、プロジェクト ファイルが、無効な HttpMessageHandler を指定するために発生する警告です。

既定で生成されたプレビュー ツールの以前のバージョンは、プロジェクト ファイルで値が無効です。

この警告を修正するには、テキスト エディターでプロジェクト ファイルを開きし、XML から HttpMessageHandler のすべてのノードを削除します。

<a name="MT2016" />

### <a name="mt2016-invalid-tlsprovider-legacy-option-the-only-valid-value-appletls-will-be-used"></a>MT2016:無効な TlsProvider`legacy`オプション。 唯一の有効な値`appletls`使用されます。

`legacy`が完全に管理された SSLv3、プロバイダー/TLSv1 唯一のプロバイダーはもはや Xamarin.iOS と共に出荷されません。 この以前のプロバイダーを使用していたし、新しいで今すぐビルドするプロジェクト`appletls`いずれか。

この警告を修正するテキスト エディターでプロジェクト ファイルを開くし、すべて削除 ' MtouchTlsProvider」XML からノード。

<a name="MT2017" />

### <a name="mt2017-could-not-process-xml-description"></a>MT2017:XML の説明を処理できませんでした。

つまりにエラーがある、 [XML リンカー構成ファイルをカスタム](https://developer.xamarin.com/guides/cross-platform/advanced/custom_linking/)指定したファイルを確認してください。

<a name="MT2018" />

### <a name="mt2018-the-assembly--is-referenced-from-two-different-locations--and-"></a>MT2018:アセンブリ '\*' は 2 つの異なる場所から参照: '\*' および ' *'。

エラー メッセージに記載されているアセンブリは、複数の場所から読み込まれます。 常に同じバージョンのアセンブリを使用することを確認してください。

<a name="MT2019" />

### <a name="mt2019-can-not-load-the-root-assembly-"></a>MT2019:ルート アセンブリを読み込むことはできません ' *'

ルート アセンブリを読み込むことができませんでした。 エラー メッセージ内のパスが既存のファイルを指すことと、有効な .NET アセンブリであることを確認してください。

<a name="MT202x" />

### <a name="mt202x-binding-optimizer-failed-processing-"></a>MT202x:オプティマイザーのバインドには、処理が失敗しました`...`します。

予期しない問題は、バインド コード生成の最適化を試みるときに発生しました。 エラー メッセージで問題を引き起こしている要素と呼びます。 という名前のアセンブリ (または、型または名前付きメソッドを含む) は、この問題を解決する必要がありますを指定する、[バグ レポート](http://bugzilla.xamarin.com)と共に完全なビルド ログ詳細度を有効になっていると (つまり`-v -v -v -v`で、**追加 mtouch引数**)。

最後の桁`x`になります。
* `0` アセンブリの名前。
* `1` 型の名前。
* `3` メソッドの名前。

<a name="MT2030" />

### <a name="mt2030-remove-user-resources-failed-processing-"></a>MT2030:削除のユーザー リソースが失敗しました処理`...`します。

予期しない問題は、ユーザーのリソースを削除しようとするときが発生しました。 エラー メッセージで問題を引き起こしているアセンブリの名前が。 アセンブリでこの問題を修正する必要がありますを指定する、[バグ レポート](http://bugzilla.xamarin.com)と共に完全なビルド ログ詳細度を有効になっていると (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

ユーザー リソースは、アプリケーション バンドルを作成する、ビルド時に、抽出する必要がある (リソース) としてアセンブリに含まれるファイルです。 バインディングには、以下の項目が含まれます。

* `__monotouch_content_*` `__monotouch_pages_*` ; のリソースと
* ネイティブ ライブラリのバインドをアセンブリ内に埋め込まれます

<a name="MT2040" />

### <a name="mt2040-default-httpmessagehandler-setter-failed-processing-"></a>MT2040:既定 HttpMessageHandler setter 失敗処理`...`します。

既定値を設定しようとするときに予期しない問題が発生しました`HttpMessageHandler`アプリケーション。 提出してください、[バグ レポート](http://bugzilla.xamarin.com)と共に完全なビルド ログ詳細度を有効になっていると (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2050" />

### <a name="mt2050-code-remover-failed-processing-"></a>MT2050:コードを削除する操作子には、処理が失敗しました。`...`します。

予期しない問題は、配布アプリケーションと BCL からコードを削除しようとするときが発生しました。 提出してください、[バグ レポート](http://bugzilla.xamarin.com)と共に完全なビルド ログ詳細度を有効になっていると (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2060" />

### <a name="mt2060-sealer-failed-processing-"></a>MT2060:シーラーが処理に失敗した`...`します。

予期しない問題は、シール型またはメソッドが (最後) しようとするとき、またはいくつかのメソッドを devirtualizing が発生しました。 エラー メッセージで問題を引き起こしているアセンブリの名前が。 アセンブリでこの問題を修正する必要がありますを指定する、[バグ レポート](http://bugzilla.xamarin.com)と共に完全なビルド ログ詳細度を有効になっていると (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2070" />

### <a name="mt2070-metadata-reducer-failed-processing-"></a>MT2070:メタデータのレジューサーが処理に失敗した`...`します。

予期しない問題は、アプリケーションからメタデータを削減しようとするときが発生しました。 エラー メッセージで問題を引き起こしているアセンブリの名前が。 アセンブリでこの問題を修正する必要がありますを指定する、[バグ レポート](http://bugzilla.xamarin.com)と共に完全なビルド ログ詳細度を有効になっていると (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2080" />

### <a name="mt2080-marknsobjects-failed-processing-"></a>MT2080:処理が失敗しました MarkNSObjects`...`します。

マークしようとするときに予期しない問題が発生しました`NSObject`アプリケーションからサブクラスです。 エラー メッセージで問題を引き起こしているアセンブリの名前が。 アセンブリでこの問題を修正する必要がありますを指定する、[バグ レポート](http://bugzilla.xamarin.com)と共に完全なビルド ログ詳細度を有効になっていると (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2090" />

### <a name="mt2090-inliner-failed-processing-"></a>MT2090:インライナの処理に失敗しました`...`します。

予期しない問題には、インライン コードをアプリケーションからときが発生しました。 エラー メッセージで問題を引き起こしているアセンブリの名前が。 アセンブリでこの問題を解決するためにを指定する必要があります、[バグ レポート](https://bugzilla.xamarin.com)と共に完全なビルド ログ詳細度を有効になっていると (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<!-- MT21xx: more linker errors -->

<!--- 2100 used by mmp -->

<a name="MT2100" />

### <a name="mt2100-smart-enum-conversion-preserver-failed-processing-"></a>MT2100:スマート列挙型の変換の保持には、処理が失敗しました。`...`します。

予期しない問題には、アプリケーションからスマート列挙型の変換メソッドをマークするときが発生しました。 エラー メッセージで問題を引き起こしているアセンブリの名前が。 アセンブリでこの問題を解決するためにを指定する必要があります、[バグ レポート](https://bugzilla.xamarin.com)と共に完全なビルド ログ詳細度を有効になっていると (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2101" />

### <a name="mt2101-cant-resolve-the-reference--referenced-from-the-method--in-"></a>MT2101:参照を解決できない '\*'、メソッドから参照される'\*' で ' *'。

エラー メッセージに記載されているメソッドを処理するときに、無効なアセンブリ参照が発生しました。

エラー メッセージで問題を引き起こしているアセンブリの名前が。 アセンブリでこの問題を修正する必要がありますを指定する、[バグ レポート](https://bugzilla.xamarin.com)と共に完全なビルド ログ詳細度を有効になっていると (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2102" />

### <a name="mt2102-error-processing-the-method--in-the-assembly--"></a>MT2102:メソッドの処理中にエラー '\*'assembly' で\*': *

予期しない問題には、エラー メッセージに記載されているメソッドをマークするときが発生しました。

エラー メッセージで問題を引き起こしているアセンブリの名前が。 アセンブリでこの問題を修正する必要がありますを指定する、[バグ レポート](https://bugzilla.xamarin.com)と共に完全なビルド ログ詳細度を有効になっていると (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2103" />

### <a name="mt2103-error-processing-assembly--"></a>MT2103:アセンブリの処理中にエラー '\*': *

アセンブリを処理するときに、予期しないエラーが発生しました。

エラー メッセージで問題を引き起こしているアセンブリの名前が。 アセンブリでこの問題を解決するためにを指定する必要があります、[バグ レポート](https://bugzilla.xamarin.com)と共に完全なビルド ログ詳細度を有効になっていると (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MT2104" />

### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104:アセンブリをリンクできません '{0}' 混合モードです。

リンカーによっては、混合モードのアセンブリを処理できません。

参照してください https://msdn.microsoft.com/library/x0w2664k.aspx 混合モード アセンブリの詳細についてはします。

## <a name="mt3xxx-aot-error-messages"></a>MT3xxx:AOT のエラー メッセージ

<!--
 MT3xxx AOT
  MT30xx AOT (general) errors
  -->

<a name="MT3001" />

### <a name="mt3001-could-not-aot-the-assembly-"></a>MT3001:AOT アセンブリではない可能性があります ' *'

これにより、AOT コンパイラのバグが通常を示します。 バグを提出してください[ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)エラーを再現するために使用できるプロジェクトです。

インクリメンタル ビルドでプロジェクトの iOS ビルド オプションを無効にしてこの問題を回避することができる場合があります (ですが、バグ、これも報告してください)。

<a name="MT3002" />

### <a name="mt3002-aot-restriction-method--must-be-static-since-it-is-decorated-with-monopinvokecallback-see-httpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbackshttpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbacks"></a>MT3002:AOT の制限:メソッド ' *' [MonoPInvokeCallback] で装飾されているために、静的なをする必要があります。 参照してください。 [https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks](https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks)

このエラー メッセージ、AOT コンパイラに由来します。

<a name="MT3003" />

### <a name="mt3003-conflicting---debug-and---llvm-options-soft-debugging-is-disabled"></a>MT3003:競合しています--デバッグと llvm - オプション。 論理的なデバッグが無効です。

LLVM を有効にすると、デバッグはサポートされていません。 アプリをデバッグする必要がある場合は、まず LLVM を無効にします。

<a name="MT3004" />

### <a name="mt3004-could-not-aot-the-assembly--because-it-doesnt-exist"></a>MT3004:AOT アセンブリではない可能性があります ' *' が存在しないためです。

<a name="MT3005" />

### <a name="mt3005-the-dependency--of-the-assembly--was-not-found-please-review-the-projects-references"></a>MT3005:依存関係 '\*'assembly' の\*' が見つかりませんでした。 プロジェクトの参照を確認してください。

これは通常、アセンブリが別のバージョンのプラットフォーム アセンブリ (通常は mscorlib.dll の .NET 4 バージョン) を参照する場合に発生します。

これはサポートされていませんし、可能性がありますいないビルドまたは正常に実行 (アセンブリと同じ .NET 4 のバージョン、Xamarin.iOS のバージョンがない mscorlib.dll の API に使用できます)。

<a name="MT3006" />

### <a name="mt3006-could-not-compute-a-complete-dependency-map-for-the-project-this-will-result-in-slower-build-times-because-xamarinios-cant-properly-detect-what-needs-to-be-rebuilt-and-what-does-not-need-to-be-rebuilt-please-review-previous-warnings-for-more-details"></a>MT3006:プロジェクトの完全な依存関係マップを計算できませんでした。 これが適切に、Xamarin.iOS にどのような再構築する必要があります (および、どのような再構築する必要はありません) を検出できないため低速のビルド時間に発生します。 詳細についてはそれまでの警告を確認してください。

 ビルドまたは正常に実行 (アセンブリと同じ .NET 4 のバージョン、Xamarin.iOS のバージョンがない mscorlib.dll の API に使用できます)。

<a name="MT3007" />

### <a name="mt3007-debug-info-files-mdb-will-not-be-loaded-when-llvm-is-enabled"></a>MT3007:Llvm を有効にすると、デバッグ情報ファイル (*.mdb) は読み込まれません。

<a name="MT3008" />

### <a name="mt3008-bitcode-support-requires-the-use-of-the-llvm-aot-backend---llvm"></a>MT3008:ビットコード サポート LLVM AOT バックエンドの使用が必要です (--llvm)

ビットコード サポート LLVM AOT バックエンドの使用が必要です (--llvm)。

ビットコード サポートを無効にするか、LLVM を有効にします。

<!--- 3009 used by mmp -->
<!--- 3010 used by mmp -->

## <a name="mt4xxx-code-generation-error-messages"></a>MT4xxx:コードの生成エラー メッセージ

### <a name="mt40xx-main"></a>MT40xx:メイン

<!--
 MT4xxx code generation
  MT40xx main.m
  -->

<a name="MT4001" />

### <a name="mt4001-the-main-template-could-not-be-expanded-to-"></a>MT4001:メイン テンプレートを展開できませんでした`*`します。

Main.m を生成するときにエラーが発生しました。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4002" />

### <a name="mt4002-failed-to-compile-the-generated-code-for-pinvoke-methods-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4002:P/invoke メソッドに対して生成されたコードのコンパイルに失敗しました。 バグ報告を提出してください。 http://bugzilla.xamarin.com

P/invoke メソッドに対して生成されたコードのコンパイルに失敗しました。 バグ報告を送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

### <a name="mt41xx-registrar"></a>MT41xx:レジストラー

<!--
  MT41xx registrar.m
  -->

<a name="MT4101" />

### <a name="mt4101-the-registrar-cannot-build-a-signature-for-type-"></a>MT4101:レジストラーは、署名の種類をビルドできません`*`します。

ランタイムは、OBJECTIVE-C との間でマーシャ リングする方法を認識しないエクスポートされた API で、種類が見つかりました

Xamarin.iOS は、問題の型をサポートする必要がありますと思われる場合での拡張機能要求を提出してください[ http://bugzilla.xamarin.com](http://bugzilla.xamarin.com)します。

<a name="MT4102" />

### <a name="mt4102-the-registrar-found-an-invalid-type--in-signature-for-method--use--instead"></a>MT4102:レジストラーが、無効な型が見つかりました`*`メソッドのシグネチャで`*`します。 代わりに、`*` を使用してください。

現在のみ 1 つの型で、次が発生します。System.DateTime します。 Objective C と同じ (NSDate) を使用してください。

<a name="MT4103" />

### <a name="mt4103-the-registrar-found-an-invalid-type--in-signature-for-method--the-type-implements-inativeobject-but-does-not-have-a-constructor-that-takes-two-intptr-bool-arguments"></a>MT4103:レジストラーが、無効な型が見つかりました`*`メソッドのシグネチャで`*`:型が INativeObject を実装されていますが、2 つ受け取るコンス トラクターがありません (IntPtr、bool) の引数

これが発生した、レジストラーが、上記の特性を持つシグネチャの型がどのように発生する場合。 レジストラーは、型の新しいインスタンスを作成する必要があります、(IntPtr、bool) を持つコンス トラクターを必要とここでは、呼び出し元がネイティブの所有権を渡す場合に、2 番目の署名の最初の引数 (IntPtr) 指定管理対象のハンドル(この値がオブジェクトで呼び出される '保持' が false の場合) を処理します。

<a name="MT4104" />

### <a name="mt4104-the-registrar-cannot-marshal-the-return-value-for-type--in-signature-for-method-"></a>MT4104:型の戻り値のマーシャ リングすることはできません、レジストラー`*`メソッドのシグネチャで`*`します。

ランタイムは、OBJECTIVE-C との間でマーシャ リングする方法を認識しないエクスポートされた API で、種類が見つかりました

Xamarin.iOS は、問題の型をサポートする必要がありますと思われる場合での拡張機能要求を提出してください[ http://bugzilla.xamarin.com](http://bugzilla.xamarin.com)します。

<a name="MT4105" />

### <a name="mt4105-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4105:型のパラメーターのマーシャ リングすることはできません、レジストラー`*`メソッドのシグネチャで`*`します。

Xamarin.iOS は、問題の型をサポートする必要がありますと思われる場合での拡張機能要求を提出してください[ http://bugzilla.xamarin.com](http://bugzilla.xamarin.com)します。

<a name="MT4106" />

### <a name="mt4106-the-registrar-cannot-marshal-the-return-value-for-structure--in-signature-for-method-"></a>MT4106:構造体の戻り値のマーシャ リングすることはできません、レジストラー`*`メソッドのシグネチャで`*`します。

ランタイムは、OBJECTIVE-C との間でマーシャ リングする方法を認識しないエクスポートされた API で、種類が見つかりました

Xamarin.iOS は、問題の型をサポートする必要がありますと思われる場合での拡張機能要求を提出してください[ http://bugzilla.xamarin.com](http://bugzilla.xamarin.com)します。

<a name="MT4107" />

### <a name="mt4107-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4107:型のパラメーターのマーシャ リングすることはできません、レジストラー`*`メソッドのシグネチャで`+`します。

ランタイムは、OBJECTIVE-C との間でマーシャ リングする方法を認識しないエクスポートされた API で、種類が見つかりました

Xamarin.iOS は、問題の型をサポートする必要がありますと思われる場合での拡張機能要求を提出してください[ http://bugzilla.xamarin.com](http://bugzilla.xamarin.com)します。

<a name="MT4108" />

### <a name="mt4108-the-registrar-cannot-get-the-objectivec-type-for-managed-type-"></a>MT4108:レジストラーは、マネージ型の ObjectiveC 種類を取得できません`*`します。

ランタイムは、OBJECTIVE-C との間でマーシャ リングする方法を認識しないエクスポートされた API で、種類が見つかりました

Xamarin.iOS は、問題の型をサポートする必要がありますと思われる場合での拡張機能要求を提出してください[ http://bugzilla.xamarin.com](http://bugzilla.xamarin.com)します。

<a name="MT4109" />

### <a name="mt4109-failed-to-compile-the-generated-registrar-code-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4109:レジストラーが生成されたコードのコンパイルに失敗しました。 バグ報告を提出してください。 http://bugzilla.xamarin.com

レジストラーの生成されたコードをコンパイルに失敗しました。 ビルド ログには、コードがコンパイルされていない理由を説明する、ネイティブ コンパイラからの出力が含まれます。

これは、常に Xamarin.iOS; のバグバグ報告を提出してください[ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com)プロジェクトまたはテスト_ケースを使用します。

<a name="MT4110" />

### <a name="mt4110-the-registrar-cannot-marshal-the-out-parameter-of-type--in-signature-for-method-"></a>MT4110:型の出力パラメーターのマーシャ リングすることはできません、レジストラー`*`メソッドのシグネチャで`*`します。

<a name="MT4111" />

### <a name="mt4111-the-registrar-cannot-build-a-signature-for-type--in-method-"></a>MT4111:レジストラーは、署名の種類をビルドできません`*`メソッドで`*`します。

<a name="MT4112" />

### <a name="mt4112-the-registrar-found-an-invalid-type--registering-generic-types-with-objective-c-is-not-supported-and-may-lead-to-random-behavior-andor-crashes-for-backwards-compatibility-with-older-versions-of-xamarinios-it-is-possible-to-ignore-this-error-by-passing---unsupported--enable-generics-in-registrar-as-an-additional-mtouch-argument-in-the-projects-ios-build-options-page-see-developerxamarincomguidesiosadvancedtopicsregistrarhttpsdeveloperxamarincomguidesiosadvancedtopicsregistrar-for-more-information"></a>MT4112:レジストラーが、無効な型が見つかりました`*`します。 Objective C のジェネリック型の登録はサポートされていませんし、ランダムな動作やクラッシュにつながる可能性があります (の旧バージョンと以前のバージョンの Xamarin.iOS との互換性をすることができますを渡すことによってこのエラーは無視`--unsupported--enable-generics-in-registrar`としてその他の mtouchプロジェクトの iOS ビルド オプション ページでの引数。 参照してください[developer.xamarin.com/guides/ios/advanced_topics/registrar](https://developer.xamarin.com/guides/ios/advanced_topics/registrar)詳細については)。

<a name="MT4113" />

### <a name="mt4113-the-registrar-found-a-generic-method--exporting-generic-methods-is-not-supported-and-will-lead-to-random-behavior-andor-crashes"></a>MT4113:レジストラーがジェネリック メソッドが見つかりません: '\*.\*'。 ジェネリック メソッドをエクスポートして、サポートされていませんは、ランダムな動作やクラッシュにつながります。

<a name="MT4114" />

### <a name="mt4114-unexpected-error-in-the-registrar-for-the-method----please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4114:メソッドのレジストラーで予期しないエラー '\*.\*'-でバグ報告を提出してください http://bugzilla.xamarin.com

<a name="MT4116" />

### <a name="mt4116-could-not-register-the-assembly--"></a>MT4116:アセンブリを登録できませんでした ' *': *

<a name="MT4117" />

### <a name="mt4117-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4117:レジストラーが、メソッドの署名の不一致を検出 '*.*'-メソッドには、セレクターの表示 * パラメーターは、マネージ メソッドがあるときに * パラメーター。

<a name="MT4118" />

### <a name="mt4118-cannot-register-two-managed-types--and--with-the-same-native-name-"></a>MT4118:2 つのマネージ型を登録することはできません ('\*'と'\*') と同じネイティブ名前 ('* ')。

<a name="MT4119" />

### <a name="mt4119-could-not-register-the-selector--of-the-member--because-the-selector-is-already-registered-on-a-different-member"></a>MT4119:セレクターを登録できませんでした '\*'member' の\*。 *' セレクターは既に別のメンバーに登録されているためです。

<a name="MT4120" />

### <a name="mt4120-the-registrar-found-an-unknown-field-type--in-field--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4120:レジストラーが、不明なフィールド タイプを見つけました '\*'field' in\*。 *'。 バグ報告を提出してください。 http://bugzilla.xamarin.com

このエラーは、Xamarin.iOS のバグを示します。 バグ報告を送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4121" />

### <a name="mt4121-cannot-use-gccg-to-compile-the-generated-code-from-the-static-registrar-when-using-the-accounts-framework-the-header-files-provided-by-apple-used-during-the-compilation-require-clang-either-use-clang---compilerclang-or-the-dynamic-registrar---registrardynamic"></a>MT4121:GCC を使用することはできません/g++ (コンパイル時に使用する Apple によって提供されるヘッダー ファイルを必要と Clang) アカウント フレームワークを使用するときに、静的なレジストラーから生成されたコードをコンパイルします。 Clang を使用するか (--: clang コンパイラ) または動的なレジストラー (--レジストラー: 動的)。

<a name="MT4122" />

### <a name="mt4122-cannot-use-the-clang-compiler-provided-in-the--sdk-to-compile-the-generated-code-from-the-static-registrar-when-non-ascii-type-names--are-present-in-the-application-either-use-gccg---compilergccg-the-dynamic-registrar---registrardynamic-or-a-newer-sdk"></a>MT4122:提供される、Clang コンパイラを使用することはできません、*します。* ASCII 以外の場合は、静的なレジストラーから生成されたコードをコンパイルする SDK の名前を入力 ('* ') が、アプリケーション内に存在します。 GCC を使用するか/g++ (--: gcc コンパイラ | g++)、動的なレジストラー (--レジストラー: 動的) または新しい SDK。

<a name="MT4123" />

### <a name="mt4123-the-type-of-the-variadic-parameter-in-the-variadic-function--must-be-systemintptr"></a>MT4123:可変個引数関数に可変個引数パラメーターの型 ' *' System.IntPtr 必要があります。

<a name="MT4124" />

### <a name="mt4124-invalid--found-on--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4124:無効な * で見つかった ' *'。 バグ報告を提出してください。 http://bugzilla.xamarin.com

このエラーは、Xamarin.iOS のバグを示します。 バグ報告を送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4125" />

### <a name="mt4125-the-registrar-found-an-invalid-type--in-signature-for-method--the-interface-must-have-a-protocol-attribute-specifying-its-wrapper-type"></a>MT4125:レジストラーが、無効な型が見つかりません '\*メソッドのシグネチャ' で'\*'。インターフェイスのラッパー型を指定するプロトコルの属性が必要です。

<a name="MT4126" />

### <a name="mt4126-cannot-register-two-managed-protocols--and--with-the-same-native-name-"></a>MT4126:管理対象の 2 つのプロトコルを登録することはできません ('\*'と'\*') と同じネイティブ名前 ('* ')。

<a name="MT4127" />

### <a name="mt4127-cannot-register-more-than-one-interface-method-for-the-method--which-is-implementing-"></a>MT4127:メソッドの 1 つ以上のインターフェイス メソッドを登録できません '\*' (これを実装する '\*')。

<a name="MT4128" />

### <a name="mt4128-the-registrar-found-an-invalid-generic-parameter-type--in-the-method--the-generic-parameter-must-have-an-nsobject-constraint"></a>MT4128:レジストラーが、ジェネリック パラメーターが無効な型が見つかりません '\*'method' で\*'。 ジェネリック パラメーターには、'NSObject' 制約が必要です。

<a name="MT4129" />

### <a name="mt4129-the-registrar-found-an-invalid-generic-return-type--in-the-method--the-generic-return-type-must-have-an-nsobject-constraint"></a>MT4129:レジストラーが無効なジェネリック戻り値の型が見つかりません '\*'method' で\*'。 ジェネリック戻り値の型には、'NSObject' 制約が必要です。

<a name="MT4130" />

### <a name="mt4130-the-registrar-cannot-export-static-methods-in-generic-classes-"></a>MT4130:レジストラーは、ジェネリック クラスの静的メソッドをエクスポートできません ('* ')。

<a name="MT4131" />

### <a name="mt4131-the-registrar-cannot-export-static-properties-in-generic-classes-"></a>MT4131:レジストラーは、ジェネリック クラスの静的プロパティをエクスポートできません ('\*.\*')。

<a name="MT4132" />

### <a name="mt4132-the-registrar-found-an-invalid-generic-return-type--in-the-property--the-return-type-must-have-an-nsobject-constraint"></a>MT4132:レジストラーが無効なジェネリック戻り値の型が見つかりません '\*'property' in\*'。 戻り値の型には、'NSObject' 制約が必要です。

<a name="MT4133" />

### <a name="mt4133-cannot-construct-an-instance-of-the-type--from-objective-c-because-the-type-is-generic-runtime-exception"></a>MT4133:型のインスタンスは作成できません ' *' OBJECTIVE-C から型がジェネリックであるためです。 [ランタイムの例外]

<a name="MT4134" />

### <a name="mt4134-your-application-is-using-the--framework-which-isnt-included-in-the-ios-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-ios--while-youre-building-with-the-ios--sdk-please-select-a-newer-sdk-in-your-apps-ios-build-options"></a>MT4134:アプリケーションを使用して、' *' フレームワークで、iOS アプリのビルドを使用している SDK に含まれていません (このフレームワークは、iOS で導入された * iOS で構築しているときに、* SDK)。アプリの iOS ビルド オプションでは、新しい SDK を選択してください。

<a name="MT4135" />

### <a name="mt4135-the-member--has-an-export-attribute-that-doesnt-specify-a-selector-a-selector-is-required"></a>MT4135:メンバー '\*.\*' セレクターを指定していないエクスポート属性があります。 セレクターが必要です。

<a name="MT4136" />

### <a name="mt4136-the-registrar-cannot-marshal-the-parameter-type--of-the-parameter--in-the-method-"></a>MT4136:レジストラーは、パラメーターの型をマーシャ リングできません '\*'parameter' の\*'method' の\**'。

<!-- MT4137 is unused -->

<a name="MT4138" />

### <a name="mt4138-the-registrar-cannot-marshal-the-property-type--of-the-property-"></a>MT4138:レジストラーは、プロパティの型をマーシャ リングできません '\*'property' の\*。 *'。

<a name="MT4139" />

### <a name="mt4139-the-registrar-cannot-marshal-the-property-type--of-the-property--properties-with-the-connect-attribute-must-have-a-property-type-of-nsobject-or-a-subclass-of-nsobject"></a>MT4139:レジストラーは、プロパティの型をマーシャ リングできません '\*'property' の\*。 *'。 NSObject のプロパティの型 (または NSObject のサブクラス)、[Connect] 属性を持つプロパティがあります。

<a name="MT4140" />

### <a name="mt4140-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-variadic-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4140:レジストラーが、メソッドの署名の不一致を検出 '*.*'-可変個引数メソッドには、セレクターの表示 * パラメーターは、マネージ メソッドがあるときに * パラメーター。

<a name="MT4141" />

### <a name="mt4141-cannot-register-the-selector--on-the-member--because-xamarinios-implicitly-registers-this-selector"></a>MT4141:セレクターを登録できません '\*'member' on\*' Xamarin.iOS は、このセレクターを暗黙的に登録するためです。

これは、framework の型をサブクラス化と実装、'' を保持しようとしています。 'release' ときに発生します。 または、'dealloc' メソッド。

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

ただし、クラスが、framework の最初のサブクラスでない場合は、これらのメソッドをオーバーライドするには、可能な限りの型をお勧めします。

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

ここで Xamarin.iOS を上書`retain`、`release`と`dealloc`で、`MyNSObject`クラス、競合はありません。

<a name="MT4142" />

### <a name="mt4142-failed-to-register-the-type-"></a>MT4142:型の登録に失敗しました ' *'。

<a name="MT4143" />

### <a name="mt4143-the-objectivec-class--could-not-be-registered-it-does-not-seem-to-derive-from-any-known-objectivec-class-including-nsobject"></a>MT4143:ObjectiveC クラス ' *' できなかった登録されると、それはしていない (NSObject を含む) 任意の既知の ObjectiveC クラスから派生します。

<a name="MT4144" />

### <a name="mt4144-cannot-register-the-method--since-it-does-not-have-an-associated-trampoline-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4144:メソッドを登録できません ' *'、関連付けられているトランポリンがないためです。 バグ報告を送信してください http://bugzilla.xamarin.com です。

これは、Xamarin.iOS のバグを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4145" />

### <a name="mt4145-invalid-enum--enums-with-the-native-attribute-must-have-a-underlying-enum-type-of-either-long-or-ulong"></a>MT4145:無効な列挙 ' *': [ネイティブ] 属性を持つ列挙型は 'long' または 'ulong' のいずれかの基になる列挙型である必要があります。

<a name="MT4146" />

### <a name="mt4146-the-name-parameter-of-the-registrar-attribute-on-the-class---contains-an-invalid-character--"></a>MT4146:クラスのレジストラー属性の Name パラメーター\*'('\*') に無効な文字が含まれています:'\*' (\*)。

Objectice C クラスの名前にすることで、空白文字を含めることはできません、`Register`対応するマネージ クラスの属性を持つことはできません、`Name`か、パラメーターに空白を含めることはできません。

確認してください、`Register`エラー メッセージに記載されているマネージ クラスの属性に空白が含まれていません。

<a name="MT4147" />

### <a name="mt4147-detected-a-protocol-inheriting-from-the-jsexport-protocol-while-using-the-dynamic-registrar-it-is-not-possible-to-export-protocols-to-javascriptcore-dynamically-the-static-registrar-must-be-used-add---registrarstatic-to-the-additional-mtouch-arguments-in-the-projects-ios-build-options-to-select-the-static-registrar"></a>MT4147:動的なレジストラーを使用しているときに、JSExport プロトコルから継承するプロトコルが検出されました。 JavaScriptCore にプロトコルを動的にエクスポートすることはできません。静的なレジストラーを使用する必要があります (追加 '--追加 mtouch 引数に、プロジェクトの iOS ビルド オプションを静的なレジストラーを選択するには、レジストラー: 静的)。

<a name="MT4148" />

### <a name="mt4148-the-registrar-found-a-generic-protocol--exporting-generic-protocols-is-not-supported"></a>MT4148:レジストラーが汎用プロトコルが見つかりました: ' *'。 一般的なプロトコルをエクスポートすることはサポートされていません。

<a name="MT4149" />

### <a name="mt4149-cannot-register-the-method--because-the-type-of-the-first-parameter--does-not-match-the-category-type-"></a>MT4149:メソッドを登録できません '\*.\*' ため、最初のパラメーターの型 ('\*') カテゴリの種類と一致しません ('\*')。

<a name="MT4150" />

### <a name="mt4150-cannot-register-the-type--because-the-type-property--in-its-category-attribute-does-not-inherit-from-nsobject"></a>MT4150:型を登録することはできません '\*' ため、Type プロパティ ('\*') のカテゴリでは、属性は NSObject から継承しません。

<a name="MT4151" />

### <a name="mt4151-cannot-register-the-type--because-the-type-property-in-its-category-attribute-isnt-set"></a>MT4151:型を登録することはできません ' *'、カテゴリの属性の Type プロパティが設定されていないためです。

<a name="MT4152" />

### <a name="mt4152-cannot-register-the-type--as-a-category-because-it-implements-inativeobject-or-subclasses-nsobject"></a>MT4152:型を登録することはできません ' *'、カテゴリとして INativeObject または NSObject のサブクラスを実装するためです。

<a name="MT4153" />

### <a name="mt4153-cannot-register-the-type--as-a-category-because-its-generic"></a>MT4153:型を登録できません '\*' カテゴリとしてジェネリックであるためです。

<a name="MT4154" />

### <a name="mt4154-cannot-register-the-method--as-a-category-method-because-its-generic"></a>MT4154:メソッドを登録できません '\*' カテゴリ メソッドとしてジェネリックであるためです。

<a name="MT4155" />

### <a name="mt4155-cannot-register-the-method--with-the-selector--as-a-category-method-on--because-the-objective-c-already-has-an-implementation-for-this-selector"></a>MT4155:メソッドを登録できません '\*'with 'セレクター\*' カテゴリのメソッドとして ' *' Objective C には既にこのセレクターに実装されているためです。

<a name="MT4156" />

### <a name="mt4156-cannot-register-two-categories--and--with-the-same-native-name-"></a>MT4156:2 つのカテゴリを登録することはできません ('\*'と'\*') と同じネイティブ名前 ('* ')。

<a name="MT4157" />

### <a name="mt4157-cannot-register-the-category-method--because-at-least-one-parameter-is-required-and-its-type-must-match-the-category-type-"></a>MT4157:カテゴリのメソッドを登録できません '\*' には少なくとも 1 つのパラメーターが必要なため (その型がカテゴリの種類と一致する必要があります '\*')

<a name="MT4158" />

### <a name="mt4158-cannot-register-the-constructor--in-the-category--because-constructors-in-categories-are-not-supported"></a>MT4158:コンス トラクターを登録することはできません * カテゴリ * カテゴリのコンス トラクターはサポートされていません。

<a name="MT4159" />

### <a name="mt4159-cannot-register-the-method--as-a-category-method-because-category-methods-must-be-static"></a>MT4159:メソッドを登録できません ' *' カテゴリ メソッドとしてカテゴリ メソッドは静的である必要があるためです。

<a name="MT4160" />

### <a name="mt4160-invalid-character---found-in-selector--for-"></a>MT4160:無効な文字 '\*' (\*) セレクターで見つかった '\*'for'\*'。

<a name="MT4161" />

### <a name="mt4161-the-registrar-found-an-unsupported-structure--all-fields-in-a-structure-must-also-be-structures-field--with-type-2-is-not-a-structure"></a>MT4161:レジストラーが、サポートされていない構造が見つかりません '\*'。構造体のすべてのフィールドは構造体にもあります (フィールド '\*'type' with{2}' 構造体ではありません)。

レジストラーでは、サポートされていないフィールドを持つ構造体が見つかりました。

Objective C に公開されている構造体のすべてのフィールドの構造 (クラスではなく) である必要があります。

<a name="MT4162" />

### <a name="mt4162-the-type--used-as--2-is-not-available-in---it-was-introduced-in---please-build-with-a-newer--sdk-usually-done-by-using-the-most-recent-version-of-xcode"></a>MT4162:型 '\*' (として使用される * {2}) では使用できません * * (で導入された * *)\*新しいをビルドしてください * SDK (通常は Xcode の最新バージョンを使用して行われます。

レジストラーでは、現在の SDK に含まれていない型が見つかりました。

Xcode をアップグレードしてください。

<a name="MT4163" />

### <a name="mt4163-internal-error-in-the-registrar--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4163:レジストラー (*) で内部エラーです。 バグ報告を提出してください。 http://bugzilla.xamarin.com

このエラーは、Xamarin.iOS のバグを示します。 バグ報告を送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4164" />

### <a name="mt4164-cannot-export-the-property--because-its-selector--is-an-objective-c-keyword-please-use-a-different-name"></a>MT4164:プロパティをエクスポートできません '\*' ため、セレクター '\*' Objective C キーワードです。 別の名前を使用してください。

対象のプロパティのセレクターは、有効な識別子で Objective C ではありません。

セレクターとして有効な Objective C 識別子を使用してください。

<a name="MT4165" />

### <a name="mt4165-the-registrar-couldnt-find-the-type-systemvoid-in-any-of-the-referenced-assemblies"></a>MT4165:レジストラーは、参照先アセンブリのいずれかで型 'system.void' に見つかりませんでした。

このエラーの考えでは、Xamarin.iOS のバグを示します。 バグ報告を送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4166" />

### <a name="mt4166-cannot-register-the-method--because-the-signature-contains-a-type--that-isnt-a-reference-type"></a>MT4166:メソッドを登録できません '\*' 署名には、型が含まれているため (\*) 参照型ではありません。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4167" />

### <a name="mt4167-cannot-register-the-method--because-the-signature-contains-a-generic-type--with-a-generic-argument-type-that-isnt-an-nsobject-subclass-"></a>MT4167:メソッドを登録できません '\*' 署名には、ジェネリック型が含まれているため (\*) NSObject のサブクラスです (*) はないジェネリック引数型とします。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT4168" />

### <a name="mt4168-cannot-register-the-type-managedname-because-its-objective-c-name-exportedname-is-an-objective-c-keyword-please-use-a-different-name"></a>MT4168:型を登録することはできません '{管理\_名前}' ため、OBJECTIVE-C で名前' {0} エクスポート\_名前}' Objective C キーワードです。 別の名前を使用してください。

問題の型の Objective C の名前は、有効な Objective C 識別子ではありません。

Objective C の有効な識別子を使用してください。

<a name="MT4169" />

### <a name="mt4169-failed-to-generate-a-pinvoke-wrapper-for-method-message"></a>MT4169:{0} メソッド} の P/invoke ラッパーを生成できませんでした: {message}

Xamarin.iOS は、上記の P/invoke ラッパー関数を生成できませんでした。
基になる原因の報告されたエラー メッセージを確認してください。

<a name="MT4170" />

### <a name="mt4170-the-registrar-cant-convert-from-managed-type-to-native-type-for-the-return-value-in-the-method-method"></a>MT4170:レジストラーは、{メソッド} メソッドの戻り値の '{ネイティブな型}' に '{マネージ型}' から変換できません。

エラーの説明を参照してください。 <a href="#MT4172">MT4172</a>します。

<a name="MT4171" />

### <a name="mt4171-the-bindas-attribute-on-the-member-member-is-invalid-the-bindas-type-type-is-different-from-the-property-type-type"></a>MT4171:メンバー {0} のメンバー} BindAs 属性は無効です: {type} BindAs 型がプロパティ型 {type} 異なります。

BindAs 属性の型にアタッチされているメンバーの種類に対応を確認してください。

<a name="MT4172" />

### <a name="mt4172-the-registrar-cant-convert-from-native-type-to-managed-type-for-the-parameter-parameter-name-in-the-method-method"></a>MT4172:レジストラーは、{メソッド} メソッドのパラメーター '{パラメーター名}' に '{マネージ型}' に '{ネイティブな型}' から変換できません。

レジストラーはこれらのコンテンツ タイプ間の変換をサポートしていません。

これは、対象の API は、Xamarin.iOS; によって提供される場合の Xamarin.iOS のバグバグを送信してください [http://bugzilla.xamarin.com][1]です。

ネイティブ ライブラリのバインド プロジェクトの開発中に、これを実行する場合は、型の新しい組み合わせのサポートを追加する open いたします。 拡張機能の要求を提出してください、ケースの場合は、([http://bugzilla.xamarin.com][2]) と、テスト ケースとを評価しますが。

[1]: https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS
[2]: https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS&component=General&bug_severity=enhancement

## <a name="mt5xxx-gcc-and-toolchain-error-messages"></a>MT5xxx:GCC と、ツール チェーンのエラー メッセージ

### <a name="mt51xx-compilation"></a>MT51xx:コンパイル

<!--
 MT5xxx GCC and toolchain
  MT51xx compilation
  -->

<a name="MT5101" />

### <a name="mt5101-missing--compiler-please-install-xcode-command-line-tools-component"></a>MT5101:不足している ' *' コンパイラ。 Xcode コマンド ライン ツールのコンポーネントをインストールしてください。

<a name="MT5102" />

### <a name="mt5102-failed-to-assemble-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5102:ファイルの作成に失敗しました ' *'。 バグ報告を提出してください。 http://bugzilla.xamarin.com

<a name="MT5103" />

### <a name="mt5103-failed-to-compile-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5103:ファイルのコンパイルに失敗しました ' *'。 バグ報告を提出してください。 http://bugzilla.xamarin.com

<a name="MT5104" />

### <a name="mt5104-could-not-find-neither-the--nor-the--compiler-please-install-xcode-command-line-tools-component"></a>MT5104:どちらが見つかりませんでした、'\*'も'\*' コンパイラ。 Xcode コマンド ライン ツールのコンポーネントをインストールしてください。

<!-- 5105 is used by mmp -->

<a name="MT5106" />

### <a name="mt5106-could-not-compile-the-files--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5106:ファイルをコンパイルできませんでした ' *'。 バグ報告を提出してください。 http://bugzilla.xamarin.com

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

### <a name="mt52xx-linking"></a>MT52xx:リンク

<!--
  MT52xx linking
  -->

<a name="MT5201" />

### <a name="mt5201-native-linking-failed-please-review-the-build-log-and-the-user-flags-provided-to-gcc-"></a>MT5201:ネイティブのリンクに失敗しました。 ビルド ログと gcc に提供されるユーザー フラグを確認してください *。

<a name="MT5202" />

### <a name="mt5202-native-linking-failed-please-review-the-build-log"></a>MT5202:ネイティブのリンクに失敗しました。 ビルド ログを確認してください。

<a name="MT5203" />

### <a name="mt5203-native-linking-warning-"></a>MT5203:ネイティブの警告をリンクします *。

<!--- 5204-5208 are not used -->

<a name="MT5209" />

### <a name="mt5209-native-linking-error-"></a>MT5209:ネイティブ エラー リンク: *

<a name="MT5210" />

### <a name="mt5210-native-linking-failed-undefined-symbol--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-are-properly-linked-in"></a>MT5210:失敗した場合は、リンクをネイティブにシンボルが定義されていません: *。 必要なすべてのフレームワークを参照しています、ネイティブ ライブラリにリンクされて適切であることを確認してください。

これは、ネイティブ リンカーの動作は、どこかで参照されているシンボルを見つけられないときに発生します。 これが発生するいくつかの理由はあります。

* サード パーティのバインドには、フレームワークが必要ですが、これには、バインドを指定しないその`[LinkWith]`属性。 ソリューション:
  - サード パーティのバインドの作成者をしているソースにアクセスするか、変更、バインディングの`[LinkWith]`属性が必要なフレームワークを含めます。

            [LinkWith ("mylib.a", Frameworks = "SystemConfiguration")]

  - サード パーティのバインドを変更することはできません場合、手動でリンクできますフレームワークを渡すことによって`-gcc_flags '-framework SystemFramework'`に`mtouch`(これは、プロジェクトの iOS ビルド オプション ページで追加 mtouch 引数を変更することによって行われます。 注意してくださいこれをすべてのプロジェクト構成を実行すること)。
* 場合によっては、管理対象のバインディングがいくつかのネイティブ ライブラリので構成され、バインドですべて含める必要があります。 解決するにはバインド プロジェクトに必要なすべてのネイティブ ライブラリを追加するために、各バインド プロジェクトの 1 つ以上のネイティブ ライブラリを含めることは可能になります。</li>
* 管理対象のバインドは、ネイティブ ライブラリに存在しないネイティブのシンボルを参照します。
    これは通常、バインディングは、しばらくの間存在しており、ネイティブ コードが変更されて期間中にいるため、特定のネイティブ クラスか、削除された名前の変更、バインディングが更新されていないときに発生します。
* P/invoke は、存在しないネイティブのシンボルを参照します。 Xamarin.iOS 7.4 以降、 <a href="#MT5214">MT5214</a>この場合のエラーが報告されます (詳細については、MT5214 を参照してください)。
* サード パーティ製バインディング ライブラリは、C++ を使用して作成されましたが、これには、バインドを指定しない/その`[LinkWith]`属性。 これは実に簡単に認識、変形した C++ のシンボルがシンボルがあるために、(1 つの一般的な例は`__ZNKSt9exception4whatEv`)。
  - サード パーティのバインドの作成者をしているソースにアクセスするか、変更、バインディングの`[LinkWith]`を設定する属性、`IsCxx`フラグ。

            [LinkWith ("mylib.a", IsCxx = true)]

  - 渡すことによって同等フラグを設定するにはサード パーティのバインドを変更することはできませんまたはサード パーティ製ライブラリを手動でリンクしている、 <code>-cxx</code> mtouch (ここでは、プロジェクトの iOS ビルド オプション ページで追加 mtouch 引数を変更するには. 注意してくださいこれをすべてのプロジェクト構成を実行すること)。

<a name="MT5211" />

### <a name="mt5211-native-linking-failed-undefined-objective-c-class--the-symbol--could-not-be-found-in-any-of-the-libraries-or-frameworks-linked-with-your-application"></a>MT5211:リンク失敗した場合は、ネイティブには、OBJECTIVE-C のクラスが定義されていません:\*します。 シンボル '\*' ライブラリやアプリケーションにリンクされているフレームワークのいずれかで見つかりませんでした。

これは、ネイティブ リンカーの動作は、どこかで参照されている OBJECTIVE-C クラスを見つけられないときに発生します。 これが発生するいくつかの理由がある: 場合と同じ[MT5210](#MT5210)とさらに。

* サード パーティのバインド、OBJECTIVE-C プロトコルのバインドがしなかったいない注釈を付けることで、<code>[Protocol]</code>の api 定義の属性。 ソリューション:
  - 不足している追加`[Protocol]`属性。

              [BaseType (typeof (NSObject))]
              [Protocol] // Add this
              public interface MyProtocol
              {
              }

<a name="MT5212" />

### <a name="mt5212-native-linking-failed-duplicate-symbol-"></a>MT5212:失敗した場合は、リンクをネイティブにシンボルが重複しています: *。

これは、ネイティブ リンカーの動作には、すべてのネイティブ ライブラリの間で重複しているシンボルがで検出したときに発生します。 次のこのエラーがあります 1 つまたは複数[MT5213](#MT5213)エラーが出現するたびに、シンボルの場所を使用します。 このエラーの考えられる理由:

* 同じネイティブ ライブラリが 2 回含まれます。
* 同じシンボルを定義する 2 つの個別のネイティブ ライブラリが行われます。
* ネイティブ ライブラリは、正しく組み込まれていないと、複数回、同じシンボルが含まれています。
  これは、次の一連のターミナルからコマンドを使用して確認できます (i386 は x86_64/armv7 と armv7s/arm64 を構築するアーキテクチャに従ってに置き換えてください)。

        # Native libraries are usually fat libraries, containing binary code for
        # several architectures in the same file. First we extract the binary
        # code for the architecture we're interested in.
        lipo libNative.a -thin i386 -output libNative.i386.a

        # Now query the native library for the duplicated symbol.
        nm libNative.i386.a | fgrep 'SYMBOL'

        # You can also list the object files inside the native library.
        # In most cases this will reveal duplicated object files.
        ar -t libNative.i386.a

  これを解決するいくつかの可能な方法はあります。

  - ネイティブ ライブラリのプロバイダーが問題を修正し、更新されたバージョンの提供を要求します。
  - (この場合にのみ機能、問題は実際には重複しているオブジェクト ファイル)、余分なオブジェクト ファイルを削除することで自分で解決すること

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

  - 未使用のコードを削除するリンカーに問い合わせてください。 Xamarin.iOS は次のように自動的にすべての次の条件が満たされた場合
    - すべてのサードパーティのバインドの`[LinkWith]`SmartLink 属性を有効にします。

            [assembly: LinkWith ("libNative.a", SmartLink = true)]

    - いいえ`-gcc_flags`mtouch (プロジェクトの iOS ビルド オプションの追加 mtouch 引数フィールド) 内に渡されます。
    - 追加することで未使用のコードを削除するには、直接リンカーを依頼することも`-gcc_flags -dead_strip`プロジェクトの iOS の他の mtouch 引数には、ビルド オプション。

<a name="MT5213" />

### <a name="mt5213-duplicate-symbol-in--location-related-to-previous-error"></a>MT5213:重複するシンボル: * (場所は、前のエラーに関連する)

このエラーが報告と共に[MT5212](#MT5212)します。 参照してください[MT5212](#MT5212)詳細についてはします。

<a name="MT5214" />

### <a name="mt5214-native-linking-failed-undefined-symbol--this-symbol-was-referenced-the-managed-member--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-linked"></a>MT5214:失敗した場合は、リンクをネイティブにシンボルが定義されていません: *。 このシンボルが参照されたマネージ メンバー *。 必要なすべてのフレームワークが参照されていると、ネイティブ ライブラリがリンクされていることを確認してください。

マネージ コードが存在しないネイティブ メソッドに P/invoke を含まれている場合、このエラーが報告されます。 例えば:

```csharp
using System.Runtime.InteropServices;
class MyImports {
   [DllImport ("__Internal")]
   public static extern void MyInexistentFunc ();
}
```

これには、いくつかの考えられる解決策があります。

  -  ソース コードから P/invoke の問題を削除します。
  -  (これは、プロジェクトの ios ビルド オプションでは、「すべてのアセンブリ」を「リンカーの動作」を設定する) すべてのアセンブリのマネージ リンカーを有効にします。 使用しないすべての P/invoke アプリから効果的に削除します (自動的には、代わりに手動でなどの以前の時点)。 欠点は、シミュレーターのビルドを少し遅くなりますが、このようにして、リフレクションを使用している場合、リンカーの詳細についてを参照して、アプリを壊す可能性があることを[ここ](~/ios/deploy-test/linker.md))
  -  不足しているネイティブのシンボルを含む 2 つ目のネイティブ ライブラリを作成します。 問題を回避するだけでは、このことに注意してください (これらの関数を呼び出すしようとする場合は、アプリがクラッシュ)。

<a name="MT5215" />

### <a name="mt5215-references-to--might-require-additional--frameworkxxx-or--lxxx-instructions-to-the-native-linker"></a>MT5215:参照 ' *' その他の必要があります framework = ネイティブ リンカー XXX または lXXX - 指示

これは、警告を対象のライブラリを参照して、P/invoke が検出されましたが、アプリがそれにリンクしないことを示すです。

<a name="MT5216" />

### <a name="mt5216-native-linking-failed-for--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5216:ネイティブのリンクが失敗しました *。 バグ報告を提出してください。 http://bugzilla.xamarin.com

AOT コンパイラからの出力をリンクするときにこのエラーが報告されます。

このエラーの考えでは、Xamarin.iOS のバグを示します。 バグ報告を送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT5217" />

### <a name="mt5217-native-linking-possibly-failed-because-the-linker-command-line-was-too-long--characters"></a>MT5217:リンカーのコマンドラインが長すぎるため、失敗したネイティブ リンク (* 文字)。

ネイティブのリンクが失敗し、リンカーのコマンドが長すぎるため、これが発生したことができます。

Xamarin.iOS プロジェクトは多くの場合、ネイティブ シンボル参照を動的にネイティブ リンカーでこれらのシンボルを使用することが表示されないため、ネイティブ リンカーの動作がネイティブのリンク プロセス中にこのようなネイティブのシンボルを削除可能性があることを意味します。

Xamarin.iOS を使用してこのようなシンボルを保持するネイティブ リンカーの動作を要求する通常の`-u symbol`シンボル数が多など、コマンド ライン全体がある場合は、リンカー フラグは、オペレーティング システムで指定された最大のコマンドラインの長さを超える場合があります。

このような動的シンボルのいくつかの可能なソースがあります。

* P/invoke メソッドに静的にリンクされたライブラリ (dll の名前は`__Internal`DllImport 属性で`[DllImport ("__Internal")]`)。
* プロジェクトのバインドから静的にリンクされたライブラリ内のメモリ ロケーションへの参照をフィールド (`[Field]`属性)。
* OBJECTIVE-C のバインドでは、(インクリメンタル ビルドを使用する場合または静的なレジストラーを使用していない場合など) をプロジェクトから静的にリンクされたライブラリで参照されているクラス。

考えられる解決策:

* (唯一の SDK アセンブリではなく、すべてのアセンブリに対して可能な) 場合は、管理対象のリンカーを有効にします。 動的シンボルできるように、リンカーのコマンドラインが最大数を超えてのソースの十分なこの削除可能性があります。
* P/invoke、フィールドへの参照や OBJECTIVE-C のクラスの数を減らします。
* 短い名前を持つ動的なシンボルを書き直してください。
* 渡す`-dlsym:false`プロジェクトの iOS の他の mtouch 引数としてビルド オプション。 このオプションでは、Xamarin.iOS は AOT コンパイルのコードでネイティブの参照を生成およびこのシンボルを保持するようにリンカーを依頼する必要はありません。 ただし、デバイスにのみこの機能は、次のビルド、および、リンカー エラーが発生するスタティック ライブラリに存在しない関数に P/invoke がある場合は、します。
* 渡す`--dynamic-symbol-mode=code`プロジェクトの iOS での他の mtouch 引数としてビルド オプション。 Xamarin.iOS は、このオプションは、コマンドライン引数を使用してこれらのシンボルを保持するネイティブ リンカーの動作を確認するのではなく、これらのシンボルを参照するその他のネイティブ コードに生成されます。 このアプローチの欠点は、それが増加する実行可能ファイルのサイズややです。
* 渡すことによって、静的なレジストラーを有効にする`--registrar:static`ビルド オプション (シミュレーターは、静的なレジストラーは既に既定のデバイスのビルド後にビルド) の場合、プロジェクトの iOS の他の mtouch 引数として。 このようなクラスを保持するネイティブ リンカーの動作を確認する必要はありません、静的なレジストラーは Objective C のクラスを静的に参照するコードを生成します。
* デバイスのビルド) (インクリメンタル ビルドを無効にします。 インクリメンタル ビルドが有効な場合は、静的なレジストラーによって生成されたコードは、Xamarin.iOS は保持するようにリンカーを問い合わせる必要がありますもことを意味には、OBJECTIVE-C のクラスが参照されているネイティブのリンカーによってと見なされません。 インクリメンタル ビルドを無効化させると、このニーズができなくなります。

<a name="MT5218" />

### <a name="mt5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MT5218:動的シンボル {symbol} を無視することはできません (--無視動的シンボル = {シンボル}) ため、動的な型のシンボルとして検出されませんでした。

コマンドライン引数`--ignore-dynamic-symbol=symbol`が渡されましたが、このシンボルは手動で保持する必要がある動的記号として認識されたシンボルではありません。

この 2 つの主な理由があります。

* シンボル名が正しくありません。
    * シンボル名にアンダー スコアを付加しません。
    * Objective C クラスの記号が`OBJC_CLASS_$_<classname>`します。
* シンボルが正しいことが、通常の方法 (いくつかのビルド オプションの原因を変更するための動的なシンボルの正確な一覧) で既に保存されているシンボル。

### <a name="mt53xx-other-tools"></a>MT53xx:その他のツール

<!--
  MT53xx other tools
  -->

<a name="MT5301" />

### <a name="mt5301-missing-strip-tool-please-install-xcode-command-line-tools-component"></a>MT5301:'削除' ツールがありません。 Xcode コマンド ライン ツールのコンポーネントをインストールしてください。

<a name="MT5302" />

### <a name="mt5302-missing-dsymutil-tool-please-install-xcode-command-line-tools-component"></a>MT5302:不足している 'dsymutil' ツールです。 Xcode コマンド ライン ツールのコンポーネントをインストールしてください。

<a name="MT5303" />

### <a name="mt5303-failed-to-generate-the-debug-symbols-dsym-directory-please-review-the-build-log"></a>MT5303:デバッグ シンボル (dSYM ディレクトリ) を生成できませんでした。 ビルド ログを確認してください。

デバッグ シンボルを作成する最後の .app ディレクトリで dsymutil の実行時にエラーが発生しました。 Dsymutil の出力が表示および解決方法を確認するビルド ログを確認してください。

<a name="MT5304" />

### <a name="mt5304-failed-to-strip-the-final-binary-please-review-the-build-log"></a>MT5304:最終的なバイナリを削除できませんでした。 ビルド ログを確認してください。

アプリケーションからデバッグ情報を削除する 'ストリップ' ツールの実行中にエラーが発生しました。

<a name="MT5305" />

### <a name="mt5305-missing-lipo-tool-please-install-xcode-command-line-tools-component"></a>MT5305:不足している 'lipo' ツールです。 Xcode コマンド ライン ツールのコンポーネントをインストールしてください。

<a name="MT5306" />

### <a name="mt5306-failed-to-create-the-a-fat-library-please-review-the-build-log"></a>MT5306:作成できませんでした、fat ライブラリ。 ビルド ログを確認してください。

'Lipo' ツールの実行中にエラーが発生しました。 'Lipo' によって報告されたエラーを表示するビルド ログを確認してください。

<a name="MT5307" />

### <a name="mt5307-failed-to-sign-the-executable-please-review-the-build-log"></a>MT5307:実行可能ファイルの署名に失敗しました。 ビルド ログを確認してください。

アプリケーションに署名するときにエラーが発生しました。 'Codesign' によって報告されたエラーを表示するビルド ログを確認してください。

<!-- 5308 is used by mmp -->
<!-- 5309 is used by mmp -->
<!-- 5310 is used by mmp -->

## <a name="mt6xxx-mtouch-internal-tools-error-messages"></a>MT6xxx: 内部の mtouch ツールのエラー メッセージ

### <a name="mt600x-stripper"></a>MT600x:削除

<!--
 MT6xxx mtouch internal tools
  MT600x Stripper
  -->

<a name="MT6001" />

### <a name="mt6001-running-version-of-cecil-doesnt-support-assembly-stripping"></a>MT6001:Cecil の実行中のバージョンは、アセンブリの削除をサポートしていません

<a name="MT6002" />

### <a name="mt6002-could-not-strip-assembly-"></a>MT6002:アセンブリを削除できませんでした`*`します。

アプリケーションのアセンブリからマネージ コード (IL コードを削除する) を削除する場合、エラーが発生しました。

<a name="MT6003" />

### <a name="mt6003-unauthorizedaccessexception-message"></a>MT6003: [UnauthorizedAccessException message]

セキュリティ エラーが発生しましたデバッグ、アプリケーションからのシンボルを削除します。

## <a name="mt7xxx-msbuild-error-messages"></a>MT7xxx:MSBuild エラー メッセージ

<!--
 MT7xxx msbuild errors
  -->

<a name="MT7001" />

### <a name="mt7001-could-not-resolve-host-ips-for-wifi-debugger-settings"></a>MT7001:WiFi デバッガーの設定のホストの ip アドレスを解決できませんでした。

*MSBuild タスク。DetectDebugNetworkConfigurationTaskBase*

トラブルシューティングの手順。

- 実行しようとしています。 `csharp -e 'System.Net.Dns.GetHostEntry (System.Net.Dns.GetHostName ()).AddressList'` (が表示する、IP アドレスとエラーではなく明らかです)。
- 実行しようとしています。"ping \`hostname\`"可能性がありますを提供する詳細については、このような。 `cannot resolve MyHost.local: Unknown host`

場合によっては「ローカル ネットワーク」問題と、不明なホストを追加することでアドレス指定できます`127.0.0.1   MyHost.local`で`/etc/hosts`します。

<a name="MT7002" />

### <a name="mt7002-this-machine-does-not-have-any-network-adapters-this-is-required-when-debugging-or-profiling-on-device-over-wifi"></a>MT7002:このマシンには、任意のネットワーク アダプターはありません。 デバッグまたは WiFi 経由でのデバイスでプロファイリングする場合に必要です。

*MSBuild タスク。DetectDebugNetworkConfigurationTaskBase*

<a name="MT7003" />

### <a name="mt7003-the-app-extension--does-not-contain-an-infoplist"></a>MT7003:アプリの拡張機能 ' *' に Info.plist が含まれていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7004" />

### <a name="mt7004-the-app-extension--does-not-specify-a-cfbundleidentifier"></a>MT7004:アプリの拡張機能 ' *'、CFBundleIdentifier は指定されていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7005" />

### <a name="mt7005-the-app-extension--does-not-specify-a-cfbundleexecutable"></a>MT7005:アプリの拡張機能 ' *'、CFBundleExecutable は指定されていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7006" />

### <a name="mt7006-the-app-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7006:アプリの拡張機能 '\*' が、無効な CFBundleIdentifier (\*)、メイン アプリケーション バンドルの CFBundleIdentifier (*) で始まらないです。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7007" />

### <a name="mt7007-the-app-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>MT7007:アプリ拡張機能 '\*'、CFBundleIdentifier が (\*) 無効なサフィックス".key"で終了します。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7008" />

### <a name="mt7008-the-app-extension--does-not-specify-a-cfbundleshortversionstring"></a>MT7008:アプリの拡張機能 ' *'、CFBundleShortVersionString は指定されていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7009" />

### <a name="mt7009-the-app-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>MT7009:アプリの拡張機能 ' *' が無効な Info.plist: NSExtension ディクショナリが含まれていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7010" />

### <a name="mt7010-the-app-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionpointidentifier-value"></a>MT7010:アプリの拡張機能 ' *' が無効な Info.plist: NSExtension ディクショナリに、NSExtensionPointIdentifier 値が含まれていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7011" />

### <a name="mt7011-the-watchkit-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionattributes-dictionary"></a>MT7011:WatchKit 拡張機能 ' *' が無効な Info.plist: NSExtension ディクショナリに、NSExtensionAttributes ディクショナリが含まれていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7012" />

### <a name="mt7012-the-watchkit-extension--does-not-have-exactly-one-watch-app"></a>MT7012:WatchKit 拡張機能 ' *' が 1 つだけの watch アプリはありません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7013" />

### <a name="mt7013-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-must-contain-the-watch-companion-capability"></a>MT7013:WatchKit 拡張機能 ' *' が無効な Info.plist:UIRequiredDeviceCapabilities 'watch コンパニオン' 機能を含める必要があります。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7014" />

### <a name="mt7014-the-watch-app--does-not-contain-an-infoplist"></a>MT7014:Watch アプリ ' *' に Info.plist が含まれていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7015" />

### <a name="mt7015-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>MT7015:Watch アプリ ' *'、CFBundleIdentifier は指定されていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7016" />

### <a name="mt7016-the-watch-app--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7016:Watch アプリ '\*' が、無効な CFBundleIdentifier (\*)、メイン アプリケーション バンドルの CFBundleIdentifier (*) で始まらないです。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7017" />

### <a name="mt7017-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected-watch-4-but-found--"></a>MT7017:Watch アプリ '\*' UIDeviceFamily の有効な値はありません。 予想される 'Watch (4)' が見つかりました。 '\* (*)' です。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7018" />

### <a name="mt7018-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>MT7018:Watch アプリ ' *'、CFBundleExecutable は指定されていません

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7019" />

### <a name="mt7019-the-watch-app--has-an-invalid-wkcompanionappbundleidentifier-value--it-does-not-match-the-main-app-bundles-cfbundleidentifier-"></a>MT7019:Watch アプリ '\*' が無効な WKCompanionAppBundleIdentifier 値 ('\*')、メイン アプリケーション バンドルの CFBundleIdentifier と一致しません ('* ')。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7020" />

### <a name="mt7020-the-watch-app--has-an-invalid-infoplist-the-wkwatchkitapp-key-must-be-present-and-have-a-value-of-true"></a>MT7020:Watch アプリ ' *' が無効な Info.plist: WKWatchKitApp キーが存在して 'true' の値を指定する必要があります。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7021" />

### <a name="mt7021-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>MT7021:Watch アプリ ' *' が無効な Info.plist: LSRequiresIPhoneOS キーが存在しない場合があります。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7022" />

### <a name="mt7022-the-watch-app--does-not-contain-a-watch-extension"></a>MT7022:Watch アプリ ' *' ウォッチ拡張機能が含まれていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7023" />

### <a name="mt7023-the-watch-extension--does-not-contain-an-infoplist"></a>MT7023:ウォッチ拡張機能 ' *' に Info.plist が含まれていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7024" />

### <a name="mt7024-the-watch-extension--does-not-specify-a-cfbundleidentifier"></a>MT7024:ウォッチ拡張機能 ' *'、CFBundleIdentifier は指定されていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7025" />

### <a name="mt7025-the-watch-extension--does-not-specify-a-cfbundleexecutable"></a>MT7025:ウォッチ拡張機能 ' *'、CFBundleExecutable は指定されていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7026" />

### <a name="mt7026-the-watch-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7026:ウォッチ拡張機能 '\*' が、無効な CFBundleIdentifier (\*)、メイン アプリケーション バンドルの CFBundleIdentifier (*) で始まらないです。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7027" />

### <a name="mt7027-the-watch-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>MT7027:ウォッチ拡張機能 '\*'、CFBundleIdentifier が (\*) 無効なサフィックス".key"で終了します。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7028" />

### <a name="mt7028-the-watch-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>MT7028:ウォッチ拡張機能 ' *' が無効な Info.plist: NSExtension ディクショナリが含まれていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7029" />

### <a name="mt7029-the-watch-extension--has-an-invalid-infoplist-the-nsextensionpointidentifier-must-be-comapplewatchkit"></a>MT7029:ウォッチ拡張機能 ' *' が無効な Info.plist: NSExtensionPointIdentifier は"com.apple.watchkit"である必要があります。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7030" />

### <a name="mt7030-the-watch-extension--has-an-invalid-infoplist-the-nsextension-dictionary-must-contain-nsextensionattributes"></a>MT7030:ウォッチ拡張機能 ' *' が無効な Info.plist: NSExtension ディクショナリ NSExtensionAttributes を含める必要があります。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7031" />

### <a name="mt7031-the-watch-extension--has-an-invalid-infoplist-the-nsextensionattributes-dictionary-must-contain-a-wkappbundleidentifier"></a>MT7031:ウォッチ拡張機能 ' *' が無効な Info.plist: NSExtensionAttributes ディクショナリは、WKAppBundleIdentifier を含める必要があります。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7032" />

### <a name="mt7032-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-should-not-contain-the-watch-companion-capability"></a>MT7032:WatchKit 拡張機能 ' *' が無効な Info.plist:UIRequiredDeviceCapabilities では、' watch コンパニオン ' 機能を含めることはできません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7033" />

### <a name="mt7033-the-watch-app--does-not-contain-an-infoplist"></a>MT7033:Watch アプリ ' *' に Info.plist が含まれていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7034" />

### <a name="mt7034-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>MT7034:Watch アプリ ' *'、CFBundleIdentifier は指定されていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7035" />

### <a name="mt7035-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected--but-found--"></a>MT7035:Watch アプリ '\*' UIDeviceFamily の有効な値はありません。 予想 '\*'ですが'\* (\*)' です。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7036" />

### <a name="mt7036-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>MT7036:Watch アプリ ' *'、CFBundleExecutable は指定されていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7037" />

### <a name="mt7037-the-watchkit-extension-extensionname-has-an-invalid-wkappbundleidentifier-value--it-does-not-match-the-watch-apps-cfbundleidentifier-"></a>MT7037:WatchKit 拡張機能 '{{publishername}}' が無効な WKAppBundleIdentifier 値 ('\*')、Watch アプリの CFBundleIdentifier と一致しません ('\*')。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7038" />

### <a name="mt7038-the-watch-app--has-an-invalid-infoplist-the-wkcompanionappbundleidentifier-must-exist-and-must-match-the-main-app-bundles-cfbundleidentifier"></a>MT7038:Watch アプリ ' *' が無効な Info.plist: WKCompanionAppBundleIdentifier が存在し、メイン アプリケーション バンドルの CFBundleIdentifier に一致する必要があります。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7039" />

### <a name="mt7039-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>MT7039:Watch アプリ ' *' が無効な Info.plist: LSRequiresIPhoneOS キーが存在しない場合があります。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7040" />

### <a name="mt7040-the-app-bundle-appbundlepath-does-not-contain-an-infoplist"></a>MT7040:アプリ バンドル {AppBundlePath} に Info.plist が含まれていません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7041" />

### <a name="mt7041-main-infoplist-path-does-not-specify-a-cfbundleidentifier"></a>MT7041:Info.plist のメイン パスには、CFBundleIdentifier は指定しません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7042" />

### <a name="mt7042-main-infoplist-path-does-not-specify-a-cfbundleexecutable"></a>MT7042:Info.plist のメイン パスには、CFBundleExecutable は指定しません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7043" />

### <a name="mt7043-main-infoplist-path-does-not-specify-a-cfbundlesupportedplatforms"></a>MT7043:Info.plist のメイン パスには、CFBundleSupportedPlatforms は指定しません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7044" />

### <a name="mt7044-main-infoplist-path-does-not-specify-a-uidevicefamily"></a>MT7044:Info.plist のメイン パスには、UIDeviceFamily は指定しません。

*MSBuild タスク。ValidateAppBundleTaskBase*

<a name="MT7045" />

### <a name="mt7045-unrecognized-format-"></a>MT7045:形式を認識できません: *。

*MSBuild タスク。PropertyListEditorTaskBase*

場所 * を指定できます。

- string
- array
- dict
- bool
- 実数
- 整数
- date
- [データ]

<a name="MT7046" />

### <a name="mt7046-add-entry--incorrectly-specified"></a>MT7046:追加:エントリ、*、正しく指定されていません。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7047" />

### <a name="mt7047-add-entry--contains-invalid-array-index"></a>MT7047:追加:エントリ、*、無効な配列のインデックスが含まれています。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7048" />

### <a name="mt7048-add--entry-already-exists"></a>MT7048:追加します。 * エントリが既に存在します。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7049" />

### <a name="mt7049-add-cant-add-entry--to-parent"></a>MT7049:追加:エントリを追加することはできません *、親にします。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7050" />

### <a name="mt7050-delete-cant-delete-entry--from-parent"></a>MT7050:削除: エントリを削除することはできません *、親から。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7051" />

### <a name="mt7051-delete-entry--contains-invalid-array-index"></a>MT7051:削除: エントリ、*、無効な配列のインデックスが含まれています。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7052" />

### <a name="mt7052-delete-entry--does-not-exist"></a>MT7052:削除: エントリ、*、存在しません。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7053" />

### <a name="mt7053-import-entry--incorrectly-specified"></a>MT7053:インポート:エントリ、*、正しく指定されていません。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7054" />

### <a name="mt7054-import-entry--contains-invalid-array-index"></a>MT7054:インポート:エントリ、*、無効な配列のインデックスが含まれています。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7055" />

### <a name="mt7055-import-error-reading-file-"></a>MT7055:インポート:ファイルの読み取りエラー: *。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7056" />

### <a name="mt7056-import-cant-add-entry--to-parent"></a>MT7056:インポート:エントリを追加することはできません *、親にします。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7057" />

### <a name="mt7057-merge-cant-add-array-entries-to-dict"></a>MT7057:マージします。Dict. に配列のエントリを追加することはできません。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7058" />

### <a name="mt7058-merge-specified-entry-must-be-a-container"></a>MT7058:マージします。指定されたエントリは、コンテナーである必要があります。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7059" />

### <a name="mt7059-merge-entry--contains-invalid-array-index"></a>MT7059:マージします。エントリ、*、無効な配列のインデックスが含まれています。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7060" />

### <a name="mt7060-merge-entry--does-not-exist"></a>MT7060:マージします。エントリ、*、存在しません。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7061" />

### <a name="mt7061-merge-error-reading-file-"></a>MT7061:マージします。ファイルの読み取りエラー: *。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7062" />

### <a name="mt7062-set-entry--incorrectly-specified"></a>MT7062:設定できる値:エントリ、*、正しく指定されていません。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7063" />

### <a name="mt7063-set-entry--contains-invalid-array-index"></a>MT7063:設定できる値:エントリ、*、無効な配列のインデックスが含まれています。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7064" />

### <a name="mt7064-set-entry--does-not-exist"></a>MT7064:設定できる値:エントリ、*、存在しません。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7065" />

### <a name="mt7065-unknown-propertylist-editor-action-"></a>MT7065:不明な PropertyList エディターの動作: *。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7066" />

### <a name="mt7066-error-loading--"></a>MT7066:読み込みエラー ' *': *。

*MSBuild タスク。PropertyListEditorTaskBase*

<a name="MT7067" />

### <a name="mt7067-error-saving--"></a>MT7067:保存中のエラー ' *': *。

*MSBuild タスク。PropertyListEditorTaskBase*

## <a name="mt8xxx-runtime-error-messages"></a>MT8xxx:ランタイム エラー メッセージ

<!--
 MT8xxx runtime
  MT800x misc
  -->

<a name="MT8001" />

### <a name="mt8001-version-mismatch-between-the-native-xamarinios-runtime-and-monotouchdll-please-reinstall-xamarinios"></a>MT8001:バージョンは、ネイティブの Xamarin.iOS ランタイム monotouch.dll とが一致しません。 Xamarin.iOS をインストールし直してください。

<a name="MT8002" />

### <a name="mt8002-could-not-find-the-method--in-the-type-"></a>MT8002:メソッドが見つかりませんでした '\*'type' で\*'。

<a name="MT8003" />

### <a name="mt8003-failed-to-find-the-closed-generic-method--on-the-type-"></a>MT8003:クローズ ジェネリック メソッドが見つかりませんでした '\*'type' で\*'。

<a name="MT8004" />

### <a name="mt8004-cannot-create-an-instance-of--for-the-native-object-0x-of-type--because-another-instance-already-exists-for-this-native-object-of-type-"></a>MT8004:インスタンスを作成することはできません * のネイティブ オブジェクトの 0 x * (型の ' *')、このネイティブ オブジェクトの別のインスタンスが既に存在するので (型の *)。

<a name="MT8005" />

### <a name="mt8005-wrapper-type--is-missing-its-native-objectivec-class-"></a>MT8005:ラッパーの型 '\*'そのネイティブ ObjectiveC クラスがありません'\*'。

<a name="MT8006" />

### <a name="mt8006-failed-to-find-the-selector--on-the-type-"></a>MT8006:セレクターが見つかりませんでした '\*'type' で\*'

<a name="MT8007" />

### <a name="mt8007-cannot-get-the-method-descriptor-for-the-selector--on-the-type--because-the-selector-does-not-correspond-to-a-method"></a>MT8007:セレクターのメソッドの記述子を取得できません '\*'type' で\*' セレクターがメソッドに対応していないため、

<a name="MT8008" />

### <a name="mt8008-the-loaded-version-of-xamariniosdll-was-compiled-for--bits-while-the-process-is--bits-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8008:Xamarin.iOS.dll の読み込まれているのバージョン用にコンパイルされた * 処理中に、ビット * ビット。 バグを送信してください http://bugzilla.xamarin.com です。

これは、ビルド処理で問題がありますを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8009" />

### <a name="mt8009-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-s-parameter--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8009:メソッドの変換メソッドをデリゲートするブロックが見つかりません *.*'s パラメーター # *。 バグを送信してください http://bugzilla.xamarin.com です。

これは、API が正しくバインドされているを示します。 Xamarin によって公開される API の場合は、当社 bugzilla でバグを提出してください ([http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS))、サード パーティのバインドでは、場合、製造元に問い合わせてください。

<a name="MT8010" />

### <a name="mt8010-native-type-size-mismatch-between-xamariniosmacdll-and-the-executing-architecture-xamariniosmacdll-was-built-for--bit-while-the-current-process-is--bit"></a>MT8010:Xamarin との間には、サイズの不一致をネイティブ型です。[iOS |Mac] .dll、および実行中のアーキテクチャ。 Xamarin。[iOS |用にビルドされた Mac] .dll *-bit で、現在のプロセス中に *-ビット。

これは、ビルド処理で問題がありますを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8011" />

### <a name="mt8011-unable-to-locate-the-delegate-to-block-conversion-attribute-delegateproxy-for-the-return-value-for-the-method--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8011:メソッドの戻り値のブロック変換属性 ([DelegateProxy]) にデリゲートが見つかりません *.* します。 バグを送信してください http://bugzilla.xamarin.com です。

Xamarin.iOS は、(ブロックをデリゲートに変換) を実行時に必要なメソッドを見つけられませんでした。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8012" />

### <a name="mt8012-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8012:メソッドの戻り値の無効な DelegateProxyAttribute *.*:DelegateType が null です。 バグを送信してください http://bugzilla.xamarin.com です。

該当するメソッドの DelegateProxy 属性が無効です。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8013" />

### <a name="mt8013-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-2-specifies-a-type-without-a-handler-field-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8013:メソッドの戻り値の無効な DelegateProxyAttribute *.*:DelegateType ({2}) なし 'Handler' フィールドの種類を指定します。 バグを送信してください http://bugzilla.xamarin.com です。

該当するメソッドの DelegateProxy 属性が無効です。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8014" />

### <a name="mt8014-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8014:メソッドの戻り値の無効な DelegateProxyAttribute *.*:DelegateType の ({2}) 'Handler' フィールドが null です。 バグを送信してください http://bugzilla.xamarin.com です。

該当するメソッドの DelegateProxy 属性が無効です。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8015" />

### <a name="mt8015-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-not-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8015:メソッドの戻り値の無効な DelegateProxyAttribute *.*:DelegateType の ({2}) 'Handler' フィールドは、デリゲートは、*。 バグを送信してください http://bugzilla.xamarin.com です。

該当するメソッドの DelegateProxy 属性が無効です。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8016" />

### <a name="mt8016-unable-to-convert-delegate-to-block-for-the-return-value-for-the-method--because-the-input-isnt-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8016:メソッドの戻り値のブロックをデリゲートに変換できません *.* 入力が、デリゲートがないためは、*。 バグを送信してください http://bugzilla.xamarin.com です。

該当するメソッドの DelegateProxy 属性が無効です。

これは通常 Xamarin.iOS; のバグを示しますバグを送信してください[http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<!-- 8017 is used by mmp -->

<a name="MT8018" />

### <a name="mt8018-internal-consistency-error-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8018:内部一貫性エラーです。 バグ報告を送信してください http://bugzilla.xamarin.com です。

これは、Xamarin.iOS のバグを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8019" />

### <a name="mt8019-could-not-find-the-assembly--in-the-loaded-assemblies"></a>MT8019:アセンブリが見つかりませんでした。 * で読み込まれたアセンブリ。

これは、Xamarin.iOS のバグを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8020" />

### <a name="mt8020-could-not-find-the-module-with-metadatatoken--in-the-assembly-"></a>MT8020:MetadataToken 使用して、モジュールが見つかりませんでした。 *、アセンブリで *。

これは、Xamarin.iOS のバグを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8021" />

### <a name="mt8021-unknown-implicit-token-type-"></a>MT8021:不明な暗黙的なトークンの型: *。

これは、Xamarin.iOS のバグを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8022" />

### <a name="mt8022-expected-the-token-reference--to-be-a--but-its-a--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8022:トークンの参照が想定されました * にする、* が、*。 バグ報告を送信してください http://bugzilla.xamarin.com です。

これは、Xamarin.iOS のバグを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8023" />

### <a name="mt8023-an-instance-object-is-required-to-construct-a-closed-generic-method-for-the-open-generic-method--token-reference--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8023:オープン ジェネリック メソッドのクローズ ジェネリック メソッドを作成するインスタンス オブジェクトが必要です: * (トークン リファレンス: *)。 バグ報告を送信してください http://bugzilla.xamarin.com です。

これは、Xamarin.iOS のバグを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

<a name="MT8024" />

### <a name="mt8024-could-not-find-a-valid-extension-type-for-the-smart-enum-smarttype-please-file-a-bug-at-httpsbugzillaxamarincom"></a>MT8024:スマート列挙型 '{0} smart_type}' の有効な拡張機能の種類が見つかりませんでした。 バグを送信してください https://bugzilla.xamarin.com です。

これは、Xamarin.iOS のバグを示します。 バグを送信してください [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。
