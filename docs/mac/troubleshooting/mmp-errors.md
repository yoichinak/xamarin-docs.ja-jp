---
title: Xamarin. Mac エラーメッセージ (mmp)
description: このドキュメントでは、コンパイルされたアセンブリを実行可能な Mac アプリケーションにパッケージ化するために使用するツールである mmp によって生成されるエラーを示します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5B26339F-A202-4E41-9229-D0BC9E77868E
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/27/2018
ms.openlocfilehash: 32fecaff7f6896c03f3ac094d478fef3338f844d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025722"
---
# <a name="xamarinmac-error-messages-mmp"></a>Xamarin. Mac エラーメッセージ (mmp)

## <a name="mm0xxx-mmp-error-messages"></a>MM0xxx: mmp エラーメッセージ

たとえば、 パラメーター、環境、ツールがありません。

<a name="MM0000" />

#### <a name="mm0000-unexpected-error---please-file-a-bug-report-at-httpsgithubcomxamarinxamarin-maciosissuesnew"></a>MM0000: 予期しないエラー- https://github.com/xamarin/xamarin-macios/issues/new でバグレポートをファイルに登録してください

予期しないエラー状態が発生しました。 次のような、できるだけ多くの情報を含む[バグレポートをファイル](https://github.com/xamarin/xamarin-macios/issues/new)に登録してください。

* 最大の詳細度を持つ完全なビルドログ (たとえば、**追加の mmp 引数**には `-v -v -v -v`)。
* エラーを再現する最小限のテストケースそして
* すべてのバージョンの解説

正確なバージョン情報を取得する最も簡単な方法は、 **[Xamarin Studio]** メニューを使用し**て、Xamarin Studio 項目について**、 **[詳細の表示]** ボタンをクリックし、バージョン情報をコピー/貼り付けすることです ( **[情報のコピー]** ボタンを使用できます)。

<a name="MM0001" />

#### <a name="mm0001-this-version-of-xamarinmac-requires-mono-0-the-current-mono-version-is-1-please-update-the-monoframework-from-httpmono-projectcomdownloads"></a>MM0001: このバージョンの Xamarin. Mac では、Mono {0} が必要です (現在の Mono バージョンは {1})。 http://mono-project.com/Downloads から Mono フレームワークを更新してください

<a name="MM0003" />

#### <a name="mm0003-application-name-0exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MM0003: アプリケーション名 '{0}' が SDK または製品アセンブリ (.dll) 名と競合しています。

<a name="MM0007" />

#### <a name="mm0007-the-root-assembly-0-does-not-exist"></a>MM0007: ルートアセンブリ '{0}' が存在しません

<a name="MM0008" />

#### <a name="mm0008-you-should-provide-one-root-assembly-only-found-0-assemblies-1"></a>MM0008: 1 つのルートアセンブリのみを指定する必要があります。 {0} アセンブリ: '{1}'

<a name="MM0009" />

#### <a name="mm0009-error-while-loading-assemblies-"></a>MM0009: アセンブリの読み込み中にエラーが発生しました: *。

ルートアセンブリ参照からアセンブリを読み込み中にエラーが発生しました。 ビルド出力で詳細情報が提供される場合があります。

<a name="MM0010" />

#### <a name="mm0010-could-not-parse-the-command-line-arguments-0"></a>MM0010: コマンドライン引数を解析できませんでした: {0}

<!-- 0013 is unused -->

<a name="MM0016" />

#### <a name="mm0016-the-option-0-has-been-deprecated"></a>MM0016: オプション '{0}' は非推奨とされました。

<a name="MM0017" />

#### <a name="mm0017-you-should-provide-a-root-assembly"></a>MM0017: ルートアセンブリを指定する必要があります

<a name="MM0018" />

#### <a name="mm0018-unknown-command-line-argument-0"></a>MM0018: 不明なコマンドライン引数: '{0}'

<a name="MM0020" />

#### <a name="mm0020-the-valid-options-for-0-are-1"></a>MM0020: '{0}' の有効なオプションは '{1}' です。

<a name="MM0023" />

#### <a name="mm0023-application-name-0exe-conflicts-with-another-user-assembly"></a>MM0023: アプリケーション名 '{0}' が別のユーザーアセンブリと競合しています。

<a name="MM0026" />

#### <a name="mm0026-could-not-parse-the-command-line-argument-0-1"></a>MM0026: コマンドライン引数 '{0}' を解析できませんでした: {1}

<a name="MM0043" />

#### <a name="mm0043-the-boehm-garbage-collector-is-not-supported-the-sgen-garbage-collector-has-been-selected-instead"></a>MM0043: Boehm ガベージコレクターはサポートされていません。 代わりに、SGen ガベージコレクターが選択されています。

<a name="MM0050" />

#### <a name="mm0050-you-cannot-provide-a-root-assembly-if---no-root-assembly-is-passed"></a>MM0050:--root アセンブリが渡された場合、ルートアセンブリを指定することはできません。

<a name="MM0051" />

#### <a name="mm0051-an-output-directory---output-is-required-if---no-root-assembly-is-passed"></a>MM0051:--root アセンブリが渡された場合、出力ディレクトリ (--output) が必要です。

<a name="MM0053" />

#### <a name="mm0053-cannot-disable-new-refcount-with-the-unified-api"></a>MM0053: Unified API で新しい refcount を無効にすることはできません。

<a name="MM0056" />

#### <a name="mm0056-cannot-find-xcode-in-any-of-our-default-locations-please-install-xcode-or-pass-a-custom-path-using---sdkrootpath"></a>MM0056: 既定の場所のいずれにも Xcode が見つかりません。 Xcode をインストールするか、--sdkroot =\<パスを使用してカスタムパスを渡してください >

<a name="MM0059" />

#### <a name="mm0059-could-not-find-the-currently-selected-xcode-on-the-system-0"></a>MM0059: 現在選択されている Xcode がシステムで見つかりませんでした: {0};

<a name="MM0060" />

#### <a name="mm0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned-0-but-that-directory-does-not-exist"></a>MM0060: 現在選択されている Xcode がシステムで見つかりませんでした。 ' xcode--print-path ' から '{0}' が返されましたが、そのディレクトリは存在しません。

<a name="MM0068" />

#### <a name="mm0068-invalid-value-for-target-framework-0"></a>MM0068: ターゲットフレームワークの値が無効です: {0}。

<a name="MM0071" />

#### <a name="mm0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinmac-please-file-a-bug-report-at-httpsbugzillaxamarincom-with-a-test-case"></a>MM0071: 不明なプラットフォーム: *。 これは通常 Xamarin.Mac; のバグを示しますバグ報告を送信してください https://bugzilla.xamarin.com とテスト_ケースをします。

これは通常、Xamarin. Mac のバグであることを示します。テストケースがある[https://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac)でバグレポートをファイルに登録してください。

<a name="MM0073" />

#### <a name="mm0073-xamarinmac--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a>MM0073: Xamarin \* は \* の配置ターゲットをサポートしていません (最小値は \*)。 プロジェクトの情報で、新しい配置ターゲットを選択してください。

最小配置ターゲットは、エラーメッセージに指定されているものです。プロジェクトの情報で、新しい配置ターゲットを選択してください。

配置ターゲットを更新できない場合は、古いバージョンの Xamarin. Mac を使用してください。

<a name="MM0074" />

#### <a name="mm0074-xamarinmac--does-not-support-a-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinmac"></a>MM0074: Xamarin \* は \* の配置ターゲットをサポートしていません (最大値は \*)。 プロジェクトの情報で古い配置ターゲットを選択するか、新しいバージョンの Xamarin. Mac にアップグレードしてください。

Xamarin では、最小の配置ターゲットを、この特定のバージョンの Xamarin のバージョンよりも上位のバージョンに設定することはサポートされていません。

プロジェクトの情報で古い最小配置ターゲットを選択するか、新しいバージョンの Xamarin. Mac にアップグレードしてください。

<a name="MM0079" />

#### <a name="mm0079-internal-error---no-executable-was-copied-into-the-app-bundle-please-contact-supportxamarincom"></a>MM0079: 内部エラー-実行可能ファイルがアプリバンドルにコピーされませんでした。 'support@xamarin.com' にお問い合わせください

<a name="MM0080" />

#### <a name="mm0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MM0080: NewRefCount を無効にしています。--new-refcount: false は非推奨とされます。

<!-- 0088 used by mtouch -->
<!-- 0089 used by mtouch -->

<a name="MM0091" />

#### <a name="mm0091-this-version-of-xamarinmac-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-use-the-dynamic-registrar-or-set-the-managed-linker-behaviour-to-link-platform-or-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MM0091: このバージョンの Xamarin. Mac には \* SDK (Xcode \*に付属) が必要です。 Xcode をアップグレードして必要なヘッダーファイルを取得するか、動的レジストラーを使用するか、またはマネージリンカーの動作を設定してプラットフォームまたはリンクフレームワーク Sdk のみをリンクします (新しい Api を使用しないようにしてください)。

Xamarin. Mac では、エラーメッセージに示されている SDK バージョンのヘッダーファイルを使用して、静的レジストラーでアプリケーションをビルドする必要があります。 このエラーを修正するには、Xcode をアップグレードして必要な SDK を取得することをお勧めします。これには、必要なすべてのヘッダーファイルが含まれます。 複数のバージョンの Xcode がインストールされている場合、または既定以外の場所で Xcode を使用する場合は、IDE の設定で正しい Xcode の場所を設定してください。

もう1つの方法は、マネージリンカーを有効にすることです。 これにより、ほとんどの場合、ヘッダーファイルが存在しない (または不完全な) 新しい API を含む、未使用の API が削除されます。 ただし、Xcode が提供する API よりも新しい SDK で導入された API がプロジェクトで使用されている場合、これは機能しません。

別の方法としては、代わりに動的レジストラーを使用します。 これにより、型を動的に登録し、ヘッダーファイルの要件を削除することで、スタートアップコストがかかります。 

Straw の最後の解決策は、プロジェクトに必要な SDK をサポートする古いバージョンの Xamarin. Mac を使用することです。

<a name="MM0097" />

#### <a name="mm0097-machineconfig-file-0-can-not-be-found"></a>MM0097: machine.config ファイル '{0}' が見つかりません。

<a name="MM0098" />

#### <a name="mm0098-aot-compilation-is-only-available-on-unified"></a>MM0098: AOT のコンパイルは統合でのみ使用できます

<a name="MM0099" />

#### <a name="mm0099-internal-error-0-please-file-a-bug-report-with-a-test-case-httpsbugzillaxamarincom"></a>MM0099: 内部エラー {0}。 テストケース (https://bugzilla.xamarin.com)を含むバグレポートをファイルに登録してください。

<a name="MM0114" />

#### <a name="mm0114-hybrid-aot-compilation-requires-all-assemblies-to-be-aot-compiled"></a>MM0114: Hybrid AOT のコンパイルでは、すべてのアセンブリが AOT コンパイルされている必要があります。

<a name="MM0129" />

#### <a name="mm0129-debugging-symbol-file-for--does-not-match-the-assembly-and-is-ignored"></a>MM0129: ' * ' のデバッグシンボルファイルがアセンブリと一致しないため、無視されます。

デバッグシンボル-指定したアセンブリの .pdb (ポータブル pdb のみ) または .mdb ファイル-を読み込むことができませんでした。

これは通常、アセンブリがシンボルより新しいか、または古いことを意味します。 これらは一致しないため、使用することはできず、シンボルは無視されます。

この警告は、ビルドされているアプリケーションには影響しませんが、完全にデバッグできない場合があります (具体的には、指定したアセンブリからのコード)。 また、例外、スタックトレース、およびクラッシュレポートには、一部の情報が欠落している場合があります。

この問題をアセンブリパッケージの発行元 (NuGet 作成者など) に報告してください。今後のリリースで修正される可能性があります。

<a name="MM0130" />

#### <a name="mm0130-no-root-assemblies-found-you-should-provide-at-least-one-root-assembly"></a>MM0130: ルートアセンブリが見つかりませんでした。 少なくとも1つのルートアセンブリを指定する必要があります。

`--runregistrar`を実行する場合は、少なくとも1つのルートアセンブリを指定する必要があります。

<a name="MM0131" />

#### <a name="mm0131-product-assembly-0-not-found-in-assembly-list-1"></a>MM0131: 製品アセンブリ '{0}' がアセンブリリストに見つかりません: '{1}'

`--runregistrar`を実行する場合、アセンブリリストに製品アセンブリ XamMac を含める必要があります。

<a name="MM0132" />

#### <a name="mm0132-unknown-optimization--valid-values-are-"></a>MM0132: 不明な最適化: \*。 有効な値は次のとおりです: \*

指定された最適化を認識できませんでした。

許容される形式は `[+|-]optimization-name`です。 `optimization-name` は、エラーメッセージに示されている値のいずれかです。

各最適化の詳細については、「[ビルドの最適化](https://developer.xamarin.com/guides/cross-platform/macios/build-optimizations)」を参照してください。

<a name="MM0133" />

#### <a name="mm0133-found-more-than-1-assembly-matching-0-choosing-first-1"></a>MM0133: 1 つ以上のアセンブリに一致する '{0}' が見つかりました。最初の選択: '{1}'

<a name="MM0134" />

#### <a name="mm0134-32-bit-applications-should-be-migrated-to-64-bit"></a>MM0134:32 ビットアプリケーションは、64ビットに移行する必要があります。

Apple は、32ビットアプリの macOS アプリストアの送信を許可しないことを発表しました (2018 年1月から)。 

さらに、32ビットのアプリケーションは、"侵害なし" ではなく、macOS のバージョンでは実行されません。 

詳細情報: https://developer.apple.com/news/?id=06282017a

アプリケーションと依存関係をすべて64ビットに更新することを検討してください。

<a name="MM0135" />

#### <a name="mm0135-did-not-link-system-framework-0-referenced-by-assembly-1-because-it-was-introduced-in-2-3-and-were-using-the-2-4-sdk"></a>MM0135: {2} {3}で導入され、{2} {4} SDK を使用しているため、システムフレームワーク '{0}' (アセンブリ '{1}' によって参照されています) をリンクしませんでした。

アプリケーションをビルドするには、Xamarin. Mac をシステムライブラリにリンクする必要があります。その一部は、エラーメッセージに示されている SDK のバージョンによって異なります。 以前のバージョンの SDK を使用しているため、これらの Api への呼び出しは実行時に失敗する可能性があります。

このエラーを修正するには、Xcode をアップグレードして必要な SDK を取得することをお勧めします。 複数のバージョンの Xcode がインストールされている場合、または既定以外の場所で Xcode を使用する場合は、IDE の設定で適切な Xcode の場所を設定してください。

または、マネージ[リンカー](https://docs.microsoft.com/xamarin/mac/deploy-test/linker)を有効にして、指定したライブラリを必要とする新しい api (ほとんどの場合は) を含む未使用の api を削除します。 ただし、Xcode で提供されているものより新しい SDK で導入された Api がプロジェクトに必要な場合、これは機能しません。

最後の straw ソリューションとして、ビルドプロセス中にこれらの新しい Sdk が存在する必要がない古いバージョンの Xamarin. Mac を使用します。

## <a name="mm1xxx-file-copy--symlinks-project-related"></a>MM1xxx: ファイルコピー/シンボリックリンク (プロジェクト関連)

<a name="MM1034" />

#### <a name="mm1034-could-not-create-symlink-file---target-error-number"></a>MM1034: Could not create symlink '{file}' -> '{target}': error {number}

### <a name="mm14xx-product-assemblies"></a>MM14xx: Product アセンブリ

<a name="MM1401" />

#### <a name="mm1401-the-required-0-assembly-is-missing-from-the-references"></a>MM1401: 必要な '{0}' アセンブリが参照にありません

<a name="MM1402" />

#### <a name="mm1402-the-assembly-0-is-not-compatible-with-this-tool"></a>MM1402: アセンブリ '{0}' は、このツールと互換性がありません

<a name="MM1403" />

#### <a name="mm1403-0-1-could-not-be-found-target-framework-0-is-unusable-to-package-the-application"></a>MM1403: {0} '{1}' が見つかりませんでした。 ターゲットフレームワーク '{0}' は、アプリケーションのパッケージ化に使用できません。

<a name="MM1404" />

#### <a name="mm1404-target-framework-0-is-invalid"></a>MM1404: ターゲットフレームワーク '{0}' が無効です。

<a name="MM1405" />

#### <a name="mm1405-usefullxammacframework-must-always-target-framework-net-45-not-0-which-is-invalid"></a>MM1405: useFullXamMacFramework は、無効な '{0}' ではなく、常に framework .NET 4.5 をターゲットにする必要があります

<a name="MM1406" />

#### <a name="mm1406-target-framework-0-is-invalid-when-targetting-xamarinmac-45-net-framwork"></a>MM1406: ターゲットフレームワーク '{0}' は、Xamarin 4.5 .NET framwork をターゲットにしている場合は無効です。

<a name="MM1407" />

#### <a name="mm1407-mismatch-between-xamarinmac-reference-0-and-target-framework-selected-1"></a>MM1407: Xamarin. Mac 参照 '{0}' とターゲットフレームワークが選択されている '{1}' が一致しません。

### <a name="mm15xx-assembly-gathering-not-requiring-linker-errors"></a>MM15xx: アセンブリの収集 (リンカーを必要としない) エラー

<a name="MM1501" />

#### <a name="mm1501-can-not-resolve-reference-0"></a>MM1501: 参照を解決できません: {0}

### <a name="machocs"></a>MachO.cs

<a name="MM1600" />

#### <a name="mm1600-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1600: マッハ-O ダイナミックライブラリではありません (不明なヘッダー ' 0x{0}'): {1}。

<a name="MM1601" />

#### <a name="mm1601-not-a-static-library-unknown-header-0-1"></a>MM1601: スタティックライブラリではありません (不明なヘッダー '{0}'): {1}。

<a name="MM1602" />

#### <a name="mm1602-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1602: マッハ-O ダイナミックライブラリではありません (不明なヘッダー ' 0x{0}'): {1}。

<a name="MM1603" />

#### <a name="mm1603-unknown-format-for-fat-entry-at-position-0-in-1"></a>MM1603: {1}の位置 {0} にある fat エントリの形式が不明です。

<a name="MM1604" />

#### <a name="mm1604-file-of-type-0-is-not-a-macho-file-1"></a>MM1604: {0} の種類のファイルが MachO ファイル ({1}) ではありません。

## <a name="mm2xxx-linker"></a>MM2xxx: リンカー

### <a name="mm20xx-linker-general-errors"></a>MM20xx: リンカー (一般) エラー

<a name="MM2001" />

#### <a name="mm2001-could-not-link-assemblies"></a>MM2001: アセンブリをリンクできませんでした

<a name="MM2002" />

#### <a name="mm2002-can-not-resolve-reference-0"></a>MM2002: 参照を解決できません: {0}

<a name="MM2003" />

#### <a name="mm2003-option-0-will-be-ignored-since-linking-is-disabled"></a>MM2003: オプション '{0}' は、リンクが無効になっているため、無視されます

<a name="MM2004" />

#### <a name="mm2004-extra-linker-definitions-file-0-could-not-be-located"></a>MM2004: 追加のリンカー定義ファイル '{0}' が見つかりませんでした。

<a name="MM2005" />

#### <a name="mm2005-definitions-from-0-could-not-be-parsed"></a>MM2005: '{0}' からの定義を解析できませんでした。

<a name="MM2006" />

#### <a name="mm2006-native-library-0-was-referenced-but-could-not-be-found"></a>MM2006: ネイティブライブラリ '{0}' が参照されましたが、見つかりませんでした。

<a name="MM2007" />

#### <a name="mm2007-xamarinmac-unified-api-against-a-full-net-profile-does-not-support-linking-pass-the--nolink-flag"></a>MM2007: 完全な .NET プロファイルに対する Unified API は、リンクをサポートしていません。 -Nolink フラグを渡します。

<a name="MM2009" />

#### <a name="mm2009-referenced-by-01------this-message-is-related-to-mm2006-"></a>MM2009: {0}によって参照されます。{1}\*\* このメッセージは MM2006 \*に関連 \*

<a name="MM2010" />

#### <a name="mm2010-unknown-httpmessagehandler-0-valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MM2010: 不明な HttpMessageHandler `{0}`。 有効な値は HttpClientHandler (既定値)、CFNetworkHandler、または NSUrlSessionHandler です。

<a name="MM2011" />

#### <a name="mm2011-unknown-tlsprovider-0--valid-values-are-default-or-appletls"></a>MM2011: 不明な TLSProvider '{0}。  有効な値は、default または appletls です。

<a name="MM2012" />

#### <a name="mm2012-only-first-0-of-1-referenced-by-warnings-shown--this-message-related-to-2009-"></a>MM2012: {1} "" によって参照されている "警告が表示されるのは、最初の {0} のみです。 このメッセージは、2009 \*に関連する \* \*\*

<a name="MM2013" />

#### <a name="mm2013-failed-to-resolve-the-reference-to-0-referenced-in-1-the-app-will-not-include-the-referenced-assembly-and-may-fail-at-runtime"></a>MM2013: "{1}" で参照されている "{0}" への参照を解決できませんでした。 アプリには参照アセンブリは含まれず、実行時に失敗する可能性があります。

<a name="MM2014" />

#### <a name="mm2014-xamarinmac-extensions-do-not-support-linking-request-for-linking-will-be-ignored--this-message-is-obsolete-in-xm-36-"></a>MM2014: Xamarin 拡張子はリンクをサポートしていません。 リンクの要求は無視されます。 XM 3.6 + \*では、このメッセージは廃止 \* \*\*

<!-- 2015 used by mtouch -->

<a name="MM2016" />

#### <a name="mm2016-invalid-tlsprovider-0-option-the-only-valid-value-1-will-be-used"></a>MM2016: 無効な TlsProvider `{0}` オプションです。 有効な値 `{1}` のみが使用されます。

<a name="MM2017" />

#### <a name="mm2017-could-not-process-xml-description-0"></a>MM2017: XML を処理できませんでした。説明: {0}

<a name="MM202x" />

#### <a name="mm202x-binding-optimizer-failed-processing-"></a>MM202x: バインドオプティマイザーで `...`を処理できませんでした。

<a name="MM2100" />

#### <a name="mm2100-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2100: Xamarin Classic API は、プラットフォームのリンクをサポートしていません。

<a name="MM2103" />

#### <a name="mm2103-error-processing-assembly--"></a>MM2103: アセンブリ '\*' の処理中にエラーが発生した: *

アセンブリの処理中に予期しないエラーが発生しました。

問題の原因となっているアセンブリの名前は、エラーメッセージで示されます。 この問題を解決するには、アセンブリを[バグレポート](https://bugzilla.xamarin.com)に追加し、詳細を有効にした完全なビルドログ (**追加の mtouch 引数**に `-v -v -v -v`) を指定する必要があります。

<a name="MM2104" />

#### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: アセンブリ '{0}' は混合モードであるため、リンクできません。

混合モードのアセンブリは、リンカーでは処理できません。

参照してください https://docs.microsoft.com/cpp/dotnet/mixed-native-and-managed-assemblies 混合モード アセンブリの詳細についてはします。

<a name="MM2106" />

#### <a name="mm2106-could-not-optimize-the-call-to-blockliteralsetupblockunsafe-in--at-offset--because-"></a>MM2106: ブロックリテラルへの呼び出しを最適化できませんでした。 \*であるため、オフセット \* では \* 内の Blockliteral [Unsafe] を最適化できませんでした。

リンカーは、`BlockLiteral.SetupBlock` または `Block.SetupBlockUnsafe`の呼び出しを最適化できない場合に、この警告を報告します。

メッセージは `BlockLiteral.SetupBlock[Unsafe]`を呼び出すメソッドを指し、呼び出しを最適化できなかった理由について手掛かりを与えることもできます。

問題が発生した原因を調査し、将来的にはより多くのシナリオを可能にするために、完全なビルドログと共に[問題](https://github.com/xamarin/xamarin-macios/issues/new)を報告してください。

<a name="MM2107" />

#### <a name="mm2107-its-not-safe-to-remove-the-dynamic-registrar-because-reasons"></a>MM2107: {理由} により、動的レジストラーを削除するのは安全ではありません。

この警告は、開発者が (`--optimize:remove-dynamic-registrar` を mmp に渡して) 動的レジストラーの削除を要求したときに、リンカーによって安全でないと判断された場合に報告されます。

警告を削除するには、最適化引数を mmp に削除するか、または無視するように `--nowarn:2107` 渡します。

既定では、このオプションは、可能で、安全に実行できる場合は常に自動的に有効になります。

<a name="MM2108" />

#### <a name="mm2108-0-was-stripped-of-architectures-except-1-to-comply-with-app-store-restrictions-this-could-break-exisiting-codesigning-signatures-consider-stripping-the-library-with-lipo-or-disabling-with---optimize-trim-architectures"></a>MM2108: '{0}' は、App Store の制限に準拠する '{1}' 以外のアーキテクチャを削除しました。 これにより、既存のコード署名署名が壊れる可能性があります。 Lipo でライブラリを削除するか、--optimize =-trim-アーキテクチャを使用して無効にすることを検討してください。

App Store は、32ビットのバリアントを含むライブラリとフレームワークを含むアプリケーションを拒否するようになりました。 ライブラリは、最終的なアプリケーションバンドルにコピーされたときに未使用のアーキテクチャを削除しました。

これは一般に安全であり、追加の利点としてアプリケーションバンドルのサイズが減少します。 ただし、コード署名されているバンドルされたフレームワークでは、署名が無効になります (アプリケーションが署名された場合は、後で再署名されます)。

ソースライブラリから不要なアーキテクチャを完全に削除するには、`lipo` を使用することを検討してください。 アプリケーションが App Store に発行されていない場合は、`--optimize=-trim-architectures` を追加の MMP 引数として渡すことによって、この削除を無効にすることができます。

<a name="MM2109"/>

#### <a name="mm2109-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2109: Xamarin Classic API は、プラットフォームのリンクをサポートしていません。

## <a name="mm3xxx-aot"></a>MM3xxx: AOT

### <a name="mm30xx-aot-general-errors"></a>MM30xx: AOT (一般) エラー

<a name="MM3001" />

#### <a name="mm3001-could-not-aot-the-assembly-0"></a>MM3001: アセンブリ '{0}' を AOT にできませんでした

<!-- 3002 used by mtouch -->
<!-- 3003 used by mtouch -->
<!-- 3004 used by mtouch -->
<!-- 3005 used by mtouch -->
<!-- 3006 used by mtouch -->
<!-- 3007 used by mtouch -->
<!-- 3008 used by mtouch -->

<a name="MM3009" />

#### <a name="mm3009-aot-of-0-was-requested-but-was-not-found"></a>MM3009: '{0}' の AOT が要求されましたが、見つかりませんでした

<a name="MM3010" />

#### <a name="mm3010-exclusion-of-aot-of-0-was-requested-but-was-not-found"></a>MM3010: '{0}' の AOT の除外が要求されましたが、見つかりませんでした

## <a name="mm4xxx-code-generation"></a>MM4xxx: コード生成

### <a name="mm40xx-driverm"></a>MM40xx: driver. m

<a name="MM4001" />

#### <a name="mm4001-the-main-template-could-not-be-expanded-to-0"></a>MM4001: メインテンプレートを `{0}`に展開できませんでした。

### <a name="mm41xx-registrar"></a>MM41xx: レジストラー

<a name="MM4134" />

#### <a name="mm4134-your-application-is-using-the-0-framework-which-isnt-included-in-the-macos-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-osx-2-while-youre-building-with-the-macos-1-sdk-this-configuration-is-not-supported-with-the-static-registrar-pass---registrardynamic-as-an-additional-mmp-argument-in-your-projects-mac-build-option-to-select-alternatively-select-a-newer-sdk-in-your-apps-mac-build-options"></a>MM4134: アプリケーションは、アプリのビルドに使用している MacOS SDK に含まれていない '{0}' フレームワークを使用しています (このフレームワークは OSX {2}で導入されましたが、MacOS {1} SDK を使用してビルドしています)。この構成は、静的なレジストラーではサポートされていません (指定するには、プロジェクトの Mac ビルドオプションで追加の mmp 引数としてレジストラー: dynamic を渡します)。 または、アプリの Mac ビルドオプションで新しい SDK を選択します。

<a name="MM4173" />

#### <a name="mm4173-the-registrar-cant-compute-the-block-signature-for-the-delegate-of-type-delegate-type-in-the-method-method-because-"></a>MM4173: レジストラーは、* を使用して、メソッド {method} の {delegate 型} 型のデリゲートのブロックシグネチャを計算できません。

これは、レジストラーが計算できなかったため、指定されたメソッドのブロックシグネチャを、生成されたレジストラーコードに挿入できなかったことを示す警告です。

これは、実行時にブロックシグネチャを計算する必要があることを意味します。これは多少遅くなります。

現在、この警告には次の2つの原因が考えられます。

1. マネージデリゲートの型は、`System.Delegate` または `System.MulticastDelegate`のいずれかです。 これらの型は特定のシグネチャを表していません。つまり、レジスタは、対応するネイティブシグネチャを計算できません。 この場合の修正は、ブロックに特定のデリゲート型を使用することです (または、プロジェクトの Mac ビルドオプションで追加の mmp 引数として `--nowarn:4173` を追加することによって警告を無視できます)。
2. レジストラーは、デリゲートの `Invoke` メソッドを見つけることができません。 このような状況は発生しないので、テストプロジェクトに[問題](https://github.com/xamarin/xamarin-macios/issues/new)を報告して、修正できるようにしてください。

<a name="MT4174" />

#### <a name="mt4174-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-methods-parameter-parameter"></a>MT4174: メソッド {method} のパラメーター # {parameter} の変換メソッドをデリゲートするブロックが見つかりません。

これは、静的レジストラーが、目的の C ブロックのデリゲートを作成するメソッドを見つけられなかったことを示す警告です。 メソッドを検索するために実行時に試行が行われますが、(MT8009 例外がある) 同様に失敗する可能性があります。

この警告の考えられる原因の1つは、ブロックを使用する API のバインドを手動で作成することです。 バインディングプロジェクトを使用して、目的の C コードをバインドすることをお勧めします。特にブロックが関係している場合は、手動で実行したときに適切に取得するのが非常に複雑であるためです。

そうでない場合は、テストケースに[問題](https://github.com/xamarin/xamarin-macios/issues/new)を報告してください。

## <a name="mm5xxx-gcc-and-toolchain"></a>MM5xxx: GCC とツールチェーン

### <a name="mm51xx-compilation"></a>MM51xx: コンパイル

<a name="MM5101" />

#### <a name="mm5101-missing-0-compiler-please-install-xcode-command-line-tools-component"></a>MM5101: '{0}' コンパイラがありません。 Xcode ' コマンドラインツール ' コンポーネントをインストールしてください。

<!-- 5102 used by mtouch -->

<a name="MM5103" />

#### <a name="mm5103-failed-to-compile-error-code---0-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM5103: コンパイルに失敗しました。 エラーコード-{0}。 http://bugzilla.xamarin.com でバグレポートをファイルに登録してください

<!-- 5104 used by mtouch -->

### <a name="mm52xx-linking"></a>MM52xx: リンク

<a name="MM5202" />

#### <a name="mm5202-monoframework-mdk-is-missing-please-install-the-mdk-for-your-monoframework-version-from-httpmono-projectcomdownloads"></a>MM5202: Mono. framework MDK が見つかりません。 Mono の MDK をインストールしてください。 http://mono-project.com/Downloads

<a name="MM5203" />

#### <a name="mm5203-cant-find-libxammaca-likely-because-of-a-corrupted-xamarinmac-installation-please-reinstall-xamarinmac"></a>MM5203: libxammac が見つかりません。このインストールは壊れている可能性があります。 Xamarin. Mac を再インストールしてください。

<a name="MM5204" />

#### <a name="mm5204-invalid-architecture-x86_64-is-only-supported-on-non-classic-profiles"></a>MM5204: 無効なアーキテクチャです。 x86_64 は、クラシック以外のプロファイルでのみサポートされています。

<a name="MM5205" />

#### <a name="mm5205-invalid-architecture-0-valid-architectures-are-i386-and-x86_64-when---profilemobile"></a>MM5205: アーキテクチャ '{0}' が無効です。 有効なアーキテクチャは i386 と x86_64 (--profile = mobile) です。

<a name="MM5218" />

#### <a name="mm5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MM5218: 動的シンボルとして検出されなかったため、動的シンボル {symbol} (--ignore-dynamic-symbol}) を無視できません。

同等の[mtouch 警告](~/ios/troubleshooting/mtouch-errors.md#MT5218)を参照してください。

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

#### <a name="mm5301-pkg-config-could-not-be-found-please-install-the-monoframework-from-httpmono-projectcomdownloads"></a>MM5301: pkg-config が見つかりませんでした。 http://mono-project.com/Downloads から Mono フレームワークをインストールしてください

<!-- 5302 used by mtouch -->
<!-- 5303 used by mtouch -->
<!-- 5304 used by mtouch -->

<a name="MM5305" />

#### <a name="mm5305-missing-otool-tool-please-install-xcode-command-line-tools-component"></a>MM5305: ' otool ' ツールがありません。 Xcode ' コマンドラインツール ' コンポーネントをインストールしてください

<a name="MM5306" />

#### <a name="mm5306-missing-dependencies-please-install-xcode-command-line-tools-component"></a>MM5306: 依存関係がありません。 Xcode ' コマンドラインツール ' コンポーネントをインストールしてください

<a name="MM5308" />

#### <a name="mm5308-xcode-license-agreement-may-not-have-been-accepted--please-launch-xcode"></a>MM5308: Xcode 使用許諾契約書に同意していない可能性があります。  Xcode を起動してください。

<a name="MM5309" />

#### <a name="mm5309-native-linking-failed-with-error-code-1-check-build-log-for-details"></a>MM5309: ネイティブリンクがエラーコード1で失敗しました。 詳細については、ビルドログを確認してください。

<a name="MM5310" />

#### <a name="mm5310-install_name_tool-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5310: install_name_tool がエラーコード '{0}' で失敗しました。 詳細については、ビルドログを確認してください。

<a name="MM5311" />

#### <a name="mm5311-lipo-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5311: lipo がエラーコード '{0}' で失敗しました。 詳細については、ビルドログを確認してください。

<!-- MM6xxx: mmp internal tools -->
<!-- MM7xxx: reserved -->

## <a name="mm8xxx-runtime"></a>MM8xxx: runtime

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

#### <a name="mm8017-the-boehm-garbage-collector-is-not-supported-please-use-sgen-instead"></a>MM8017: Boehm ガベージコレクターはサポートされていません。 代わりに SGen を使用してください。

<a name="MM8025" />

#### <a name="mm8025-failed-to-compute-the-token-reference-for-the-type-typeassemblyqualifiedname-because-reasons"></a>MM8025: 型 ' {type ' のトークン参照を計算できませんでした。AssemblyQualifiedName} ' は {理由}

これは、Xamarin. Mac のバグを示しています。 バグを送信してください [https://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac)です。

回避策として、プロジェクトの Mac ビルドオプションで追加の mmp 引数として `--optimize:-register-protocols` を渡すことによって、`register-protocols` の最適化を無効にすることが考えられます。

<a name="MM8026" />

#### <a name="mm8026--is-not-supported-when-the-dynamic-registrar-has-been-linked-away"></a>MM8026: * は、動的レジスタがリンクされている場合はサポートされません。

これは通常、必要に応じて動的レジストラーをリンクしないようにする必要があるため、Xamarin. Mac のバグを示します。 バグを送信してください [https://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)です。

プロジェクトの Mac ビルドオプションの追加の mmp 引数に `--optimize=-remove-dynamic-registrar` を追加することによって、動的レジストラーを保持するようにリンカーを設定することができます。
