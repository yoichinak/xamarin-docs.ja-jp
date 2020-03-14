---
title: Xamarin. iOS エラー
description: このドキュメントでは、Xamarin iOS アプリケーションのバンドルに使用するツールである、mtouch によって生成されるさまざまなエラーについて説明します。 エラーはコードによって一覧表示され、詳細な説明が示されます。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9F76162B-D622-45DA-996B-2FBF8017E208
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/06/2018
ms.openlocfilehash: a26c83565e4cfa64272549e12a35206dff6ec3c0
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306117"
---
# <a name="xamarinios-errors"></a>Xamarin. iOS エラー

## <a name="mt0xxx-mtouch-error-messages"></a>MT0xxx: mtouch のエラーメッセージ

例: パラメーター、環境、ツールがありません。

<!--
 MT0xxx mtouch itself, e.g. parameters, environment (e.g. missing tools)
 https://github.com/xamarin/xamarin-macios/blob/master/tools/mtouch/error.cs
  -->

<a name="MT0000" />

### <a name="mt0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcomxamarinxamarin-maciosissuesnew"></a>MT0000: 予期しないエラー- https://github.com/xamarin/xamarin-macios/issues/new にバグレポートを入力してください

予期しないエラー状態が発生しました。 以下を含む、できるだけ多くの情報を使用して、 [GitHub](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

- 最大の詳細度を持つ完全ビルドログ (**追加の mtouch 引数**に `-v -v -v -v` など)。
- エラーを再現する最小限のテストケースそして
- すべてのバージョンの解説

正確なバージョン情報を取得する最も簡単な方法は、 **[Visual Studio for Mac]** メニューを使用し**て、Visual Studio for Mac 項目について**、 **[詳細の表示]** ボタンをクリックし、バージョン情報をコピー/貼り付けすることです ( **[情報のコピー]** ボタンを使用できます)。

<a name="MT0001" />

### <a name="mt0001--devname-was-provided-without-any-device-specific-action"></a>MT0001: '-devname ' が指定されましたが、デバイス固有の操作はありません

これは、デバイス固有のアクション (-logdev/-installdev/-devname apps) が要求されていない場合に-が mtouch に渡されると生成される警告です。

<a name="MT0002" />

### <a name="mt0002-could-not-parse-the-environment-variable-"></a>MT0002: 環境変数 * を解析できませんでした。

このエラーは、無効な環境キー = 値の変数のペアを設定しようとした場合に発生します。 正しい形式は次のとおりです: `mtouch --setenv=VARIABLE=VALUE`

<a name="MT0003" />

### <a name="mt0003-application-name-exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MT0003: アプリケーション名 ' * .exe ' が SDK または製品アセンブリ (.dll) 名と競合しています。

実行可能アセンブリの名前とアプリケーション名は、アプリ内の dll の名前と一致することはできません。 実行可能ファイルの名前を変更してください。

<a name="MT0004" />

### <a name="mt0004-new-refcounting-logic-requires-sgen-to-be-enabled-too"></a>MT0004: 新しい refcounting ロジックでは、SGen も有効にする必要があります。

Refcounting 拡張機能を有効にする場合は、プロジェクトの iOS ビルドオプション ([詳細設定] タブ) で SGen ガベージコレクターを有効にする必要もあります。

7\.2.1 以降では、この要件は解除されています。 Boehm と SGen の両方のガベージコレクターを使用して、新しい refcounting ロジックを有効にすることができます。

<a name="MT0005" />

### <a name="mt0005-the-output-directory--does-not-exist"></a>MT0005: 出力ディレクトリ * が存在しません。

ディレクトリを作成してください。

このエラーは生成されなくなりました。 mtouch によって、ディレクトリが存在しない場合は自動的に作成されます。

<a name="MT0006" />

### <a name="mt0006-there-is-no-devel-platform-at--use---platformplat-to-specify-the-sdk"></a>MT0006: * で devel プラットフォームは存在しません。--platform = プラットフォームを使用して SDK を指定します。

Xamarin. iOS は、エラーメッセージに示されている場所に SDK ディレクトリを見つけることができません。 パスが正しいことを確認してください。

<a name="MT0007" />

### <a name="mt0007-the-root-assembly--does-not-exist"></a>MT0007: ルートアセンブリ * が存在しません。

Xamarin. iOS は、エラーメッセージに示されている場所にアセンブリを見つけることができません。 パスが正しいことを確認してください。

<a name="MT0008" />

### <a name="mt0008-you-should-provide-one-root-assembly-only-found--assemblies-"></a>MT0008: ルートアセンブリを1つだけ指定してください。 # assemblies: * が見つかりました。

複数のルートアセンブリが mtouch に渡されましたが、ルートアセンブリは1つしか存在できません。

<a name="MT0009" />

### <a name="mt0009-error-while-loading-assemblies-"></a>MT0009: アセンブリの読み込み中にエラーが発生しました: *。

ルートアセンブリ参照からアセンブリを読み込み中にエラーが発生しました。 ビルド出力で詳細情報が提供される場合があります。

<a name="MT0010" />

### <a name="mt0010-could-not-parse-the-command-line-arguments-"></a>MT0010: コマンドライン引数を解析できませんでした: *。

コマンドライン引数の解析中にエラーが発生しました。 すべてが正しいことを確認してください。

<a name="MT0011" />

### <a name="mt0011--was-built-against-a-more-recent-runtime--than-monotouch-supports"></a>MT0011: \* は、Monotouch.dialog がサポートするよりも新しいランタイム (\*) に対してビルドされました。

この警告は通常、プロジェクトに Xamarin. iOS BCL を使用してビルドされていないクラスライブラリへの参照があるために報告されます。

.Net 4.0 SDK を使用しているアプリが、.net 2.0 のみをサポートしているシステムでは動作しない場合と同じように、.NET 4.0 を使用してビルドされたライブラリは、Xamarin. iOS では動作しない場合があります。これは、Xamarin. iOS に存在しない API を使用する可能性があります。

一般的な解決策は、ライブラリを Xamarin. iOS クラスライブラリとしてビルドすることです。 これを行うには、新しい Xamarin. iOS クラスライブラリプロジェクトを作成し、そのプロジェクトにすべてのソースファイルを追加します。 ライブラリのソースコードがない場合は、ベンダーに連絡して、Xamarin. iOS 互換バージョンのライブラリが提供されるように依頼する必要があります。

<a name="MT0012" />

### <a name="mt0012-incomplete-data-is-provided-to-complete-"></a>MT0012: 不完全なデータが * を完了するために提供されています。

現在のバージョンの Xamarin. iOS では、このエラーは今後報告されません。

<a name="MT0013" />

### <a name="mt0013-profiling-support-requires-sgen-to-be-enabled-too"></a>MT0013: プロファイルサポートでは、sgen も有効にする必要があります。

プロファイル (--プロファイリング) が有効になっている場合は、SGen (--sgen) を有効にする必要があります。

<a name="MT0014" />

### <a name="mt0014-the-ios--sdk-does-not-support-building-applications-targeting-"></a>MT0014: iOS \* SDK は \*を対象とするアプリケーションの構築をサポートしていません。

これは、次のような場合に発生する可能性があります。

- ARMv6 は有効になっており、Xcode 4.5 以降がインストールされています。
- ARMv7s は有効になっており、Xcode 4.4 以前がインストールされています。

インストールされている Xcode のバージョンで、選択したアーキテクチャがサポートされていることを確認してください。

<a name="MT0015" />

### <a name="mt0015-invalid-abi--supported-abis-are-i386-x86_64--armv7-armv7llvm-armv7llvmthumb2-armv7s-armv7sllvm-armv7sllvmthumb2-arm64-and-arm64llvm"></a>MT0015: 無効な ABI: *。 サポートされている ABIs: i386、x86_64、armv7、armv7 + llvm、armv7 + llvm + thumb2、armv7s、armv7s + llvm、armv7s + llvm + thumb2、arm64、arm64 + llvm。

無効な ABI が mtouch に渡されました。 有効な ABI を指定してください。

<a name="MT0016" />

### <a name="mt0016-the-option--has-been-deprecated"></a>MT0016: オプション * は非推奨とされました。

前述の mtouch オプションは推奨されていないため、無視されます。

<a name="MT0017" />

### <a name="mt0017-you-should-provide-a-root-assembly"></a>MT0017: ルートアセンブリを指定する必要があります。

アプリをビルドするときに、ルートアセンブリ (通常はメインの実行可能ファイル) を指定する必要があります。

<a name="MT0018" />

### <a name="mt0018-unknown-command-line-argument-"></a>MT0018: 不明なコマンドライン引数: *。

Mtouch は、エラーメッセージに示されているコマンドライン引数を認識しません。

<a name="MT0019" />

### <a name="mt0019-only-one---loginstallkilllaunchdev-or---launchdebugsim-option-can-be-used"></a>MT0019: 1 つの--[log | install | kill | launch] dev または--[launch | debug] sim オプションのみを使用できます。

Mtouch には、同時に使用できないオプションがいくつかあります。

- --logdev
- --installdev
- --すべての開発
- --launchdev
- --launchdebug
- --launchsim

<a name="MT0020" />

### <a name="mt0020-the-valid-options-for--are-"></a>MT0020: '\*' の有効なオプションは '\*' です。

<a name="MT0021" />

### <a name="mt0021-cannot-compile-using-gccg---use-gcc-when-using-the-static-registrar-this-is-the-default-when-compiling-for-device-either-remove-the---use-gcc-flag-or-use-the-dynamic-registrar---registrardynamic"></a>MT0021: 静的レジストラーを使用する場合、gcc/g + + (--use-gcc) を使用してコンパイルすることはできません (これはデバイス用にコンパイルする場合の既定の設定です)。 --Gcc フラグを削除するか、動的レジストラー (--レジストラー: dynamic) を使用してください。

<a name="MT0022" />

### <a name="mt0022-the-options---unsupported--enable-generics-in-registrar-and---registrar-are-not-compatible"></a>MT0022: オプション '--サポートされていない--レジストラー ' および '--レジストラー ' は互換性がありません。

`--unsupported--enable-generics-in-registrar` と `--registrar`の両方のオプションを削除します。 7\.2.1 以降では、既定のレジストラーがジェネリックをサポートしています。

このエラーは表示されなくなりました (コマンドライン引数 `--unsupported--enable-generics-in-registrar` は mtouch から削除されています)。

<a name="MT0023" />

### <a name="mt0023-application-name-exe-conflicts-with-another-user-assembly"></a>MT0023: アプリケーション名 ' * .exe ' が別のユーザーアセンブリと競合しています。

実行可能アセンブリの名前とアプリケーション名は、アプリ内の dll の名前と一致することはできません。 実行可能ファイルの名前を変更してください。

<a name="MT0024" />

### <a name="mt0024-could-not-find-required-file-"></a>MT0024: 必要なファイル ' * ' が見つかりませんでした。

<a name="MT0025" />

### <a name="mt0025-no-sdk-version-was-provided-please-add---sdkxy-to-specify-which-ios-sdk-should-be-used-to-build-your-application"></a>MT0025: SDK バージョンが指定されていません。 アプリケーションのビルドに使用する iOS SDK を指定するには、`--sdk=X.Y` を追加してください。

<a name="MT0026" />

### <a name="mt0026-could-not-parse-the-command-line-argument--"></a>MT0026: コマンドライン引数 '\*' を解析できませんでした: \*

<a name="MT0027" />

### <a name="mt0027-the-options--and--are-not-compatible"></a>MT0027: オプション '\*' と '\*' は互換性がありません。

<a name="MT0028" />

### <a name="mt0028-cannot-enable-pie--pie-when-targeting-ios-41-or-earlier-please-disable-pie--piefalse-or-set-the-deployment-target-to-at-least-ios-42"></a>MT0028: iOS 4.1 以前を対象とする場合、円 (-円) を有効にすることはできません。 円 (-円: false) を無効にするか、デプロイターゲットを iOS 4.2 以降に設定してください

<a name="MT0029" />

### <a name="mt0029-repl---enable-repl-is-only-supported-in-the-simulator---sim"></a>MT0029: REPL (--enable-repl) はシミュレーター (--sim) でのみサポートされています。

REPL は、シミュレーター用にビルドする場合にのみサポートされます。 これは、`--enable-repl` を mtouch に渡す場合は、`--sim`も渡す必要があることを意味します。

<a name="MT0030" />

### <a name="mt0030-the-executable-name--and-the-app-name--are-different-this-may-prevent-crash-logs-from-getting-symbolicated-properly"></a>MT0030: 実行可能ファイル名 (\*) とアプリ名 (\*) が異なります。これにより、クラッシュログが付加を適切に取得できなくなる可能性があります。

Xcode symbolicates (メモリアドレスを関数名とファイル/行番号に変換する) を実行すると、実行可能ファイルとアプリの名前が異なる場合 (拡張子なし)、プロセスが失敗することがあります。

この問題を解決するには、プロジェクトのビルド/iOS アプリケーションオプションで [アプリケーション名] を変更するか、プロジェクトの [ビルド/出力] オプションで [アセンブリ名] を変更します。

<a name="MT0031" />

### <a name="mt0031-the-command-line-arguments---enable-background-fetch-and---launch-for-background-fetch-require---launchsim-too"></a>MT0031: コマンドライン引数 '--enable-background-fetch ' と '--launch '--launch ' には '--launchsim ' が必要です。

<a name="MT0032" />

### <a name="mt0032-the-option---debugtrack-is-ignored-unless---debug-is-also-specified"></a>MT0032: オプション '--debugtrack ' は、'--debug ' も指定されている場合を除き、無視されます。

<a name="MT0033" />

### <a name="mt0033-a-xamarinios-project-must-reference-either-monotouchdll-or-xamariniosdll"></a>MT0033: Xamarin. iOS プロジェクトは、monotouch.dialog または Xamarin を参照する必要があります。

<a name="MT0034" />

### <a name="mt0034-cannot-include-both-monotouchdll-and-xamariniosdll-in-the-same-xamarinios-project----is-referenced-explicitly-while--is-referenced-by-"></a>MT0034: 同じ Xamarin に ' monotouch.dialog ' と ' ' の両方を含めることはできません。 iOS プロジェクト-'\*' は明示的に参照されていますが、'\*' は ' * ' によって参照されています。

<!-- MT0035 unused -->

<a name="MT0036" />

### <a name="mt0036-cannot-launch-a--simulator-for-a--app-please-enable-the-correct-architectures-in-your-projects-ios-build-options-advanced-page"></a>MT0036: \* アプリの \* シミュレーターを起動できません。 プロジェクトの iOS ビルドオプション ([詳細設定] ページ) で、正しいアーキテクチャを有効にしてください。

<a name="MT0037" />

### <a name="mt0037-monotouchdll-is-not-64-bit-compatible-either-reference-xamariniosdll-or-do-not-build-for-a-64-bit-architecture-arm64-andor-x86_64"></a>MT0037: monotouch.dialog は64ビット互換ではありません。 ARM64 を参照するか、64ビットアーキテクチャ (および/また x86_64 はその両方) 用にビルドしないようにします。

<a name="MT0038" />

### <a name="mt0038-the-old-registrars---registraroldstaticolddynamic-are-not-supported-when-referencing-xamariniosdll"></a>MT0038: 古いレジストラー (--レジストラー: oldstatic | oldstatic) は、Xamarin を参照するときにはサポートされていません。

<a name="MT0039" />

### <a name="mt0039-applications-targeting-armv6-cannot-reference-xamariniosdll"></a>MT0039: ARMv6 を対象とするアプリケーションは、Xamarin. iOS .dll を参照できません。

<a name="MT0040" />

### <a name="mt0040-could-not-find-the-assembly--referenced-by-"></a>MT0040: '\*' によって参照されているアセンブリ '\*' が見つかりませんでした。

<a name="MT0041" />

### <a name="mt0041-cannot-reference-both-monotouchdll-and-xamariniosdll"></a>MT0041: ' monotouch.dialog ' と ' ' の両方を参照することはできません。

<a name="MT0042" />

### <a name="mt0042-no-reference-to-either-monotouchdll-or-xamariniosdll-was-found-a-reference-to-monotouchdll-will-be-added"></a>MT0042: monotouch.dialog またはへの参照が見つかりませんでした。 Monotouch.dialog への参照が追加されます。

<a name="MT0043" />

### <a name="mt0043-the-boehm-garbage-collector-is-currently-not-supported-when-referencing-xamariniosdll-the-sgen-garbage-collector-has-been-selected-instead"></a>MT0043: Boehm ガベージコレクターは、' Xamarin. iOS. .dll ' を参照するときに現在サポートされていません。 代わりに、SGen ガベージコレクターが選択されています。

統合プロジェクトでは、SGen ガベージコレクターのみがサポートされています。 Boehm をガベージコレクターとして指定する追加の mtouch フラグがないことを確認します。

<a name="MT0044" />

### <a name="mt0044---listsim-is-only-supported-with-xcode-60-or-later"></a>MT0044:--listsim は Xcode 6.0 以降でのみサポートされています。

新しい Xcode バージョンをインストールします。

<a name="MT0045" />

### <a name="mt0045---extension-is-only-supported-when-using-the-ios-80-or-later-sdk"></a>MT0045:--拡張機能は、iOS 8.0 (またはそれ以降) の SDK を使用している場合にのみサポートされます。

<!-- MT0046 is not reported anymore -->

<a name="MT0047" />

### <a name="mt0047-the-minimum-deployment-target-for-unified-applications-is-511-the-current-deployment-target-is--please-select-a-newer-deployment-target-in-your-projects-ios-application-options"></a>MT0047: 統合アプリケーションの最小配置ターゲットは5.1.1 で、現在の配置ターゲットは ' * ' です。 プロジェクトの iOS アプリケーションオプションで、新しい配置ターゲットを選択してください。

<!-- MT0048 is not reported anymore -->

<a name="MT0049" />

### <a name="mt0049-framework-is-supported-only-if-deployment-target-is-80-or-later--features-might-not-work-correctly"></a>MT0049: \*は、配置ターゲットが8.0 以降の場合にのみサポートされます。 \* 機能が正しく機能しない可能性があります。

指定されたフレームワークは、デプロイターゲットが参照している iOS バージョンではサポートされていません。 デプロイターゲットを新しい iOS バージョンに更新するか、指定されたフレームワークの使用をアプリから削除します。

<!-- MT0050 is not reported anymore -->

<a name="MT0051" />

### <a name="mt0051-xamarinios--requires-xcode-50-or-later-the-current-xcode-version-found-in--is-"></a>MT0051: Xamarin \* には Xcode 5.0 以降が必要です。 現在の Xcode バージョン (\*) が \*。

新しい Xcode をインストールします。

<a name="MT0052" />

### <a name="mt0052-no-command-specified"></a>MT0052: コマンドが指定されていません。

Mtouch に対するアクションが指定されていません。

<!-- 0053 is used by mmp -->

<a name="MT0054" />

### <a name="mt0054-unable-to-canonicalize-the-path--"></a>MT0054: パス '\*' を正規化できません: \*

これは内部エラーです。 このエラーが表示される場合は、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT0055" />

### <a name="mt0055-the-xcode-path--does-not-exist"></a>MT0055: Xcode パス ' * ' が存在しません。

`--sdkroot` を使用して渡された Xcode パスが存在しません。 有効なパスを指定してください。

<a name="MT0056" />

### <a name="mt0056-cannot-find-xcode-in-the-default-location-applicationsxcodeapp-please-install-xcode-or-pass-a-custom-path-using---sdkroot-path"></a>MT0056: Xcode が既定の場所 (/Applications/Xcode.app) に見つかりません。 Xcode をインストールするか、--sdkroot \<path > を使用してカスタムパスを渡してください。

<a name="MT0057" />

### <a name="mt0057-cannot-determine-the-path-to-xcodeapp-from-the-sdk-root--please-specify-the-full-path-to-the-xcodeapp-bundle"></a>MT0057: sdk ルート ' * ' から Xcode へのパスを特定できません。 Xcode バンドルへの完全なパスを指定してください。

`--sdkroot` を使用して渡されたパスには、有効な Xcode アプリが指定されていません。 Xcode アプリへのパスを指定してください。

<a name="MT0058" />

### <a name="mt0058-the-xcodeapp--is-invalid-the-file--does-not-exist"></a>MT0058: Xcode '\*' が無効です (ファイル '\*' は存在しません)。

`--sdkroot` を使用して渡されたパスには、有効な Xcode アプリが指定されていません。 Xcode アプリへのパスを指定してください。

<a name="MT0059" />

### <a name="mt0059-could-not-find-the-currently-selected-xcode-on-the-system-"></a>MT0059: 現在選択されている Xcode がシステムで見つかりませんでした: *

<a name="MT0060" />

### <a name="mt0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned--but-that-directory-does-not-exist"></a>MT0060: 現在選択されている Xcode がシステムで見つかりませんでした。 ' xcode--print-path ' から ' * ' が返されましたが、そのディレクトリは存在しません。

<a name="MT0061" />

### <a name="mt0061-no-xcodeapp-specified-using---sdkroot-using-the-system-xcode-as-reported-by-xcode-select---print-path-"></a>MT0061: Xcode が指定されていません (--sdkroot)。 ' Xcode ': * によって報告されたシステム Xcode を使用しています。

これは、何も指定されていないために使用される Xcode を説明する情報警告です。

<a name="MT0062" />

### <a name="mt0062-no-xcodeapp-specified-using---sdkroot-or-xcode-select---print-path-using-the-default-xcode-instead-"></a>MT0062: Xcode が指定されていません (--sdkroot または ' Xcode ' を使用)。代わりに、既定の Xcode を使用します。 *

これは、何も指定されていないために使用される Xcode を説明する情報警告です。

<a name="MT0063" />

### <a name="mt0063-cannot-find-the-executable-in-the-extension--no-cfbundleexecutable-entry-in-its-infoplist"></a>MT0063: 拡張機能 * で実行可能ファイルが見つかりません (CFBundleExecutable エントリにはありません)。

すべての情報 plist には、(CFBundleExecutable エントリを使用した) 実行可能ファイルが必要ですが、ビルド中にエントリが自動的に生成される必要があります。

これは通常、Xamarin のバグであることを示します。テストケースを使用して、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT0064" />

### <a name="mt0064-xamarinios-only-supports-embedded-frameworks-with-unified-projects"></a>MT0064: Xamarin は、統合されたプロジェクトを含む埋め込みフレームワークのみをサポートしています。

Xamarin は、Unified API を使用する場合にのみ、埋め込みフレームワークをサポートします。Unified API を使用するようにプロジェクトを更新してください。

<a name="MT0065" />

### <a name="mt0065-xamarinios-only-supports-embedded-frameworks-when-deployment-target-is-at-least-80-current-deployment-target--embedded-frameworks-"></a>MT0065: Xamarin は、配置ターゲットが8.0 以上の場合にのみ、埋め込みフレームワークをサポートします (現在の配置ターゲット: \* 埋め込みフレームワーク: \*)

Xamarin では、配置ターゲットが8.0 以上の場合にのみ、埋め込みフレームワークがサポートされます (以前のバージョンの iOS では埋め込みフレームワークがサポートされていないため)。

プロジェクトの情報の配置ターゲットを8.0 以降に更新してください。

<a name="MT0066" />

### <a name="mt0066-invalid-build-registrar-assembly-"></a>MT0066: 無効なビルドレジストラーアセンブリ: *

これは通常、Xamarin のバグであることを示します。テストケースを使用して、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT0067" />

### <a name="mt0067-invalid-registrar-"></a>MT0067: 無効なレジストラー: *

これは通常、Xamarin のバグであることを示します。テストケースを使用して、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT0068" />

### <a name="mt0068-invalid-value-for-target-framework-"></a>MT0068: ターゲットフレームワークの値が無効です: *。

--Target-framework 引数を使用して無効なターゲットフレームワークが渡されました。 有効なターゲットフレームワークを指定してください。

<a name="MT0069" />

<!--### MT0069: currently unused -->

<a name="MT0070" />

### <a name="mt0070-invalid-target-framework--valid-target-frameworks-are-"></a>MT0070: 無効なターゲットフレームワーク: *。 有効なターゲットフレームワークは次のとおりです: *。

--Target-framework 引数を使用して無効なターゲットフレームワークが渡されました。 有効なターゲットフレームワークを指定してください。

<a name="MT0071" />

### <a name="mt0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinios-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT0071: 不明なプラットフォーム: *。 これは通常、Xamarin のバグであることを示します。テストケースがある http://bugzilla.xamarin.com でバグレポートをファイルに登録してください。

これは通常、Xamarin のバグであることを示します。テストケースを使用して、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT0072" />

### <a name="mt0072-extensions-are-not-supported-for-the-platform-"></a>MT0072: 拡張機能は、プラットフォーム ' * ' ではサポートされていません。

これは通常、Xamarin のバグであることを示します。テストケースを使用して、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT0073" />

### <a name="mt0073-xamarinios--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a>MT0073: Xamarin \* は \* の配置ターゲットをサポートしていません (最小値は \*)。 プロジェクトの情報で、新しい配置ターゲットを選択してください。

最小配置ターゲットは、エラーメッセージに指定されているものです。プロジェクトの情報で、新しい配置ターゲットを選択してください。

配置ターゲットを更新できない場合は、以前のバージョンの Xamarin. iOS を使用してください。

<a name="MT0074" />

### <a name="mt0074-xamarinios--does-not-support-a-minimum-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinios"></a>MT0074: Xamarin \* は \* の最小配置ターゲットをサポートしていません (最大値は \*)。 プロジェクトの情報で古い配置ターゲットを選択するか、新しいバージョンの Xamarin. iOS にアップグレードしてください。

Xamarin iOS では、最小配置ターゲットをこのバージョンの Xamarin のバージョンよりも高いバージョンに設定することはサポートされていません。 iOS は用に構築されました。

プロジェクトの情報で古い最小配置ターゲットを選択するか、新しいバージョンの Xamarin. iOS にアップグレードしてください。

<a name="MT0075" />

### <a name="mt0075-invalid-architecture--for--projects-valid-architectures-are-"></a>MT0075: \* プロジェクトのアーキテクチャ '\*' が無効です。 有効なアーキテクチャは次のとおりです: \*

無効なアーキテクチャが指定されました。 アーキテクチャが有効であることを確認してください。

<a name="MT0076" />

### <a name="mt0076-no-architecture-specified-using-the---abi-argument-an-architecture-is-required-for--projects"></a>MT0076: (--abi 引数を使用して) アーキテクチャが指定されていません。 \* プロジェクトにはアーキテクチャが必要です。

これは通常、Xamarin のバグであることを示します。テストケースを使用して、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT0077" />

### <a name="mt0077-watchos-projects-must-be-extensions"></a>MT0077: WatchOS プロジェクトは、拡張機能である必要があります。

これは通常、Xamarin のバグであることを示します。テストケースを使用して、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT0078" />

### <a name="mt0078-incremental-builds-are-enabled-with-a-deployment-target--80-currently--this-is-not-supported-the-resulting-application-will-not-launch-on-ios-9-so-the-deployment-target-will-be-set-to-80"></a>MT0078: インクリメンタルビルドは、配置ターゲット < 8.0 (現在は *) で有効になっています。 これはサポートされていません (結果として得られるアプリケーションは iOS 9 では起動されません)。そのため、デプロイターゲットは8.0 に設定されます。

これは、インクリメンタルビルドが正常に動作するように、このビルドの配置ターゲットが8.0 に設定されていることを通知する警告です。

インクリメンタルビルドは、配置ターゲットが8.0 以上の場合にのみサポートされます (それ以外の場合、結果として得られるアプリケーションは iOS 9 で起動されません)。

<a name="MT0079" />

### <a name="mt0079-the-recommended-xcode-version-for-xamarinios--is-xcode--or-later-the-current-xcode-version-found-in--is-"></a>MT0079: Xcode の推奨されるバージョン \* は、Xcode \* 以降です。 現在の Xcode バージョン (\*) が \*。

これは、Xcode の現在のバージョンが、このバージョンの Xamarin の Xcode の推奨バージョンではないことを通知する警告です。

最適な動作を保証するには、Xcode をアップグレードしてください。

<a name="MT0080" />

### <a name="mt0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MT0080: NewRefCount を無効にしています。--new-refcount: false は非推奨とされます。

これは、新しい refcount を無効にする要求が無視されたことを通知する警告です。

新しい refcount 機能はすべてのプロジェクトに必須となりました。したがって、これを無効にすることはできません。

<a name="MT0081" />

### <a name="mt0081-the-command-line-argument---download-crash-report-also-requires---download-crash-report-to"></a>MT0081: コマンドライン引数--download-crash-report------------

<a name="MT0082" />

### <a name="mt0082-repl---enable-repl-is-only-supported-when-linking-is-not-used---nolink"></a>MT0082: REPL (--enable-repl) は、リンクが使用されていない場合にのみサポートされます (--nolink)。

<a name="MT0083" />

### <a name="mt0083-asm-only-bitcode-is-not-supported-on-watchos-use-either---bitcodemarker-or---bitcodefull"></a>MT0083: Asm のみの bitcode は、watchOS ではサポートされていません。 --Bitcode: marker または--bitcode: full のいずれかを使用します。

<a name="MT0084" />

### <a name="mt0084-bitcode-is-not-supported-in-the-simulator-do-not-pass---bitcode-when-building-for-the-simulator"></a>MT0084: Bitcode はシミュレーターではサポートされていません。 シミュレーター用にビルドする場合は、--bitcode を渡さないでください。

<a name="MT0085" />

### <a name="mt0085-no-reference-to--was-found-it-will-be-added-automatically"></a>MT0085: ' * ' への参照が見つかりませんでした。 自動的に追加されます。

<a name="MT0086" />

### <a name="mt0086-a-target-framework---target-framework-must-be-specified-when-building-for-tvos-or-watchos"></a>MT0086: TVOS または WatchOS のビルド時に、ターゲットフレームワーク (--target-framework) を指定する必要があります。

これは、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT0087" />

### <a name="mt0087-incremental-builds---fastdev-is-not-supported-with-the-boehm-gc-incremental-builds-will-be-disabled"></a>MT0087: インクリメンタルビルド (--fastdev) は Boehm GC ではサポートされていません。 インクリメンタルビルドは無効になります。

<a name="MT0088" />

### <a name="mt0088-the-gc-must-be-in-cooperative-mode-for-watchos-apps-please-remove-the---coopfalse-argument-to-mtouch"></a>MT0088: GC は watchOS アプリの協調モードである必要があります。 Mtouch の--co-op: false 引数を削除してください。

<a name="MT0089" />

### <a name="mt0089-the-option--cannot-take-the-value--when-cooperative-mode-is-enabled-for-the-gc"></a>MT0089: 連携モードが GC に対して有効になっている場合、オプション '\*' は値 '\*' を取ることができません。

<a name="MT0091" />

### <a name="mt0091-this-version-of-xamarinios-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-set-the-managed-linker-behaviour-to-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MT0091: このバージョンの Xamarin. iOS には \* SDK (Xcode \*に付属) が必要です。 Xcode をアップグレードして必要なヘッダーファイルを取得するか、マネージリンカーの動作を設定して、フレームワーク Sdk のみをリンクします (新しい Api を避けるため)。

Xamarin iOS では、エラーメッセージに示されている SDK バージョンのヘッダーファイルを使用して、アプリケーションをビルドする必要があります。 このエラーを修正するには、Xcode をアップグレードして必要な SDK を取得することをお勧めします。これには、必要なすべてのヘッダーファイルが含まれます。 複数のバージョンの Xcode がインストールされている場合、または既定以外の場所で Xcode を使用する場合は、IDE の設定で正しい Xcode の場所を設定してください。

別の方法として、マネージリンカーを有効にすることもできます。 これにより、ほとんどの場合、ヘッダーファイルが存在しない (または不完全な) 新しい API を含む、未使用の API が削除されます。 ただし、Xcode が提供する API よりも新しい SDK で導入された API がプロジェクトで使用されている場合、これは機能しません。

最後の straw ソリューションでは、プロジェクトに必要な SDK をサポートする古いバージョンの Xamarin. iOS を使用します。

<!-- MT0092 used by mlaunch -->

<a name="MT0093" />

### <a name="mt0093-could-not-find-mlaunch"></a>MT0093: ' mlaunch ' が見つかりませんでした。

<!-- MT0094 is not reported anymore -->

<a name="MT0095" />

### <a name="mt0095-aot-files-could-not-be-copied-to-the-destination-directory-dest-error"></a>MT0095: Aot ファイルを宛先ディレクトリ {dest} にコピーできませんでした: {error}

<a name="MT0096" />

### <a name="mt0096-no-reference-to-xamariniosdll-was-found"></a>MT0096: Xamarin. iOS .dll への参照が見つかりませんでした。

<!-- MT0097: used by mmp -->
<!-- MT0098: used by mmp -->

<a name="MT0099" />

### <a name="mt0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsbugzillaxamarincom"></a>MT0099: 内部エラー *。 テストケース (https://bugzilla.xamarin.com)を含むバグレポートをファイルに登録してください。

このエラーメッセージは、Xamarin の内部整合性チェックに失敗した場合に報告されます。

これは通常、Xamarin のバグであることを示します。テストケースを使用して、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT0100" />

### <a name="mt0100-invalid-assembly-build-target--please-file-a-bug-report-with-a-test-case-httpsbugzillaxamarincom"></a>MT0100: アセンブリビルドターゲットが無効です: ' * '。 テストケース (https://bugzilla.xamarin.com)を含むバグレポートをファイルに登録してください。

このエラーメッセージは、Xamarin の内部整合性チェックに失敗した場合に報告されます。

これは通常、Xamarin のバグであることを示します。テストケースを使用して、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT0101" />

### <a name="mt0101-the-assembly--is-specified-multiple-times-in---assembly-build-target-arguments"></a>MT0101: アセンブリ ' * ' が--assembly-build-target 引数で複数回指定されています。

エラーメッセージに示されているアセンブリは、--assembly-build-target 引数で複数回指定されています。 各アセンブリが1回だけ記述されていることを確認してください。

<a name="MT0102" />

### <a name="mt0102-the-assemblies--and--have-the-same-target-name--but-different-targets--and-"></a>MT0102: アセンブリ '\*' と '\*' のターゲット名 ('\*') は同じですが、ターゲット ('\*' と ' * ') は異なります。

エラーメッセージに示されているアセンブリのビルドターゲットが競合しています。

例 :

```
  --assembly-build-target:Assembly1.dll=framework=MyBinary --assembly-build-target:Assembly2.dll=dynamiclibrary=MyBinary
```

この例では、同じ make (`MyBinary`) を使用して、ダイナミックライブラリとフレームワークの両方を作成しようとしています。

<a name="MT0103" />

### <a name="mt0103-the-static-object--contains-more-than-one-assembly--but-each-static-object-must-correspond-with-exactly-one-assembly"></a>MT0103: 静的オブジェクト '\*' に複数のアセンブリ ('\*') が含まれていますが、各静的オブジェクトは1つのアセンブリにのみ対応しなければなりません。

エラーメッセージに示されているアセンブリはすべて、1つの静的オブジェクトにコンパイルされます。 これは許可されていません。すべてのアセンブリを別の静的オブジェクトにコンパイルする必要があります。

例 :

```
--assembly-build-target:Assembly1.dll=staticobject=MyBinary --assembly-build-target:Assembly2.dll=staticobject=MyBinary
```

この例では、2つのアセンブリ (`Assembly1.dll` と `Assembly2.dll`) で構成される静的オブジェクト (`MyBinary`) を構築しようとしますが、これは許可されていません。

<a name="MT0105" />

### <a name="mt0105-no-assembly-build-target-was-specified-for-"></a>MT0105: ' * ' にアセンブリビルドターゲットが指定されていません。

`--assembly-build-target`を使用してアセンブリビルドターゲットを指定する場合、アプリ内のすべてのアセンブリにビルドターゲットが割り当てられている必要があります。

このエラーは、エラーメッセージに示されているアセンブリにアセンブリビルドターゲットが割り当てられていない場合に報告されます。

詳細については、`--assembly-build-target` に関するドキュメントを参照してください。

<a name="MT0106" />

### <a name="mt0106-the-assembly-build-target-name--is-invalid-the-character--is-not-allowed"></a>MT0106: アセンブリビルドターゲット名 '\*' が無効です: 文字 '\*' は許可されていません。

アセンブリビルドターゲット名は、有効なファイル名である必要があります。

たとえば、次の値を指定すると、このエラーが発生します。

```
--assembly-build-target:Assembly1.dll=staticobject=my/path.o
```

では、ディレクトリの区切り文字が原因で `my/path.o` が有効なファイル名ではありません。

<a name="MT0107" />

### <a name="mt0107-the-assemblies--have-different-custom-llvm-optimizations--which-is-not-allowed-when-they-are-all-compiled-to-a-single-binary"></a>MT0107: アセンブリ '\*' には異なるカスタム LLVM の最適化 (\*) があります。これは、すべてが1つのバイナリにコンパイルされるときには許可されません。

<a name="MT0108" />

### <a name="mt0108-the-assembly-build-target--did-not-match-any-assemblies"></a>MT0108: アセンブリビルドターゲット ' * ' がアセンブリと一致しませんでした。

<a name="MT0109" />

### <a name="mt0109-the-assembly-0-was-loaded-from-a-different-path-than-the-provided-path-provided-path-1-actual-path-2"></a>MT0109: アセンブリ '{0}' は、指定されたパスとは異なるパスから読み込まれました (指定されたパス: {1}、実際のパス: {2})。

これは、アプリケーションによって参照されるアセンブリが、要求された場所とは別の場所から読み込まれたことを示す警告です。

これは、アプリが同じ名前を持つ複数のアセンブリを参照していて、異なる場所から、予期しない結果につながる可能性があることを意味する可能性があります (最初のアセンブリのみが使用されます)。

<a name="MT0110" />

### <a name="mt0110-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-third-party-binding-libraries-and-that-compiles-to-bitcode"></a>MT0110: このバージョンの Xamarin では、インクリメンタルビルドは無効になっています。 iOS では、サードパーティ製のバインドライブラリを含み、bitcode にコンパイルされるプロジェクトのインクリメンタルビルドはサポートされていません。

このバージョンの Xamarin では、インクリメンタルビルドは無効になっています。 iOS では、サードパーティのバインドライブラリを含むプロジェクト内のインクリメンタルビルドと、bitcode (tvOS および watchOS プロジェクト) にコンパイルされるビルドはサポートされていません。

操作は必要ありません。このメッセージは純粋な情報です。

詳細については、バグ #[51710](https://bugzilla.xamarin.com/show_bug.cgi?id=51710)を参照してください。

この警告は、今後報告されません。

<a name="MT0111" />

### <a name="mt0111-bitcode-has-been-enabled-because-this-version-of-xamarinios-does-not-support-building-watchos-projects-using-llvm-without-enabling-bitcode"></a>MT0111: このバージョンの Xamarin では、Bitcode が有効になっています。 bitcode を有効にしないと、LLVM を使用した watchOS プロジェクトのビルドはサポートされていません。

Bitcode が自動的に有効になりました。このバージョンの Xamarin では、bitcode を有効にしないと、LLVM を使用した watchOS プロジェクトのビルドはサポートされていません。

操作は必要ありません。このメッセージは純粋な情報です。

詳細については、バグ #[51634](https://bugzilla.xamarin.com/show_bug.cgi?id=51634)を参照してください。

<a name="MT0112" />

### <a name="mt0112-native-code-sharing-has-been-disabled-because-"></a>MT0112: ネイティブコード共有が無効になっています。理由: *

コード共有を無効にできる理由は複数あります。

- コンテナーアプリのデプロイターゲットは iOS 8.0 (*) より前のものです。

ネイティブコード共有は、ios 8.0 で導入されたユーザーフレームワークを使用して実装されるため、iOS 8.0 が必要です。

- コンテナーアプリには I18N アセンブリ (*) が含まれているためです。

コンテナーアプリに I18N アセンブリが含まれている場合、現在、ネイティブコード共有はサポートされていません。

- コンテナーアプリには、マネージリンカー (*) のカスタム xml 定義が含まれているためです。

マネージリンカーに対してカスタム xml 定義を使用するプロジェクトでは、ネイティブコード共有が必要です。

<a name="MT0113" />

### <a name="mt0113-native-code-sharing-has-been-disabled-for-the-extension--because-"></a>MT0113: 拡張機能 ' * ' でネイティブコード共有が無効になっています。理由: *。

- bitcode オプションは、コンテナーアプリ (\*) と拡張機能 (\*) で異なります。

  ネイティブコード共有を行うには、コードを共有するプロジェクト間で bitcode オプションが一致している必要があります。

- --assembly-build-target オプションは、コンテナーアプリ (\*) と拡張機能 (\*) で異なります。

  ネイティブコード共有では、--assembly-build-target オプションがコードを共有するプロジェクト間で同一である必要があります。

  この状態は、すべてのプロジェクトでインクリメンタルビルドが有効になっていないか、無効になっている場合に発生する可能性があります。

- I18N アセンブリがコンテナーアプリ (\*) と拡張機能 (\*) で異なるためです。

  現在、ネイティブコード共有は、I18N アセンブリを含む拡張機能ではサポートされていません。

- AOT コンパイラの引数がコンテナーアプリ (\*) と拡張機能 (\*) で異なるためです。

  ネイティブコード共有を行うには、コードを共有するプロジェクト間で AOT コンパイラへの引数が異なる必要があります。

- AOT コンパイラの他の引数は、コンテナーアプリ (\*) と拡張機能 (\*) で異なります。

  ネイティブコード共有を行うには、コードを共有するプロジェクト間で AOT コンパイラへの引数が異なる必要があります。

  この状態は、' すべての32ビット浮動小数点演算を64ビット浮動小数点演算として実行すると、プロジェクト間で異なる場合に発生します。

- LLVM は、コンテナーアプリ (\*) と拡張機能 (\*) の両方で有効または無効になっているためです。

  ネイティブコード共有を使用するには、コードを共有するすべてのプロジェクトで LLVM が有効または無効になっている必要があります。

- マネージリンカーの設定は、コンテナーアプリ (\*) と拡張機能 (\*) で異なります。

  ネイティブコード共有を使用するには、コードを共有するすべてのプロジェクトでマネージリンカー設定が同一である必要があります。

- マネージリンカーのスキップされたアセンブリは、コンテナーアプリ (\*) と拡張機能 (\*) で異なります。

  ネイティブコード共有を使用するには、コードを共有するすべてのプロジェクトでマネージリンカー設定が同一である必要があります。

- 拡張機能には、マネージリンカー (*) のカスタム xml 定義が含まれているためです。

  マネージリンカーに対してカスタム xml 定義を使用するプロジェクトでは、ネイティブコード共有が必要です。

- コンテナーアプリは ABI * (この ABI 用にビルドされています) 用にビルドされません。

  ネイティブコード共有を使用するには、コンテナーアプリが、アプリ拡張機能をビルドするすべてのアーキテクチャ用にビルドする必要があります。

  例: この状態は、ARM64 + ARMv7 の拡張機能をビルドする場合に発生しますが、コンテナーアプリは ARM64 に対してのみビルドします。

- コンテナーアプリは ABI \*用にビルドしているため、拡張機能の ABI (\*) と互換性がありません。

  ネイティブコード共有を使用するには、すべてのプロジェクトがまったく同じ API に対してビルドされている必要があります。

  例: この状態は、ARMv7 + llvm + thumb2 の拡張機能がビルドされた場合に発生しますが、コンテナーアプリは ARMv7 + llvm のみを対象としてビルドされます。

- コンテナーアプリが '\*' からアセンブリ '\*' を参照していて、拡張機能が ' * ' から別のバージョンを参照しているためです。

  ネイティブコード共有を使用するには、コードを共有するすべてのプロジェクトで、すべてのアセンブリに同じバージョンが使用されている必要があります。

<!-- MT0114: used by mmp -->

<a name="MT0115" />

### <a name="mt0115-it-is-recommended-to-reference-dynamic-symbols-using-code---dynamic-symbol-modecode-when-bitcode-is-enabled"></a>MT0115: bitcode が有効になっている場合は、コードを使用して動的シンボルを参照することをお勧めします (--dynamic-symbol-mode = code)。

多くの場合、Xamarin. iOS プロジェクトはネイティブシンボルを動的に参照します。つまり、ネイティブリンカーは、これらのシンボルが使用されていることを認識しないため、ネイティブリンカーがネイティブシンボルを削除する可能性があります。

通常、Xamarin は、このようなシンボルを保持するようにネイティブリンカーに要求しますが (`-u symbol` リンカーフラグを使用します)、bitcode のコンパイル時には、ネイティブリンカーが `-u` フラグを受け入れません。

Xamarin には、代替のソリューションが実装されています。これらのシンボルを参照する追加のネイティブコードが生成されるため、ネイティブリンカーはこれらのシンボルが使用されていることを確認します。 これは、bitcode にコンパイルするときに自動的に実行されます。

`--dynamic-symbol-mode=linker` が mtouch に渡されると、この代替ソリューションは無効になり、Xamarin はネイティブリンカーに `-u` を渡そうとします。 ほとんどの場合、ネイティブリンカーエラーが発生します。

解決策として、プロジェクトのビルドオプションの追加の mtouch 引数から `--dynamic-symbol-mode=linker` 引数を削除します。

<!-- 0116 - 0124: free to use -->

<a name="MT0116" />

### <a name="mt0116-invalid-architecture-arch-32-bit-architectures-are-not-supported-when-deployment-target-is-11-or-later-make-sure-the-project-does-not-build-for-a-32-bit-architecture"></a>MT0116: 無効なアーキテクチャ: {arch}。 32ビットアーキテクチャは、配置ターゲットが11以降の場合はサポートされません。 プロジェクトが32ビットアーキテクチャ用にビルドされていないことを確認します。

iOS 11 には32ビットアプリケーションのサポートが含まれていないため、デプロイターゲットが iOS 11 以降の場合、32ビットアプリケーションのビルドはサポートされていません。

プロジェクトの iOS ビルドオプションのターゲットアーキテクチャを arm64 に変更するか、プロジェクトの情報 plist の配置ターゲットを以前の iOS バージョンに変更します。

<a name="MT0117" />

### <a name="mt0117-cant-launch-a-32-bit-app-on-a-simulator-that-only-supports-64-bit"></a>MT0117:64 ビットのみをサポートするシミュレーターで32ビットアプリを起動することはできません。

<a name="MT0118" />

### <a name="mt0118-aot-files-could-not-be-found-at-the-expected-directory-msymdir"></a>MT0118: Aot ファイルが、予期されたディレクトリ ' {msymdir} ' で見つかりませんでした。

<!-- 0119 - 0123: free to use -->

<a name="MT0123" />

### <a name="mt0123-the-executable-assembly--does-not-reference-"></a>MT0123: 実行可能アセンブリ \* が \*を参照していません。

実行可能アセンブリに、プラットフォームアセンブリ (TVOS/WatchOS) への参照が見つかりませんでした。このファイルは、実行可能アセンブリに含まれていません。

これは通常、プラットフォームアセンブリからのものをすべて使用するコードが実行可能プロジェクトにない場合に発生します。たとえば、空の Main メソッド (および他のコードは含まれません) では、次のエラーが表示されます。

```csharp
class Program {
    void Main (string[] args)
    {
    }
}
```

プラットフォームアセンブリから API を使用すると、次のエラーが解決されます。

```csharp
class Program {
    void Main (string[] args)
    {
        System.Console.WriteLine (typeof (UIKit.UIWindow));
    }
}
```

<a name="MT0124" />

### <a name="mt0124-could-not-set-the-current-language-to-lang-according-to-langlang-exception"></a>MT0124: 現在の言語を ' {lang} ' に設定できませんでした (LANG = {LANG}): {exception}

これは、現在の言語をエラーメッセージの言語に設定できなかったことを示す警告です。

現在の言語は、既定でシステム言語に設定されています。

<a name="MT0125" />

### <a name="mt0125-the---assembly-build-target-command-line-argument-is-ignored-in-the-simulator"></a>MT0125: シミュレーターでは、--assembly-build-target コマンドライン引数は無視されます。

操作は必要ありません。このメッセージは純粋な情報です。

<a name="MT0126" />

### <a name="mt0126-incremental-builds-have-been-disabled-because-incremental-builds-are-not-supported-in-the-simulator"></a>MT0126: インクリメンタルビルドは、シミュレーターではサポートされていないため、インクリメンタルビルドは無効になっています。

操作は必要ありません。このメッセージは純粋な情報です。

<a name="MT0127" />

### <a name="mt0127-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-more-than-one-third-party-binding-libraries"></a>MT0127: このバージョンの Xamarin では、インクリメンタルビルドは無効になっています。 iOS では、複数のサードパーティのバインドライブラリを含むプロジェクトのインクリメンタルビルドはサポートされていません。

このバージョンの Xamarin では、インクリメンタルビルドは自動的に無効になっています。 iOS では、複数のサードパーティ製バインドライブラリを正しく使用してプロジェクトを作成することはありません。

操作は必要ありません。このメッセージは純粋な情報です。

詳細については、バグ #[52727](https://bugzilla.xamarin.com/show_bug.cgi?id=52727)を参照してください。

<a name="MT0128" />

### <a name="mt0128-could-not-touch-the-file--"></a>MT0128: ファイル '\*' に触れることができませんでした: \*

ファイルにタッチするときにエラーが発生しました (部分的なビルドが正しく行われるようにするために行われます)。

この警告は無視される可能性があります。問題が発生した場合、 [GitHub](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題が発生すると、それが調査されます。

<a name="MT0135" />

### <a name="mt0135-did-not-link-system-framework-0-referenced-by-assembly-1-because-it-was-introduced-in-2-3-and-were-using-the-2-4-sdk"></a>MT0135: {2} {3}で導入され、{2} {4} SDK を使用しているため、システムフレームワーク '{0}' (アセンブリ '{1}' によって参照されています) をリンクしませんでした。

アプリケーションをビルドするには、Xamarin. iOS はシステムライブラリにリンクする必要があります。その一部は、エラーメッセージに示されている SDK のバージョンによって異なります。 以前のバージョンの SDK を使用しているため、これらの Api への呼び出しは実行時に失敗する可能性があります。

このエラーを修正するには、Xcode をアップグレードして必要な SDK を取得することをお勧めします。 複数のバージョンの Xcode がインストールされている場合、または既定以外の場所で Xcode を使用する場合は、IDE の設定で適切な Xcode の場所を設定してください。

または、マネージ[リンカー](https://docs.microsoft.com/xamarin/ios/deploy-test/linker)を有効にして、指定したライブラリを必要とする新しい api (ほとんどの場合は) を含む未使用の api を削除します。 ただし、Xcode で提供されているものより新しい SDK で導入された Api がプロジェクトに必要な場合、これは機能しません。

最後の straw ソリューションとして、これらの新しい Sdk がビルドプロセス中に存在する必要がない古いバージョンの Xamarin. iOS を使用します。

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx: プロジェクト関連のエラーメッセージ

### <a name="mt10xx-installer--mtouch"></a>MT10xx: インストーラー/mtouch

<!--
 MT1xxx file copy / symlinks (project related)
  MT10xx installer.cs / mtouch.cs
  -->

<a name="MT1001" />

### <a name="mt1001-could-not-find-an-application-at-the-specified-directory"></a>MT1001: 指定されたディレクトリにアプリケーションが見つかりませんでした

<a name="MT1002" />

### <a name="mt1002-could-not-create-symlinks-files-were-copied"></a>MT1002: シンボリックリンクを作成できませんでした。ファイルがコピーされました

<a name="MT1003" />

### <a name="mt1003-could-not-kill-the-application--you-may-have-to-kill-the-application-manually"></a>MT1003: アプリケーション ' * ' を強制終了できませんでした。 場合によっては、アプリケーションを手動で強制終了する必要があります。

<a name="MT1004" />

### <a name="mt1004-could-not-get-the-list-of-installed-applications"></a>MT1004: インストールされているアプリケーションの一覧を取得できませんでした。

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx: プロジェクト関連のエラーメッセージ

<a name="MT1005" />

### <a name="mt1005-could-not-kill-the-application--on-the-device----you-may-have-to-kill-the-application-manually"></a>MT1005: デバイス '\*' のアプリケーション '\*' を強制終了できませんでした: *-アプリケーションを手動で強制終了する必要がある場合があります。

<a name="MT1006" />

### <a name="mt1006-could-not-install-the-application--on-the-device--"></a>MT1006: デバイス '\*': * にアプリケーション '\*' をインストールできませんでした。

<a name="MT1007" />

### <a name="mt1007-failed-to-launch-the-application--on-the-device---you-can-still-launch-the-application-manually-by-tapping-on-it"></a>MT1007: デバイス '\*': * でアプリケーション '\*' を起動できませんでした。 アプリケーションをタップして手動で起動することもできます。

<a name="MT1008" />

### <a name="mt1008-failed-to-launch-the-simulator"></a>MT1008: シミュレーターを起動できませんでした

このエラーは、mtouch がシミュレーターを起動できなかった場合に報告されます。   これは、古いまたはデッドシミュレータープロセスが既に実行されていることが原因で発生する可能性があります。

Unix コマンドラインで発行された次のコマンドを使用して、スタックしたシミュレータープロセスを強制終了できます。

```bash
$ launchctl list|grep UIKitApplication|awk '{print $3}'|xargs launchctl remove
```

<a name="MT1009" />

### <a name="mt1009-could-not-copy-the-assembly--to--"></a>MT1009: アセンブリ '\*' を '\*' にコピーできませんでした: *

これは、Xamarin. iOS の特定のバージョンの既知の問題です。

この問題が発生した場合は、次の回避策を試してください。

```bash
sudo chmod 0644 /Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/*/*.mdb
```

ただし、この問題は最新バージョンの Xamarin. iOS で解決されているため、完全なバージョン情報とビルドログ出力を使用して、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT1010" />

### <a name="mt1010-could-not-load-the-assembly--"></a>MT1010: アセンブリ '\*' を読み込むことができませんでした: \*

<a name="MT1011" />

### <a name="mt1011-could-not-add-missing-resource-file-"></a>MT1011: 不足しているリソースファイルを追加できませんでした: ' * '

<a name="MT1012" />

### <a name="mt1012-failed-to-list-the-apps-on-the-device--"></a>MT1012: デバイス '\*' のアプリを一覧表示できませんでした: \*

<a name="MT1013" />

### <a name="mt1013-dependency-tracking-error-no-files-to-compare-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT1013: 依存関係の追跡エラー: 比較するファイルがありません。 テストケースがある http://bugzilla.xamarin.com でバグレポートをファイルに登録してください。

これは、Xamarin. iOS のバグを示しています。 テストケースを使用して、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT1014" />

### <a name="mt1014-failed-to-re-use-cached-version-of--"></a>MT1014: ' * ' のキャッシュされたバージョンを再利用できませんでした: *。

<a name="MT1015" />

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015: 実行可能ファイル '\*' を作成できませんでした: \*

<a name="MT1015" />

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015: 実行可能ファイル '\*' を作成できませんでした: \*

<a name="MT1016" />

### <a name="mt1016-failed-to-create-the-notice-file-because-a-directory-already-exists-with-the-same-name"></a>MT1016: 同じ名前のディレクトリが既に存在するため、通知ファイルを作成できませんでした。

プロジェクトからディレクトリ `NOTICE` を削除します。

<a name="MT1017" />

### <a name="mt1017-failed-to-create-the-notice-file-"></a>MT1017: 通知ファイルを作成できませんでした: *。

<a name="MT1018" />

### <a name="mt1018-your-application-failed-code-signing-checks-and-could-not-be-installed-on-the-device--check-your-certificates-provisioning-profiles-and-bundle-ids-probably-your-device-is-not-part-of-the-selected-provisioning-profile-error-0xe8008015"></a>MT1018: アプリケーションがコード署名チェックに失敗したため、デバイス ' * ' にインストールできませんでした。 証明書、プロビジョニングプロファイル、およびバンドル id を確認します。 デバイスが選択されたプロビジョニングプロファイルに含まれていない場合があります (エラー: 0xe8008015)。

<a name="MT1019" />

### <a name="mt1019-your-application-has-entitlements-not-supported-by-your-current-provisioning-profile-and-could-not-be-installed-on-the-device--please-check-the-ios-device-log-for-more-detailed-information-error-0xe8008016"></a>MT1019: アプリケーションは現在のプロビジョニングプロファイルでサポートされていない権利を持っているため、デバイス ' * ' にインストールできませんでした。 詳細については、iOS デバイスログを確認してください (エラー: 0xe8008016)。

これは次の場合に発生する可能性があります。

- アプリケーションには、現在のプロビジョニングプロファイルでサポートされていない権限があります。
  考えられる解決策:
  - アプリケーションに必要な権利をサポートする別のプロビジョニングプロファイルを指定します。
  - 現在のプロビジョニングプロファイルでサポートされていない権利を削除します。
- 展開しようとしているデバイスは、使用しているプロビジョニングプロファイルに含まれていません。
  考えられる解決策:
  - Xcode のテンプレートから新しいアプリを作成し、同じプロビジョニングプロファイルを選択して、同じデバイスにデプロイします。 場合によっては、Xcode が新しいデバイスでプロビジョニングプロファイルを自動的に更新することがあります (その他の場合、Xcode は何をするかをたずねます)。
  -IOS デベロッパーセンターにアクセスし、プロビジョニングプロファイルを新しいデバイスで更新してから、更新されたプロビジョニングプロファイルをコンピューターにダウンロードします。

ほとんどの場合、エラーに関する詳細情報が iOS デバイスログに出力されます。これは、問題の診断に役立ちます。

<a name="MT1020" />

### <a name="mt1020-failed-to-launch-the-application--on-the-device--"></a>MT1020: デバイス '\*' でアプリケーション '\*' を起動できませんでした: *

<a name="MT1021" />

### <a name="mt1021-could-not-copy-the-file--to--2"></a>MT1021: ファイル '\*' を '\*' にコピーできませんでした: {2}

ファイルをコピーできませんでした。 コピー操作のエラーメッセージには、エラーに関する詳細情報が含まれています。

<a name="MT1022" />

### <a name="mt1022-could-not-copy-the-directory--to--2"></a>MT1022: ディレクトリ '\*' を '\*' にコピーできませんでした: {2}

ディレクトリをコピーできませんでした。 コピー操作のエラーメッセージには、エラーに関する詳細情報が含まれています。

<a name="MT1023" />

### <a name="mt1023-could-not-communicate-with-the-device-to-find-the-application---"></a>MT1023: デバイスと通信してアプリケーション '\*' を見つけることができませんでした: \*

デバイスでアプリケーションを参照しようとしたときにエラーが発生しました。

この問題を解決するには、次の手順を実行します。

- デバイスからアプリケーションを削除してから、操作をやり直してください。
- デバイスを切断し、再接続します。
- デバイスを再起動します。
- Mac を再起動します。

<a name="MT1024" />

### <a name="mt1024-the-application-signature-could-not-be-verified-on-device--please-make-sure-that-the-provisioning-profile-is-installed-and-not-expired-error-0xe8008017"></a>MT1024: デバイス ' * ' でアプリケーション署名を検証できませんでした。 プロビジョニングプロファイルがインストールされ、期限切れになっていないことを確認してください (エラー: 0xe8008017)。

署名を検証できなかったため、デバイスはアプリケーションのインストールを拒否しました。

プロビジョニングプロファイルがインストールされていて、期限が切れていないことを確認してください。

<a name="MT1025" />

### <a name="mt1025-could-not-list-the-crash-reports-on-the-device-"></a>MT1025: デバイス * のクラッシュレポートを一覧表示できませんでした。

デバイスでクラッシュレポートを一覧表示しようとしているときにエラーが発生しました。

この問題を解決するには、次の手順を実行します。

- デバイスからアプリケーションを削除してから、操作をやり直してください。
- デバイスを切断し、再接続します。
- デバイスを再起動します。
- Mac を再起動します。
- デバイスを iTunes と同期します (これにより、デバイスからクラッシュレポートが削除されます)。

<a name="MT1026" />

### <a name="mt1026-could-not-download-the-crash-report--from-the-device-"></a>MT1026: デバイス \*からクラッシュレポート \* をダウンロードできませんでした。

デバイスからクラッシュレポートをダウンロードしようとしたときにエラーが発生しました。

この問題を解決するには、次の手順を実行します。

- デバイスからアプリケーションを削除してから、操作をやり直してください。
- デバイスを切断し、再接続します。
- デバイスを再起動します。
- Mac を再起動します。
- デバイスを iTunes と同期します (これにより、デバイスからクラッシュレポートが削除されます)。

<a name="MT1027" />

### <a name="mt1027-cant-use-xcode-7-to-launch-applications-on-devices-with-ios--xcode-7-only-supports-ios-6"></a>MT1027: Xcode 7 + を使用して iOS のデバイスでアプリケーションを起動することはできません * (Xcode 7 でサポートされているのは iOS 6 以降のみです)。

Xcode 7 以降を使用して、6.0 未満の iOS バージョンのデバイスでアプリケーションを起動することはできません。

古いバージョンの Xcode を使用するか、アプリを手動でタップして起動してください。

<a name="MT1028" />

### <a name="mt1028-invalid-device-specification--expected-ios-watchos-or-all"></a>MT1028: デバイスの指定が無効です: ' * '。 ' Ios '、' watchos '、または ' all ' が必要です。

--デバイスを使用して渡されたデバイス仕様が無効です。 有効な値: ' ios '、' watchos '、または ' all '。

<a name="MT1029" />

### <a name="mt1029-could-not-find-an-application-at-the-specified-directory-"></a>MT1029: 指定されたディレクトリにアプリケーションが見つかりませんでした: *

--Launchdev に渡されたアプリケーションパスが存在しません。 有効なアプリバンドルを指定してください。

<a name="MT1030" />

### <a name="mt1030-launching-applications-on-device-using-a-bundle-identifier-is-deprecated-please-pass-the-full-path-to-the-bundle-to-launch"></a>MT1030: バンドル識別子を使用してデバイスでアプリケーションを起動することは非推奨とされます。 起動するには、バンドルの完全なパスを渡してください。

バンドル id だけでなくデバイスで起動するように、アプリへのパスを渡すことをお勧めします。

<a name="MT1031" />

### <a name="mt1031-could-not-launch-the-app--on-the-device--because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1031: デバイスがロックされているため、デバイス '\*' でアプリ '\*' を起動できませんでした。 デバイスのロックを解除してから、もう一度お試しください。

デバイスのロックを解除してから、もう一度お試しください。

<a name="MT1032" />

### <a name="mt1032-this-application-executable-might-be-too-large--mb-to-execute-on-device-if-bitcode-was-enabled-you-might-want-to-disable-it-for-development-it-is-only-required-to-submit-applications-to-apple"></a>MT1032: このアプリケーションの実行可能ファイルは、デバイスで実行するには大きすぎる (* MB) 可能性があります。 Bitcode が有効になっている場合、開発用に無効にする必要があります。これは、アプリケーションを Apple に送信するだけで済みます。

<a name="MT1033" />

### <a name="mt1033-could-not-uninstall-the-application--from-the-device--"></a>MT1033: デバイス '\*' からアプリケーション '\*' をアンインストールできませんでした: *

<!-- 1034 used by mmp -->

<a name="MT1035" />

### <a name="mt1035-cannot-include-different-versions-of-the-framework-name"></a>MT1035: 異なるバージョンのフレームワーク ' {name} ' を含めることはできません

同じフレームワークの異なるバージョンとリンクすることはできません。

これは通常、拡張機能がコンテナーアプリとは異なるバージョンのフレームワークを参照している場合に発生します (サードパーティのバインドアセンブリを使用することもあります)。

このエラーの後に、さまざまなフレームワークのパスを一覧表示する複数の[MT1036](#MT1036)エラーが発生します。

<a name="MT1036" />

### <a name="mt1036-framework-name-included-from-path-related-to-previous-error"></a>MT1036: {path} からフレームワーク ' {name} ' が含まれています (以前のエラーに関連しています)

このエラーは、 [MT1036](#MT1036)と共に報告されます。 詳細については、 [MT1036](#MT1036)を参照してください。

### <a name="mt11xx-debug-service"></a>MT11xx: Debug Service

<!--
  MT11xx DebugService.cs
  -->

<a name="MT1101" />

### <a name="mt1101-could-not-start-app"></a>MT1101: アプリを開始できませんでした

<a name="MT1102" />

### <a name="mt1102-could-not-attach-to-the-app-to-kill-it-"></a>MT1102: アプリにアタッチできませんでした (強制終了): *

<a name="MT1103" />

### <a name="mt1103-could-not-detach"></a>MT1103: デタッチできませんでした

<a name="MT1104" />

### <a name="mt1104-failed-to-send-packet-"></a>MT1104: パケットを送信できませんでした: *

<a name="MT1105" />

### <a name="mt1105-unexpected-response-type"></a>MT1105: 予期しない応答の種類

<a name="MT1106" />

### <a name="mt1106-could-not-get-list-of-applications-on-the-device-request-timed-out"></a>MT1106: デバイス上のアプリケーションの一覧を取得できませんでした。要求がタイムアウトしました。

<a name="MT1107" />

### <a name="mt1107-application-failed-to-launch-"></a>MT1107: アプリケーションを起動できませんでした: *

デバイスがロックされているかどうかを確認してください。

エンタープライズアプリをデプロイしている場合や、無料プロビジョニングプロファイルを使用している場合は、開発者を信頼している可能性があります (これについては<a href="https://stackoverflow.com/a/30726375/183422">ここで</a>説明します)。

<a name="MT1108" />

### <a name="mt1108-could-not-find-developer-tools-for-this-xx-yy-device"></a>MT1108: この XX (YY) デバイスの開発者ツールが見つかりませんでした。

Mtouch からのいくつかの操作では、`DeveloperDiskImage.dmg` ファイルが存在する必要があります。   このファイルは Xcode の一部であり、通常は、`Xcode.app/Contents/Developer/iPhoneOS.platform/DeviceSupport/VERSION/DeveloperDiskImage.dmg`での構築に使用している SDK を基準としています。

このエラーは、接続しているデバイスに一致する DeveloperDiskImage がないために発生する可能性があります。

<a name="MT1109" />

### <a name="mt1109-application-failed-to-launch-because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1109: デバイスがロックされているため、アプリケーションを起動できませんでした。 デバイスのロックを解除してから、もう一度お試しください。

デバイスがロックされているかどうかを確認してください。

<a name="MT1110" />

### <a name="mt1110-application-failed-to-launch-because-of-ios-security-restrictions-please-ensure-the-developer-is-trusted"></a>MT1110: iOS のセキュリティ制限により、アプリケーションを起動できませんでした。 開発者が信頼されていることを確認してください。

エンタープライズアプリをデプロイしている場合や、無料プロビジョニングプロファイルを使用している場合は、開発者を信頼している可能性があります (これについては<a href="https://stackoverflow.com/a/30726375/183422">ここで</a>説明します)。

<a name="MT1111" />

### <a name="mt1111-application-launched-successfully-but-its-not-possible-to-wait-for-the-app-to-exit-as-requested-because-its-not-possible-to-detect-app-termination-when-launching-using-gdbserver"></a>MT1111: アプリケーションは正常に起動しましたが、gdbserver を使用して起動したときにアプリの終了を検出できないため、アプリが要求どおりに終了するのを待機することはできません。

### <a name="mt12xx-simulator"></a>MT12xx: シミュレーター

<!--
  MT12xx simcontroller.cs
  -->

<a name="MT1201" />

### <a name="mt1201-could-not-load-the-simulator-"></a>MT1201: シミュレーターを読み込めませんでした: *

<a name="MT1202" />

### <a name="mt1202-invalid-simulator-configuration-"></a>MT1202: 無効なシミュレーター構成: *

<a name="MT1203" />

### <a name="mt1203-invalid-simulator-specification-"></a>MT1203: シミュレーターの指定が無効です: *

<a name="MT1204" />

### <a name="mt1204-invalid-simulator-specification--runtime-not-specified"></a>MT1204: 無効なシミュレーターの指定 ' * ': ランタイムが指定されていません。

<a name="MT1205" />

### <a name="mt1205-invalid-simulator-specification--device-type-not-specified"></a>MT1205: シミュレーターの仕様 ' * ' が無効です: デバイスの種類が指定されていません。

<a name="MT1206" />

### <a name="mt1206-could-not-find-the-simulator-runtime-"></a>MT1206: シミュレーターランタイム ' * ' が見つかりませんでした。

<a name="MT1207" />

### <a name="mt1207-could-not-find-the-simulator-device-type-"></a>MT1207: シミュレーターデバイスの種類 ' * ' が見つかりませんでした。

<a name="MT1208" />

### <a name="mt1208-could-not-find-the-simulator-runtime-"></a>MT1208: シミュレーターランタイム ' * ' が見つかりませんでした。

<a name="MT1209" />

### <a name="mt1209-could-not-find-the-simulator-device-type-"></a>MT1209: シミュレーターデバイスの種類 ' * ' が見つかりませんでした。

<a name="MT1210" />

### <a name="mt1210-invalid-simulator-specification--unknown-key-"></a>MT1210: 無効なシミュレーターの指定: \*、不明なキー '\*'

<a name="MT1211" />

### <a name="mt1211-the-simulator-version--does-not-support-the-simulator-type-"></a>MT1211: シミュレーターのバージョン '\*' はシミュレーターの種類 '\*' をサポートしていません

<a name="MT1212" />

### <a name="mt1212-failed-to-create-a-simulator-version-where-type---and-runtime--"></a>MT1212: type = \* および runtime = \*のシミュレーターバージョンを作成できませんでした。

<a name="MT1213" />

### <a name="mt1213-invalid-simulator-specification-for-xcode-4-"></a>MT1213: Xcode 4 のシミュレーターの仕様が無効です: *

<a name="MT1214" />

### <a name="mt1214-invalid-simulator-specification-for-xcode-5-"></a>MT1214: Xcode 5 のシミュレーターの仕様が無効です: *

<a name="MT1215" />

### <a name="mt1215-invalid-sdk-specified-"></a>MT1215: 無効な SDK が指定されました: *

<a name="MT1216" />

### <a name="mt1216-could-not-find-the-simulator-udid-"></a>MT1216: シミュレーター UDID ' * ' が見つかりませんでした。

<a name="MT1217" />

### <a name="mt1217-could-not-load-the-app-bundle-at-"></a>MT1217: ' * ' でアプリバンドルを読み込めませんでした。

<a name="MT1218" />

### <a name="mt1218-no-bundle-identifier-found-in-the-app-at-"></a>MT1218: ' * ' のアプリでバンドル id が見つかりませんでした。

<a name="MT1219" />

### <a name="mt1219-could-not-find-the-simulator-for-"></a>MT1219: ' * ' のシミュレーターが見つかりませんでした。

<a name="MT1220" />

### <a name="mt1220-could-not-find-the-latest-simulator-runtime-for-device-"></a>MT1220: デバイス ' * ' の最新のシミュレーターランタイムが見つかりませんでした。

これは通常、Xcode に問題があることを示しています。

この問題を解決するには、次の手順を実行します。

- Xcode でシミュレーターを1回使用します。
- --Sdk \<version > を使用して明示的な SDK バージョンを渡してください。
- Xcode を再インストールします。

<a name="MT1221" />

### <a name="mt1221-could-not-find-the-paired-iphone-simulator-for-the-watchos-simulator-"></a>MT1221: WatchOS シミュレーター ' * ' のペアになっている iPhone シミュレーターが見つかりませんでした。

WatchOS シミュレーターで WatchOS アプリを起動する場合は、ペアリングされている iOS シミュレーターも必要です。

Xcode のデバイス UI (メニューウィンドウ-> デバイス) を使用して、Watch シミュレーターと iOS シミュレーターを組み合わせることができます。

### <a name="mt13xx-linkwith"></a>MT13xx: [LinkWith]

<!--
  MT13xx [LinkWith]
  -->

<a name="MT1301" />

### <a name="mt1301-native-library---was-ignored-since-it-does-not-match-the-current-build-architectures-"></a>MT1301: ネイティブライブラリ `*` (\*) は、現在のビルドアーキテクチャ (\*) と一致しないため、無視されました

<a name="MT1302" />

### <a name="mt1302-could-not-extract-the-native-library--from--please-ensure-the-native-library-was-properly-embedded-in-the-managed-assembly-if-the-assembly-was-built-using-a-binding-project-the-native-library-must-be-included-in-the-project-and-its-build-action-must-be-objcbindingnativelibrary"></a>MT1302: ネイティブライブラリ ' * ' を ' + ' から抽出できませんでした。 ネイティブライブラリがマネージアセンブリに正しく埋め込まれていることを確認してください (アセンブリがバインドプロジェクトを使用してビルドされている場合は、ネイティブライブラリがプロジェクトに含まれている必要があり、そのビルドアクションは ' Objcbinding、ライブラリ ' である必要があります)。

<a name="MT1303" />

### <a name="mt1303-could-not-decompress-the-native-framework--from--please-review-the-build-log-for-more-information-from-the-native-zip-command"></a>MT1303: ネイティブフレームワーク '\*' を '\*' から圧縮解除できませんでした。 ネイティブの ' zip ' コマンドの詳細については、ビルドログを確認してください。

指定されたネイティブフレームワークをバインドライブラリから圧縮解除できませんでした。

このエラーの詳細については、ネイティブの ' zip ' コマンドを参照してください。

<a name="MT1304" />

### <a name="mt1304-the-embedded-framework--in--is-invalid-it-does-not-contain-an-infoplist"></a>MT1304: \* の埋め込みフレームワーク '\*' が無効です。この型には、情報 plist が含まれていません。

指定された埋め込みフレームワークには、plist が含まれていないため、有効なフレームワークではありません。

フレームワークが有効であることを確認してください。

<a name="MT1305" />

### <a name="mt1305-the-binding-library--contains-a-user-framework--but-embedded-user-frameworks-require-ios-80-the-current-deployment-target-is--please-set-the-deployment-target-in-the-infoplist-file-to-at-least-80"></a>MT1305: バインドライブラリ '\*' にはユーザーフレームワーク (\*) が含まれていますが、埋め込みユーザーフレームワークには iOS 8.0 が必要です (現在のデプロイターゲットは *)。 情報 plist ファイルの配置ターゲットを少なくとも8.0 に設定してください。

指定されたバインディングライブラリには埋め込みフレームワークが含まれていますが、Xamarin は ios 8.0 以降の埋め込みフレームワークのみをサポートしています。

このエラーを解決するには、情報 plist ファイルの配置ターゲットを少なくとも8.0 に設定してください (または埋め込みフレームワークを使用しないようにしてください)。

### <a name="mt14xx-crash-reports"></a>MT14xx: クラッシュレポート

<!--
  MT14xx    CrashReports.cs
  -->

<a name="MT1400" />

### <a name="mt1400-could-not-open-crash-report-service-afcconnectionopen-returned-"></a>MT1400: クラッシュレポートサービスを開くことができませんでした。 AFCConnectionOpen が返されました *

デバイスからクラッシュレポートにアクセスしようとしたときにエラーが発生しました。

この問題を解決するには、次の手順を実行します。

- デバイスからアプリケーションを削除してから、操作をやり直してください。
- デバイスを切断し、再接続します。
- デバイスを再起動します。
- Mac を再起動します。
- デバイスを iTunes と同期します (これにより、デバイスからクラッシュレポートが削除されます)。

<a name="MT1401" />

### <a name="mt1401-could-not-close-crash-report-service-afcconnectionclose-returned-"></a>MT1401: クラッシュレポートサービスを閉じることができませんでした: AFCConnectionClose が返されました *

デバイスからクラッシュレポートにアクセスしようとしたときにエラーが発生しました。

この問題を解決するには、次の手順を実行します。

- デバイスからアプリケーションを削除してから、操作をやり直してください。
- デバイスを切断し、再接続します。
- デバイスを再起動します。
- Mac を再起動します。
- デバイスを iTunes と同期します (これにより、デバイスからクラッシュレポートが削除されます)。

<a name="MT1402" />

### <a name="mt1402-could-not-read-file-info-for--afcfileinfoopen-returned-"></a>MT1402: AFCFileInfoOpen が返された \*のファイル情報を読み取れませんでした \*

デバイスからクラッシュレポートにアクセスしようとしたときにエラーが発生しました。

この問題を解決するには、次の手順を実行します。

- デバイスからアプリケーションを削除してから、操作をやり直してください。
- デバイスを切断し、再接続します。
- デバイスを再起動します。
- Mac を再起動します。
- デバイスを iTunes と同期します (これにより、デバイスからクラッシュレポートが削除されます)。

<a name="MT1403" />

### <a name="mt1403-could-not-read-crash-report-afcdirectoryopen--returned-"></a>MT1403: クラッシュレポートを読み取れませんでした: AFCDirectoryOpen (\*) が返されました: \*

デバイスからクラッシュレポートにアクセスしようとしたときにエラーが発生しました。

この問題を解決するには、次の手順を実行します。

- デバイスからアプリケーションを削除してから、操作をやり直してください。
- デバイスを切断し、再接続します。
- デバイスを再起動します。
- Mac を再起動します。
- デバイスを iTunes と同期します (これにより、デバイスからクラッシュレポートが削除されます)。

<a name="MT1404" />

### <a name="mt1404-could-not-read-crash-report-afcfilerefopen--returned-"></a>MT1404: クラッシュレポートを読み取れませんでした: AFCFileRefOpen (\*) が返されました: \*

デバイスからクラッシュレポートにアクセスしようとしたときにエラーが発生しました。

この問題を解決するには、次の手順を実行します。

- デバイスからアプリケーションを削除してから、操作をやり直してください。
- デバイスを切断し、再接続します。
- デバイスを再起動します。
- Mac を再起動します。
- デバイスを iTunes と同期します (これにより、デバイスからクラッシュレポートが削除されます)。

<a name="MT1405" />

### <a name="mt1405-could-not-read-crash-report-afcfilerefread--returned-"></a>MT1405: クラッシュレポートを読み取れませんでした: AFCFileRefRead (\*) が返されました: \*

デバイスからクラッシュレポートにアクセスしようとしたときにエラーが発生しました。

この問題を解決するには、次の手順を実行します。

- デバイスからアプリケーションを削除してから、操作をやり直してください。
- デバイスを切断し、再接続します。
- デバイスを再起動します。
- Mac を再起動します。
- デバイスを iTunes と同期します (これにより、デバイスからクラッシュレポートが削除されます)。

<a name="MT1406" />

### <a name="mt1406-could-not-list-crash-reports-afcdirectoryopen--returned-"></a>MT1406: クラッシュレポートを一覧表示できませんでした: AFCDirectoryOpen (\*) が返されました: \*

デバイスからクラッシュレポートにアクセスしようとしたときにエラーが発生しました。

この問題を解決するには、次の手順を実行します。

- デバイスからアプリケーションを削除してから、操作をやり直してください。
- デバイスを切断し、再接続します。
- デバイスを再起動します。
- Mac を再起動します。
- デバイスを iTunes と同期します (これにより、デバイスからクラッシュレポートが削除されます)。

<!--- 1407 used by mmp -->

### <a name="mt16xx-macho"></a>MT16xx: MachO

<!--
  MT16xx    MachO.cs
  -->

<a name="MT1600" />

### <a name="mt1600-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1600: マッハ O ダイナミックライブラリではありません (不明なヘッダー ' 0x * '): *。

問題のダイナミックライブラリを処理中にエラーが発生しました。

ダイナミックライブラリが有効なマッハ O ダイナミックライブラリであることを確認してください。

ライブラリの形式は、ターミナルから `file` コマンドを使用して確認できます。

```
file -arch all -l /path/to/library.dylib
```

<a name="MT1601" />

### <a name="mt1601-not-a-static-library-unknown-header--"></a>MT1601: スタティックライブラリではありません (不明なヘッダー ' * '): *。

問題のスタティックライブラリの処理中にエラーが発生しました。

スタティックライブラリが有効なマッハ O スタティックライブラリであることを確認してください。

ライブラリの形式は、ターミナルから `file` コマンドを使用して確認できます。

```
file -arch all -l /path/to/library.a
```

<a name="MT1602" />

### <a name="mt1602-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1602: マッハ O ダイナミックライブラリではありません (不明なヘッダー ' 0x * '): *。

問題のダイナミックライブラリを処理中にエラーが発生しました。

ダイナミックライブラリが有効なマッハ O ダイナミックライブラリであることを確認してください。

ライブラリの形式は、ターミナルから `file` コマンドを使用して確認できます。

```
file -arch all -l /path/to/library.dylib
```

<a name="MT1603" />

### <a name="mt1603-unknown-format-for-fat-entry-at-position--in-"></a>MT1603: \*の位置 \* にある fat エントリの形式が不明です。

問題の fat アーカイブの処理中にエラーが発生しました。

Fat アーカイブが有効であることを確認してください。

Fat アーカイブのフォーマットは、ターミナルから `file` コマンドを使用して確認できます。

```
file -arch all -l /path/to/file
```

<a name="MT1604" />

### <a name="mt1604-file-of-type--is-not-a-macho-file-"></a>MT1604: \* の種類のファイルが MachO ファイル (\*) ではありません。

問題の MachO ファイルの処理中にエラーが発生しました。

ファイルが有効なマッハ O ダイナミックライブラリであることを確認してください。

ファイルの形式は、ターミナルから `file` コマンドを使用して確認できます。

```
file -arch all -l /path/to/file
```

## <a name="mt2xxx-linker-error-messages"></a>MT2xxx: リンカーのエラーメッセージ

<!--
 MT2xxx Linker
  MT20xx Linker (general) errors
  -->

<a name="MT2001" />

### <a name="mt2001-could-not-link-assemblies"></a>MT2001: アセンブリをリンクできませんでした

このエラーは、マネージリンカーで、例外などの予期しないエラーが発生し、処理中のアセンブリを完了または保存できなかったことを示します。 正確なエラーに関する詳細情報は、ビルドログに含まれます。例:

```
error MT2001: Could not link assemblies.
    Method: `System.Void Todo.TodoListPageCS/<<-ctor>b__1_0>d::SetStateMachine(System.Runtime.CompilerServices.IAsyncStateMachine)`
    Assembly: `QuickTodo, Version=1.0.6297.28241, Culture=neutral, PublicKeyToken=null`
Reason: Value cannot be null.
Parameter name: instruction
```

このような問題については、バグレポートを作成することが重要です。 ほとんどの場合、適切な修正が発行されるまで、回避策を提供できます。 上記の情報は、問題を解決するために重要です (テストケースやアセンブリ binairy)。

<a name="MT2002" />

### <a name="mt2002-can-not-resolve-reference-"></a>MT2002: 参照を解決できません: *

<a name="MT2003" />

### <a name="mt2003-option--will-be-ignored-since-linking-is-disabled"></a>MT2003: オプション ' * ' は、リンクが無効になっているため無視されます

<a name="MT2004" />

### <a name="mt2004-extra-linker-definitions-file--could-not-be-located"></a>MT2004: 追加のリンカー定義ファイル ' * ' が見つかりませんでした。

<a name="MT2005" />

### <a name="mt2005-definitions-from--could-not-be-parsed"></a>MT2005: ' * ' からの定義を解析できませんでした。

<a name="MT2006" />

### <a name="mt2006-can-not-load-mscorlibdll-from--please-reinstall-xamarinios"></a>MT2006: * から mscorlib.dll を読み込むことはできません。 Xamarin. iOS を再インストールしてください。

これは、通常、Xamarin の iOS のインストールに問題があることを示しています。 Xamarin. iOS を再インストールしてみてください。

<!--- 2007 used by mmp -->
<!--- 2009 used by mmp -->

<a name="MT2010" />

### <a name="mt2010-unknown-httpmessagehandler--valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MT2010: 不明な HttpMessageHandler `*`。 有効な値は HttpClientHandler (既定値)、CFNetworkHandler、または NSUrlSessionHandler です。

<a name="MT2011" />

### <a name="mt2011-unknown-tlsprovider---valid-values-are-default-or-appletls"></a>MT2011: 不明な TlsProvider `*`です。  有効な値は、default または appletls です。

`tls-provider=` に指定された値は、有効な TLS (Transport Layer Security) プロバイダーではありません。

`default` と `appletls` は唯一の有効な値であり、どちらも同じオプションを表します。これは、ネイティブの Apple TLS API を使用して SSL/TLS サポートを提供するためのものです。

<!--- 2012 used by mmp -->
<!--- 2013 used by mmp -->
<!--- 2014 used by mmp -->

<a name="MT2015" />

### <a name="mt2015-invalid-httpmessagehandler--for-watchos-the-only-valid-value-is-nsurlsessionhandler"></a>MT2015: watchOS の HttpMessageHandler `*` が無効です。 有効な値は Nの Lsessionhandler だけです。

これは、プロジェクトファイルで無効な HttpMessageHandler が指定されていることが原因で発生する警告です。

既定で生成される、以前のバージョンのプレビューツールは、プロジェクトファイル内の無効な値です。

この警告を解決するには、テキストエディターでプロジェクトファイルを開き、すべての HttpMessageHandler ノードを XML から削除します。

<a name="MT2016" />

### <a name="mt2016-invalid-tlsprovider-legacy-option-the-only-valid-value-appletls-will-be-used"></a>MT2016: 無効な TlsProvider `legacy` オプションです。 有効な値 `appletls` のみが使用されます。

完全に管理された SSLv3/TLSv1 only プロバイダーであった `legacy` プロバイダーは、Xamarin には付属していません。 この古いプロバイダーを使用していて、新しい `appletls` 1 つでビルドするプロジェクト。

この警告を解決するには、テキストエディターでプロジェクトファイルを開き、XML からすべての ' MtouchTlsProvider ' ' ノードを削除します。

<a name="MT2017" />

### <a name="mt2017-could-not-process-xml-description"></a>MT2017: XML 記述を処理できませんでした。

これは、指定された[カスタム XML リンカー構成ファイル](~/cross-platform/deploy-test/linker.md)にエラーがあることを意味します。ファイルを確認してください。

<a name="MT2018" />

### <a name="mt2018-the-assembly--is-referenced-from-two-different-locations--and-"></a>MT2018: アセンブリ '\*' は、'\*' と ' * ' の2つの異なる場所から参照されています。

エラーメッセージに示されているアセンブリが、複数の場所から読み込まれています。 常に同じバージョンのアセンブリを使用するようにしてください。

<a name="MT2019" />

### <a name="mt2019-can-not-load-the-root-assembly-"></a>MT2019: ルートアセンブリ ' * ' を読み込むことができません

ルートアセンブリを読み込めませんでした。 エラーメッセージのパスが既存のファイルを参照していることと、有効な .NET アセンブリであることを確認してください。

<a name="MT202x" />

### <a name="mt202x-binding-optimizer-failed-processing-"></a>MT202x: バインドオプティマイザーで `...`を処理できませんでした。

生成されたバインドコードを最適化しようとしたときに、予期しない問題が発生しました。 問題の原因となっている要素の名前は、エラーメッセージで示されます。 この問題を解決するには、(またはという名前の型またはメソッドを含む) という名前のアセンブリを[github](https://github.com/xamarin/xamarin-macios/issues/new)の新しい問題に追加し、詳細が有効になっている完全なビルドログ (たとえば、**追加の mtouch 引数**に `-v -v -v -v`) を指定する必要があります。

最後の桁 `x` は次のようになります。

- アセンブリ名を `0` します。
- 型名の `1`
- メソッド名の `3`

<a name="MT2030" />

### <a name="mt2030-remove-user-resources-failed-processing-"></a>MT2030: ユーザーリソースを削除すると、`...`を処理できませんでした。

ユーザーリソースを削除しようとしたときに予期しない問題が発生しました。 問題の原因となっているアセンブリの名前は、エラーメッセージで示されます。 この問題を解決するには、 [github](https://github.com/xamarin/xamarin-macios/issues/new)の新しい問題にアセンブリを提供し、詳細を有効にした完全なビルドログ (**追加の mtouch 引数**に `-v -v -v -v`) を指定する必要があります。

ユーザーリソースは、アプリケーションバンドルを作成するために、ビルド時に抽出する必要があるアセンブリ (リソースとして) 内に含まれるファイルです。 これには次のものが含まれます

- リソースの `__monotouch_content_*` と `__monotouch_pages_*`そして
- バインドアセンブリ内に埋め込まれたネイティブライブラリ。

<a name="MT2040" />

### <a name="mt2040-default-httpmessagehandler-setter-failed-processing-"></a>MT2040: 既定の HttpMessageHandler setter は `...`を処理できませんでした。

アプリケーションの既定の `HttpMessageHandler` を設定しようとしたときに、予期しない問題が発生しました。 詳細が有効になっている完全なビルドログ (たとえば、**追加の mtouch 引数**に `-v -v -v -v`) と共に、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を作成してください。

<a name="MT2050" />

### <a name="mt2050-code-remover-failed-processing-"></a>MT2050: Code 人で `...`を処理できませんでした。

アプリケーションで BCL 配布からコードを削除しようとしたときに、予期しない問題が発生しました。 詳細が有効になっている完全なビルドログ (たとえば、**追加の mtouch 引数**に `-v -v -v -v`) と共に、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を作成してください。

<a name="MT2060" />

### <a name="mt2060-sealer-failed-processing-"></a>MT2060: シーラーで `...`を処理できませんでした。

型またはメソッド (final) を封印しようとしたとき、または一部のメソッドを仮想化するときに、予期しない問題が発生しました。 問題の原因となっているアセンブリの名前は、エラーメッセージで示されます。 この問題を解決するには、 [github](https://github.com/xamarin/xamarin-macios/issues/new)の新しい問題にアセンブリを提供し、詳細を有効にした完全なビルドログ (**追加の mtouch 引数**に `-v -v -v -v`) を指定する必要があります。

<a name="MT2070" />

### <a name="mt2070-metadata-reducer-failed-processing-"></a>MT2070: Metadata Reducer は `...`を処理できませんでした。

アプリケーションからメタデータを縮小しようとしたときに、予期しない問題が発生しました。 問題の原因となっているアセンブリの名前は、エラーメッセージで示されます。 この問題を解決するには、 [github](https://github.com/xamarin/xamarin-macios/issues/new)の新しい問題にアセンブリを提供し、詳細を有効にした完全なビルドログ (**追加の mtouch 引数**に `-v -v -v -v`) を指定する必要があります。

<a name="MT2080" />

### <a name="mt2080-marknsobjects-failed-processing-"></a>MT2080: MarkNSObjects で `...`を処理できませんでした。

アプリケーションから `NSObject` サブクラスをマークしようとしたときに、予期しない問題が発生しました。 問題の原因となっているアセンブリの名前は、エラーメッセージで示されます。 この問題を解決するには、 [github](https://github.com/xamarin/xamarin-macios/issues/new)の新しい問題にアセンブリを提供し、詳細を有効にした完全なビルドログ (**追加の mtouch 引数**に `-v -v -v -v`) を指定する必要があります。

<a name="MT2090" />

### <a name="mt2090-inliner-failed-processing-"></a>MT2090: Inliner で `...`を処理できませんでした。

アプリケーションからコードをインライン化しようとしたときに、予期しない問題が発生しました。 問題の原因となっているアセンブリの名前は、エラーメッセージで示されます。 この問題を解決するには、 [github](https://github.com/xamarin/xamarin-macios/issues/new)の新しい問題にアセンブリを提供し、詳細を有効にした完全なビルドログ (**追加の mtouch 引数**に `-v -v -v -v`) を指定する必要があります。

<!-- MT21xx: more linker errors -->

<!--- 2100 used by mmp -->

<a name="MT2100" />

### <a name="mt2100-smart-enum-conversion-preserver-failed-processing-"></a>MT2100: スマート Enum 変換順序で `...`を処理できませんでした。

アプリケーションからスマート列挙の変換メソッドをマークしようとしたときに、予期しない問題が発生しました。 問題の原因となっているアセンブリの名前は、エラーメッセージで示されます。 この問題を解決するには、 [github](https://github.com/xamarin/xamarin-macios/issues/new)の新しい問題にアセンブリを提供し、詳細を有効にした完全なビルドログ (**追加の mtouch 引数**に `-v -v -v -v`) を指定する必要があります。

<a name="MT2101" />

### <a name="mt2101-cant-resolve-the-reference--referenced-from-the-method--in-"></a>MT2101: ' * ' のメソッド '\*' から参照されている参照 '\*' を解決できません。

エラーメッセージに示されているメソッドを処理するときに、無効なアセンブリ参照が見つかりました。

問題の原因となっているアセンブリの名前は、エラーメッセージで示されます。 この問題を解決するには、 [github](https://github.com/xamarin/xamarin-macios/issues/new)の新しい問題にアセンブリを提供し、詳細を有効にした完全なビルドログ (**追加の mtouch 引数**に `-v -v -v -v`) を指定する必要があります。

<a name="MT2102" />

### <a name="mt2102-error-processing-the-method--in-the-assembly--"></a>MT2102: アセンブリ '\*' でメソッド '\*' を処理中にエラーが発生した: *

エラーメッセージに示されているメソッドをマークしようとしたときに、予期しない問題が発生しました。

問題の原因となっているアセンブリの名前は、エラーメッセージで示されます。 この問題を解決するには、 [github](https://github.com/xamarin/xamarin-macios/issues/new)の新しい問題にアセンブリを提供し、詳細を有効にした完全なビルドログ (**追加の mtouch 引数**に `-v -v -v -v`) を指定する必要があります。

<a name="MT2103" />

### <a name="mt2103-error-processing-assembly--"></a>MT2103: アセンブリ '\*' の処理中にエラーが発生した: *

アセンブリの処理中に予期しないエラーが発生しました。

問題の原因となっているアセンブリの名前は、エラーメッセージで示されます。 この問題を解決するには、 [github](https://github.com/xamarin/xamarin-macios/issues/new)の新しい問題にアセンブリを提供し、詳細を有効にした完全なビルドログ (**追加の mtouch 引数**に `-v -v -v -v`) を指定する必要があります。

<a name="MT2104" />

### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: アセンブリ '{0}' は混合モードであるため、リンクできません。

混合モードのアセンブリは、リンカーでは処理できません。

混合モードのアセンブリの詳細については、「 https://docs.microsoft.com/cpp/dotnet/mixed-native-and-managed-assemblies」を参照してください。

## <a name="mt3xxx-aot-error-messages"></a>MT3xxx: AOT エラーメッセージ

<!--
 MT3xxx AOT
  MT30xx AOT (general) errors
  -->

<a name="MT3001" />

### <a name="mt3001-could-not-aot-the-assembly-"></a>MT3001: アセンブリ ' * ' を AOT にできませんでした

通常、これは AOT コンパイラのバグを示しています。 エラーを再現するために使用できるプロジェクトと共に、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

場合によっては、プロジェクトの iOS ビルドオプションでインクリメンタルビルドを無効にすることで、この問題を回避できることもあります (ただし、それでもバグであるため、それを報告してください)。

<a name="MT3002" />

### <a name="mt3002-aot-restriction-method--must-be-static-since-it-is-decorated-with-monopinvokecallback-see-developerxamarincomguidesiosadvanced_topicslimitationsreverse_callbacks"></a>MT3002: AOT 制限: メソッド ' * ' は、[MonoPInvokeCallback] で修飾されているため、静的である必要があります。 Developer.xamarin.com/guides/ios/advanced_topics/limitations/を参照してください[#Reverse_Callbacks](~/ios/internals/limitations.md#reverse-callbacks)

このエラーメッセージは AOT コンパイラからのものです。

<a name="MT3003" />

### <a name="mt3003-conflicting---debug-and---llvm-options-soft-debugging-is-disabled"></a>MT3003: 競合しています--debug および--llvm オプション。 ソフトデバッグは無効になっています。

LLVM が有効になっている場合、デバッグはサポートされません。 アプリをデバッグする必要がある場合は、最初に LLVM を無効にします。

<a name="MT3004" />

### <a name="mt3004-could-not-aot-the-assembly--because-it-doesnt-exist"></a>MT3004: アセンブリ ' * ' を AOT にできませんでした。このアセンブリは存在しません。

<a name="MT3005" />

### <a name="mt3005-the-dependency--of-the-assembly--was-not-found-please-review-the-projects-references"></a>MT3005: アセンブリ '\*' の依存関係 '\*' が見つかりませんでした。 プロジェクトの参照を確認してください。

これは通常、アセンブリが別のバージョンのプラットフォームアセンブリを参照している場合に発生します (通常は .NET 4 バージョンの mscorlib.dll)。

これはサポートされていないため、正しくビルドまたは実行されない可能性があります (このアセンブリでは、Xamarin. iOS バージョンには含まれていない .NET 4 バージョンの mscorlib.dll の API が使用される場合があります)。

<a name="MT3006" />

### <a name="mt3006-could-not-compute-a-complete-dependency-map-for-the-project-this-will-result-in-slower-build-times-because-xamarinios-cant-properly-detect-what-needs-to-be-rebuilt-and-what-does-not-need-to-be-rebuilt-please-review-previous-warnings-for-more-details"></a>MT3006: プロジェクトの完全な依存関係マップを計算できませんでした。 この結果、ビルド時間が長くなります。これは、Xamarin では、再構築する必要があるもの (および再構築が必要ないもの) を正しく検出できないためです。 詳細については、前の警告を確認してください。

 ビルドまたは正常に実行されます (アセンブリは、Xamarin. iOS のバージョンには含まれていない .NET 4 バージョンの mscorlib.dll の API を使用する場合があります)。

<a name="MT3007" />

### <a name="mt3007-debug-info-files-mdb-will-not-be-loaded-when-llvm-is-enabled"></a>MT3007: llvm が有効になっている場合、デバッグ情報ファイル (* .mdb) は読み込まれません。

<a name="MT3008" />

### <a name="mt3008-bitcode-support-requires-the-use-of-the-llvm-aot-backend---llvm"></a>MT3008: Bitcode のサポートには、LLVM AOT バックエンドを使用する必要があります (--llvm)

Bitcode のサポートでは、LLVM AOT バックエンド (--llvm) を使用する必要があります。

Bitcode サポートを無効にするか、LLVM を有効にしてください。

<!--- 3009 used by mmp -->
<!--- 3010 used by mmp -->

## <a name="mt4xxx-code-generation-error-messages"></a>MT4xxx: コード生成エラーメッセージ

### <a name="mt40xx-main"></a>MT40xx: Main

<!--
 MT4xxx code generation
  MT40xx main.m
  -->

<a name="MT4001" />

### <a name="mt4001-the-main-template-could-not-be-expanded-to-"></a>MT4001: メインテンプレートを `*`に展開できませんでした。

`main.m`の生成中にエラーが発生しました。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT4002" />

### <a name="mt4002-failed-to-compile-the-generated-code-for-pinvoke-methods-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4002: P/Invoke メソッド用に生成されたコードをコンパイルできませんでした。 http://bugzilla.xamarin.com でバグレポートをファイルに登録してください

P/Invoke メソッド用に生成されたコードをコンパイルできませんでした。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

### <a name="mt41xx-registrar"></a>MT41xx: レジストラー

<!--
  MT41xx registrar.m
  -->

<a name="MT4101" />

### <a name="mt4101-the-registrar-cannot-build-a-signature-for-type-"></a>MT4101: レジストラーは `*`型の署名を作成できません。

エクスポートされた API に型が見つかりました。これは、ランタイムが目的の C をマーシャリングする方法を認識していません。

Xamarin では、問題の種類をサポートする必要がある場合は、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で拡張要求を行います。

<a name="MT4102" />

### <a name="mt4102-the-registrar-found-an-invalid-type--in-signature-for-method--use--instead"></a>MT4102: レジストラーは、メソッド `*`のシグネチャに無効な型 `*` を検出しました。 代わりに `*` を使用してください

現在は、1つの型 (system.string) でのみ発生します。 代わりに、目的の C と同等の (NSDate) を使用してください。

<a name="MT4103" />

### <a name="mt4103-the-registrar-found-an-invalid-type--in-signature-for-method--the-type-implements-inativeobject-but-does-not-have-a-constructor-that-takes-two-intptr-bool-arguments"></a>MT4103: レジストラーは、メソッド `*`のシグネチャに無効な型 `*` を検出しました: この型は INativeObject を実装していますが、2つ (IntPtr、bool) の引数を受け取るコンストラクターがありません

このエラーは、レジストラーが、示されている特性を持つシグネチャ内の型を検出した場合に発生します。 レジストラーは、型の新しいインスタンスを作成する必要がある場合があります。この場合は、(IntPtr, bool) シグネチャを持つコンストラクターが必要です。最初の引数 (IntPtr) はマネージハンドルを指定しますが、呼び出し元がネイティブの所有権を取得する場合は2番目の引数を指定します。handle (この値が false の場合、オブジェクトに対して "retain" が呼び出されます)。

<a name="MT4104" />

### <a name="mt4104-the-registrar-cannot-marshal-the-return-value-for-type--in-signature-for-method-"></a>MT4104: レジストラーは、メソッド `*`のシグネチャの `*` 型の戻り値をマーシャリングできません。

エクスポートされた API に型が見つかりました。これは、ランタイムが目的の C をマーシャリングする方法を認識していません。

Xamarin では、問題の種類をサポートする必要がある場合は、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で拡張要求を行います。

<a name="MT4105" />

### <a name="mt4105-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4105: レジストラーは、メソッド `*`のシグネチャの `*` 型のパラメーターをマーシャリングできません。

Xamarin では、問題の種類をサポートする必要がある場合は、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で拡張要求を行います。

<a name="MT4106" />

### <a name="mt4106-the-registrar-cannot-marshal-the-return-value-for-structure--in-signature-for-method-"></a>MT4106: レジストラーは、メソッド `*`のシグネチャの構造 `*` の戻り値をマーシャリングできません。

エクスポートされた API に型が見つかりました。これは、ランタイムが目的の C をマーシャリングする方法を認識していません。

Xamarin では、問題の種類をサポートする必要がある場合は、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で拡張要求を行います。

<a name="MT4107" />

### <a name="mt4107-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4107: レジストラーは、メソッド `+`のシグネチャの `*` 型のパラメーターをマーシャリングできません。

エクスポートされた API に型が見つかりました。これは、ランタイムが目的の C をマーシャリングする方法を認識していません。

Xamarin では、問題の種類をサポートする必要がある場合は、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で拡張要求を行います。

<a name="MT4108" />

### <a name="mt4108-the-registrar-cannot-get-the-objectivec-type-for-managed-type-"></a>MT4108: レジストラーは、マネージ型 `*`に対して、対象の型を取得できません。

エクスポートされた API に型が見つかりました。これは、ランタイムが目的の C をマーシャリングする方法を認識していません。

Xamarin では、問題の種類をサポートする必要がある場合は、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で拡張要求を行います。

<a name="MT4109" />

### <a name="mt4109-failed-to-compile-the-generated-registrar-code-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4109: 生成されたレジストラーコードをコンパイルできませんでした。 http://bugzilla.xamarin.com でバグレポートをファイルに登録してください

レジストラー用に生成されたコードをコンパイルできませんでした。 ビルドログには、コードがコンパイルされていない理由を説明する、ネイティブコンパイラからの出力が含まれます。

これは常に Xamarin のバグです。 iOS です。プロジェクトまたはテストケースで[github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT4110" />

### <a name="mt4110-the-registrar-cannot-marshal-the-out-parameter-of-type--in-signature-for-method-"></a>MT4110: レジストラーは、メソッド `*`のシグネチャの `*` 型の out パラメーターをマーシャリングできません。

<a name="MT4111" />

### <a name="mt4111-the-registrar-cannot-build-a-signature-for-type--in-method-"></a>MT4111: レジストラーは、メソッド `*`の `*` 型の署名を構築できません。

<a name="MT4112" />

### <a name="mt4112-the-registrar-found-an-invalid-type--registering-generic-types-with-objective-c-is-not-supported-and-may-lead-to-random-behavior-andor-crashes-for-backwards-compatibility-with-older-versions-of-xamarinios-it-is-possible-to-ignore-this-error-by-passing---unsupported--enable-generics-in-registrar-as-an-additional-mtouch-argument-in-the-projects-ios-build-options-page-see-developerxamarincomguidesiosadvanced_topicsregistrar-for-more-information"></a>MT4112: レジストラーで `*`無効な型が見つかりました。 旧バージョンの Xamarin との下位互換性を確保するために、ジェネリック型を目的の C に登録することはサポートされていません。また、(以前のバージョンの Xamarin との下位互換性のために) クラッシュする可能性があります。 iOS では、この `--unsupported--enable-generics-in-registrar` エラーを無視することができます。 詳細については、「 [developer.xamarin.com/guides/ios/advanced_topics/registrar](~/ios/internals/registrar.md) 」を参照してください)。

<a name="MT4113" />

### <a name="mt4113-the-registrar-found-a-generic-method--exporting-generic-methods-is-not-supported-and-will-lead-to-random-behavior-andor-crashes"></a>MT4113: レジストラーがジェネリックメソッドを検出しました: '\*。\*' です。 ジェネリックメソッドのエクスポートはサポートされていないため、ランダムな動作やクラッシュが発生する可能性があります。

<a name="MT4114" />

### <a name="mt4114-unexpected-error-in-the-registrar-for-the-method----please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4114: メソッド '\*のレジストラーで予期しないエラーが発生しています。\*'- http://bugzilla.xamarin.com でバグレポートをファイルに登録してください

<a name="MT4116" />

### <a name="mt4116-could-not-register-the-assembly--"></a>MT4116: アセンブリ '\*' を登録できませんでした: \*

<a name="MT4117" />

### <a name="mt4117-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4117: レジストラーは、メソッド '\*でシグネチャの不一致を検出しました。\*'-このセレクターは、メソッドがパラメーターを \* 受け取ることを示しますが、マネージメソッドには \* パラメーターがあります。

<a name="MT4118" />

### <a name="mt4118-cannot-register-two-managed-types--and--with-the-same-native-name-"></a>MT4118: 2 つのマネージ型 ('\*' と '\*') を同じネイティブ名 (' * ') で登録することはできません。

<a name="MT4119" />

### <a name="mt4119-could-not-register-the-selector--of-the-member--because-the-selector-is-already-registered-on-a-different-member"></a>MT4119: メンバー '\*. * ' のセレクター '\*' を登録できませんでした。セレクターが既に別のメンバーに登録されています。

<a name="MT4120" />

### <a name="mt4120-the-registrar-found-an-unknown-field-type--in-field--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4120: レジストラーで、フィールド '\*. * ' に不明なフィールドの種類 '\*' が見つかりました。 http://bugzilla.xamarin.com でバグレポートをファイルに登録してください

このエラーは、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT4121" />

### <a name="mt4121-cannot-use-gccg-to-compile-the-generated-code-from-the-static-registrar-when-using-the-accounts-framework-the-header-files-provided-by-apple-used-during-the-compilation-require-clang-either-use-clang---compilerclang-or-the-dynamic-registrar---registrardynamic"></a>MT4121: アカウントフレームワークを使用する場合、GCC/G + + を使用して静的レジストラーから生成されたコードをコンパイルすることはできません (コンパイル中に使用される Apple が提供するヘッダーファイルに Clang が必要です)。 Clang (--compiler: clang) または動的レジストラー (--レジストラー: dynamic) を使用します。

<a name="MT4122" />

### <a name="mt4122-cannot-use-the-clang-compiler-provided-in-the--sdk-to-compile-the-generated-code-from-the-static-registrar-when-non-ascii-type-names--are-present-in-the-application-either-use-gccg---compilergccg-the-dynamic-registrar---registrardynamic-or-a-newer-sdk"></a>MT4122: に用意されている Clang コンパイラは使用できません *。* アプリケーションに非 ASCII 型名 (' * ') が存在する場合に、生成されたコードを静的レジストラーからコンパイルするための SDK。 GCC/G + + (--compiler: GCC | G + +)、動的レジストラー (--レジストラー: dynamic)、またはそれより新しい SDK を使用します。

<a name="MT4123" />

### <a name="mt4123-the-type-of-the-variadic-parameter-in-the-variadic-function--must-be-systemintptr"></a>MT4123: 可変個引数関数 ' * ' の可変個引数パラメーターの型は、である必要があります。

<a name="MT4124" />

### <a name="mt4124-invalid--found-on--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4124: '\*' に無効な \* が見つかりました。 http://bugzilla.xamarin.com でバグレポートをファイルに登録してください

このエラーは、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT4125" />

### <a name="mt4125-the-registrar-found-an-invalid-type--in-signature-for-method--the-interface-must-have-a-protocol-attribute-specifying-its-wrapper-type"></a>MT4125: レジストラーで、メソッド '\*' の署名に無効な型 '\*' が見つかりました: インターフェイスには、そのラッパー型を指定する Protocol 属性が必要です。

<a name="MT4126" />

### <a name="mt4126-cannot-register-two-managed-protocols--and--with-the-same-native-name-"></a>MT4126: 2 つのマネージプロトコル ('\*' と '\*') を同じネイティブ名 (' * ') で登録することはできません。

<a name="MT4127" />

### <a name="mt4127-cannot-register-more-than-one-interface-method-for-the-method--which-is-implementing-"></a>MT4127: '\*' を実装しているメソッド '\*' に対して複数のインターフェイスメソッドを登録することはできません。

<a name="MT4128" />

### <a name="mt4128-the-registrar-found-an-invalid-generic-parameter-type--in-the-method--the-generic-parameter-must-have-an-nsobject-constraint"></a>MT4128: レジストラーで、メソッド '\*' に無効なジェネリックパラメーター型 '\*' が見つかりました。 ジェネリックパラメーターには ' NSObject ' 制約を指定しなければなりません。

<a name="MT4129" />

### <a name="mt4129-the-registrar-found-an-invalid-generic-return-type--in-the-method--the-generic-return-type-must-have-an-nsobject-constraint"></a>MT4129: レジストラーで、メソッド '\*' に無効なジェネリック戻り値の型 '\*' が見つかりました。 ジェネリック戻り値の型には ' NSObject ' 制約が必要です。

<a name="MT4130" />

### <a name="mt4130-the-registrar-cannot-export-static-methods-in-generic-classes-"></a>MT4130: レジストラーは、ジェネリッククラス (' * ') の静的メソッドをエクスポートできません。

<a name="MT4131" />

### <a name="mt4131-the-registrar-cannot-export-static-properties-in-generic-classes-"></a>MT4131: レジストラーは、ジェネリッククラス ('\*.\*') の静的プロパティをエクスポートできません。

<a name="MT4132" />

### <a name="mt4132-the-registrar-found-an-invalid-generic-return-type--in-the-property--the-return-type-must-have-an-nsobject-constraint"></a>MT4132: レジストラーで、プロパティ '\*' に無効なジェネリック戻り値の型 '\*' が見つかりました。 戻り値の型には ' NSObject ' 制約が必要です。

<a name="MT4133" />

### <a name="mt4133-cannot-construct-an-instance-of-the-type--from-objective-c-because-the-type-is-generic-runtime-exception"></a>MT4133: 型がジェネリックであるため、型 ' * ' のインスタンスを目的の C から構築することはできません。 [ランタイム例外]

<a name="MT4134" />

### <a name="mt4134-your-application-is-using-the--framework-which-isnt-included-in-the-ios-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-ios--while-youre-building-with-the-ios--sdk-please-select-a-newer-sdk-in-your-apps-ios-build-options"></a>MT4134: アプリケーションは、アプリのビルドに使用している iOS SDK に含まれていない '\*' フレームワークを使用しています (このフレームワークは ios \*で導入されましたが、ios \* SDK を使用してビルドしています)。アプリの iOS ビルドオプションで新しい SDK を選択してください。

<a name="MT4135" />

### <a name="mt4135-the-member--has-an-export-attribute-that-doesnt-specify-a-selector-a-selector-is-required"></a>MT4135: メンバー '\*。\*' には、セレクターを指定しない Export 属性があります。 セレクターが必要です。

<a name="MT4136" />

### <a name="mt4136-the-registrar-cannot-marshal-the-parameter-type--of-the-parameter--in-the-method-"></a>MT4136: レジストラーは、メソッド '\*. * ' のパラメーター '\*' のパラメーターの型 '\*' をマーシャリングできません

<!-- MT4137 is unused -->

<a name="MT4138" />

### <a name="mt4138-the-registrar-cannot-marshal-the-property-type--of-the-property-"></a>MT4138: レジストラーは、プロパティ '\*. * ' のプロパティ型 '\*' をマーシャリングできません。

<a name="MT4139" />

### <a name="mt4139-the-registrar-cannot-marshal-the-property-type--of-the-property--properties-with-the-connect-attribute-must-have-a-property-type-of-nsobject-or-a-subclass-of-nsobject"></a>MT4139: レジストラーは、プロパティ '\*. * ' のプロパティ型 '\*' をマーシャリングできません。 [Connect] 属性を持つプロパティには、NSObject (または NSObject のサブクラス) のプロパティの型を指定する必要があります。

<a name="MT4140" />

### <a name="mt4140-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-variadic-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4140: レジストラーは、メソッド '\*でシグネチャの不一致を検出しました。\*'-セレクターは、可変個引数メソッドがパラメーターを受け取る \* を示しますが、マネージメソッドには \* パラメーターがあります。

<a name="MT4141" />

### <a name="mt4141-cannot-register-the-selector--on-the-member--because-xamarinios-implicitly-registers-this-selector"></a>MT4141: このセレクターは、Xamarin によって暗黙的に登録されるため、メンバー '\*' にセレクター '\*' を登録できません。

これは、フレームワーク型をサブクラス化し、' retain '、' release '、または ' dealloc ' メソッドを実装しようとした場合に発生します。

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

ただし、クラスがフレームワーク型の最初のサブクラスでない場合は、これらのメソッドをオーバーライドできます。

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

この場合、Xamarin は `MyNSObject` クラスの `retain`、`release`、および `dealloc` をオーバーライドし、競合はありません。

<a name="MT4142" />

### <a name="mt4142-failed-to-register-the-type-"></a>MT4142: 型 ' * ' を登録できませんでした。

<a name="MT4143" />

### <a name="mt4143-the-objectivec-class--could-not-be-registered-it-does-not-seem-to-derive-from-any-known-objectivec-class-including-nsobject"></a>MT4143: 対象クラス ' * ' を登録できませんでした。既知のすべての Ec クラス (NSObject を含む) から派生していないようです。

<a name="MT4144" />

### <a name="mt4144-cannot-register-the-method--since-it-does-not-have-an-associated-trampoline-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4144: メソッド ' * ' は、関連付けられている trampoline がないため、登録できません。 http://bugzilla.xamarin.comでバグレポートをファイルに登録してください。

これは、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT4145" />

### <a name="mt4145-invalid-enum--enums-with-the-native-attribute-must-have-a-underlying-enum-type-of-either-long-or-ulong"></a>MT4145: 無効な enum ' * ': [Native] 属性を持つ列挙型には、' long ' または ' ulong ' の基になる列挙型を指定しなければなりません。

<a name="MT4146" />

### <a name="mt4146-the-name-parameter-of-the-registrar-attribute-on-the-class---contains-an-invalid-character--"></a>MT4146: クラス '\*' ('\*') のレジストラー属性の Name パラメーターに無効な文字 '\*' が含まれています: ' ' (\*)。

Objectice-C クラスの名前に空白を含めることはできません。これは、対応するマネージ `Name` クラスの `Register` 属性に空白を含めることができないことを意味します。

エラーメッセージに示されているマネージクラスの `Register` 属性に空白が含まれていないことを確認してください。

<a name="MT4147" />

### <a name="mt4147-detected-a-protocol-inheriting-from-the-jsexport-protocol-while-using-the-dynamic-registrar-it-is-not-possible-to-export-protocols-to-javascriptcore-dynamically-the-static-registrar-must-be-used-add---registrarstatic-to-the-additional-mtouch-arguments-in-the-projects-ios-build-options-to-select-the-static-registrar"></a>MT4147: 動的レジストラーを使用しているときに、JSExport プロトコルを継承するプロトコルを検出しました。 プロトコルを JavaScriptCore に動的にエクスポートすることはできません。静的レジストラーを使用する必要があります (静的レジストラーを選択するには、プロジェクトの iOS ビルドオプションの追加の mtouch 引数に '--レジストラー: static を追加します)。

<a name="MT4148" />

### <a name="mt4148-the-registrar-found-a-generic-protocol--exporting-generic-protocols-is-not-supported"></a>MT4148: レジストラーが汎用プロトコルを検出しました: ' * '。 汎用プロトコルのエクスポートはサポートされていません。

<a name="MT4149" />

### <a name="mt4149-cannot-register-the-method--because-the-type-of-the-first-parameter--does-not-match-the-category-type-"></a>MT4149: メソッド '\*を登録できません。最初のパラメーター ('\*') の型がカテゴリの型 ('\*') と一致しないため、\*' になります。

<a name="MT4150" />

### <a name="mt4150-cannot-register-the-type--because-the-type-property--in-its-category-attribute-does-not-inherit-from-nsobject"></a>MT4150: Category 属性の Type プロパティ ('\*') が NSObject から継承していないため、型 '\*' を登録できません。

<a name="MT4151" />

### <a name="mt4151-cannot-register-the-type--because-the-type-property-in-its-category-attribute-isnt-set"></a>MT4151: Category 属性の Type プロパティが設定されていないため、型 ' * ' を登録できません。

<a name="MT4152" />

### <a name="mt4152-cannot-register-the-type--as-a-category-because-it-implements-inativeobject-or-subclasses-nsobject"></a>MT4152: INativeObject またはサブクラス NSObject を実装しているため、型 ' * ' をカテゴリとして登録できません。

<a name="MT4153" />

### <a name="mt4153-cannot-register-the-type--as-a-category-because-its-generic"></a>MT4153: ジェネリックであるため、型 '\*' をカテゴリとして登録できません。

<a name="MT4154" />

### <a name="mt4154-cannot-register-the-method--as-a-category-method-because-its-generic"></a>MT4154: メソッド '\*' はジェネリックであるため、カテゴリメソッドとして登録できません。

<a name="MT4155" />

### <a name="mt4155-cannot-register-the-method--with-the-selector--as-a-category-method-on--because-the-objective-c-already-has-an-implementation-for-this-selector"></a>MT4155: セレクター '\*' を持つメソッド '\*' を ' * ' 上のカテゴリメソッドとして登録できません。これは、目的 C が既にこのセレクターの実装を持っているためです。

<a name="MT4156" />

### <a name="mt4156-cannot-register-two-categories--and--with-the-same-native-name-"></a>MT4156: 2 つのカテゴリ ('\*' と '\*') を同じネイティブ名 (' * ') で登録することはできません。

<a name="MT4157" />

### <a name="mt4157-cannot-register-the-category-method--because-at-least-one-parameter-is-required-and-its-type-must-match-the-category-type-"></a>MT4157: 少なくとも1つのパラメーターが必要ですが (その型がカテゴリの型 '\*' と一致する必要があります)、カテゴリメソッド '\*' を登録できません

<a name="MT4158" />

### <a name="mt4158-cannot-register-the-constructor--in-the-category--because-constructors-in-categories-are-not-supported"></a>MT4158: カテゴリ \* のコンストラクターはサポートされていないため、\* のコンストラクターをカテゴリに登録できません。

<a name="MT4159" />

### <a name="mt4159-cannot-register-the-method--as-a-category-method-because-category-methods-must-be-static"></a>MT4159: カテゴリメソッドが静的である必要があるため、メソッド ' * ' をカテゴリメソッドとして登録できません。

<a name="MT4160" />

### <a name="mt4160-invalid-character---found-in-selector--for-"></a>MT4160: '\*' のセレクター '\*' に無効な文字 '\*' (\*) が見つかりました。

<a name="MT4161" />

### <a name="mt4161-the-registrar-found-an-unsupported-structure--all-fields-in-a-structure-must-also-be-structures-field--with-type-2-is-not-a-structure"></a>MT4161: レジストラーでサポートされていない構造 '\*' が見つかりました: 構造体内のすべてのフィールドは構造体でなければなりません (型 '{2}' のフィールド '\*' は構造体ではありません)。

レジストラーにより、サポートされていないフィールドを含む構造体が見つかりました。

また、目的の C に公開されている構造体のすべてのフィールドは、(クラスではなく) 構造体である必要があります。

<a name="MT4162" />

### <a name="mt4162-the-type--used-as--2-is-not-available-in---it-was-introduced-in---please-build-with-a-newer--sdk-usually-done-by-using-the-most-recent-version-of-xcode"></a>MT4162: 型 '\*' (* {2}として使用) は、* * では使用できません (* * で導入されました)\* 新しい * SDK でビルドしてください (通常は、最新バージョンの Xcode を使用します。

レジストラーで、現在の SDK に含まれていない型が検出されました。

Xcode をアップグレードしてください。

<a name="MT4163" />

### <a name="mt4163-internal-error-in-the-registrar--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4163: レジストラー (*) で内部エラーが発生しています。 http://bugzilla.xamarin.com でバグレポートをファイルに登録してください

このエラーは、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT4164" />

### <a name="mt4164-cannot-export-the-property--because-its-selector--is-an-objective-c-keyword-please-use-a-different-name"></a>MT4164: セレクター '\*' がキーワードであるため、プロパティ '\*' をエクスポートできません。 別の名前を使用してください。

対象のプロパティのセレクターが、有効な目標 C 識別子ではありません。

セレクターとして有効な目標 C 識別子を使用してください。

<a name="MT4165" />

### <a name="mt4165-the-registrar-couldnt-find-the-type-systemvoid-in-any-of-the-referenced-assemblies"></a>MT4165: レジストラーは、参照されたアセンブリのいずれかで型 ' System.void ' を見つけることができませんでした。

このエラーは、多くの場合、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT4166" />

### <a name="mt4166-cannot-register-the-method--because-the-signature-contains-a-type--that-isnt-a-reference-type"></a>MT4166: メソッド '\*' を登録できません。シグネチャには参照型ではない型 (\*) が含まれています。

これは通常、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT4167" />

### <a name="mt4167-cannot-register-the-method--because-the-signature-contains-a-generic-type--with-a-generic-argument-type-that-isnt-an-nsobject-subclass-"></a>MT4167: メソッド '\*' を登録できません。シグネチャには、NSObject サブクラス (*) ではない汎用引数型を持つジェネリック型 (\*) が含まれています。

これは通常、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT4168" />

### <a name="mt4168-cannot-register-the-type-managed_name-because-its-objective-c-name-exported_name-is-an-objective-c-keyword-please-use-a-different-name"></a>MT4168: 型 ' {managed\_name} ' を登録できません。目標 C 名 ' {エクスポートされた\_名前} ' がキーワードです。 別の名前を使用してください。

対象の型の目的の C 名が、有効な目標 C 識別子ではありません。

有効な目標 C 識別子を使用してください。

<a name="MT4169" />

### <a name="mt4169-failed-to-generate-a-pinvoke-wrapper-for-method-message"></a>MT4169: {method} の P/Invoke ラッパーを生成できませんでした: {message}

Xamarin. iOS は、前述のの P/Invoke ラッパー関数を生成できませんでした。
報告されたエラーメッセージで根底にある原因を確認してください。

<a name="MT4170" />

### <a name="mt4170-the-registrar-cant-convert-from-managed-type-to-native-type-for-the-return-value-in-the-method-method"></a>MT4170: レジストラーは ' {managed type} ' から ' {native type} ' に変換できません。メソッド {method} の戻り値を使用してください。

エラー <a href="#MT4172">MT4172</a>の説明を参照してください。

<a name="MT4171" />

### <a name="mt4171-the-bindas-attribute-on-the-member-member-is-invalid-the-bindas-type-type-is-different-from-the-property-type-type"></a>MT4171: メンバー {member} の BindAs 属性が無効です。 BindAs の種類 {type} がプロパティの型 {type} と異なります。

BindAs 属性の型が、それがアタッチされているメンバーの型と一致していることを確認してください。

<a name="MT4172" />

### <a name="mt4172-the-registrar-cant-convert-from-native-type-to-managed-type-for-the-parameter-parameter-name-in-the-method-method"></a>MT4172: レジストラーは、メソッド {method} のパラメーター ' {parameter name} ' に対して ' {native type} ' から ' {managed type} ' に変換できません。

レジストラーは、前述の型間の変換をサポートしていません。

問題の API が Xamarin. iOS によって提供されている場合、これは Xamarin. iOS のバグです。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

ネイティブライブラリのバインドプロジェクトを開発しているときにこの操作を実行すると、型の新しい組み合わせのサポートが追加されます。 この場合は、テストケースを使用して[github](https://github.com/xamarin/xamarin-macios/issues/new)で拡張要求をファイルに記入し、評価します。

## <a name="mt5xxx-gcc-and-toolchain-error-messages"></a>MT5xxx: GCC とツールチェーンのエラーメッセージ

### <a name="mt51xx-compilation"></a>MT51xx: コンパイル

<!--
 MT5xxx GCC and toolchain
  MT51xx compilation
  -->

<a name="MT5101" />

### <a name="mt5101-missing--compiler-please-install-xcode-command-line-tools-component"></a>MT5101: ' * ' コンパイラがありません。 Xcode ' コマンドラインツール ' コンポーネントをインストールしてください

<a name="MT5102" />

### <a name="mt5102-failed-to-assemble-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5102: ファイル ' * ' をアセンブルできませんでした。 http://bugzilla.xamarin.com でバグレポートをファイルに登録してください

<a name="MT5103" />

### <a name="mt5103-failed-to-compile-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5103: ファイル ' * ' をコンパイルできませんでした。 http://bugzilla.xamarin.com でバグレポートをファイルに登録してください

<a name="MT5104" />

### <a name="mt5104-could-not-find-neither-the--nor-the--compiler-please-install-xcode-command-line-tools-component"></a>MT5104: '\*' と '\*' コンパイラのどちらも見つかりませんでした。 Xcode ' コマンドラインツール ' コンポーネントをインストールしてください

<!-- 5105 is used by mmp -->

<a name="MT5106" />

### <a name="mt5106-could-not-compile-the-files--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5106: ファイル ' * ' をコンパイルできませんでした。 http://bugzilla.xamarin.com でバグレポートをファイルに登録してください

これは通常、Xamarin のバグであることを示します。[github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

### <a name="mt52xx-linking"></a>MT52xx: リンク

<!--
  MT52xx linking
  -->

<a name="MT5201" />

### <a name="mt5201-native-linking-failed-please-review-the-build-log-and-the-user-flags-provided-to-gcc-"></a>MT5201: ネイティブリンクに失敗しました。 ビルドログと gcc: * に提供されているユーザーフラグを確認してください。

<a name="MT5202" />

### <a name="mt5202-native-linking-failed-please-review-the-build-log"></a>MT5202: ネイティブリンクに失敗しました。 ビルドログを確認してください。

<a name="MT5203" />

### <a name="mt5203-native-linking-warning-"></a>MT5203: ネイティブリンクの警告: *

<!--- 5204-5208 are not used -->

<a name="MT5209" />

### <a name="mt5209-native-linking-error-"></a>MT5209: ネイティブリンクエラー: *

<a name="MT5210" />

### <a name="mt5210-native-linking-failed-undefined-symbol--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-are-properly-linked-in"></a>MT5210: ネイティブリンクに失敗しました。未定義のシンボル: *。 必要なすべてのフレームワークが参照されており、ネイティブライブラリが正しくリンクされていることを確認してください。

このエラーは、ネイティブリンカーがどこかで参照されているシンボルを見つけられない場合に発生します。 これにはいくつかの原因が考えられます。

- サードパーティのバインドにはフレームワークが必要ですが、バインドでは、その `[LinkWith]` 属性では指定されていません。 ソリューション:
  - サードパーティのバインドの作成者である場合、またはソースへのアクセス権を持っている場合は、バインドの `[LinkWith]` 属性を変更して、必要なフレームワークを含めます。

    ```csharp
    [LinkWith ("mylib.a", Frameworks = "SystemConfiguration")]
    ```

  - サードパーティのバインドを変更できない場合は、`-gcc_flags '-framework SystemFramework'` を `mtouch` に渡すことによって、必要なフレームワークと手動でリンクすることができます (これは、プロジェクトの iOS ビルドオプションページで追加の mtouch 引数を変更することによって行われます)。 これは、すべてのプロジェクト構成に対して実行する必要があることに注意してください。
- 場合によっては、マネージバインドが複数のネイティブライブラリで構成されており、すべてがバインドに含まれている必要があります。 各バインドプロジェクトに複数のネイティブライブラリを含めることができるので、ソリューションでは、必要なすべてのネイティブライブラリをバインドプロジェクトに追加するだけです。</li>
- マネージバインディングは、ネイティブライブラリに存在しないネイティブシンボルを参照します。
    これは通常、バインドがしばらく存在し、その間にネイティブコードが変更されて、バインドが更新されていない状態で特定のネイティブクラスが削除または名前変更された場合に発生します。
- P/Invoke が、存在しないネイティブシンボルを参照しています。 Xamarin. iOS 7.4 では、この場合に<a href="#MT5214">MT5214</a>エラーが報告されます (詳細については、MT5214 を参照してください)。
- サードパーティのバインド/ライブラリはを使用しC++て構築されましたが、その `[LinkWith]` 属性ではバインドによって指定されていません。 シンボルは破損C++したシンボルであるため、通常、これは非常に簡単に認識できます (1 つの一般的な例は `__ZNKSt9exception4whatEv`)。
  - サードパーティのバインドの作成者である場合、またはソースへのアクセス権を持っている場合は、バインドの `[LinkWith]` 属性を変更して、`IsCxx` フラグを設定します。

    ```csharp
    [LinkWith ("mylib.a", IsCxx = true)]
    ```

  - サードパーティのバインドを変更できない場合、またはサードパーティのライブラリと手動でリンクしている場合は、`-cxx` を mtouch に渡すことによって、同等のフラグを設定できます (これは、プロジェクトの iOS ビルドオプションページで追加の mtouch 引数を変更することによって行います)。 これは、すべてのプロジェクト構成に対して実行する必要があることに注意してください。

<a name="MT5211" />

### <a name="mt5211-native-linking-failed-undefined-objective-c-class--the-symbol--could-not-be-found-in-any-of-the-libraries-or-frameworks-linked-with-your-application"></a>MT5211: ネイティブリンクに失敗しました。未定義の目標-C クラス: \*。 アプリケーションにリンクされているライブラリまたはフレームワークで、シンボル '\*' が見つかりませんでした。

これは、ネイティブリンカーがどこかで参照されている目的 C クラスを見つけられない場合に発生します。 これにはいくつかの原因が考えられます。 [MT5210](#MT5210)の場合と同様です。

- サードパーティのバインドは、目標 C プロトコルにバインドされましたが、その api 定義の `[Protocol]` 属性で注釈を付けませんでした。 ソリューション:
  - 不足している `[Protocol]` 属性を追加します。

    ```csharp
    [BaseType (typeof (NSObject))]
    [Protocol] // Add this
    public interface MyProtocol
    {
    }
    ```

<a name="MT5212" />

### <a name="mt5212-native-linking-failed-duplicate-symbol-"></a>MT5212: ネイティブリンクに失敗しました。重複するシンボル: *。

これは、ネイティブリンカーがすべてのネイティブライブラリ間で重複するシンボルを検出した場合に発生します。 このエラーの後に、シンボルの出現箇所ごとに1つ以上の[MT5213](#MT5213)エラーが発生する可能性があります。 このエラーの考えられる原因:

- 同じネイティブライブラリが2回含まれています。
- 同じシンボルを定義するために、2つの異なるネイティブライブラリが発生します。
- ネイティブライブラリが正しく作成されておらず、同じシンボルが複数回含まれています。
  これを確認するには、ターミナルで次のコマンドセットを使用します (作成しているアーキテクチャに応じて i386 を x86_64/armv7/armv7s/arm64 に置き換えます)。

  ```
  # Native libraries are usually fat libraries, containing binary code for
  # several architectures in the same file. First we extract the binary
  # code for the architecture we're interested in.
  lipo libNative.a -thin i386 -output libNative.i386.a

  # Now query the native library for the duplicated symbol.
  nm libNative.i386.a | fgrep 'SYMBOL'

  # You can also list the object files inside the native library.
  # In most cases this will reveal duplicated object files.
  ar -t libNative.i386.a
  ```

  この問題を解決するには、いくつかの方法があります。

  - ネイティブライブラリのプロバイダーがそれを修正し、更新されたバージョンを提供するように要求します。
  - 余分なオブジェクトファイルを削除して自分で修正します (これは、問題が実際に複製されたオブジェクトファイル内にある場合にのみ機能します)

  ```
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
  ```

  - 使用されていないコードを削除するようにリンカーに依頼します。 次のすべての条件が満たされた場合、Xamarin は自動的にこれを実行します。
    - すべてのサードパーティバインドの `[LinkWith]` 属性で SmartLink が有効になりました。

      ```csharp
      [assembly: LinkWith ("libNative.a", SmartLink = true)]
      ```

    - Mtouch に `-gcc_flags` が渡されません (プロジェクトの iOS ビルドオプションの [追加の mtouch 引数] フィールド)。
    - また、プロジェクトの iOS ビルドオプションの追加の mtouch 引数に `-gcc_flags -dead_strip` を追加することにより、使用されていないコードを削除するようにリンカーに直接要求することもできます。

<a name="MT5213" />

### <a name="mt5213-duplicate-symbol-in--location-related-to-previous-error"></a>MT5213: * (前のエラーに関連する場所) にシンボルが重複しています

このエラーは、 [MT5212](#MT5212)と共に報告されます。 詳細については、 [MT5212](#MT5212)を参照してください。

<a name="MT5214" />

### <a name="mt5214-native-linking-failed-undefined-symbol--this-symbol-was-referenced-the-managed-member--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-linked"></a>MT5214: ネイティブリンクに失敗しました。未定義のシンボル: *。 このシンボルはマネージドメンバー * を参照しました。 必要なすべてのフレームワークが参照されていて、ネイティブライブラリがリンクされていることを確認してください。

このエラーは、マネージコードに、存在しないネイティブメソッドへの P/Invoke が含まれている場合に報告されます。 例 :

```csharp
using System.Runtime.InteropServices;
class MyImports {
   [DllImport ("__Internal")]
   public static extern void MyInexistentFunc ();
}
```

考えられる解決策はいくつかあります。

- ソースコードから、問題の P/Invoke を削除します。
- すべてのアセンブリに対してマネージリンカーを有効にします (これは、"リンカーの動作" を "すべてのアセンブリ" に設定することにより、プロジェクトの iOS ビルドオプションで実行されます)。 これにより、(前の点とは異なり、手動ではなく、自動的に) アプリから使用しないすべての P/Invoke が削除されます。 欠点は、シミュレーターのビルドに多少の時間がかかることです。リフレクションを使用していると、アプリが壊れる可能性があります。リンカーに関する詳細については、[こちら](~/ios/deploy-test/linker.md)を参照してください)。
- 存在しないネイティブシンボルを含む2番目のネイティブライブラリを作成します。 これは単なる回避策であることに注意してください (これらの関数を呼び出そうとすると、アプリがクラッシュします)。

<a name="MT5215" />

### <a name="mt5215-references-to--might-require-additional--frameworkxxx-or--lxxx-instructions-to-the-native-linker"></a>MT5215: ' * ' への参照には、ネイティブリンカーに対する追加の-framework = XXX または-lXXX 命令が必要になる場合があります

これは、問題のライブラリを参照するために P/Invoke が検出されたが、アプリがそれとリンクされていないことを示す警告です。

<a name="MT5216" />

### <a name="mt5216-native-linking-failed-for--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5216: * のネイティブリンクに失敗しました。 http://bugzilla.xamarin.com でバグレポートをファイルに登録してください

このエラーは、AOT コンパイラからの出力をリンクするときに報告されます。

このエラーは、多くの場合、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT5217" />

### <a name="mt5217-native-linking-possibly-failed-because-the-linker-command-line-was-too-long--characters"></a>MT5217: リンカーコマンドラインが長すぎるため (* 文字)、ネイティブリンクが失敗する可能性があります。

ネイティブリンクに失敗しました。リンカーコマンドが長すぎるため、このエラーが発生する可能性があります。

多くの場合、Xamarin. iOS プロジェクトはネイティブシンボルを動的に参照します。つまり、ネイティブリンカーは、これらのシンボルが使用されていることを認識しないため、ネイティブリンカーがネイティブシンボルを削除する可能性があります。

通常、Xamarin は、ネイティブリンカーに `-u symbol` リンカーフラグを使用してこのような記号を保持するように要求しますが、そのような記号が多数ある場合は、コマンドライン全体がオペレーティングシステムで指定されているコマンドラインの最大長を超えている可能性があります。

このような動的シンボルには、いくつかのソースが考えられます。

- P/は、静的にリンクされたライブラリ内のメソッドに対して呼び出します (この場合、dll 名は DllImport 属性 `[DllImport ("__Internal")]`で `__Internal` ます)。
- バインドプロジェクト (`[Field]` 属性) からの静的にリンクされたライブラリ内のメモリ位置へのフィールド参照。
- バインドプロジェクトから静的にリンクされたライブラリで参照される (インクリメンタルビルドを使用している場合、または静的レジスタを使用していない場合)、目的の C クラス。

考えられる解決策:

- マネージリンカーを有効にします (SDK アセンブリだけではなく、すべてのアセンブリで可能な場合)。 これにより、リンカーのコマンドラインが最大値を超えないように、動的シンボルのソースが不足する可能性があります。
- P/Invoke、フィールド参照、または目標 C クラスの数を減らします。
- 短い名前を持つように動的シンボルを書き直してください。
- プロジェクトの iOS ビルドオプションで、追加の mtouch 引数として `-dlsym:false` を渡します。 このオプションを使用すると、Xamarin. iOS は AOT でコンパイルされたコード内にネイティブ参照を生成します。このシンボルを保持するようにリンカーに要求する必要はありません。 ただし、これはデバイスビルドに対してのみ機能し、静的ライブラリに存在しない関数に対して P/Invoke がある場合、リンカーエラーが発生します。
- プロジェクトの iOS ビルドオプションで、追加の mtouch 引数として `--dynamic-symbol-mode=code` を渡します。 このオプションを使用すると、コマンドライン引数を使用してこれらのシンボルを保持するようにネイティブリンカーに指示する代わりに、これらのシンボルを参照する追加のネイティブコードが生成されます。 この方法の欠点は、実行可能ファイルのサイズが多少大きくなることです。
- プロジェクトの iOS ビルドオプションで追加の mtouch 引数として `--registrar:static` を渡すことによって、静的レジストラーを有効にします (シミュレータービルドの場合は、静的レジストラーが既にデバイスビルドの既定値であるため)。 静的レジストラーは、目的の C クラスを静的に参照するコードを生成するため、ネイティブリンカーにこのようなクラスを保持するように要求する必要はありません。
- インクリメンタルビルドを無効にします (デバイスビルドの場合)。 インクリメンタルビルドが有効になっている場合、静的レジストラーによって生成されるコードはネイティブリンカーによって考慮されません。つまり、Xamarin では、参照されている目的 C クラスを引き続きリンカーに要求する必要があります。 そのため、インクリメンタルビルドを無効にすると、このニーズを回避できます。

<a name="MT5218" />

### <a name="mt5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MT5218: 動的シンボルとして検出されなかったため、動的シンボル {symbol} (--ignore-dynamic-symbol}) を無視できません。

コマンドライン引数 `--ignore-dynamic-symbol=symbol` が渡されましたが、このシンボルは、手動で保存する必要がある動的シンボルとして認識されたシンボルではありません。

これには主に次の2つの理由があります。

- シンボル名が正しくありません。
  - シンボル名にアンダースコアを付加しないでください。
  - 目的の C クラスのシンボルは `OBJC_CLASS_$_<classname>`です。
- シンボルは正しいものですが、通常の方法で既に保持されているシンボルです (一部のビルドオプションでは、動的シンボルの正確な一覧が異なります)。

### <a name="mt53xx-other-tools"></a>MT53xx: その他のツール

<!--
  MT53xx other tools
  -->

<a name="MT5301" />

### <a name="mt5301-missing-strip-tool-please-install-xcode-command-line-tools-component"></a>MT5301: ' strip ' ツールがありません。 Xcode ' コマンドラインツール ' コンポーネントをインストールしてください

<a name="MT5302" />

### <a name="mt5302-missing-dsymutil-tool-please-install-xcode-command-line-tools-component"></a>MT5302: ' dsymutil ' ツールがありません。 Xcode ' コマンドラインツール ' コンポーネントをインストールしてください

<a name="MT5303" />

### <a name="mt5303-failed-to-generate-the-debug-symbols-dsym-directory-please-review-the-build-log"></a>MT5303: デバッグシンボル (dSYM ディレクトリ) を生成できませんでした。 ビルドログを確認してください。

デバッグシンボルを作成するために、最後のアプリケーションディレクトリで dsymutil を実行中にエラーが発生しました。 Dsymutil の出力を確認し、それを修正する方法を確認するには、ビルドログを確認してください。

<a name="MT5304" />

### <a name="mt5304-failed-to-strip-the-final-binary-please-review-the-build-log"></a>MT5304: 最終バイナリを削除できませんでした。 ビルドログを確認してください。

' Strip ' ツールを実行して、アプリケーションからデバッグ情報を削除するときにエラーが発生しました。

<a name="MT5305" />

### <a name="mt5305-missing-lipo-tool-please-install-xcode-command-line-tools-component"></a>MT5305: ' lipo ' ツールがありません。 Xcode ' コマンドラインツール ' コンポーネントをインストールしてください

<a name="MT5306" />

### <a name="mt5306-failed-to-create-the-a-fat-library-please-review-the-build-log"></a>MT5306: fat ライブラリを作成できませんでした。 ビルドログを確認してください。

' Lipo ' ツールの実行中にエラーが発生しました。 ビルドログを確認して、' lipo ' によって報告されたエラーを確認してください。

<a name="MT5307" />

### <a name="mt5307-failed-to-sign-the-executable-please-review-the-build-log"></a>MT5307: 実行可能ファイルに署名できませんでした。 ビルドログを確認してください。

アプリケーションの署名中にエラーが発生しました。 ビルドログを確認して、' codesign ' によって報告されたエラーを確認してください。

<!-- 5308 is used by mmp -->
<!-- 5309 is used by mmp -->
<!-- 5310 is used by mmp -->

## <a name="mt6xxx-mtouch-internal-tools-error-messages"></a>MT6xxx: mtouch 内部ツールのエラーメッセージ

### <a name="mt600x-stripper"></a>MT600x: Stripper

<!--
 MT6xxx mtouch internal tools
  MT600x Stripper
  -->

<a name="MT6001" />

### <a name="mt6001-running-version-of-cecil-doesnt-support-assembly-stripping"></a>MT6001: Cecil の実行バージョンはアセンブリの削除をサポートしていません

<a name="MT6002" />

### <a name="mt6002-could-not-strip-assembly-"></a>MT6002: アセンブリ `*`を削除できませんでした。

アプリケーションのアセンブリからマネージコード (IL コードを削除) を削除するときにエラーが発生しました。

<a name="MT6003" />

### <a name="mt6003-unauthorizedaccessexception-message"></a>MT6003: [System.unauthorizedaccessexception message]

アプリケーションからデバッグシンボルを削除しているときに、セキュリティエラーが発生しました。

## <a name="mt7xxx-msbuild-error-messages"></a>MT7xxx: MSBuild のエラーメッセージ

<!--
 MT7xxx msbuild errors
  -->

<a name="MT7001" />

### <a name="mt7001-could-not-resolve-host-ips-for-wifi-debugger-settings"></a>MT7001: WiFi デバッガー設定のホスト Ip を解決できませんでした。

*MSBuild タスク: 検出 Debugnetworkconfigurationtaskbase*

トラブルシューティングの手順:

- `csharp -e 'System.Net.Dns.GetHostEntry (System.Net.Dns.GetHostName ()).AddressList'` の実行を試みます (これにより、エラーではなく IP アドレスを指定する必要があります)。
- "ping \`hostname\`" を実行してみてください。次のような詳細情報が表示される可能性があり `cannot resolve MyHost.local: Unknown host`

場合によっては、これが "ローカルネットワーク" の問題であり、`/etc/hosts`に不明なホスト `127.0.0.1    MyHost.local` を追加することで対処できます。

<a name="MT7002" />

### <a name="mt7002-this-machine-does-not-have-any-network-adapters-this-is-required-when-debugging-or-profiling-on-device-over-wifi"></a>MT7002: このコンピューターにはネットワークアダプターがありません。 これは、デバイス上で WiFi 経由でデバッグまたはプロファイリングを行う場合に必要です。

*MSBuild タスク: 検出 Debugnetworkconfigurationtaskbase*

<a name="MT7003" />

### <a name="mt7003-the-app-extension--does-not-contain-an-infoplist"></a>MT7003: アプリ拡張機能 ' * ' には、情報 plist が含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7004" />

### <a name="mt7004-the-app-extension--does-not-specify-a-cfbundleidentifier"></a>MT7004: アプリ拡張機能 ' * ' は CFBundleIdentifier を指定していません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7005" />

### <a name="mt7005-the-app-extension--does-not-specify-a-cfbundleexecutable"></a>MT7005: アプリ拡張機能 ' * ' は CFBundleExecutable を指定していません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7006" />

### <a name="mt7006-the-app-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7006: アプリ拡張機能 '\*' に無効な CFBundleIdentifier (\*) が含まれています。メインアプリバンドルの CFBundleIdentifier (*) で開始されていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7007" />

### <a name="mt7007-the-app-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>MT7007: アプリ拡張機能 '\*' には、無効なサフィックス ". key" で終わる CFBundleIdentifier (\*) が含まれています。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7008" />

### <a name="mt7008-the-app-extension--does-not-specify-a-cfbundleshortversionstring"></a>MT7008: アプリ拡張機能 ' * ' は CFBundleShortVersionString を指定していません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7009" />

### <a name="mt7009-the-app-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>MT7009: アプリ拡張機能 ' * ' に無効な情報が含まれています。 plist: NSExtension ディクショナリが含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7010" />

### <a name="mt7010-the-app-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionpointidentifier-value"></a>MT7010: アプリ拡張機能 ' * ' に無効な情報が含まれています。 plist: NSExtension ディクショナリに NSExtensionPointIdentifier 値が含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7011" />

### <a name="mt7011-the-watchkit-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionattributes-dictionary"></a>MT7011: WatchKit Extension ' * ' に無効な情報が含まれています。 plist: NSExtension ディクショナリには、NSExtensionAttributes 辞書が含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7012" />

### <a name="mt7012-the-watchkit-extension--does-not-have-exactly-one-watch-app"></a>MT7012: WatchKit Extension ' * ' には、完全に1つの watch アプリがありません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7013" />

### <a name="mt7013-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-must-contain-the-watch-companion-capability"></a>MT7013: WatchKit Extension ' * ' に無効な情報が含まれています。 plist: UIRequiredDeviceCapabilities には ' watch コンパニオン ' 機能が含まれている必要があります。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7014" />

### <a name="mt7014-the-watch-app--does-not-contain-an-infoplist"></a>MT7014: ウォッチアプリ ' * ' には、情報 plist が含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7015" />

### <a name="mt7015-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>MT7015: ウォッチアプリ ' * ' は CFBundleIdentifier を指定していません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7016" />

### <a name="mt7016-the-watch-app--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7016: Watch アプリ '\*' に無効な CFBundleIdentifier (\*) が含まれています。メインアプリバンドルの CFBundleIdentifier (*) で開始されていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7017" />

### <a name="mt7017-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected-watch-4-but-found--"></a>MT7017: Watch アプリ '\*' に有効な UIDeviceFamily 値がありません。 ' Watch (4) ' が必要ですが、'\* (*) ' が見つかりました。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7018" />

### <a name="mt7018-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>MT7018: ウォッチアプリ ' * ' は CFBundleExecutable を指定していません

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7019" />

### <a name="mt7019-the-watch-app--has-an-invalid-wkcompanionappbundleidentifier-value--it-does-not-match-the-main-app-bundles-cfbundleidentifier-"></a>MT7019: Watch アプリ '\*' に無効な WKCompanionAppBundleIdentifier 値 ('\*') が含まれています。これはメインアプリバンドルの CFBundleIdentifier (' * ') と一致しません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7020" />

### <a name="mt7020-the-watch-app--has-an-invalid-infoplist-the-wkwatchkitapp-key-must-be-present-and-have-a-value-of-true"></a>MT7020: ウォッチアプリ ' * ' に無効な情報が含まれています。 plist: WKWatchKitApp キーが存在し、値が ' true ' である必要があります。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7021" />

### <a name="mt7021-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>MT7021: ウォッチアプリ ' * ' に無効な情報が含まれています。 plist: Lsrequiresi Os キーが存在していてはなりません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7022" />

### <a name="mt7022-the-watch-app--does-not-contain-a-watch-extension"></a>MT7022: Watch アプリ ' * ' に Watch 拡張機能が含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7023" />

### <a name="mt7023-the-watch-extension--does-not-contain-an-infoplist"></a>MT7023: Watch 拡張機能 ' * ' には、情報 plist が含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7024" />

### <a name="mt7024-the-watch-extension--does-not-specify-a-cfbundleidentifier"></a>MT7024: Watch 拡張機能 ' * ' は CFBundleIdentifier を指定していません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7025" />

### <a name="mt7025-the-watch-extension--does-not-specify-a-cfbundleexecutable"></a>MT7025: Watch 拡張機能 ' * ' は CFBundleExecutable を指定していません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7026" />

### <a name="mt7026-the-watch-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7026: Watch 拡張機能 '\*' に無効な CFBundleIdentifier (\*) が含まれています。メインアプリバンドルの CFBundleIdentifier (*) で開始されていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7027" />

### <a name="mt7027-the-watch-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>MT7027: Watch 拡張機能 '\*' には、無効なサフィックス ". key" で終わる CFBundleIdentifier (\*) が含まれています。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7028" />

### <a name="mt7028-the-watch-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>MT7028: Watch 拡張機能 ' * ' に無効な情報が含まれています。 plist: NSExtension ディクショナリが含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7029" />

### <a name="mt7029-the-watch-extension--has-an-invalid-infoplist-the-nsextensionpointidentifier-must-be-comapplewatchkit"></a>MT7029: Watch 拡張機能 ' * ' に無効な情報が含まれています。 plist: NSExtensionPointIdentifier は "watchkit" である必要があります。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7030" />

### <a name="mt7030-the-watch-extension--has-an-invalid-infoplist-the-nsextension-dictionary-must-contain-nsextensionattributes"></a>MT7030: Watch 拡張機能 ' * ' に無効な情報が含まれています。 plist: NSExtension ディクショナリには、NSExtensionAttributes を含める必要があります。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7031" />

### <a name="mt7031-the-watch-extension--has-an-invalid-infoplist-the-nsextensionattributes-dictionary-must-contain-a-wkappbundleidentifier"></a>MT7031: Watch 拡張機能 ' * ' に無効な情報が含まれています。 plist: NSExtensionAttributes dictionary には WKAppBundleIdentifier が含まれている必要があります。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7032" />

### <a name="mt7032-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-should-not-contain-the-watch-companion-capability"></a>MT7032: WatchKit Extension ' * ' に無効な情報が含まれています。 plist: UIRequiredDeviceCapabilities に ' watch コンパニオン ' 機能を含めることはできません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7033" />

### <a name="mt7033-the-watch-app--does-not-contain-an-infoplist"></a>MT7033: ウォッチアプリ ' * ' には、情報 plist が含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7034" />

### <a name="mt7034-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>MT7034: ウォッチアプリ ' * ' は CFBundleIdentifier を指定していません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7035" />

### <a name="mt7035-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected--but-found--"></a>MT7035: Watch アプリ '\*' に有効な UIDeviceFamily 値がありません。 '\*' が必要ですが、'\* (\*) ' が見つかりました。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7036" />

### <a name="mt7036-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>MT7036: ウォッチアプリ ' * ' は CFBundleExecutable を指定していません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7037" />

### <a name="mt7037-the-watchkit-extension-extensionname-has-an-invalid-wkappbundleidentifier-value--it-does-not-match-the-watch-apps-cfbundleidentifier-"></a>MT7037: WatchKit Extension ' {extensionName} ' に無効な WKAppBundleIdentifier 値 ('\*') が含まれています。この拡張機能は、Watch アプリの CFBundleIdentifier ('\*') と一致しません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7038" />

### <a name="mt7038-the-watch-app--has-an-invalid-infoplist-the-wkcompanionappbundleidentifier-must-exist-and-must-match-the-main-app-bundles-cfbundleidentifier"></a>MT7038: ウォッチアプリ ' * ' に無効な情報が含まれています。 plist: WKCompanionAppBundleIdentifier は存在する必要があり、メインアプリバンドルの CFBundleIdentifier と一致する必要があります。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7039" />

### <a name="mt7039-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>MT7039: ウォッチアプリ ' * ' に無効な情報が含まれています。 plist: Lsrequiresi Os キーが存在していてはなりません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7040" />

### <a name="mt7040-the-app-bundle-appbundlepath-does-not-contain-an-infoplist"></a>MT7040: アプリケーションバンドル {AppBundlePath} には、情報 plist が含まれていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7041" />

### <a name="mt7041-main-infoplist-path-does-not-specify-a-cfbundleidentifier"></a>MT7041: Main Info. plist パスに CFBundleIdentifier が指定されていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7042" />

### <a name="mt7042-main-infoplist-path-does-not-specify-a-cfbundleexecutable"></a>MT7042: Main Info. plist パスに CFBundleExecutable が指定されていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7043" />

### <a name="mt7043-main-infoplist-path-does-not-specify-a-cfbundlesupportedplatforms"></a>MT7043: Main Info. plist パスに CFBundleSupportedPlatforms が指定されていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7044" />

### <a name="mt7044-main-infoplist-path-does-not-specify-a-uidevicefamily"></a>MT7044: Main Info. plist パスに UIDeviceFamily が指定されていません。

*MSBuild タスク: ValidateAppBundleTaskBase*

<a name="MT7045" />

### <a name="mt7045-unrecognized-format-"></a>MT7045: 認識できない形式です: *。

*MSBuild タスク: PropertyListEditorTaskBase*

ここで * は次のようになります。

- string
- array
- かつ
- bool
- real
- 整数 (integer)
- date
- data

<a name="MT7046" />

### <a name="mt7046-add-entry--incorrectly-specified"></a>MT7046: Add: Entry、*、正しく指定されていません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7047" />

### <a name="mt7047-add-entry--contains-invalid-array-index"></a>MT7047: Add: Entry, *, に無効な配列インデックスが含まれています。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7048" />

### <a name="mt7048-add--entry-already-exists"></a>MT7048: Add: * エントリは既に存在します。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7049" />

### <a name="mt7049-add-cant-add-entry--to-parent"></a>MT7049: Add: 親にエントリ * を追加できません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7050" />

### <a name="mt7050-delete-cant-delete-entry--from-parent"></a>MT7050: Delete: 親からエントリ * を削除できません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7051" />

### <a name="mt7051-delete-entry--contains-invalid-array-index"></a>MT7051: Delete: Entry, *, に無効な配列インデックスが含まれています。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7052" />

### <a name="mt7052-delete-entry--does-not-exist"></a>MT7052: Delete: Entry、*、は存在しません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7053" />

### <a name="mt7053-import-entry--incorrectly-specified"></a>MT7053: Import: Entry、*、正しく指定されていません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7054" />

### <a name="mt7054-import-entry--contains-invalid-array-index"></a>MT7054: Import: Entry, *, に無効な配列インデックスが含まれています。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7055" />

### <a name="mt7055-import-error-reading-file-"></a>MT7055: Import: ファイルの読み取り中にエラーが発生します: *。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7056" />

### <a name="mt7056-import-cant-add-entry--to-parent"></a>MT7056: Import: 親にエントリ * を追加できません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7057" />

### <a name="mt7057-merge-cant-add-array-entries-to-dict"></a>MT7057: Merge: 辞書に配列のエントリを追加することはできません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7058" />

### <a name="mt7058-merge-specified-entry-must-be-a-container"></a>MT7058: Merge: 指定されたエントリはコンテナーである必要があります。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7059" />

### <a name="mt7059-merge-entry--contains-invalid-array-index"></a>MT7059: Merge: Entry, * に無効な配列インデックスが含まれています。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7060" />

### <a name="mt7060-merge-entry--does-not-exist"></a>MT7060: Merge: Entry, *, は存在しません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7061" />

### <a name="mt7061-merge-error-reading-file-"></a>MT7061: Merge: ファイルの読み取り中にエラーが発生します: *。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7062" />

### <a name="mt7062-set-entry--incorrectly-specified"></a>MT7062: Set: Entry、*、正しく指定されていません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7063" />

### <a name="mt7063-set-entry--contains-invalid-array-index"></a>MT7063: Set: Entry, * に無効な配列インデックスが含まれています。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7064" />

### <a name="mt7064-set-entry--does-not-exist"></a>MT7064: Set: Entry, *, は存在しません。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7065" />

### <a name="mt7065-unknown-propertylist-editor-action-"></a>MT7065: 不明な PropertyList エディターアクション: *。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7066" />

### <a name="mt7066-error-loading--"></a>MT7066: ' * ' の読み込み中にエラーが発生しています: *。

*MSBuild タスク: PropertyListEditorTaskBase*

<a name="MT7067" />

### <a name="mt7067-error-saving--"></a>MT7067: ' * ' を保存中にエラーが発生しています: *。

*MSBuild タスク: PropertyListEditorTaskBase*

## <a name="mt8xxx-runtime-error-messages"></a>MT8xxx: ランタイムエラーメッセージ

<!--
 MT8xxx runtime
  MT800x misc
  -->

<a name="MT8001" />

### <a name="mt8001-version-mismatch-between-the-native-xamarinios-runtime-and-monotouchdll-please-reinstall-xamarinios"></a>MT8001: ネイティブ Xamarin. iOS ランタイムと monotouch.dialog のバージョンが一致していません。 Xamarin. iOS を再インストールしてください。

<a name="MT8002" />

### <a name="mt8002-could-not-find-the-method--in-the-type-"></a>MT8002: '\*' 型のメソッド '\*' が見つかりませんでした。

<a name="MT8003" />

### <a name="mt8003-failed-to-find-the-closed-generic-method--on-the-type-"></a>MT8003: 型 '\*' のクローズジェネリックメソッド '\*' が見つかりませんでした。

<a name="MT8004" />

### <a name="mt8004-cannot-create-an-instance-of--for-the-native-object-0x-of-type--because-another-instance-already-exists-for-this-native-object-of-type-"></a>MT8004: ネイティブオブジェクト 0x * (型 '\*') の \* のインスタンスを作成できません。このネイティブオブジェクト (\*型) には別のインスタンスが既に存在します。

<a name="MT8005" />

### <a name="mt8005-wrapper-type--is-missing-its-native-objectivec-class-"></a>MT8005: ラッパー型 '\*' に、ネイティブな型指定されたクラス '\*' がありません。

<a name="MT8006" />

### <a name="mt8006-failed-to-find-the-selector--on-the-type-"></a>MT8006: 型 '\*' でセレクター '\*' が見つかりませんでした

<a name="MT8007" />

### <a name="mt8007-cannot-get-the-method-descriptor-for-the-selector--on-the-type--because-the-selector-does-not-correspond-to-a-method"></a>MT8007: セレクターがメソッドに対応していないため、型 '\*' のセレクター '\*' のメソッド記述子を取得できません

<a name="MT8008" />

### <a name="mt8008-the-loaded-version-of-xamariniosdll-was-compiled-for--bits-while-the-process-is--bits-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8008: 読み込まれたバージョンの Xamarin. iOS .dll は \* ビット用にコンパイルされましたが、プロセスは \* ビットです。 http://bugzilla.xamarin.comでバグを報告してください。

これは、ビルドプロセスで何らかの問題が発生していることを示します。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT8009" />

### <a name="mt8009-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-s-parameter--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8009: メソッドの変換メソッドを委任するブロックが見つかりません *。* 's パラメーター # *。 http://bugzilla.xamarin.comでバグを報告してください。

これは、API が正しくバインドされていないことを示します。 これが Xamarin によって公開される API である場合は、 [github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。 サードパーティのバインドの場合は、ベンダーにお問い合わせください。

<a name="MT8010" />

### <a name="mt8010-native-type-size-mismatch-between-xamariniosmacdll-and-the-executing-architecture-xamariniosmacdll-was-built-for--bit-while-the-current-process-is--bit"></a>MT8010: Xamarin のネイティブ型のサイズが一致していません。[iOS |Mac] .dll と実行中のアーキテクチャ。 Xamarin.[iOS |Mac] .dll は *-bit 用に構築されていますが、現在のプロセスは * ビットです。

これは、ビルドプロセスで何らかの問題が発生していることを示します。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT8011" />

### <a name="mt8011-unable-to-locate-the-delegate-to-block-conversion-attribute-delegateproxy-for-the-return-value-for-the-method--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8011: メソッドの戻り値の変換属性 ([DelegateProxy]) をブロックするデリゲートが見つかりませ*ん。* http://bugzilla.xamarin.comでバグを報告してください。

Xamarin. iOS は、実行時に必要なメソッドを見つけることができませんでした (デリゲートをブロックに変換するため)。

これは通常、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT8012" />

### <a name="mt8012-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8012: メソッドの戻り値の DelegateProxyAttribute が無効です *。* : DelegateType が null です。 http://bugzilla.xamarin.comでバグを報告してください。

対象のメソッドの DelegateProxy 属性が無効です。

これは通常、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT8013" />

### <a name="mt8013-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-2-specifies-a-type-without-a-handler-field-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8013: メソッドの戻り値の DelegateProxyAttribute が無効*です。* : DelegateType ({2}) では、' Handler ' フィールドのない型が指定されています。 http://bugzilla.xamarin.comでバグを報告してください。

対象のメソッドの `[DelegateProxy]` 属性が無効です。

これは通常、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT8014" />

### <a name="mt8014-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8014: メソッドの戻り値の DelegateProxyAttribute が無効です *。* : DelegateType の ({2}) ' Handler ' フィールドが null です。 http://bugzilla.xamarin.comでバグを報告してください。

対象のメソッドの `[DelegateProxy]` 属性が無効です。

これは通常、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT8015" />

### <a name="mt8015-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-not-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8015: メソッドの戻り値の DelegateProxyAttribute が無効*です。* : DelegateType の ({2}) ' Handler ' フィールドはデリゲートではなく、* です。 http://bugzilla.xamarin.comでバグを報告してください。

対象のメソッドの DelegateProxy 属性が無効です。

これは通常、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT8016" />

### <a name="mt8016-unable-to-convert-delegate-to-block-for-the-return-value-for-the-method--because-the-input-isnt-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8016: メソッドの戻り値に対してデリゲートを block に変換できません *。* 入力がデリゲートではないため、これは * です。 http://bugzilla.xamarin.comでバグを報告してください。

対象のメソッドの `[DelegateProxy]` 属性が無効です。

これは通常、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<!-- 8017 is used by mmp -->

<a name="MT8018" />

### <a name="mt8018-internal-consistency-error-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8018: 内部一貫性エラーが発生しています。 http://bugzilla.xamarin.comでバグレポートをファイルに登録してください。

これは、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT8019" />

### <a name="mt8019-could-not-find-the-assembly--in-the-loaded-assemblies"></a>MT8019: 読み込まれたアセンブリにアセンブリ * が見つかりませんでした。

これは、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT8020" />

### <a name="mt8020-could-not-find-the-module-with-metadatatoken--in-the-assembly-"></a>MT8020: アセンブリ \*に、が含まれ \* ているモジュールが見つかりませんでした。

これは、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT8021" />

### <a name="mt8021-unknown-implicit-token-type-"></a>MT8021: 不明な暗黙的なトークン型です: *。

これは、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT8022" />

### <a name="mt8022-expected-the-token-reference--to-be-a--but-its-a--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8022: トークン参照 \* \*である必要がありますが、\*です。 http://bugzilla.xamarin.comでバグレポートをファイルに登録してください。

これは、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT8023" />

### <a name="mt8023-an-instance-object-is-required-to-construct-a-closed-generic-method-for-the-open-generic-method--token-reference--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8023: open ジェネリックメソッドのクローズジェネリックメソッドを構築するには、インスタンスオブジェクトが必要です。 \* (トークン参照: \*)。 http://bugzilla.xamarin.comでバグレポートをファイルに登録してください。

これは、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。

<a name="MT8024" />

### <a name="mt8024-could-not-find-a-valid-extension-type-for-the-smart-enum-smart_type-please-file-a-bug-at-httpsbugzillaxamarincom"></a>MT8024: スマート enum ' {smart_type} ' の有効な拡張機能の種類が見つかりませんでした。 https://bugzilla.xamarin.comでバグを報告してください。

これは、Xamarin. iOS のバグを示しています。 [Github](https://github.com/xamarin/xamarin-macios/issues/new)で新しい問題を報告してください。
