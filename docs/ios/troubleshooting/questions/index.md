---
title: Xamarin.iOS のよく寄せられる質問
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 65E04188-185D-493D-BA3C-A89711CB6CAF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 8b90b76cad5277fe76fc476a0bcd6f600e91b256
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2019
ms.locfileid: "57981693"
---
# <a name="ios-frequently-asked-questions"></a>iOS のよく寄せられる質問

## <a name="general-questions"></a>一般的な質問

### <a name="can-i-use-a-mac-vm-with-xamarinmac-vmmd"></a>[Xamarin で Mac VM を使用できますか。](mac-vm.md)
はい、Mac のハードウェアでのみです。

### <a name="how-can-i-downgrade-xcodedowngrade-xcodemd"></a>[Xcode をダウングレードする方法を教えてください。](downgrade-xcode.md)
このガイドでは、Xcode の以前のバージョンと最新バージョンにアクセスするリンクを示します。

### <a name="where-can-i-set-my-ios-sdk-locationsios-sdkmd"></a>[iOS SDK の場所はどこで設定できますか。](ios-sdk.md)
ほとんどのユーザーに対してこれらを適切な場所に自動的に設定されます。 このガイドでは、SDK の既定の場所と必要な場合は、それらを変更する方法を示します。

### <a name="how-can-i-reenable-developer-options-after-updating-iosupdate-developer-optionsmd"></a>[開発者向けオプションを再度有効 iOS の更新後に方法は?](update-developer-options.md)
IOS のバグを iOS のバージョンを更新した後消滅する開発者向けのオプションがあります、これが確認されていますから iOS への切り替え 8.x します。 このガイドでは、オプションを再度有効にする方法について説明します。

### <a name="user-location-not-working-in-ios-8ios8-user-locationmd"></a>[iOS 8 でユーザーの場所が機能しません](ios8-user-location.md)
このガイドでは、iOS 8 でユーザーの場所を有効にする info.plist を編集する方法を指示します。

### <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logssymbolicate-ios-crashmd"></a>[iOS クラッシュ ログにシンボル名を付加するための .dSYM ファイルはどこで入手できますか。](symbolicate-ios-crash.md)
このガイドでは、クラッシュを診断するために iOS クラッシュ ログ symbolicating の基本的な手順について説明します。 高度なシンボル化手法 & iOS クラッシュ ログを解釈する方法についての情報の他のリソースにもリンクします。


### <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studioxs-mono-runtimemd"></a>[Xamarin Studio で iOS プロジェクトの Mono ランタイム環境変数を設定する方法を教えてください。](xs-mono-runtime.md)
Mono の任意のランタイム環境変数を設定する必要がある場合で設定できますが、**プロジェクト オプション > 実行 > 全般**ページ。

## <a name="publishing-questions"></a>発行の質問

### <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submissioninvalid-bundle-bitcodemd"></a>[App Store に送信するときに、エラー:「無効なバンドル - オプションをビットコードに埋め込むことはできませんが、送信で検出された」](invalid-bundle-bitcode.md)

アプリを送信する_必要_Xcode 9 でビットコードは、watchOS、tvOS アプリなどを行う必要があります。

### <a name="can-i-change-the-output-path-of-the-ipa-fileipa-output-pathmd"></a>[IPA ファイルの出力パスを変更することができますか。](ipa-output-path.md)
Xamarin サイクルの 7 の時点では、これを実現するためにカスタマイズされた MSBuild のターゲットを使用できます。

### <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folderipa-tfsmd"></a>[IPA 出力ファイルを TFS 格納フォルダーにコピーする方法はありますか](ipa-tfs.md)
はい、このガイドで説明する方法。

### <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studiomodify-ipamd"></a>[ファイルを追加または Visual Studio でビルドした後で IPA ファイルからファイルを削除できますか。](modify-ipa.md)
はい、できますが、再署名する必要は通常、`.app`変更を行った後にバンドルします。 変更することに注意してください、`.ipa`ファイルは通常の使用に必要ではありません。 この記事では情報提供を目的の純粋に提供します。

### <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studiocreate-xcarchivemd"></a>[Visual Studio から .xcarchive アーカイブを作成することはできますか。](create-xcarchive.md)
Xamarin 4 では、これで作成する、 `.xcarchive` Windows を設定してから、`ArchiveOnBuild`プロパティを`true`します。

### <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--itunesmetadata-disallowed-pathsmd"></a>[アプリの送信が "Disallowed paths ( "iTunesMetadata.plist" ) found at ..."\(許可されていないパス ("iTunesMetadata.plist") が...で見つかりました\) で失敗するのはなぜですか。](itunesmetadata-disallowed-paths.md)
このエラーは、Apple の App Store の検証プロセスでの変更の結果です。 この特定のエラーは_いない_は Xamarin がインストールされている特定のバージョンに関連して、そのためにダウン グレード_いない_ヘルプします。 このガイドは、問題を解決する方法の詳細については、ページにリンクしています。


## <a name="diagnosing-specific-error-messages"></a>特定のエラー メッセージを診断します。

### <a name="ios-designer-error-with-registerserviceporterror-registerserviceportmd"></a>[RegisterServicePort での iOS Designer エラー](error-registerserviceport.md)
エラーの`RegisterServicePort`上などのようなエラー メッセージは通常、コンピューターにスパイウェア/マルウェアの問題があるとします。 このガイドは、診断およびマルウェア/スパイウェアを削除する情報を確認する詳細です。

### <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychainno-codesigning-keysmd"></a>[iOS のビルドが失敗して "no valid iPhone code signing keys found in keychain" (キーチェーンに有効な iPhone コード署名キーがありません) と表示されるのはなぜですか。](no-codesigning-keys.md)
このエラー メッセージは、対象のプロジェクトが有効なコード署名の資格情報を検索するときに発生しますが、それらを検索することができません。 コード署名は、テストと物理 iOS デバイスに展開するために必要です。だけでなく、アドホックとアプリのビルドを格納します。

### <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-objectexception-marshal-obj-cmd"></a>[iOS 9 アプリが System.Exception: Failed to marshal the Objective-C object\(System.Exception: Objective C オブジェクトのマーシャリングが失敗しました\) で失敗するのはなぜですか。](exception-marshal-obj-c.md)
IOS 9 の API の変更は、基になる API を今すぐとしてアンマネージ コードを呼び出すことが期待したときにコールバック コンス トラクターを使用することが必要です。

### <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loadederror-mscorlib-not-foundmd"></a>[ランタイム エラー: The assembly mscorlib.dll was not found or could not be loaded (アセンブリ mscorlib.dll が見つからないか、読み込めませんでした)](error-mscorlib-not-found.md)
この問題が発生したときに、*隠し*`.monotouch-32`と`.monotouch-64`フォルダーが見つかりません、`.xcarchive`の署名/IPA の作成、実行時エラーをトリガーします。

## <a name="deprecated"></a>非推奨

> [!IMPORTANT]
> 以下の記事は、Xamarin の最近のバージョンで解決された問題に適用されます。 ただし、ソフトウェアの最新バージョンで問題が発生した場合を提出してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)完全なバージョン管理情報と完全のビルド ログ出力します。



### <a name="ipa-file-is-0-bytesipa-zero-bytesmd"></a>[IPA ファイルが 0 バイトです](ipa-zero-bytes.md)
以前のバージョンの Xamarin 0 バイトにする Windows 上の IPA ファイルを引き起こす可能性のある既知の問題がありました。

### <a name="ibtool-error-the-operation-couldnt-be-completederror-ibtoolmd"></a>[IBTool エラー: 操作を完了できませんでした。](error-ibtool.md)
Apple[固定](https://developer.apple.com/library/ios/releasenotes/DeveloperTools/RN-Xcode/Chapters/xc6_release_notes.html)この`ibtool`Xcode 6.1.1、したがって Xcode 6.1.1 をアップグレードまたはそれ以降のバグが最も簡単な修正します。

### <a name="error-mt1009-could-not-copy-the-assemblyerror-mt1009md"></a>[エラー MT1009: Could not copy the assembly (アセンブリをコピーできませんでした)](error-mt1009.md)
これは、Xamarin.iOS 7.2.6 を実行しているユーザーに影響します。 この問題は、開発者の主なアカウントの別のユーザー アカウントを使用して Xamarin.iOS がインストールされている場合は、高い特権を必要とするファイルのアクセス許可のため、します。

### <a name="systemexception-amdevicenotificationsubscribe-returned-exception-amddevicenotificationsubscribemd"></a>[System.Exception AMDeviceNotificationSubscribe returned ... (System.Exception AMDeviceNotificationSubscribe が ... を返しました)](exception-amddevicenotificationsubscribe.md)
For Mac、または初めて Visual Studio を起動すると、このメッセージがエラー ダイアログに表示されることができます、`mtbserver.log`ファイル。 これは、一般的でない問題であることに注意してください。 表示される可能性が高いその他のエラーがある場合、Visual Studio には、Mac ビルド ホストへの接続に問題がある、`mtbserver.log`ファイル。

### <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequestmdocarchivetomsxdocconverter-not-foundmd"></a>[MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest (MDocArchiveToMsxDocConverter.exe が rver.BaseCommand.OnRequest に見つかりません)](mdocarchivetomsxdocconverter-not-found.md)
このエラーが含まれる、 `Mac Server Log` Visual Studio でします。
