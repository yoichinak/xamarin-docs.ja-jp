---
title: Android SDK ツールへの変更
description: Android SDK がインストール済みの API レベルおよび Avd を管理する方法を変更します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/21/2018
ms.openlocfilehash: dbd3287e7c646be7fd969699eab685906a1c6c1a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112807"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Android SDK ツールへの変更

_Android SDK がインストール済みの API レベルおよび Avd を管理する方法を変更します。_

## <a name="changes-to-android-sdk-tooling"></a>Android SDK ツールへの変更

Android SDK Tools の最新バージョンに Google が新しい CLI (コマンド ライン インターフェイス) ツールを優先して、既存の AVD および SDK マネージャーを削除します。 **Android**プログラムが削除され、過去の Android SDK Tools バージョン 25.2.5 Visual Studio for Mac と Visual Studio Tools for Xamarin の以前のバージョンで Google GUI (グラフィカル ユーザー インターフェイス) の管理者が機能しなくなります。 たとえば、使用しよう、 **android**コマンドラインを使用してプログラムは、次のようなエラー メッセージになります。

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

次のセクションでは、Android SDK と Android SDK 25.3.0 を使用して、Android 仮想デバイスを管理する方法を説明します。 以降。

### <a name="ui-tools"></a>UI ツール

Visual Studio と Visual Studio for Mac は、提供が中止された Google GUI ベースのマネージャー用の Xamarin 置換を指定ようになりました。

-   Android SDK ツール、プラットフォーム、および Xamarin.Android アプリの開発に必要なその他のコンポーネントをダウンロードするには、使用、 [Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md)従来の Google SDK Manager の代わりにします。

-   作成し、Android 仮想デバイスの構成を使用して、 [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)従来の Google エミュレーター マネージャーではなく。

これらのツールは機能的には、Google の GUI ベース マネージャーの代わりになるものです。

### <a name="cli-tools"></a>CLI ツール

または、エミュレーター、Android SDK の更新の管理に CLI ツールを使用できます。 次のプログラムは、今すぐ、Android SDK ツールのコマンド ライン インターフェイスを構成します。

#### <a name="sdkmanager"></a>sdkmanager

**追加されました:** Android SDK Tools 25.2.3 (2016 年 11 月) 以降。

呼ばれる新しいプログラムがある**sdkmanager**で、**ツール/bin** Android SDK のフォルダー。 このツールは、コマンドラインでの Android SDK を維持するために使用されます。 詳細については、このツールを使用して、次を参照してください。 [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)します。

#### <a name="avdmanager"></a>avdmanager

**追加されました:** Android SDK Tools 25.3.0 (2017 年 3 月) 以降。

呼ばれる新しいプログラムがある**avdmanager**で、**ツール/bin** Android SDK のフォルダー。 このツールは、Android emulator、Avd を維持するために使用されます。 詳細については、このツールを使用して、次を参照してください。 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)します。

### <a name="downgrading"></a>ダウン グレード

ダウン グレードすることができます、 **Android SDK Tools**バージョンからの Android SDK の以前のバージョンをインストールすることによって、 [Android デベロッパー web サイト](https://developer.android.com/studio/index.html)します。

### <a name="using-the-old-gui"></a>古い GUI を使用してください。

実行して、元の GUI を使用することもできます、 **android**プログラム内で、**ツール**フォルダーを使用している限り**Android SDK Tools**バージョン**25.2.5**以下。


## <a name="related-links"></a>関連リンク

- [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)
- [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)
- [Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)
- [SDK Tools のリリース ノート (Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
