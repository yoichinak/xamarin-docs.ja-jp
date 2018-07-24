---
title: Android SDK ツールへの変更
description: Android SDK がインストールされている API レベルおよび Avd を管理する方法を変更します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2018
ms.openlocfilehash: 4e808736fd92fa40ecbf0c24938c0fedd7afcff9
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935452"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Android SDK ツールへの変更

_Android SDK がインストールされている API レベルおよび Avd を管理する方法を変更します。_

## <a name="changes-to-android-sdk-tooling"></a>Android SDK ツールへの変更

Android SDK ツールの最新バージョンは、Google が新しい CLI (コマンド ライン インターフェイス) ツールを優先するため既存と SDK の AVD マネージャーを削除します。 **Android**プログラムが削除され、過去の Android SDK ツールのバージョン 25.2.5 Mac と Visual Studio Tools for Xamarin の以前のバージョンの Visual Studio で Google GUI (グラフィカル ユーザー インターフェイス) の管理者が機能しなくなります。 たとえば、使用すると、 **android**コマンドラインを使用してプログラムは、次のようなエラー メッセージになります。

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

次のセクションでは、Android SDK と Android SDK 25.3.0 を使用して Android 仮想デバイスを管理する方法を説明するおよびそれ以降。

### <a name="ui-tools"></a>UI ツール

Visual Studio と Visual Studio for Mac Xamarin の提供が中止された Google GUI ベース マネージャーによる代替が提供されています。

-   Android SDK ツール、プラットフォーム、および Xamarin.Android アプリの開発に必要なその他のコンポーネントをダウンロードするには、使用、 [Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md)従来の Google SDK Manager ではなくです。

-   作成し、Android 仮想デバイスを構成するには、 [Android デバイス マネージャー](~/android/get-started/installation/android-emulator/device-manager.md)レガシの Google エミュレーター マネージャーではなくです。

これらのツールは機能的には、Google の GUI ベース マネージャー置き換わるものです。

### <a name="cli-tools"></a>CLI ツール

代わりに、CLI ツールを使用して、管理およびエミュレーター、Android SDK を更新することができます。 次のプログラムは、Android SDK ツールのコマンド ライン インターフェイスを今すぐ構成します。

#### <a name="sdkmanager"></a>sdkmanager

**追加されました:** Android SDK ツール 25.2.3 (2016 年 11 月) 以降。

呼ばれる新しいプログラムがある**sdkmanager**で、**ツール/bin** Android SDK のフォルダーです。 このツールは、コマンドラインでの Android SDK を維持するために使用されます。 詳細については、このツールを使用して、次を参照してください。 [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)です。

#### <a name="avdmanager"></a>avdmanager

**追加されました:** Android SDK ツール 25.3.0 (年 3 月、2017) 以上です。

呼ばれる新しいプログラムがある**avdmanager**で、**ツール/bin** Android SDK のフォルダーです。 このツールは、Android エミュレーターの場合、Avd を維持するために使用されます。 詳細については、このツールを使用して、次を参照してください。 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)です。

### <a name="downgrading"></a>ダウン グレード

ダウン グレードすることができます、 **Android SDK ツール**バージョンからの Android SDK の以前のバージョンをインストールすることによって、 [Android Developer web サイト](https://developer.android.com/studio/index.html)です。

### <a name="using-the-old-gui"></a>古い GUI を使用してください。

実行して、元の GUI を使用することもできます、 **android**内のプログラム、**ツール**フォルダーが表示されている限り**Android SDK ツール**バージョン**25.2.5**以下です。


## <a name="related-links"></a>関連リンク

- [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)
- [Android デバイス マネージャー](~/android/get-started/installation/android-emulator/device-manager.md)
- [Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)
- [SDK Tools のリリース ノート (Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
