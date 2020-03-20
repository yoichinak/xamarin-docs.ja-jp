---
title: Android SDK ツールへの変更
description: インストールされる API レベルと AVD の Android SDK による管理方法の変更。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: 4c559a76d7354fd957d065717ef14d91591d1be0
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73019493"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Android SDK ツールへの変更

_インストールされる API のレベルと AVD の Android SDK による管理方法の変更。_

## <a name="changes-to-android-sdk-tooling"></a>Android SDK ツールへの変更

Google は、Android SDK Tools の最新バージョンでは新しい CLI (コマンド ライン インターフェイス) ツールを採用し、既存の AVD Manager と SDK Manager を削除しています。 **android** プログラムが削除され、Visual Studio for Mac の Google GUI (グラフィカル ユーザー インターフェイス) マネージャーと以前のバージョンの Visual Studio Tools for Xamarin は、Android SDK Tools の過去バージョン 25.2.5 では動作しなくなります。 たとえば、コマンド ラインで **android** プログラムを使用しようとすると、次のようなエラー メッセージが表示されます。

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

以下のセクションで、Android SDK 25.3.0 以降を使用して Android SDK と Android 仮想デバイスを管理する方法について説明します。

### <a name="ui-tools"></a>UI ツール

Visual Studio と Visual Studio for Mac には、廃止された Google GUI ベースのマネージャーに代わる Xamarin の機能が用意されています。

- Xamarin.Android アプリの開発に必要な Android SDK のツール、プラットフォーム、およびその他のコンポーネントをダウンロードするには、従来の Google SDK Manager の代わりに [Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md) を使用します。

- Android 仮想デバイスを作成して構成するには、従来の Google Emulator Manager の代わりに [Android デバイス マネージャー](~/android/get-started/installation/android-emulator/device-manager.md)を使用します。

これらのツールは、それらが置き換える Google GUI ベースのマネージャーと機能的に同等です。

### <a name="cli-tools"></a>CLI ツール

別の方法として、CLI ツールを使用して、エミュレーターと Android SDK の管理と更新を実行できます。 次のプログラムで、Android SDK ツール用のコマンドライン インターフェイスが構成されるようになりました。

#### <a name="sdkmanager"></a>sdkmanager

**追加先:** Android SDK Tools 25.2.3 (2016 年 11 月) 以降。

Android SDK の **tools/bin** フォルダーに **sdkmanager** という名前の新しいプログラムがあります。 このツールを使用して、コマンド ラインで Android SDK を管理します。 このツールの使用方法の詳細については、「[sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)」を参照してください。

#### <a name="avdmanager"></a>avdmanager

**追加先:** Android SDK Tools 25.3.0 (2017 年 3 月) 以降。

Android SDK の **tools/bin** フォルダーに **avdmanager** という名前の新しいプログラムがあります。 このツールを使用して、Android エミュレーター用の AVD を管理します。 このツールの使用方法の詳細については、「[avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)」を参照してください。

### <a name="downgrading"></a>ダウングレード

[Android Developer Web サイト](https://developer.android.com/studio/index.html)から前のバージョンの Android SDK をインストールすることで、お使いの **Android SDK ツール**のバージョンをダウングレードできます。

### <a name="using-the-old-gui"></a>古い GUI の使用

**Android SDK Tools** のバージョン **25.2.5** 以前を使用しているのであれば、**tools** フォルダー内の **android** プログラムを実行することで、元の GUI を引き続き使用できます。

## <a name="related-links"></a>関連リンク

- [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)
- [Android デバイス マネージャー](~/android/get-started/installation/android-emulator/device-manager.md)
- [Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)
- [SDK Tools のリリース ノート (Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
