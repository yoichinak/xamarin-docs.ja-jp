---
title: Xamarin.Android に関してよく寄せられる質問
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 28b13951a0d681ffdb8e643667e496e0b3606628
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "76724003"
---
# <a name="android-frequently-asked-questions"></a>Android に関してよく寄せられる質問

## <a name="installation--setup"></a>インストールとセットアップ

### <a name="which-android-sdk-packages-should-i-install"></a>[インストールする必要がある Android SDK パッケージを教えてください](install-android-sdk-packages.md)

Android SDK をインストールしても、開発に必要な最小限のパッケージが自動的に含まれるわけではありません。 個々の開発者のニーズは異なりますが、このガイドでは、Xamarin Android を使用した開発に通常必要なパッケージについて説明します。

### <a name="where-can-i-set-my-android-sdk-locations"></a>[Android SDK の場所はどこで設定できますか](android-sdk-location.md)

このガイドでは、Android SDK の既定の設定について説明します。これはほとんどのセットアップで機能します。また、必要に応じて Visual Studio for Mac または Visual Studio でこれらの既定値を変更する方法についても説明します。

### <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>[Java Development Kit (JDK) のバージョンの更新方法を教えてください](update-jdk.md)

この記事では、Windows および Mac で Java Development Kit (JDK) のバージョンを更新する方法について説明します。

### <a name="can-i-use-java-development-kit-jdk-version-9-or-later"></a>[Java Development Kit (JDK) バージョン 9 以降は使用できますか](jdk9-errors.md)

Xamarin Android には、JDK 8 または Microsoft Mobile OpenJDK が必要です。 この記事では、JDK 9 以降がインストールされている場合に表示される可能性がある一般的なエラー メッセージと、JDK のバージョンを確認するための手順を示します。

### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>[Xamarin.Android.Support パッケージに必要な Android サポート ライブラリを手動でインストールする方法を教えてください](install-android-support-library.md)

このガイドでは、Windows と Mac に `Xamarin.Android.Support.v4` サポート ライブラリをインストールする手順の例を示します。

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>[Windows 上で Android をデバッグするために必要な USB ドライバーを教えてください](android-drivers-debug-windows.md)

Windows で開発しているときに Android デバイスでデバッグを行うには、互換性のある USB ドライバーをインストールする必要があります。 Android SDK マネージャーには、既定で "Google USB ドライバー" が含まれています。これにより、Nexus デバイスのサポートが追加されます。
それ以外のデバイスの場合は、デバイスの製造元によって公開されている USB ドライバーが必要です。 このガイドでは、それらのドライバーの検索方法と、その他のテスト方法について説明します。

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>[Windows VM から Mac 上で動作する Android エミュレーターに接続できますか](connect-android-emulator-mac-windows.md)

このガイドでは、Android エミュレーターを使用する場合の方法について説明します。

## <a name="general-questions"></a>一般的な質問

### <a name="how-do-i-automate-an-android-nunit-test-project"></a>[Android NUnit テスト プロジェクトを自動化する方法を教えてください](automate-android-nunit-test.md)

このガイドでは、Xamarin.UITest プロジェクト _ではなく_、Android NUnit テスト プロジェクトを設定する手順について説明します。 Xamarin.UITest のガイドについては、[こちら](/appcenter/test-cloud/preparing-for-upload)をご覧ください。

### <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>[Android のリリース ビルドがインターネットに接続できないのはなぜですか](android-internet.md)

この問題の最も一般的な原因は、**INTERNET** アクセス許可が、デバッグ ビルドには自動的に含まれるのに対し、リリース ビルドでは手動で設定する必要があることです。 このガイドでは、リリース ビルドでアクセス許可を有効にする方法について説明します。

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>[より高度な Xamarin Android サポート v4/v13 NuGet パッケージ](android-support-v4v13-libraries.md)

`Support-v4` と `Support-v13` を同じアプリで同時に使用することはできません。つまり、それらは相互に排他的です。 これは、`Support-v13` には実際には `Support-v4` のすべての型と実装が含まれているためです。 同じプロジェクト内で両方を参照しようとすると、型の重複エラーが発生します。

### <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>[PathTooLongException エラーを解決するにはどうすればいいですか](path-too-long-exception.md)

この記事では、Xamarin.Android プロジェクトのビルド中に発生する可能性がある **PathTooLongException** エラーの解決方法について説明します。

## <a name="deprecated"></a>非推奨

> [!NOTE]
> 以下の記事は、Xamarin の最近のバージョンで解決された問題に適用されます。 ただし、ソフトウェアの最新のバージョンで問題が発生している場合は、すべてのバージョン管理情報およびすべてのビルド ログ出力と共に[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)を提出してください。

### <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>[Lollipop のサポートが追加された Xamarin.Android のバージョンを教えてください](xa-lollipop.md)

このガイドは、元々 Android L Preview 用に書かれました。Xamarin.Android 4.17 では Android L Preview のサポート、Xamarin.Android 4.20 では Android Lollipop のサポートが追加されました。

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>[Android.Support.v7.AppCompat - No resource found that matches the given name: attr 'android:actionModeShareDrawable'](missing-action-mode-share-drawable.md)

このエラーは、必要な Android SDK パッケージの一部が不足している場合に、Xamarin の以前のバージョンで発生する可能性があります。

### <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>[Android Designer の Java メモリ パラメーターの調整](android-designer-java-memory.md)

Android Designer 用に `java` プロセスを開始するときに使用される既定のメモリ パラメーターは、一部のシステム構成と互換性がない場合があります。 Xamarin Studio 5.7.2.7 および Xamarin for Visual Studio 3.9.344 以降では、これらの設定をプロジェクトごとにカスタマイズできます。

### <a name="my-android-resourcedesignercs-file-will-not-update"></a>[Android Resource.designer.cs ファイルが更新されない](resource-designer-wont-update.md)

以前は Xamarin.Studio 5.1 のバグによって .csproj ファイル内の xml コードが一部または完全に削除されることで、.csproj ファイルが破損していました。 これにより、Android ビルド システムの重要な部分 (Android Resource.designer.cs の更新など) が失敗していました。 7 月 15 日の 5.1.4 安定リリースの時点でこのバグは修正されていますが、多くの場合、このガイドで説明されているように、プロジェクト ファイルを手動で修復する必要があります。
