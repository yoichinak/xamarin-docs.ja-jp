---
title: "Android SDK ツールへの変更"
description: "Android SDK がインストールされている API レベルおよび Avd を管理する方法を変更します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 69e9f08870a01c056951700978d07277af5edfa8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="changes-to-the-android-sdk-tooling"></a>Android SDK ツールへの変更

_Android SDK がインストールされている API レベルおよび Avd を管理する方法を変更します。_

## <a name="changes-to--android-sdk-tooling"></a>Android SDK ツールへの変更

Google android SDK ツールの最新バージョンは、新しいいる既存と SDK の AVD マネージャーを削除が_コマンド ライン インターフェイス_(CLI) ツール。 前者**android**プログラムは削除されましたと過去のバージョンの Android SDK ツール Mac と Xamarin for Visual Studio の以前のバージョンの Visual Studio での GUI (グラフィカル ユーザー インターフェイス) マネージャーが機能しなくなります。


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

呼ばれる新しいプログラムがある**avdmanager**で、**ツール/bin** Android SDK のフォルダーです。 このツールは、Google Android エミュレーターの AVD を維持するために使用されます。 詳細については、このツールを使用して、次を参照してください。 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)です。

### <a name="downgrading"></a>ダウン グレード

ダウン グレードすることができます、 **Android SDK ツール**バージョンからの Android SDK の以前のバージョンをインストールすることによって、 [Android Developer web サイト](https://developer.android.com/studio/index.html)です。

### <a name="using-the-old-gui"></a>古い GUI を使用してください。

実行して、元の GUI を使用することもできます、 **android**内のプログラム、**ツール**フォルダーが表示されている限り**Android SDK ツール**バージョン**25.2.5**以下です。


## <a name="related-links"></a>関連リンク

- [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)
- [Android API レベルを理解します。](~/android/app-fundamentals/android-api-levels.md)
- [SDK ツールのリリース ノート (Google)](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
