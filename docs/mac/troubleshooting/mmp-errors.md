---
title: Xamarin.Mac のエラー メッセージ (mmp)
description: このドキュメントでは、mmp、コンパイル済みアセンブリを実行可能ファイルの Mac アプリケーションをパッケージ化するために使用するツールによって生成されたエラーを示します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5B26339F-A202-4E41-9229-D0BC9E77868E
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/27/2018
ms.openlocfilehash: 640d1adc048bec167508d8c288b62d498f061b0d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61233203"
---
# <a name="xamarinmac-error-messages-mmp"></a>Xamarin.Mac のエラー メッセージ (mmp)

## <a name="mm0xxx-mmp-error-messages"></a>MM0xxx: mmp エラー メッセージ

たとえば、 パラメーター、環境、ツールがありません。

<a name="MM0000" />

#### <a name="mm0000-unexpected-error---please-file-a-bug-report-at-httpsgithubcomxamarinxamarin-maciosissuesnew"></a>MM0000:予期しないエラー - ファイルのバグを報告してくださいで https://github.com/xamarin/xamarin-macios/issues/new

予期しないエラーが発生しました。 ください[バグ レポートを](https://github.com/xamarin/xamarin-macios/issues/new)できるだけ多くの情報を含みます。

* 完全な詳細レベルでログをビルド (例:`-v -v -v -v`で、**追加の mmp 引数**)。
* エラーを再現する最小のテスト_ケースそして
* すべてのバージョン情報

正確なバージョン情報を取得する最も簡単な方法が使用するには、 **Xamarin Studio** ] メニューの [ **Xamarin Studio のバージョン情報**項目、**詳細の表示**ボタンをクリックし、バージョンのコピー/貼り付け情報 (使用することができます、**コピー情報**ボタン)。

<a name="MM0001" />

#### <a name="mm0001-this-version-of-xamarinmac-requires-mono-0-the-current-mono-version-is-1-please-update-the-monoframework-from-httpmono-projectcomdownloads"></a>MM0001:Xamarin.Mac のこのバージョンには、Mono がで必要な{0}(Mono の現在のバージョンは{1})。 Mono.framework を更新してください。 http://mono-project.com/Downloads

<a name="MM0003" />

#### <a name="mm0003-application-name-0exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MM0003:アプリケーション名 '{0}.exe' SDK またはプロダクト アセンブリ (.dll) 名と競合します。

<a name="MM0007" />

#### <a name="mm0007-the-root-assembly-0-does-not-exist"></a>MM0007:ルート アセンブリ '{0}' が存在しません。

<a name="MM0008" />

#### <a name="mm0008-you-should-provide-one-root-assembly-only-found-0-assemblies-1"></a>MM0008:1 つのルート アセンブリのみ、見つかったを提供する必要があります{0}アセンブリ: '{1}'

<a name="MM0010" />

#### <a name="mm0010-could-not-parse-the-command-line-arguments-0"></a>MM0010:コマンドライン引数を解析できませんでした。 {0}

<!-- 0013 is unused -->

<a name="MM0016" />

#### <a name="mm0016-the-option-0-has-been-deprecated"></a>MM0016:オプション '{0}' は非推奨とされました。

<a name="MM0017" />

#### <a name="mm0017-you-should-provide-a-root-assembly"></a>MM0017:ルート アセンブリを指定する必要があります。

<a name="MM0018" />

#### <a name="mm0018-unknown-command-line-argument-0"></a>MM0018:不明なコマンドライン引数: '{0}'

<a name="MM0020" />

#### <a name="mm0020-the-valid-options-for-0-are-1"></a>MM0020:有効なオプション '{0}'は'{1}'。

<a name="MM0023" />

#### <a name="mm0023-application-name-0exe-conflicts-with-another-user-assembly"></a>MM0023:アプリケーション名 '{0}.exe' 別のユーザーのアセンブリと競合します。

<a name="MM0026" />

#### <a name="mm0026-could-not-parse-the-command-line-argument-0-1"></a>MM0026:コマンドライン引数を解析できませんでした '{0}'。 {1}

<a name="MM0043" />

#### <a name="mm0043-the-boehm-garbage-collector-is-not-supported-the-sgen-garbage-collector-has-been-selected-instead"></a>MM0043:Boehm ガベージ コレクターがサポートされていません。 SGen ガベージ コレクターが代わりに選択されています。

<a name="MM0050" />

#### <a name="mm0050-you-cannot-provide-a-root-assembly-if---no-root-assembly-is-passed"></a>MM0050:-いいえ-ルート アセンブリが渡された場合は、ルート アセンブリを提供できません。

<a name="MM0051" />

#### <a name="mm0051-an-output-directory---output-is-required-if---no-root-assembly-is-passed"></a>MM0051:出力ディレクトリ (--出力)--いいえ-ルート アセンブリが渡される場合は必須です。

<a name="MM0053" />

#### <a name="mm0053-cannot-disable-new-refcount-with-the-unified-api"></a>MM0053:Unified API を使用した新しい参照カウントを無効にすることはできません。

<a name="MM0056" />

#### <a name="mm0056-cannot-find-xcode-in-any-of-our-default-locations-please-install-xcode-or-pass-a-custom-path-using---sdkrootpath"></a>MM0056:この既定の場所のいずれかで Xcode が見つかりません。 Xcode をインストールするか、--sdkroot を使用して、カスタム パスを渡すください =<path>

<a name="MM0059" />

#### <a name="mm0059-could-not-find-the-currently-selected-xcode-on-the-system-0"></a>MM0059:システムで現在選択されている Xcode が見つかりませんでした: {0};

<a name="MM0060" />

#### <a name="mm0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned-0-but-that-directory-does-not-exist"></a>MM0060:システムで現在選択されている Xcode が見つかりませんでした。 'xcode 選択--印刷パス' 返される '{0}' が、そのディレクトリが存在しません。

<a name="MM0068" />

#### <a name="mm0068-invalid-value-for-target-framework-0"></a>MM0068:ターゲット フレームワークに無効な値:{0}します。

<a name="MM0071" />

#### <a name="mm0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinmac-please-file-a-bug-report-at-httpsbugzillaxamarincom-with-a-test-case"></a>MM0071:不明なプラットフォーム: *。 これは通常 Xamarin.Mac; のバグを示しますバグ報告を送信してください https://bugzilla.xamarin.com とテスト_ケースをします。

これは通常、Xamarin.Mac; でのバグを示しますバグ報告を提出してください[ https://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac)テスト_ケースを使用します。

<a name="MM0079" />

#### <a name="mm0079-internal-error---no-executable-was-copied-into-the-app-bundle-please-contact-supportxamarincom"></a>MM0079:内部エラーのない実行可能ファイルは、アプリ バンドルにコピーされました。 お問い合わせください 'support@xamarin.com'

<a name="MM0080" />

#### <a name="mm0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MM0080:NewRefCount を無効にすると、- 新規-refcount:false が非推奨とされます。

<!-- 0088 used by mtouch -->
<!-- 0089 used by mtouch -->

<a name="MM0091" />

#### <a name="mm0091-this-version-of-xamarinmac-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-use-the-dynamic-registrar-or-set-the-managed-linker-behaviour-to-link-platform-or-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MM0091:Xamarin.Mac のこのバージョンで、* SDK (Xcode に同梱されて *)。 いずれかの必須のヘッダー ファイルを取得または動的なレジストラーを使用して、またはリンクまたはプラットフォームにリンク フレームワーク Sdk のみ (新しい Api を回避しようとしてください) をマネージ リンカーの動作を設定する Xcode をアップグレードします。

Xamarin.Mac では、静的なレジストラーを使用してアプリケーションを構築する、エラー メッセージで指定された SDK バージョンから、ヘッダー ファイルが必要です。 このエラーを解決するには、必要な SDK を取得する Xcode のアップグレードをお勧めしますが、これは、すべての必須のヘッダー ファイルが含まれます、です。 インストールされている場合、Xcode のバージョンが複数ある場合または既定以外の場所で、Xcode を使用する場合は、場合は、IDE の基本設定で正しい Xcode の場所を設定することを確認してください。

1 つの潜在的な代替ソリューションでは、管理対象のリンカーを有効にします。 これにより、使用されていない API を含む、ほとんどの場合、ヘッダー ファイルが不足している (または未完了) を新しい API が削除されます。 ただしこれは機能しません、プロジェクトは、Xcode 1 よりも新しい SDK で導入された API を使用している場合を提供します。

2 つ目の潜在的な代替ソリューションでは、代わりに、動的なレジストラーを使用が。 型を動的に登録することによって起動コストをかけるこのことは、ヘッダー ファイルの要件を削除することができます。 

Xamarin.Mac の以前のバージョンを使用するには、限界ソリューションがあります、SDK、プロジェクトをサポートするいると、次の必要があります。

<a name="MM0097" />

#### <a name="mm0097-machineconfig-file-0-can-not-be-found"></a>MM0097: machine.config ファイル '{0}' が見つかりません。

<a name="MM0098" />

#### <a name="mm0098-aot-compilation-is-only-available-on-unified"></a>MM0098:AOT コンパイルは統合で使用できる、のみ

<a name="MM0099" />

#### <a name="mm0099-internal-error-0-please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MM0099:内部エラー{0}します。 テスト_ケースとバグの報告を提出してください (http://bugzilla.xamarin.com)します。

<a name="MM0114" />

#### <a name="mm0114-hybrid-aot-compilation-requires-all-assemblies-to-be-aot-compiled"></a>MM0114:ハイブリッド AOT コンパイルには、すべてのアセンブリを AOT コンパイルにする必要があります。

## <a name="mm1xxx-file-copy--symlinks-project-related"></a>MM1xxx: ファイルのコピー/シンボリック リンク (関連するプロジェクト)

<a name="MM1034" />

#### <a name="mm1034-could-not-create-symlink-file---target-error-number"></a>MM1034:Could not create symlink '{file}' -> '{target}': error {number}

### <a name="mm14xx-product-assemblies"></a>MM14xx:製品アセンブリ

<a name="MM1401" />

#### <a name="mm1401-the-required-0-assembly-is-missing-from-the-references"></a>MM1401:必要な '{0}' アセンブリが参照から不足しています。

<a name="MM1402" />

#### <a name="mm1402-the-assembly-0-is-not-compatible-with-this-tool"></a>MM1402:アセンブリ '{0}' このツールと互換性がありません

<a name="MM1403" />

#### <a name="mm1403-0-1-could-not-be-found-target-framework-0-is-unusable-to-package-the-application"></a>MM1403: {0} '{1}' は見つかりませんでした。 ターゲット フレームワーク '{0}' は、アプリケーション パッケージを使用できません。

<a name="MM1404" />

#### <a name="mm1404-target-framework-0-is-invalid"></a>MM1404:ターゲット フレームワーク '{0}' が無効です。

<a name="MM1405" />

#### <a name="mm1405-usefullxammacframework-must-always-target-framework-net-45-not-0-which-is-invalid"></a>MM1405: useFullXamMacFramework する必要があります常にターゲット フレームワークが .NET 4.5 では、'{0}' これが無効です

<a name="MM1406" />

#### <a name="mm1406-target-framework-0-is-invalid-when-targetting-xamarinmac-45-net-framwork"></a>MM1406:ターゲット フレームワーク '{0}' 場合は無効です Xamarin.Mac 4.5 .NET framwork の対象とします。

<a name="MM1407" />

#### <a name="mm1407-mismatch-between-xamarinmac-reference-0-and-target-framework-selected-1"></a>MM1407:Xamarin.Mac の参照を不一致 '{0}'と選択したターゲット フレームワーク'{1}'。

### <a name="mm15xx-assembly-gathering-not-requiring-linker-errors"></a>MM15xx:アセンブリの収集 (リンカー必要としない) エラー

<a name="MM1501" />

#### <a name="mm1501-can-not-resolve-reference-0"></a>MM1501:参照を解決できません。 {0}

### <a name="machocs"></a>MachO.cs

<a name="MM1600" />

#### <a name="mm1600-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1600:MACH-O ダイナミック ライブラリではありません (不明なヘッダー ' 0 x{0}'):{1}します。

<a name="MM1601" />

#### <a name="mm1601-not-a-static-library-unknown-header-0-1"></a>MM1601:スタティック ライブラリではありません (不明なヘッダー '{0}'):{1}します。

<a name="MM1602" />

#### <a name="mm1602-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1602:MACH-O ダイナミック ライブラリではありません (不明なヘッダー ' 0 x{0}'):{1}します。

<a name="MM1603" />

#### <a name="mm1603-unknown-format-for-fat-entry-at-position-0-in-1"></a>MM1603:不明な位置にあるエントリを fat 形式{0}で{1}します。

<a name="MM1604" />

#### <a name="mm1604-file-of-type-0-is-not-a-macho-file-1"></a>MM1604:ファイルの種類{0}MachO ファイルではありません ({1})。

## <a name="mm2xxx-linker"></a>MM2xxx:リンカー

### <a name="mm20xx-linker-general-errors"></a>MM20xx:リンカー (全般) エラー

<a name="MM2001" />

#### <a name="mm2001-could-not-link-assemblies"></a>MM2001:アセンブリをリンクできませんでした。

<a name="MM2002" />

#### <a name="mm2002-can-not-resolve-reference-0"></a>MM2002:参照を解決できません。 {0}

<a name="MM2003" />

#### <a name="mm2003-option-0-will-be-ignored-since-linking-is-disabled"></a>MM2003:オプション '{0}' はリンクが無効になっているために無視されます

<a name="MM2004" />

#### <a name="mm2004-extra-linker-definitions-file-0-could-not-be-located"></a>MM2004:追加のリンカーの定義ファイル '{0}' に見つかりませんでした。

<a name="MM2005" />

#### <a name="mm2005-definitions-from-0-could-not-be-parsed"></a>MM2005:定義 '{0}' を解析できませんでした。

<a name="MM2006" />

#### <a name="mm2006-native-library-0-was-referenced-but-could-not-be-found"></a>MM2006:ネイティブ ライブラリ '{0}' が参照されましたが、見つかりませんでした。

<a name="MM2007" />

#### <a name="mm2007-xamarinmac-unified-api-against-a-full-net-profile-does-not-support-linking-pass-the--nolink-flag"></a>MM2007:Xamarin.Mac Unified API は、完全な .NET プロファイルに対しては、リンクをサポートしていません。 -Nolink フラグを渡します。

<a name="MM2009" />

#### <a name="mm2009-referenced-by-01------this-message-is-related-to-mm2006-"></a>MM2009:によって参照される{0}.{1}    * * このメッセージに関連する MM2006 * *

<a name="MM2010" />

#### <a name="mm2010-unknown-httpmessagehandler-0-valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MM2010:不明な HttpMessageHandler`{0}`します。 有効な値は HttpClientHandler (既定値)、CFNetworkHandler または NSUrlSessionHandler です。

<a name="MM2011" />

#### <a name="mm2011-unknown-tlsprovider-0--valid-values-are-default-or-appletls"></a>MM2011:不明な TLSProvider '{0}します。  有効な値は既定値または appletls です。

<a name="MM2012" />

#### <a name="mm2012-only-first-0-of-1-referenced-by-warnings-shown--this-message-related-to-2009-"></a>MM2012:最初のみ{0}の{1}「によって参照される」警告を表示します。 ** このメッセージは、2009 年に関連する \*\*

<a name="MM2013" />

#### <a name="mm2013-failed-to-resolve-the-reference-to-0-referenced-in-1-the-app-will-not-include-the-referenced-assembly-and-may-fail-at-runtime"></a>MM2013:参照を解決できませんでした"{0}「, で参照される」{1}"。 アプリでは、参照先のアセンブリは含まれませんし、実行時に失敗する可能性があります。

<a name="MM2014" />

#### <a name="mm2014-xamarinmac-extensions-do-not-support-linking-request-for-linking-will-be-ignored--this-message-is-obsolete-in-xm-36-"></a>MM2014:Xamarin.Mac 拡張機能は、リンクをサポートしていません。 要求のリンクは無視されます。 ** このメッセージは XM 3.6 以降で不使用 \*\*

<!-- 2015 used by mtouch -->

<a name="MM2016" />

#### <a name="mm2016-invalid-tlsprovider-0-option-the-only-valid-value-1-will-be-used"></a>MM2016:無効な TlsProvider`{0}`オプション。 唯一の有効な値`{1}`使用されます。

<a name="MM2017" />

#### <a name="mm2017-could-not-process-xml-description-0"></a>MM2017:XML の説明を処理できませんでした。 {0}

<a name="MM202x" />

#### <a name="mm202x-binding-optimizer-failed-processing-"></a>MM202x:オプティマイザーのバインドには、処理が失敗しました`...`します。

<a name="MM2100" />

#### <a name="mm2100-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2100:Xamarin.Mac のクラシック API では、プラットフォームのリンクはサポートされていません。

<a name="MM2103" />

#### <a name="mm2103-error-processing-assembly--"></a>MM2103:アセンブリの処理中にエラー '\*': *

アセンブリを処理するときに、予期しないエラーが発生しました。

エラー メッセージで問題を引き起こしているアセンブリの名前が。 アセンブリでこの問題を解決するためにを指定する必要があります、[バグ レポート](https://bugzilla.xamarin.com)と共に完全なビルド ログ詳細度を有効になっていると (つまり`-v -v -v -v`で、**追加 mtouch 引数**)。

<a name="MM2104" />

#### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104:アセンブリをリンクできません '{0}' 混合モードです。

リンカーによっては、混合モードのアセンブリを処理できません。

参照してください https://msdn.microsoft.com/library/x0w2664k.aspx 混合モード アセンブリの詳細についてはします。

## <a name="mm3xxx-aot"></a>MM3xxx:AOT

### <a name="mm30xx-aot-general-errors"></a>MM30xx:AOT (全般) エラー

<a name="MM3001" />

#### <a name="mm3001-could-not-aot-the-assembly-0"></a>MM3001:AOT アセンブリではない可能性があります '{0}'

<!-- 3002 used by mtouch -->
<!-- 3003 used by mtouch -->
<!-- 3004 used by mtouch -->
<!-- 3005 used by mtouch -->
<!-- 3006 used by mtouch -->
<!-- 3007 used by mtouch -->
<!-- 3008 used by mtouch -->

<a name="MM3009" />

#### <a name="mm3009-aot-of-0-was-requested-but-was-not-found"></a>MM3009:AOT の '{0}' が要求されましたが、見つかりませんでした

<a name="MM3010" />

#### <a name="mm3010-exclusion-of-aot-of-0-was-requested-but-was-not-found"></a>MM3010:除外の AOT の '{0}' が要求されましたが、見つかりませんでした

## <a name="mm4xxx-code-generation"></a>MM4xxx: コードの生成

### <a name="mm40xx-driverm"></a>MM40xx: driver.m

<a name="MM4001" />

#### <a name="mm4001-the-main-template-could-not-be-expanded-to-0"></a>MM4001:メイン テンプレートを展開できませんでした`{0}`します。

### <a name="mm41xx-registrar"></a>MM41xx: レジストラー

<a name="MM4134" />

#### <a name="mm4134-your-application-is-using-the-0-framework-which-isnt-included-in-the-macos-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-osx-2-while-youre-building-with-the-macos-1-sdk-this-configuration-is-not-supported-with-the-static-registrar-pass---registrardynamic-as-an-additional-mmp-argument-in-your-projects-mac-build-option-to-select-alternatively-select-a-newer-sdk-in-your-apps-mac-build-options"></a>MM4134:アプリケーションを使用して、'{0}' フレームワークで、アプリのビルドを使用している MacOS SDK に含まれていません (このフレームワークは、os X で導入された{2}、MacOS で構築しているときに、 {1} SDK)。この構成は静的登録機関 (パス--レジストラー: 動的な選択では、プロジェクトの Mac ビルド オプションで追加の mmp 引数として) でサポートされていません。 また、アプリの Mac ビルド オプションで新しい SDK を選択します。

## <a name="mm5xxx-gcc-and-toolchain"></a>MM5xxx:GCC と、ツール チェーン

### <a name="mm51xx-compilation"></a>MM51xx: コンパイル

<a name="MM5101" />

#### <a name="mm5101-missing-0-compiler-please-install-xcode-command-line-tools-component"></a>MM5101:不足している '{0}' コンパイラ。 Xcode コマンド ライン ツールのコンポーネントをインストールしてください。

<!-- 5102 used by mtouch -->

<a name="MM5103" />

#### <a name="mm5103-failed-to-compile-error-code---0-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM5103:コンパイルに失敗しました。 エラー コード -{0}します。 バグ報告を提出してください。 http://bugzilla.xamarin.com

<!-- 5104 used by mtouch -->

### <a name="mm52xx-linking"></a>MM52xx: リンク

<a name="MM5202" />

#### <a name="mm5202-monoframework-mdk-is-missing-please-install-the-mdk-for-your-monoframework-version-from-httpmono-projectcomdownloads"></a>MM5202:Mono.framework MDK がありません。 Mono.framework バージョン MDK をインストールしてください。 http://mono-project.com/Downloads

<a name="MM5203" />

#### <a name="mm5203-cant-find-libxammaca-likely-because-of-a-corrupted-xamarinmac-installation-please-reinstall-xamarinmac"></a>MM5203:Libxammac.a、破損した Xamarin.Mac のインストールによる可能性がありますを見つけることができません。 Xamarin.Mac を再インストールしてください。

<a name="MM5204" />

#### <a name="mm5204-invalid-architecture-x8664-is-only-supported-on-non-classic-profiles"></a>MM5204:無効なアーキテクチャです。 x86_64 はクラシック以外のプロファイルでのみサポートされます。

<a name="MM5205" />

#### <a name="mm5205-invalid-architecture-0-valid-architectures-are-i386-and-x8664-when---profilemobile"></a>MM5205:無効なアーキテクチャ '{0}'。 有効なアーキテクチャには、i386 と x86_64 (プロファイルの場合--= モバイル)。

<a name="MM5218" />

#### <a name="mm5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MM5218:動的シンボル {symbol} を無視することはできません (--無視動的シンボル = {シンボル}) ため、動的な型のシンボルとして検出されませんでした。

参照してください、[同等 mtouch 警告](~/ios/troubleshooting/mtouch-errors.md#MT5218)します。

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

#### <a name="mm5305-missing-otool-tool-please-install-xcode-command-line-tools-component"></a>MM5305:不足している 'otool' ツールです。 Xcode コマンド ライン ツールのコンポーネントをインストールしてください。

<a name="MM5306" />

#### <a name="mm5306-missing-dependencies-please-install-xcode-command-line-tools-component"></a>MM5306:依存関係がないです。 Xcode コマンド ライン ツールのコンポーネントをインストールしてください。

<a name="MM5308" />

#### <a name="mm5308-xcode-license-agreement-may-not-have-been-accepted--please-launch-xcode"></a>MM5308:Xcode の使用許諾契約書が受け入れられない可能性があります。  Xcode を起動してください。

<a name="MM5309" />

#### <a name="mm5309-native-linking-failed-with-error-code-1--check-build-log-for-details"></a>MM5309:ネイティブのリンクは、エラー コード 1 で失敗しました。  詳細については、ビルド ログを確認します。

<a name="MM5310" />

#### <a name="mm5310-installnametool-failed-with-an-error-code-0-check-build-log-for-details"></a>: MM5310 エラー コードに失敗しました install_name_tool '{0}'。 詳細については、ビルド ログを確認します。

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

#### <a name="mm8017-the-boehm-garbage-collector-is-not-supported-please-use-sgen-instead"></a>MM8017:Boehm ガベージ コレクターがサポートされていません。 SGen を使用してください。

