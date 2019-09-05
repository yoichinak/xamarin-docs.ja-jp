---
title: Xamarin. iOS に関してよく寄せられる質問
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 65E04188-185D-493D-BA3C-A89711CB6CAF
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 57908027c6e9215504f410c0f9c989d6fbe3adad
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290506"
---
# <a name="ios-frequently-asked-questions"></a>iOS に関してよく寄せられる質問

## <a name="general-questions"></a>一般的な質問

### <a name="can-i-use-a-mac-vm-with-xamarinmac-vmmd"></a>[Xamarin で Mac VM を使用できますか。](mac-vm.md)
はい。ただし、Mac ハードウェアでのみです。

### <a name="how-can-i-downgrade-xcodedowngrade-xcodemd"></a>[Xcode をダウングレードする方法を教えてください。](downgrade-xcode.md)
このガイドでは、以前のバージョンの Xcode と最新バージョンにアクセスするためのリンクを提供します。

### <a name="where-can-i-set-my-ios-sdk-locationsios-sdkmd"></a>[iOS SDK の場所はどこで設定できますか。](ios-sdk.md)
ほとんどのユーザーに対して、これらは自動的に適切な場所に設定されます。 このガイドでは、既定の SDK の場所と、必要に応じて変更する方法を示します。

### <a name="how-can-i-reenable-developer-options-after-updating-iosupdate-developer-optionsmd"></a>[IOS の更新後に開発者オプションを再び有効にするにはどうすればよいですか。](update-developer-options.md)
Ios のバグによっては、ios バージョンを更新した後に開発者オプションが表示されなくなることがあります。これは、iOS 8.x に切り替えると発生します。 このガイドでは、オプションを再び有効にする方法について説明します。

### <a name="user-location-not-working-in-ios-8ios8-user-locationmd"></a>[iOS 8 でユーザーの場所が機能しません](ios8-user-location.md)
このガイドでは、iOS 8 でユーザーの場所を有効にするために情報を編集する方法について説明します。

### <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logssymbolicate-ios-crashmd"></a>[iOS クラッシュ ログにシンボル名を付加するための .dSYM ファイルはどこで入手できますか。](symbolicate-ios-crash.md)
このガイドでは、クラッシュの診断に役立つように、iOS のクラッシュログを symbolicating するための基本的な手順について説明します。 また、iOS のクラッシュログを解釈する方法について & 詳細な情報を参照するための追加リソースへのリンクもあります。


### <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studioxs-mono-runtimemd"></a>[Xamarin Studio で iOS プロジェクトの Mono ランタイム環境変数を設定する方法を教えてください。](xs-mono-runtime.md)
Mono のランタイム環境変数を設定する必要がある場合は **> [全般] ページで [プロジェクトオプション > 実行**する] を設定できます。

## <a name="publishing-questions"></a>発行に関する質問

### <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submissioninvalid-bundle-bitcodemd"></a>[App Store への送信中にエラーが発生した場合:"無効なバンドル-bitcode への埋め込みを許可されていないオプションが送信時に検出されました"](invalid-bundle-bitcode.md)

WatchOS や tvOS apps など、bitcode を_必要_とするアプリの送信は、Xcode 9 を使用して行う必要があります。

### <a name="can-i-change-the-output-path-of-the-ipa-fileipa-output-pathmd"></a>[IPA ファイルの出力パスを変更できますか。](ipa-output-path.md)
Xamarin サイクル7以降では、カスタマイズされた MSBuild ターゲットを使用してこれを実現できます。

### <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folderipa-tfsmd"></a>[IPA の出力ファイルを TFS の格納フォルダーにコピーする方法はありますか](ipa-tfs.md)
はい、このガイドでは、その方法について説明します。

### <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studiomodify-ipamd"></a>[Visual Studio でビルドした後、IPA ファイルにファイルを追加したり、ファイルを削除したりすることはできますか。](modify-ipa.md)
はい、できますが、変更を行った後でバンドルに`.app`再署名する必要があります。 通常の使用で`.ipa`は、ファイルの変更は必要ありません。 この記事は、あくまで情報提供を目的として提供されています。

### <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studiocreate-xcarchivemd"></a>[Visual Studio から .xcarchive アーカイブを作成することはできますか?](create-xcarchive.md)
Xamarin 4 の時点では、 `.xcarchive` `ArchiveOnBuild`プロパティをに`true`設定することによって、Windows からを作成できるようになりました。

### <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--itunesmetadata-disallowed-pathsmd"></a>[アプリの送信が "Disallowed paths ( "iTunesMetadata.plist" ) found at ..."\(許可されていないパス ("iTunesMetadata.plist") が...で見つかりました\) で失敗するのはなぜですか。](itunesmetadata-disallowed-paths.md)
このエラーは、Apple の App Store の検証プロセスが変更された結果です。 この特定のエラーは、インストールした Xamarin の特定のバージョンには関連して_いない_ため、ダウングレードは役に立ち_ません_。 このガイドでは、この問題の解決方法に関する詳細情報へのリンクを示します。


## <a name="diagnosing-specific-error-messages"></a>特定のエラーメッセージの診断

### <a name="ios-designer-error-with-registerserviceporterror-registerserviceportmd"></a>[RegisterServicePort での iOS Designer エラー](error-registerserviceport.md)
上記の`RegisterServicePort`ようなエラーメッセージが発生したエラーは、通常、コンピューターのスパイウェア/マルウェアに関する問題です。 このガイドでは、スパイウェア/マルウェアの削除に関する診断と情報を確認する方法について詳しく説明します。

### <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychainno-codesigning-keysmd"></a>[iOS のビルドが失敗して "no valid iPhone code signing keys found in keychain" (キーチェーンに有効な iPhone コード署名キーがありません) と表示されるのはなぜですか。](no-codesigning-keys.md)
このエラーメッセージは、問題のプロジェクトが、有効なコード署名資格情報を探しているものの、見つからない場合に発生します。 物理 iOS デバイスでのテストと展開には、コード署名が必要です。また、アドホック & App store ビルドもあります。

### <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-objectexception-marshal-obj-cmd"></a>[iOS 9 アプリが System.Exception: Failed to marshal the Objective-C object\(System.Exception: Objective C オブジェクトのマーシャリングが失敗しました\) で失敗するのはなぜですか。](exception-marshal-obj-c.md)
IOS 9 での API の変更では、基になる API が想定しているように、アンマネージコードを呼び出すときにコールバックコンストラクターを使用する必要があります。

### <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loadederror-mscorlib-not-foundmd"></a>[ランタイム エラー: The assembly mscorlib.dll was not found or could not be loaded (アセンブリ mscorlib.dll が見つからないか、読み込めませんでした)](error-mscorlib-not-found.md)
この問題が発生するのは`.monotouch-64` 、署名/IPA 作成`.xcarchive`のためにで*非表示* `.monotouch-32`のおよびフォルダーが不足している場合に、ランタイムエラーがトリガーされることです。

### <a name="compile-error-can-not-encode-offset-x-in-resulting-scattered-relocationerror-encode-offset-scattered-relocationmd"></a>[コンパイルエラー:拡散した再配置ではオフセット X をエンコードできません](error-encode-offset-scattered-relocation.md)
この問題は、ARMv7 などの32ビットアーキテクチャのビルド時に、最終的なバイナリがネイティブツールチェーンに対して大きすぎる場合に発生します。

## <a name="deprecated"></a>非推奨

> [!IMPORTANT]
> 以下の記事は、Xamarin の最近のバージョンで解決された問題に適用されます。 ただし、最新バージョンのソフトウェアで問題が発生した場合は、完全なバージョン管理情報と完全ビルドログ出力を使用して[新しいバグを作成](~/cross-platform/troubleshooting/questions/howto-file-bug.md)してください。



### <a name="ipa-file-is-0-bytesipa-zero-bytesmd"></a>[IPA ファイルが 0 バイトです](ipa-zero-bytes.md)
以前のバージョンの Xamarin では、Windows 上の IPA ファイルが0バイトになる可能性があるという既知の問題がいくつかありました。

### <a name="ibtool-error-the-operation-couldnt-be-completederror-ibtoolmd"></a>[IBTool エラー: 操作を完了できませんでした。](error-ibtool.md)
Apple は Xcode 6.1.1 でこの `ibtool` バグを[修正](https://developer.apple.com/library/ios/releasenotes/DeveloperTools/RN-Xcode/Chapters/xc6_release_notes.html)したため、Xcode 6.1.1 以降へのアップグレードが最も簡単な修正です。

### <a name="error-mt1009-could-not-copy-the-assemblyerror-mt1009md"></a>[エラー MT1009: Could not copy the assembly (アセンブリをコピーできませんでした)](error-mt1009.md)
これは、Xamarin. iOS 7.2.6 を実行しているユーザーに影響します。 この問題が発生するのは、Xamarin が、別のユーザーアカウントを使用してインストールされている場合に、より高い特権を必要とするファイルのアクセス許可があるためです。

### <a name="systemexception-amdevicenotificationsubscribe-returned-exception-amddevicenotificationsubscribemd"></a>[System.Exception AMDeviceNotificationSubscribe returned ... (System.Exception AMDeviceNotificationSubscribe が ... を返しました)](exception-amddevicenotificationsubscribe.md)
このメッセージは、Visual Studio for Mac を最初に起動したとき、または`mtbserver.log`ファイルでエラーダイアログに表示される場合があります。 これは珍しい問題ではないことに注意してください。 Visual Studio が Mac ビルドホストへの接続に問題がある場合は、その他のエラーが`mtbserver.log`ファイルに表示される可能性が高くなります。

### <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequestmdocarchivetomsxdocconverter-not-foundmd"></a>[MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest (MDocArchiveToMsxDocConverter.exe が rver.BaseCommand.OnRequest に見つかりません)](mdocarchivetomsxdocconverter-not-found.md)
このエラーは、 `Mac Server Log` Visual Studio ので表示される場合があります。
