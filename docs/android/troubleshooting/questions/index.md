---
title: よく寄せられる質問
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/19/2018
ms.openlocfilehash: 2deeb4309be44c1c5f8d78980707336ac8301761
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774357"
---
# <a name="frequently-asked-questions"></a>よく寄せられる質問

## <a name="installation--setup"></a>インストールとセットアップ

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[インストールする必要がある Android SDK パッケージを教えてください](install-android-sdk-packages.md)

Android SDK をインストールすると、すべての最低限必要なパッケージを開発するために自動的に含まれません。 個々 の開発者は異なる必要がある、一般的に必要な Xamarin.Android での開発用パッケージについて説明します。

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Android SDK の場所はどこで設定できますか](android-sdk-location.md)

このガイドには、両方の既定の設定、Android SDK、ほとんどの設定で動作するはずがについて説明します必要な場合は、Mac または Visual Studio の Visual Studio でこれらの既定値を変更する方法です。

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[Java Development Kit (JDK) のバージョンの更新方法を教えてください](update-jdk.md)

この記事は、Windows およびファルダで Java Development Kit (JDK) バージョンを更新する方法を示しています。

### <a name="can-i-use-java-development-kit-jdk-version-9jdk9-errorsmd"></a>[Java Development Kit (JDK) バージョン 9 は使用できますか](jdk9-errors.md)

Xamarin.Android では、JDK 9 ではなく、JDK 8 が必要です。 この記事では、一般的なエラー メッセージと共に、JDK バージョンを確認する方法について、JDK 9 をインストールするかどうかが生じる場合を一覧表示します。


### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Xamarin.Android.Support パッケージに必要な Android サポート ライブラリを手動でインストールする方法を教えてください](install-android-support-library.md)

このガイドをインストールするための例の手順では、 `Xamarin.Android.Support.v4` Windows & ファルダ サポート ライブラリ

### <a name="how-do-i-install-google-play-services-in-an-emulatorinstall-gpsmd"></a>[エミュレーターに Google Play 開発者サービスをインストールする方法を教えてください](install-gps.md)

このガイドは、Visual Studio の Android エミュレーターで Google Play サービスをインストールする方法についてをページにリンクしています。

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Windows 上で Android をデバッグするために必要な USB ドライバーを教えてください](android-drivers-debug-windows.md)

Windows; で開発するときに Android デバイスでデバッグするには互換性のある USB ドライバーをインストールする必要があります。 Android SDK Manager には、既定では、Nexus デバイスのサポートを追加する「Google USB ドライバー」が含まれています。
その他のデバイスでは、具体的には、デバイスの製造元によって発行された USB ドライバーが必要です。 このガイドでは、これらのドライバーも、別のテスト メソッドを検索するについて説明します。

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Windows VM から Mac 上で動作する Android エミュレーターに接続できますか](connect-android-emulator-mac-windows.md)

このガイドでは、Google Android エミュレーターを使用する場合、メソッドがについて説明します。

## <a name="general-questions"></a>一般的な質問

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Android NUnit テスト プロジェクトを自動化する方法を教えてください](automate-android-nunit-test.md)

このガイドは、Android NUnit テスト プロジェクトを設定するための手順を含んで_いない_Xamarin.UITest プロジェクト。 Xamarin.UITest ガイドを参照して[ここ](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest)です。

### <a name="how-do-i-enable-intellisense-in-android-axml-filesenable-axml-intellisensemd"></a>[Android の .axml ファイルで Intellisense を有効にする方法を教えてください](enable-axml-intellisense.md)

このガイドでは、Android .axml ファイル用の Visual Studio の Intellisense をアクティブ化する方法について説明します。

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Android のリリース ビルドがインターネットに接続できないのはなぜですか](android-internet.md)

この問題の最も一般的な原因は、**インターネット**権限は、デバッグ ビルドで自動的に含まれるが、リリース ビルドを手動で設定する必要があります。 このガイドでは、リリース ビルドのアクセス許可を有効にする方法について説明します。

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[より高度な Xamarin Android サポート v4/v13 NuGet パッケージ](android-support-v4v13-libraries.md)

`Support-v4` および`Support-v13`は使用できません同時に、同じアプリでは、これらは相互に排他的です。 これは、ため`Support-v13`実際にすべての型との実装を含む`Support-v4`です。 しようとすると、同じプロジェクトの両方を参照は、重複する型のエラーが発生します。

### <a name="how-do-i-resolve-a-pathtoolongexception-errorpath-too-long-exceptionmd"></a>[PathTooLongException エラーを解決する方法](path-too-long-exception.md)

この記事を解決する方法を説明します、 **PathTooLongException** Xamarin.Android プロジェクトのビルド中に発生するエラーです。



## <a name="deprecated"></a>非推奨

> [!NOTE]
> 以下の記事は、Xamarin の最近のバージョンに解決した問題に適用されます。 ただし、ソフトウェアの最新のバージョンで問題が発生した場合を送信してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)すべてのバージョン管理情報と完全のビルド ログ出力します。

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[Lollipop のサポートが追加された Xamarin.Android のバージョンを教えてください](xa-lollipop.md)

このガイドは、Android L プレビュー用に作成されたされました。Xamarin.Android 4.17 を追加 Android L のプレビューがサポートおよび Xamarin.Android 4.20 ロリポップの Android サポートを追加します。

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat - No resource found that matches the given name: attr 'android:actionModeShareDrawable'](missing-action-mode-share-drawable.md)

このエラーは、必要な Android SDK pacakages の一部が見つからない場合 Xamarin の以前のバージョンで発生する可能性があります。

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Android Designer の Java メモリ パラメーターの調整](android-designer-java-memory.md)

開始するときに使用される既定のメモリのパラメーター、`java`プロセスでは、Android デザイナーが一部のシステム構成と互換性がない可能性があります。 Xamarin Studio 5.7.2.7 から始まって Xamarin for Visual Studio 3.9.344 これらの設定は、プロジェクトごとにカスタマイズできます。

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Android Resource.designer.cs ファイルが更新されない](resource-designer-wont-update.md)

Xamarin.Studio 5.1 のバグでは、部分的または完全には、.csproj ファイルの xml コードを削除することによって、.csproj ファイルを以前と破損しています。 これが原因で重要な部分、Android のビルド システム (Android Resource.designer.cs の更新) などが失敗します。 5.1.4 安定した時点では、年 7 月 15 日にリリースでは、このバグが修正されました。多くの場合、プロジェクト ファイルを手動で、このガイドで説明したように修復します。



