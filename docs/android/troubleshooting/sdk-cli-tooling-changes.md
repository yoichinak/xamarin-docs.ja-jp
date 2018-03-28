---
title: Android SDK ツールへの変更
description: Android SDK がインストールされている API レベルおよび Avd を管理する方法を変更します。
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: a16aa3704d9e0a63cfabde4b620452e7e2a5bf57
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
---
# <a name="changes-to-the-android-sdk-tooling"></a>Android SDK ツールへの変更

_Android SDK がインストールされている API レベルおよび Avd を管理する方法を変更します。_

## <a name="changes-to--android-sdk-tooling"></a>Android SDK ツールへの変更

Android SDK ツールの最新バージョンは、Google が新しい CLI (コマンド ライン インターフェイス) ツールいる既存と SDK の AVD マネージャーを削除します。 前者**android**プログラムが削除され、過去の Android SDK ツールのバージョン 25.2.5 Mac と Xamarin for Visual Studio の以前のバージョンの Visual Studio での GUI (グラフィカル ユーザー インターフェイス) マネージャーが機能しなくなります。


![Visual Studio での android IDE メニュー](sdk-cli-tooling-changes-images/android-ide-menu.png)

使用すると、 **android**コマンドラインを使用してプログラムは、次のようなエラー メッセージになります。

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

したがって CLI ツールを使用して管理およびエミュレーター、Android SDK を更新する必要があります。

### <a name="cli-tools"></a>CLI ツール

次のプログラムは、Android SDK ツールのコマンド ライン インターフェイスを今すぐ構成します。

#### <a name="sdkmanager"></a>sdkmanager

**追加されました:** Android SDK ツール 25.2.3 (2016 年 11 月) 以降。

呼ばれる新しいプログラムがある**sdkmanager**で、**ツール/bin** Android SDK のフォルダーです。 このツールは、コマンドラインでの Android SDK を維持するために使用されます。 詳細については、このツールを使用して、次を参照してください。 [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)です。

#### <a name="avdmanager"></a>avdmanager

**追加されました:** Android SDK ツール 25.3.0 (年 3 月、2017) 以上です。

呼ばれる新しいプログラムがある**avdmanager**で、**ツール/bin** Android SDK のフォルダーです。 このツールは、Google Android エミュレーターの場合、Avd を維持するために使用されます。 詳細については、このツールを使用して、次を参照してください。 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)です。

### <a name="downgrading"></a>ダウン グレード

ダウン グレードすることができます、 **Android SDK ツール**バージョンからの Android SDK の以前のバージョンをインストールすることによって、 [Android Developer web サイト](https://developer.android.com/studio/index.html)です。

### <a name="using-the-old-gui"></a>古い GUI を使用してください。

実行して、元の GUI を使用することもできます、 **android**内のプログラム、**ツール**フォルダーが表示されている限り**Android SDK ツール**バージョン**25.2.5**以下です。


## <a name="related-links"></a>関連リンク

- [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)
- [Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)
- [SDK Tools のリリース ノート (Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
