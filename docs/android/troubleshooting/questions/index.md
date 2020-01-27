---
title: Xamarin. Android に関してよく寄せられる質問
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 28b13951a0d681ffdb8e643667e496e0b3606628
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724003"
---
# <a name="android-frequently-asked-questions"></a>Android に関してよく寄せられる質問

## <a name="installation--setup"></a>インストール & セットアップ

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[インストールする必要がある Android SDK パッケージを教えてください](install-android-sdk-packages.md)

Android SDK をインストールすると、開発に必要な最小限のパッケージが自動的に含まれなくなります。 個々の開発者のニーズは異なりますが、このガイドでは、Xamarin Android を使用した開発に通常必要なパッケージについて説明します。

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Android SDK の場所はどこで設定できますか](android-sdk-location.md)

このガイドでは、Android SDK の既定の設定について説明します。これはほとんどのセットアップで機能します。また、必要に応じて Visual Studio for Mac または Visual Studio でこれらの既定値を変更する方法についても説明します。

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[Java Development Kit (JDK) のバージョンの更新方法を教えてください](update-jdk.md)

この記事では、Windows および Mac で Java Development Kit (JDK) のバージョンを更新する方法について説明します。

### <a name="can-i-use-java-development-kit-jdk-version-9-or-laterjdk9-errorsmd"></a>[Java Development Kit (JDK) バージョン9以降を使用できますか。](jdk9-errors.md)

Xamarin Android には、JDK 8 または Microsoft Mobile OpenJDK が必要です。 この記事では、jdk 9 以降がインストールされている場合に発生する可能性がある一般的なエラーメッセージと、JDK のバージョンを確認するための手順を示します。

### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Xamarin.Android.Support パッケージに必要な Android サポート ライブラリを手動でインストールする方法を教えてください](install-android-support-library.md)

このガイドでは、Windows & Mac に `Xamarin.Android.Support.v4` サポートライブラリをインストールする手順の例を示します。

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Windows 上で Android をデバッグするために必要な USB ドライバーを教えてください](android-drivers-debug-windows.md)

Windows で開発するときに Android デバイスでデバッグするには互換性のある USB ドライバーをインストールする必要があります。 Android SDK Manager には、"Google USB Driver" が既定で含まれています。これにより、デバイスの追加がサポートされます。
その他のデバイスには、デバイスの製造元によって発行された USB ドライバーが必要です。 このガイドでは、これらのドライバーの検索方法と、その他のテスト方法について説明します。

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Windows VM から Mac 上で動作する Android エミュレーターに接続できますか](connect-android-emulator-mac-windows.md)

このガイドでは、Android エミュレーターを使用する場合の方法について説明します。

## <a name="general-questions"></a>一般的な質問

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Android NUnit テスト プロジェクトを自動化する方法を教えてください](automate-android-nunit-test.md)

このガイドでは、UITest プロジェクトでは_なく_、Android NUnit テストプロジェクトを設定する手順について説明します。 UITest のガイドについては、[こちら](/appcenter/test-cloud/preparing-for-upload)を参照してください。

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Android のリリース ビルドがインターネットに接続できないのはなぜですか](android-internet.md)

この問題の最も一般的な原因は、**インターネット**のアクセス許可がデバッグビルドに自動的に含まれ、リリースビルドに対して手動で設定する必要があることです。 このガイドでは、リリースビルドに対するアクセス許可を有効にする方法について説明します。

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[より高度な Xamarin Android サポート v4/v13 NuGet パッケージ](android-support-v4v13-libraries.md)

`Support-v4` と `Support-v13` を同じアプリで同時に使用することはできません。つまり、相互に排他的です。 これは、`Support-v13` 実際に `Support-v4`のすべての型と実装が含まれているためです。 同じプロジェクト内で両方を参照しようとすると、重複する型エラーが発生します。

### <a name="how-do-i-resolve-a-pathtoolongexception-errorpath-too-long-exceptionmd"></a>[PathTooLongException エラーを解決操作方法には](path-too-long-exception.md)

この記事では、Xamarin Android プロジェクトのビルド中に発生する可能性がある**PathTooLongException**エラーの解決方法について説明します。

## <a name="deprecated"></a>非推奨

> [!NOTE]
> 以下の記事は、Xamarin の最近のバージョンで解決された問題に適用されます。 ただし、最新バージョンのソフトウェアで問題が発生した場合は、完全なバージョン管理情報と完全ビルドログ出力を使用して[新しいバグを作成](~/cross-platform/troubleshooting/questions/howto-file-bug.md)してください。

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[Lollipop のサポートが追加された Xamarin.Android のバージョンを教えてください](xa-lollipop.md)

このガイドは、当初は Android L preview 用に書かれています。Xamarin. Android 4.17 では、Android L Preview サポートが Xamarin & 追加されました。 Android 4.20 には、Android ロリポップのサポートが追加されました。

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat - No resource found that matches the given name: attr 'android:actionModeShareDrawable'](missing-action-mode-share-drawable.md)

このエラーは、必要な Android SDK パッケージの一部が不足している場合に、以前のバージョンの Xamarin で発生する可能性があります。

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Android Designer の Java メモリ パラメーターの調整](android-designer-java-memory.md)

Android designer の `java` プロセスを開始するときに使用される既定のメモリパラメーターは、一部のシステム構成と互換性がない場合があります。 Xamarin Studio 5.7.2.7 と Xamarin for Visual Studio 3.9.344 以降では、これらの設定はプロジェクトごとにカスタマイズできます。

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Android Resource.designer.cs ファイルが更新されない](resource-designer-wont-update.md)

以前には、.csproj ファイルの xml コードを部分的または完全に削除することで .csproj ファイルが破損しています5.1。 これにより、Android ビルドシステムの重要な部分 (Android Resource.designer.cs の更新など) が失敗します。 7月15日の5.1.4 安定リリースの時点で、このバグは修正されています。ただし、多くの場合、このガイドで説明されているように、プロジェクトファイルを手動で修復する必要があります。
