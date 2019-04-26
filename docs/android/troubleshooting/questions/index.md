---
title: Xamarin.Android のよく寄せられる質問
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 9c0e6d014f27651710bf3b8e713b2bf80322d628
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61153400"
---
# <a name="android-frequently-asked-questions"></a>Android のよく寄せられる質問

## <a name="installation--setup"></a>インストールとセットアップ

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[インストールする必要がある Android SDK パッケージを教えてください](install-android-sdk-packages.md)

Android SDK をインストールすると、開発の最低限必要なパッケージを自動的に含まれていません。 個々 の開発者が異なる必要がある場合、このガイドには、Xamarin.Android を使用した開発に必要なは、一般のパッケージがについて説明します。

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Android SDK の場所はどこで設定できますか](android-sdk-location.md)

このガイドは、Android SDK には、ほとんどの設定の動作の既定設定を説明します必要な場合は、Mac または Visual Studio の Visual Studio でこれらの既定値を変更する方法。

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[Java Development Kit (JDK) のバージョンの更新方法を教えてください](update-jdk.md)

この記事では、Windows とファルダに Java Development Kit (JDK) バージョンを更新する方法を示しています。

### <a name="can-i-use-java-development-kit-jdk-version-9-or-laterjdk9-errorsmd"></a>[Java Development Kit (JDK) バージョン 9 以降を使用できますか。](jdk9-errors.md)

Xamarin.Android は JDK 8 または Microsoft Mobile OpenJDK が必要です。 この記事では、一般的なエラー メッセージが表示されます。 手順については、JDK バージョンを確認すると共に、JDK 9 以降をインストールするかどうかを示します。


### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Xamarin.Android.Support パッケージに必要な Android サポート ライブラリを手動でインストールする方法を教えてください](install-android-support-library.md)

このガイドは、インストールするための手順の例を示します、`Xamarin.Android.Support.v4`ファルダ (&)、Windows のサポート ライブラリ

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Windows 上で Android をデバッグするために必要な USB ドライバーを教えてください](android-drivers-debug-windows.md)

Windows で開発するときに、Android デバイスでデバッグするには互換性のある USB ドライバーをインストールする必要があります。 Android SDK Manager には、既定で Nexus デバイスのサポートを追加する"Google USB Driver"が含まれています。
その他のデバイスには、デバイスの製造元によって発行された USB ドライバーが必要です。 このガイドでは、これらのドライバーも、別のテスト メソッドの検索について説明します。

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Windows VM から Mac 上で動作する Android エミュレーターに接続できますか](connect-android-emulator-mac-windows.md)

このガイドでは、Android エミュレーターを使用する場合、メソッドがについて説明します。

## <a name="general-questions"></a>一般的な質問

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Android NUnit テスト プロジェクトを自動化する方法を教えてください](automate-android-nunit-test.md)

このガイドは、Android NUnit テストのプロジェクトをセットアップするための手順を説明して_いない_Xamarin.UITest プロジェクト。 Xamarin.UITest ガイド[ここ](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest)します。

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Android のリリース ビルドがインターネットに接続できないのはなぜですか](android-internet.md)

この問題の最も一般的な原因は、**インターネット**アクセス許可は、デバッグ ビルドでは自動的に追加しますが、リリース ビルドを手動で設定する必要があります。 このガイドでは、リリース ビルドのアクセス許可を有効にする方法について説明します。

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[より高度な Xamarin Android サポート v4/v13 NuGet パッケージ](android-support-v4v13-libraries.md)

`Support-v4` `Support-v13`は使用できませんまとめて、同じアプリでは、これらは相互に排他的です。 これは、ため`Support-v13`実際にすべての型およびの実装が含まれています`Support-v4`します。 同じプロジェクト内の両方を参照して、重複する型のエラーが発生します。

### <a name="how-do-i-resolve-a-pathtoolongexception-errorpath-too-long-exceptionmd"></a>[PathTooLongException エラーを解決する方法](path-too-long-exception.md)

この記事は、解決する方法を説明します、 **PathTooLongException** Xamarin.Android プロジェクトのビルド中に発生するエラー。



## <a name="deprecated"></a>非推奨

> [!NOTE]
> 以下の記事は、Xamarin の最近のバージョンで解決された問題に適用されます。 ただし、ソフトウェアの最新バージョンで問題が発生した場合を提出してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)完全なバージョン管理情報と完全のビルド ログ出力します。

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[Lollipop のサポートが追加された Xamarin.Android のバージョンを教えてください](xa-lollipop.md)

このガイドは、Android L プレビュー用に作成されたでした。Xamarin.Android 4.17 L Preview のサポートが Android と Xamarin.Android 4.20 追加 Android Lollipop のサポートが追加されます。

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat - No resource found that matches the given name: attr 'android:actionModeShareDrawable'](missing-action-mode-share-drawable.md)

このエラーは、必要な Android SDK パッケージの一部が見つからない場合、以前のバージョンの Xamarin で発生可能性があります。

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Android Designer の Java メモリ パラメーターの調整](android-designer-java-memory.md)

開始するときに使用される既定のメモリ パラメーター、`java`プロセスでは、Android designer がいくつかのシステム構成と互換性がない可能性があります。 以降で Xamarin Studio 5.7.2.7 と Xamarin for Visual Studio 3.9.344 これらの設定は、プロジェクトごとにカスタマイズできます。

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Android Resource.designer.cs ファイルが更新されない](resource-designer-wont-update.md)

Xamarin.Studio 5.1 のバグでは、部分的または完全には、.csproj ファイルの xml コードを削除することによって、.csproj ファイルを以前と破損しています。 これにより、Android の部分 (Android Resource.designer.cs の更新) などのシステムを構築する重要なが失敗します。 5.1.4 安定した時点では、年 7 月 15 日にリリースでは、このバグが修正されました。多くの場合、プロジェクト ファイルがこのガイドの説明に従って、手動で修復します。



