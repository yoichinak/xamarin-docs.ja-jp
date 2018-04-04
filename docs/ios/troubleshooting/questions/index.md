---
title: よく寄せられる質問
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 65E04188-185D-493D-BA3C-A89711CB6CAF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: f8264c48b3b4679d45d5603b5637df0016cd3d17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="frequently-asked-questions"></a>よく寄せられる質問

## <a name="general-questions"></a>一般的な質問

### <a name="can-i-use-a-mac-vm-with-xamarinmac-vmmd"></a>[Xamarin で Mac VM を使用できますか。](mac-vm.md)
はい、ただし、に対してのみ Mac のハードウェア。

### <a name="how-can-i-downgrade-xcodedowngrade-xcodemd"></a>[Xcode をダウングレードする方法を教えてください。](downgrade-xcode.md)
このガイドでは、Xcode の以前のバージョンと最新バージョンにアクセスするリンクを示します。

### <a name="where-can-i-set-my-ios-sdk-locationsios-sdkmd"></a>[iOS SDK の場所はどこで設定できますか。](ios-sdk.md)
ほとんどのユーザーにこれらを適切な場所に自動的に設定されます。 このガイドでは、SDK の既定の場所と必要な場合は、それらを変更する方法を示します。

### <a name="how-can-i-reenable-developer-options-after-updating-iosupdate-developer-optionsmd"></a>[開発者オプションを再有効化 iOS を更新した後に方法は?](update-developer-options.md)
IOS バグが iOS のバージョンを更新した後に表示されなくなった開発者オプションがあります、これが監視されて iOS に切り替えるときに 8.x です。 このガイドでは、オプションを再有効化する方法について説明します。

### <a name="user-location-not-working-in-ios-8ios8-user-locationmd"></a>[iOS 8 でユーザーの場所が機能しません](ios8-user-location.md)
このガイドでは、8、iOS でユーザーの場所を有効にする info.plist を編集する方法を説明します。

### <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logssymbolicate-ios-crashmd"></a>[iOS クラッシュ ログにシンボル名を付加するための .dSYM ファイルはどこで入手できますか。](symbolicate-ios-crash.md)
このガイドでは、クラッシュを診断するために iOS クラッシュ ログを symbolicating の基本的な手順について説明します。 Symbolication 手法 & iOS クラッシュ ログを解釈する方法についての情報をより高度なその他のリソースにもリンクします。


### <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studioxs-mono-runtimemd"></a>[Xamarin Studio で iOS プロジェクトの Mono ランタイム環境変数を設定する方法を教えてください。](xs-mono-runtime.md)
モノラルの任意のランタイム環境変数を設定する必要がある場合設定できます、**プロジェクトのオプション > 実行 > 全般**ページ。

## <a name="publishing-questions"></a>発行の質問

### <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submissioninvalid-bundle-bitcodemd"></a>[アプリ ストアに送信するときのエラー:「送信で検出された無効なバンドルのオプションを bitcode に埋め込むことはできません」](invalid-bundle-bitcode.md)

アプリを送信する_を必要と_watchOS、tvOS のアプリなど、bitcode は Xcode 9 で行う必要があります。

### <a name="can-i-change-the-output-path-of-the-ipa-fileipa-output-pathmd"></a>[IPA ファイルの出力パスを変更することができますか。](ipa-output-path.md)
Xamarin サイクル 7、時点では、これを実現するためにカスタマイズされた MSBuild ターゲットを使用できます。

### <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folderipa-tfsmd"></a>[IPA 出力ファイルを TFS のドロップ フォルダーにコピーする方法は?](ipa-tfs.md)
はい、このガイドで説明する方法です。

### <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studiomodify-ipamd"></a>[ファイルを追加または Visual Studio でビルドした後、IPA ファイルからファイルを削除できますか。](modify-ipa.md)
はい、できますが、再署名する必要は通常、`.app`変更を行った後にバンドルできます。 変更することに注意してください、`.ipa`ファイルは通常の使用に必要ではありません。 この記事の内容は情報提供を目的にのみ提供されます。

### <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studiocreate-xcarchivemd"></a>[Visual Studio から .xcarchive アーカイブを作成することは?](create-xcarchive.md)
Xamarin 4 では、これで作成すること、 `.xcarchive` Windows を設定してから、`ArchiveOnBuild`プロパティを`true`です。

### <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--itunesmetadata-disallowed-pathsmd"></a>[アプリの送信が失敗して "Disallowed paths ( "iTunesMetadata.plist" ) found at ..." (許可されないパス ( "iTunesMetadata.plist" ) が ... で見つかりました) と表示されるのはなぜですか。](itunesmetadata-disallowed-paths.md)
このエラーは、Apple の App Store の検証プロセスの変更の結果を示します。 この特定のエラーは_いない_Xamarin がインストールされている特定のバージョンに関連しているためにダウン グレードは_いない_ヘルプします。 このガイドは、問題を解決する方法の詳細については、ページにリンクしています。


## <a name="diagnosing-specific-error-messages"></a>特定のエラー メッセージを診断します。

### <a name="ios-designer-error-with-registerserviceporterror-registerserviceportmd"></a>[RegisterServicePort での iOS Designer エラー](error-registerserviceport.md)
エラーの`RegisterServicePort`上と同様に同様のエラー メッセージは通常、コンピューターにスパイウェア/マルウェアの問題とします。 このガイドは、診断およびスパイウェア/マルウェアの削除に関する情報を確認する詳細です。

### <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychainno-codesigning-keysmd"></a>[iOS のビルドが失敗して "no valid iPhone code signing keys found in keychain" (キーチェーンに有効な iPhone コード署名キーがありません) と表示されるのはなぜですか。](no-codesigning-keys.md)
このエラー メッセージは、対象のプロジェクトが有効なコード署名資格情報を検索するときに発生するが、それらを検索することはできません。 テストと物理的な iOS デバイス上の展開に必要なコード署名できるだけでなく、アドホックおよびアプリのビルドを格納できます。

### <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-objectexception-marshal-obj-cmd"></a>[iOS 9 アプリが失敗して "System.Exception: Failed to marshal the Objective-C object" (System.Exception: Objective-C object をマーシャリングできませんでした) と表示されるのはなぜですか。](exception-marshal-obj-c.md)
IOS 9 の API の変更は、基になる API を今すぐとして、アンマネージ コードを呼び出すことが必要とするときにコールバック コンス トラクターを使用することが必要です。

### <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loadederror-mscorlib-not-foundmd"></a>[ランタイム エラー: "Runtime error: The assembly mscorlib.dll was not found or could not be loaded" (アセンブリ mscorlib.dll が見つからないか、読み込めませんでした)](error-mscorlib-not-found.md)
この問題が発生したときに、*非表示*`.monotouch-32`と`.monotouch-64`フォルダーが見つかりません、`.xcarchive`の署名/IPA 作成、実行時エラーをトリガーします。

## <a name="deprecated"></a>非推奨

> [!IMPORTANT]
> 以下の記事は、Xamarin の最近のバージョンに解決した問題に適用されます。 ただし、ソフトウェアの最新のバージョンで問題が発生した場合を送信してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)すべてのバージョン管理情報と完全のビルド ログ出力します。



### <a name="ipa-file-is-0-bytesipa-zero-bytesmd"></a>[IPA ファイルが 0 バイトです](ipa-zero-bytes.md)
以前のバージョンの Xamarin 0 バイトである Windows 上の IPA ファイルを引き起こす可能性のある既知の問題がありました。

### <a name="ibtool-error-the-operation-couldnt-be-completederror-ibtoolmd"></a>[IBTool エラー: "The operation couldn’t be completed." (操作を完了できませんでした。)](error-ibtool.md)
Apple[固定](https://developer.apple.com/library/ios/releasenotes/DeveloperTools/RN-Xcode/Chapters/xc6_release_notes.html)この`ibtool`Xcode 6.1.1、したがって Xcode 6.1.1 をアップグレードまたはそれ以降のバグが最も簡単な解決策です。

### <a name="error-mt1009-could-not-copy-the-assemblyerror-mt1009md"></a>[エラー MT1009: "Could not copy the assembly" (アセンブリをコピーできませんでした)](error-mt1009.md)
これは、Xamarin.iOS 7.2.6 を実行しているユーザーに影響します。 この問題は Xamarin.iOS を別のユーザー アカウントを使用してインストールするより高い特権を必要とするファイルのアクセス許可により、開発者の主なアカウントです。

### <a name="systemexception-amdevicenotificationsubscribe-returned-exception-amddevicenotificationsubscribemd"></a>[System.Exception AMDeviceNotificationSubscribe returned ... (System.Exception AMDeviceNotificationSubscribe が ... を返しました)](exception-amddevicenotificationsubscribe.md)
For Mac またはで初めて Visual Studio を起動するときに、このメッセージがエラー ダイアログに表示されることができます、`mtbserver.log`ファイル。 これは、一般的でない問題であることに注意してください。 表示される可能性が高いその他のエラーがある場合は、Visual Studio には、Mac ビルド ホストへの接続に問題があることを`mtbserver.log`ファイル。

### <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequestmdocarchivetomsxdocconverter-not-foundmd"></a>[MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest (MDocArchiveToMsxDocConverter.exe が rver.BaseCommand.OnRequest に見つかりません)](mdocarchivetomsxdocconverter-not-found.md)
このエラーに表示される可能性があります、 `Mac Server Log` Visual Studio でします。
