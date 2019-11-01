---
title: Android SDK ツールへの変更
description: インストールされている API レベルと AVDs を Android SDK が管理する方法を変更しました。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: 4c559a76d7354fd957d065717ef14d91591d1be0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019493"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Android SDK ツールへの変更

_インストールされている API レベルと AVDs を Android SDK が管理する方法を変更しました。_

## <a name="changes-to-android-sdk-tooling"></a>Android SDK ツールに対する変更

Android 用の SDK Tools の最近のバージョンでは、新しい CLI (コマンドラインインターフェイス) ツールを優先して、既存の AVD および SDK マネージャーが Google によって削除されました。 **Android**プログラムは削除されており、Visual Studio for Mac の Google GUI (グラフィカルユーザーインターフェイス) マネージャーと、Xamarin 用の古いバージョンの Visual Studio Tools は、バージョン25.2.5 以前の Android SDK Tools を超えて動作しなくなりました。 たとえば、コマンドラインで**android**プログラムを使用しようとすると、次のようなエラーメッセージが表示されます。

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

以下のセクションでは、Android SDK 25.3.0 以降を使用して Android SDK と Android の仮想デバイスを管理する方法について説明します。

### <a name="ui-tools"></a>UI ツール

Visual Studio と Visual Studio for Mac は、廃止された Google GUI ベースのマネージャーに対して Xamarin の置換を提供するようになりました。

- Xamarin Android アプリの開発に必要な Android SDK ツール、プラットフォーム、およびその他のコンポーネントをダウンロードするには、従来の Google SDK Manager の代わりに[xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md)を使用します。

- Android 仮想デバイスを作成して構成するには、従来の Google Emulator Manager の代わりに[Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)を使用します。

これらのツールは、それらが置き換える Google GUI ベースの管理者と機能的に同等です。

### <a name="cli-tools"></a>CLI ツール

または、CLI ツールを使用して、エミュレーターと Android SDK を管理および更新することもできます。 次のプログラムは、Android SDK ツールのコマンドラインインターフェイスを構成するようになりました。

#### <a name="sdkmanager"></a>sdkmanager

**追加:** Android SDK Tools 25.2.3 (11 月、2016) 以降。

Android SDK の [**ツール]/[bin** ] フォルダーに、 **sdkmanager**という名前の新しいプログラムが追加されています。 このツールは、コマンドラインで Android SDK を維持するために使用されます。 このツールの使用方法の詳細については、「 [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)」を参照してください。

#### <a name="avdmanager"></a>avdmanager

**追加:** Android SDK Tools 25.3.0 (3 月、2017) 以降。

**Avdmanager**という新しいプログラムが Android SDK の [**ツール]/[bin** ] フォルダーにあります。 このツールは、Android Emulator の AVDs を維持するために使用されます。 このツールの使用方法の詳細については、「 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)」を参照してください。

### <a name="downgrading"></a>ダウングレード

[Android Developer web サイト](https://developer.android.com/studio/index.html)から以前のバージョンの Android SDK をインストールすることによって、 **Android SDK Tools**バージョンをダウングレードできます。

### <a name="using-the-old-gui"></a>古い GUI の使用

**Android SDK Tools**バージョン**25.2.5 以前**以下であれば、**ツール**フォルダー内で**android**プログラムを実行することで、元の GUI を引き続き使用できます。

## <a name="related-links"></a>関連リンク

- [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)
- [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)
- [Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)
- [SDK Tools のリリース ノート (Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
