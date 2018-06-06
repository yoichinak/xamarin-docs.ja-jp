---
title: Xamarin.Mac エラー メッセージ (mmp)
description: このドキュメントでは、mmp、Mac アプリケーションが実行可能ファイルにコンパイルされたアセンブリをパッケージ化するためのツールによって生成されたエラーが一覧表示します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5B26339F-A202-4E41-9229-D0BC9E77868E
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/27/2018
ms.openlocfilehash: f5cf4a9003d3fb468ffcf337c33730cb12238c44
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792760"
---
# <a name="xamarinmac-error-messages-mmp"></a>Xamarin.Mac エラー メッセージ (mmp)

## <a name="mm0xxx-mmp-error-messages"></a>MM0xxx: mmp エラー メッセージ

たとえば、 パラメーター、環境、ツールがありません。

<a name="MM0000" />

#### <a name="mm0000-unexpected-error---please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM0000: 予期しないエラー - ファイルのバグを報告してくださいで http://bugzilla.xamarin.com

予期しないエラーが発生しました。 ください[バグ レポートの提出](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac)できるだけ多くの情報を含みます。

* 冗長性を最大にビルド ログが完全 (例:`-v -v -v -v`で、**追加 mmp 引数**) です。
* エラーを再現最小限テスト_ケースそして
* すべてのバージョン情報

正確なバージョン情報を取得する最も簡単な方法を使用する、 **Xamarin Studio** ] メニューの [ **Xamarin Studio に関する**項目、**詳細の表示**ボタンをクリックし、バージョンのコピー/貼り付け情報 (使用することができます、**コピー情報**ボタン)。

<a name="MM0001" />

#### <a name="mm0001-this-version-of-xamarinmac-requires-mono-0-the-current-mono-version-is-1-please-update-the-monoframework-from-httpmono-projectcomdownloads"></a>MM0001: このバージョンの Xamarin.Mac 必要モノラル{0}(モノの現在のバージョンは{1})。 Mono.framework を更新してください。 http://mono-project.com/Downloads

<a name="MM0003" />

#### <a name="mm0003-application-name-0exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MM0003: アプリケーション名 '{0}.exe' SDK または製品アセンブリ (.dll) 名と競合しています。

<a name="MM0007" />

#### <a name="mm0007-the-root-assembly-0-does-not-exist"></a>MM0007: ルート アセンブリ '{0}' がありません

<a name="MM0008" />

#### <a name="mm0008-you-should-provide-one-root-assembly-only-found-0-assemblies-1"></a>MM0008: 1 つのルート アセンブリのみ、見つかったを指定する必要があります{0}アセンブリ: '{1}'

<a name="MM0010" />

#### <a name="mm0010-could-not-parse-the-command-line-arguments-0"></a>MM0010: コマンドライン引数を解析できませんでした。 {0}

<!-- 0013 is unused -->

<a name="MM0016" />

#### <a name="mm0016-the-option-0-has-been-deprecated"></a>MM0016: オプション '{0}' は推奨されていません。

<a name="MM0017" />

#### <a name="mm0017-you-should-provide-a-root-assembly"></a>MM0017: ルート アセンブリを指定する必要があります。

<a name="MM0018" />

#### <a name="mm0018-unknown-command-line-argument-0"></a>MM0018: 不明なコマンドライン引数: '{0}'

<a name="MM0020" />

#### <a name="mm0020-the-valid-options-for-0-are-1"></a>MM0020: に対して有効なオプション '{0}'は'{1}' です。

<a name="MM0023" />

#### <a name="mm0023-application-name-0exe-conflicts-with-another-user-assembly"></a>MM0023: アプリケーション名 '{0}.exe' 別のユーザー アセンブリと競合しています。

<a name="MM0026" />

#### <a name="mm0026-could-not-parse-the-command-line-argument-0-1"></a>MM0026: コマンドライン引数を解析できませんでした '{0}'。 {1}

<a name="MM0043" />

#### <a name="mm0043-the-boehm-garbage-collector-is-not-supported-the-sgen-garbage-collector-has-been-selected-instead"></a>MM0043: Boehm ガベージ コレクターがサポートされていません。 SGen ガベージ コレクターは、代わりに選択されています。

<a name="MM0050" />

#### <a name="mm0050-you-cannot-provide-a-root-assembly-if---no-root-assembly-is-passed"></a>MM0050:--いいえルート アセンブリが渡された場合は、ルート アセンブリを提供できません。

<a name="MM0051" />

#### <a name="mm0051-an-output-directory---output-is-required-if---no-root-assembly-is-passed"></a>MM0051: 出力ディレクトリ (--出力)--いいえルート アセンブリが渡された場合は必須です。

<a name="MM0053" />

#### <a name="mm0053-cannot-disable-new-refcount-with-the-unified-api"></a>MM0053:、統合 API の新しい参照カウントが無効にすることはできません。

<a name="MM0056" />

#### <a name="mm0056-cannot-find-xcode-in-any-of-our-default-locations-please-install-xcode-or-pass-a-custom-path-using---sdkrootpath"></a>MM0056: は、既定の場所のいずれかで Xcode を見つけることができません。 Xcode をインストールしてください--sdkroot を使用してカスタム パスを渡す =<path>

<a name="MM0059" />

#### <a name="mm0059-could-not-find-the-currently-selected-xcode-on-the-system-0"></a>MM0059: で、システム上の現在選択されている Xcode を見つけることができませんでした:{0}です。

<a name="MM0060" />

#### <a name="mm0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned-0-but-that-directory-does-not-exist"></a>MM0060: は、システムで現在選択されている Xcode を見つけられませんでした。 'xcode 選択--印刷パス' が返されます '{0}' が、そのディレクトリは存在しません。

<a name="MM0068" />

#### <a name="mm0068-invalid-value-for-target-framework-0"></a>MM0068: ターゲット フレームワークの無効な値:{0}です。

<a name="MM0071" />

#### <a name="mm0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinmac-please-file-a-bug-report-at-httpsbugzillaxamarincom-with-a-test-case"></a>MM0071: 不明なプラットフォーム: * です。 これは通常 Xamarin.Mac; のバグを示しますバグ報告を送信してくださいhttps://bugzilla.xamarin.comとテスト_ケースをします。

これは通常 Xamarin.Mac; のバグを示しますバグ報告を送信してください[ https://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac)とテスト_ケースをします。

<a name="MM0079" />

#### <a name="mm0079-internal-error---no-executable-was-copied-into-the-app-bundle-please-contact-supportxamarincom"></a>MM0079: 内部エラーのない実行可能ファイルは、アプリ バンドルにコピーされました。 お問い合わせください 'support@xamarin.com'

<a name="MM0080" />

#### <a name="mm0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MM0080: NewRefCount 無効にすると、--新しい-refcount:false は推奨されなくなりました。

<!-- 0088 used by mtouch -->
<!-- 0089 used by mtouch -->

<a name="MM0091" />

#### <a name="mm0091-this-version-of-xamarinmac-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-use-the-dynamic-registrar-or-set-the-managed-linker-behaviour-to-link-platform-or-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MM0091: Xamarin.Mac のこのバージョンので、* SDK (Xcode に同梱されて *)。 か、必要なヘッダー ファイルを取得または動的なレジストラーを使用してまたはリンク プラットフォームまたはリンク フレームワーク Sdk のみ (新しい Api を回避しようとしてください) をマネージ リンカー動作を設定する Xcode をアップグレードします。

Xamarin.Mac には、静的なレジストラーを使用してアプリケーションを構築する、エラー メッセージで指定された SDK のバージョンからのヘッダー ファイルが必要です。 このエラーを解決するための推奨方法は、必要な SDK を取得する Xcode にアップグレードする、これは、すべての必要なヘッダー ファイルが含まれます。 インストールされている、Xcode のバージョンが複数ある場合や既定以外の場所に、Xcode を使用する場合は場合、は、IDE の設定 で Xcode の正しい場所を設定することを確認してください。

1 つの代替、潜在的なソリューションは、マネージ リンカーを有効にするのにです。 これにより、使用されていない API を含む、ほとんどの場合、ヘッダー ファイルが不足している (または未完了) の新しい API が削除されます。 ただしこの場合は機能しません、プロジェクトは、Xcode 1 よりも新しい SDK で導入された API を使用して提供します。

潜在的な 2 つ目の代替ソリューションが代わりに動的レジストラー 型を動的に登録してスタートアップ コストを示しているこのことは、ヘッダー ファイルの要件を削除することができます。 

最終 straw ソリューションは、Xamarin.Mac の古いバージョンを使用することは、SDK プロジェクトをサポートするいると、次の必要があります。

<a name="MM0097" />

#### <a name="mm0097-machineconfig-file-0-can-not-be-found"></a>MM0097: machine.config ファイル '{0}' が見つかりません。

<a name="MM0098" />

#### <a name="mm0098-aot-compilation-is-only-available-on-unified"></a>MM0098: AOT コンパイルでのみ利用統合

<a name="MM0099" />

#### <a name="mm0099-internal-error-0-please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MM0099: 内部エラー{0}です。 テスト_ケースとバグのレポートを送信してください (http://bugzilla.xamarin.com)です。

<a name="MM0114" />

#### <a name="mm0114-hybrid-aot-compilation-requires-all-assemblies-to-be-aot-compiled"></a>MM0114: ハイブリッド AOT コンパイル AOT コンパイルであるすべてのアセンブリが必要です。

## <a name="mm1xxx-file-copy--symlinks-project-related"></a>MM1xxx: ファイルをコピー/シンボリック リンク (関連するプロジェクト)

<a name="MM1034" />

#### <a name="mm1034-could-not-create-symlink-file---target-error-number"></a>MM1034: Could not create symlink '{file}' -> '{target}': error {number}

### <a name="mm14xx-product-assemblies"></a>MM14xx: 製品アセンブリ

<a name="MM1401" />

#### <a name="mm1401-the-required-0-assembly-is-missing-from-the-references"></a>MM1401: 必要な '{0}' アセンブリが参照から不足しています。

<a name="MM1402" />

#### <a name="mm1402-the-assembly-0-is-not-compatible-with-this-tool"></a>MM1402: アセンブリ '{0}' このツールと互換性がありません

<a name="MM1403" />

#### <a name="mm1403-0-1-could-not-be-found-target-framework-0-is-unusable-to-package-the-application"></a>MM1403: {0} '{1}' に見つかりませんでした。 ターゲット フレームワーク '{0}' は、アプリケーション パッケージを使用できません。

<a name="MM1404" />

#### <a name="mm1404-target-framework-0-is-invalid"></a>MM1404: ターゲット フレームワーク '{0}' が無効です。

<a name="MM1405" />

#### <a name="mm1405-usefullxammacframework-must-always-target-framework-net-45-not-0-which-is-invalid"></a>MM1405: useFullXamMacFramework 常にターゲットでなければなりません framework .NET 4.5、いない '{0}' は無効です

<a name="MM1406" />

#### <a name="mm1406-target-framework-0-is-invalid-when-targetting-xamarinmac-45-net-framwork"></a>MM1406: ターゲット フレームワーク '{0}' は有効な場合に Xamarin.Mac 4.5 .NET framwork の対象とします。

<a name="MM1407" />

#### <a name="mm1407-mismatch-between-xamarinmac-reference-0-and-target-framework-selected-1"></a>MM1407: に不一致が Xamarin.Mac 参照 '{0}'および選択されているターゲット フレームワーク'{1}' です。

### <a name="mm15xx-assembly-gathering-not-requiring-linker-errors"></a>MM15xx: アセンブリの収集 (リンカーを必要としない) エラー

<a name="MM1501" />

#### <a name="mm1501-can-not-resolve-reference-0"></a>MM1501: 参照を解決できません。 {0}

### <a name="machocs"></a>MachO.cs

<a name="MM1600" />

#### <a name="mm1600-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1600: 一致する O ダイナミック ライブラリではない (不明なヘッダー ' 0 x{0}'):{1}です。

<a name="MM1601" />

#### <a name="mm1601-not-a-static-library-unknown-header-0-1"></a>MM1601: スタティック ライブラリではない (不明なヘッダー '{0}'):{1}です。

<a name="MM1602" />

#### <a name="mm1602-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1602: 一致する O ダイナミック ライブラリではない (不明なヘッダー ' 0 x{0}'):{1}です。

<a name="MM1603" />

#### <a name="mm1603-unknown-format-for-fat-entry-at-position-0-in-1"></a>MM1603: 位置にあるエントリを fat 形式が不明な{0}で{1}です。

<a name="MM1604" />

#### <a name="mm1604-file-of-type-0-is-not-a-macho-file-1"></a>MM1604: ファイルの種類{0}MachO ファイルではありません ({1})。

## <a name="mm2xxx-linker"></a>MM2xxx: リンカー

### <a name="mm20xx-linker-general-errors"></a>MM20xx: リンカー (全般) のエラー

<a name="MM2001" />

#### <a name="mm2001-could-not-link-assemblies"></a>MM2001: アセンブリをリンクできませんでした。

<a name="MM2002" />

#### <a name="mm2002-can-not-resolve-reference-0"></a>MM2002: 参照を解決できません。 {0}

<a name="MM2003" />

#### <a name="mm2003-option-0-will-be-ignored-since-linking-is-disabled"></a>MM2003: オプション '{0}' リンクが無効になるので無視されます

<a name="MM2004" />

#### <a name="mm2004-extra-linker-definitions-file-0-could-not-be-located"></a>MM2004: Extra リンカー定義ファイル '{0}' に見つかりませんでした。

<a name="MM2005" />

#### <a name="mm2005-definitions-from-0-could-not-be-parsed"></a>MM2005: 定義 '{0}' を解析できませんでした。

<a name="MM2006" />

#### <a name="mm2006-native-library-0-was-referenced-but-could-not-be-found"></a>MM2006: ネイティブ ライブラリ '{0}' が参照されましたが、見つかりませんでした。

<a name="MM2007" />

#### <a name="mm2007-xamarinmac-unified-api-against-a-full-net-profile-does-not-support-linking-pass-the--nolink-flag"></a>MM2007: Xamarin.Mac Unified API は、完全な .NET プロファイルに対してはサポートしませんリンク。 -Nolink フラグを渡します。

<a name="MM2009" />

#### <a name="mm2009-referenced-by-01------this-message-is-related-to-mm2006-"></a>MM2009: によって参照される{0}.{1}    * * このメッセージは MM2006 に関連する * *

<a name="MM2010" />

#### <a name="mm2010-unknown-httpmessagehandler-0-valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MM2010: 不明な HttpMessageHandler`{0}`です。 有効な値は HttpClientHandler (既定値)、CFNetworkHandler または NSUrlSessionHandler です。

<a name="MM2011" />

#### <a name="mm2011-unknown-tlsprovider-0--valid-values-are-default-or-appletls"></a>MM2011: 不明な TLSProvider '{0}です。  有効な値は、default または appletls です。

<a name="MM2012" />

#### <a name="mm2012-only-first-0-of-1-referenced-by-warnings-shown--this-message-related-to-2009-"></a>MM2012: のみ最初{0}の{1}「によって参照される」警告を表示します。 * * このメッセージは、2009 年に関連する * *

<a name="MM2013" />

#### <a name="mm2013-failed-to-resolve-the-reference-to-0-referenced-in-1-the-app-will-not-include-the-referenced-assembly-and-may-fail-at-runtime"></a>MM2013: への参照を解決できませんでした"{0}「, で参照されている」{1}"です。 アプリでは、参照されたアセンブリは含まれません、実行時に失敗する可能性があります。

<a name="MM2014" />

#### <a name="mm2014-xamarinmac-extensions-do-not-support-linking-request-for-linking-will-be-ignored--this-message-is-obsolete-in-xm-36-"></a>MM2014: Xamarin.Mac 拡張機能をサポートしませんリンク。 要求のリンクは無視されます。 * * このメッセージは XM 3.6 以降で不使用 * *

<!-- 2015 used by mtouch -->

<a name="MM2016" />

#### <a name="mm2016-invalid-tlsprovider-0-option-the-only-valid-value-1-will-be-used"></a>MM2016: 無効な TlsProvider`{0}`オプション。 唯一の有効値`{1}`使用されます。

<a name="MM2017" />

#### <a name="mm2017-could-not-process-xml-description-0"></a>MM2017: は、XML の説明を処理できませんでした。 {0}

<a name="MM202x" />

#### <a name="mm202x-binding-optimizer-failed-processing-"></a>MM202x: は、バインディング オプティマイザーが処理に失敗しました。`...`です。

<a name="MM2100" />

#### <a name="mm2100-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2100: Xamarin.Mac クラシック API はサポートされませんプラットフォームのリンク。

<a name="MM2103" />

#### <a name="mm2103-error-processing-assembly--"></a>MM2103: エラー処理のアセンブリ '\*': *

アセンブリを処理するときに、予期しないエラーが発生しました。

問題の原因で、アセンブリは、エラー メッセージにという名前です。 アセンブリにこの問題を修正するのにはで指定する必要があります、[バグ レポート](https://bugzilla.xamarin.com)有効になっている詳細度で完全なビルド ログと共に (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MM2104" />

#### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: アセンブリをリンクできません '{0}' 混在モードであるとします。

混合モード アセンブリは、リンカーによっては処理できません。

参照してくださいhttps://msdn.microsoft.com/library/x0w2664k.aspx混合モード アセンブリの詳細についてはします。

## <a name="mm3xxx-aot"></a>MM3xxx: AOT

### <a name="mm30xx-aot-general-errors"></a>MM30xx: AOT (全般) のエラー

<a name="MM3001" />

#### <a name="mm3001-could-not-aot-the-assembly-0"></a>MM3001: でした AOT アセンブリではない '{0}'

<!-- 3002 used by mtouch -->
<!-- 3003 used by mtouch -->
<!-- 3004 used by mtouch -->
<!-- 3005 used by mtouch -->
<!-- 3006 used by mtouch -->
<!-- 3007 used by mtouch -->
<!-- 3008 used by mtouch -->

<a name="MM3009" />

#### <a name="mm3009-aot-of-0-was-requested-but-was-not-found"></a>MM3009: AOT '{0}' が要求されましたが、見つかりませんでした

<a name="MM3010" />

#### <a name="mm3010-exclusion-of-aot-of-0-was-requested-but-was-not-found"></a>MM3010: AOT 除外 '{0}' が要求されましたが、見つかりませんでした

## <a name="mm4xxx-code-generation"></a>MM4xxx: コードの生成

### <a name="mm40xx-driverm"></a>MM40xx: driver.m

<a name="MM4001" />

#### <a name="mm4001-the-main-template-could-not-be-expanded-to-0"></a>MM4001: メインのテンプレートを展開できませんを`{0}`です。

### <a name="mm41xx-registrar"></a>MM41xx: レジストラー

<a name="MM4134" />

#### <a name="mm4134-your-application-is-using-the-0-framework-which-isnt-included-in-the-macos-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-osx-2-while-youre-building-with-the-macos-1-sdk-this-configuration-is-not-supported-with-the-static-registrar-pass---registrardynamic-as-an-additional-mmp-argument-in-your-projects-mac-build-option-to-select-alternatively-select-a-newer-sdk-in-your-apps-mac-build-options"></a>MM4134: アプリケーションを使用して、'{0}' フレームワークで、アプリのビルドを使用する、MacOS SDK に含まれていない (このフレームワークは、OSX で導入された{2}、MacOS でビルドするときに、 {1} SDK)。この構成は、静的レジストラー (パス--レジストラー: 動的、プロジェクトの Mac をビルドを選択するオプションで追加の mmp 引数として) でサポートされていません。 また、アプリの Mac をビルド オプションで新しい SDK を選択します。

## <a name="mm5xxx-gcc-and-toolchain"></a>MM5xxx: GCC およびツール チェーン

### <a name="mm51xx-compilation"></a>MM51xx: コンパイル

<a name="MM5101" />

#### <a name="mm5101-missing-0-compiler-please-install-xcode-command-line-tools-component"></a>MM5101: 見つからない '{0}' コンパイラです。 Xcode コマンド ライン ツール コンポーネントをインストールしてください。

<!-- 5102 used by mtouch -->

<a name="MM5103" />

#### <a name="mm5103-failed-to-compile-error-code---0-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM5103: は、コンパイルに失敗しました。 エラー コード -{0}です。 バグ報告を送信してください。 http://bugzilla.xamarin.com

<!-- 5104 used by mtouch -->

### <a name="mm52xx-linking"></a>MM52xx: リンク

<a name="MM5202" />

#### <a name="mm5202-monoframework-mdk-is-missing-please-install-the-mdk-for-your-monoframework-version-from-httpmono-projectcomdownloads"></a>MM5202: Mono.framework MDK がありません。 MDK から Mono.framework バージョンをインストールしてください。 http://mono-project.com/Downloads

<a name="MM5203" />

#### <a name="mm5203-cant-find-libxammaca-likely-because-of-a-corrupted-xamarinmac-installation-please-reinstall-xamarinmac"></a>MM5203: libxammac.a、Xamarin.Mac インストールの破損が原因の可能性を見つけることができません。 Xamarin.Mac を再インストールしてください。

<a name="MM5204" />

#### <a name="mm5204-invalid-architecture-x8664-is-only-supported-on-non-classic-profiles"></a>MM5204: 無効なアーキテクチャ。 x86_64 は、クラシックのプロファイルでのみサポートされます。

<a name="MM5205" />

#### <a name="mm5205-invalid-architecture-0-valid-architectures-are-i386-and-x8664-when---profilemobile"></a>MM5205: 無効なアーキテクチャ '{0}' です。 有効なアーキテクチャは、i386 と x86_64 (プロファイルと--= モバイル)。

<a name="MM5218" />

#### <a name="mm5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MM5218: は動的なシンボル {シンボル} を無視することはできません (--無視ダイナミック シンボル = {シンボル})、動的なシンボルが検出されなかったためです。

参照してください、[等価 mtouch 警告](~/ios/troubleshooting/mtouch-errors.md#MT5218)です。

<!-- 5206 used by mtouch -->
<!-- 5207 used by mtouch -->
<!-- 5208 used by mtouch -->
<!-- 5209 used by mtouch -->
<!-- 5210 used by mtouch -->
<!-- 5211 used by mtouch -->
<!-- 5212 used by mtouch -->
<!-- 5213 used by mtouch -->
<!-- 5214 used by mtouch -->
<!-- 5215 used by mtouch -->
<!-- 5216 used by mtouch -->
<!-- 5217 used by mtouch -->

### <a name="mm53xx-other-tools"></a>MM53xx: その他のツール

<a name="MM5301" />

#### <a name="mm5301-pkg-config-could-not-be-found-please-install-the-monoframework-from-httpmono-projectcomdownloads"></a>MM5301: パッケージ構成が見つかりませんでした。 Mono.framework をインストールしてください。 http://mono-project.com/Downloads

<!-- 5302 used by mtouch -->
<!-- 5303 used by mtouch -->
<!-- 5304 used by mtouch -->

<a name="MM5305" />

#### <a name="mm5305-missing-otool-tool-please-install-xcode-command-line-tools-component"></a>MM5305: 不足している 'otool' するツールです。 Xcode コマンド ライン ツール コンポーネントをインストールしてください。

<a name="MM5306" />

#### <a name="mm5306-missing-dependencies-please-install-xcode-command-line-tools-component"></a>MM5306:、依存関係がありません。 Xcode コマンド ライン ツール コンポーネントをインストールしてください。

<a name="MM5308" />

#### <a name="mm5308-xcode-license-agreement-may-not-have-been-accepted--please-launch-xcode"></a>MM5308: Xcode 使用許諾契約書可能性がありますが同意されていません。  Xcode を起動してください。

<a name="MM5309" />

#### <a name="mm5309-native-linking-failed-with-error-code-1--check-build-log-for-details"></a>MM5309: リンクのネイティブ エラー コード 1 で失敗しました。  詳細については、ビルド ログを確認します。

<a name="MM5310" />

#### <a name="mm5310-installnametool-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5310: install_name_tool がエラー コードで失敗しました。 '{0}' です。 詳細については、ビルド ログを確認します。

<!-- MM6xxx: mmp internal tools -->
<!-- MM7xxx: reserved -->

## <a name="mm8xxx-runtime"></a>MM8xxx: ランタイム

### <a name="mm800x-misc"></a>MM800x: その他

<!-- 8000 used by mtouch -->
<!-- 8001 used by mtouch -->
<!-- 8002 used by mtouch -->
<!-- 8003 used by mtouch -->
<!-- 8004 used by mtouch -->
<!-- 8005 used by mtouch -->
<!-- 8006 used by mtouch -->
<!-- 8007 used by mtouch -->
<!-- 8008 used by mtouch -->
<!-- 8009 used by mtouch -->
<!-- 8010 used by mtouch -->
<!-- 8011 used by mtouch -->
<!-- 8012 used by mtouch -->
<!-- 8013 used by mtouch -->
<!-- 8014 used by mtouch -->
<!-- 8015 used by mtouch -->
<!-- 8016 used by mtouch -->

<a name="MM8017" />

#### <a name="mm8017-the-boehm-garbage-collector-is-not-supported-please-use-sgen-instead"></a>MM8017: Boehm ガベージ コレクターがサポートされていません。 SGen を使用してください。

