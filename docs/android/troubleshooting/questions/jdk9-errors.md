---
title: Xamarin.Android と Java Development Kit 9
description: この記事では、Java Development Kit (JDK) 9 または Xamarin.Android での以降のエラーを解決する方法について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: d8c64ff79367d93e282edd9534ffb98f5bb90c93
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/24/2018
ms.locfileid: "36269674"
---
# <a name="xamarinandroid-and-java-development-kit-9-or-later"></a>Xamarin.Android と Java Development Kit 9 以降

_この記事では、Java Development Kit (JDK) 9 または Xamarin.Android での以降のエラーを解決する方法について説明します。_


## <a name="overview"></a>概要

Xamarin.Android では、Java Development Kit (JDK) を使用して、Android アプリを構築して、Android designer を実行するための Android SDK と統合します。 Android SDK (API 24 以降) の最新バージョンでは、JDK 8 (1.8) または Microsoft Mobile の OpenJDK Preview が必要です。 **Google から使用できる Android SDK ツールは、まだ JDK 9 と互換性がありません、ため、Xamarin.Android は JDK 9 以降機能しません。**

## <a name="jdk-errors"></a>JDK エラー

JDK 8 よりも後で、JDK のバージョンの Xamarin.Android プロジェクトをビルドしようとする場合は、このバージョンの JDK がサポートされていないことを示す明示的なエラーが発生します。 例えば:

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors  
```

これらのエラーを解決するで説明したように、JDK 8 (1.8) をインストールする必要があります[Java Development Kit (JDK) バージョンを更新する方法でしょうか。](~/android/troubleshooting/questions/update-jdk.md)します。
または、インストールすることができます、 [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md) The Microsoft Mobile OpenJDK で、Xamarin.Android 開発のための JDK 8 を置き換えることが最終的には。


## <a name="checking-the-jdk-version"></a>JDK のバージョンのチェック

次のコマンドを入力して、インストールした Java のバージョンを確認できます (JDK`bin`ディレクトリがである必要があります、 `PATH`)。

```shell
java -version
```

JDK 9 がインストールされている場合は、次のようなメッセージが表示されます。

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

JDK 9 以降がインストールされている場合は、Java JDK 8 (1.8) または Microsoft Mobile OpenJDK のプレビューをインストールする必要があります。 JDK 8 をインストールする方法については、[Java Development Kit (JDK) バージョンを更新する方法でしょうか。](~/android/troubleshooting/questions/update-jdk.md)を参照してください。 Microsoft Mobile OpenJDK をインストールする方法については、[Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md)を参照してください。

以降のバージョンの JDK; をアンインストールする必要はありません。ただし、それ以降の JDK バージョンではなく、JDK 8 Xamarin を使用していることを確認する必要があります。 Visual Studio で、次のようにクリックします。**ツール > オプション > Xamarin > Android 設定**します。 場合**Java Development Kit の場所**JDK 8 の場所に設定されていない (など**c:\\Program Files\\Java\\jdk1.8.0_111**)、をクリックして**変更** JDK 8 がインストールされている場所に設定します。 Visual studio for Mac に移動します。**設定 > プロジェクト > SDK の場所 > Android > Java SDK (JDK)**  をクリック**参照**このパスを更新します。

## <a name="known-issues-with-jdk-9"></a>JDK 9 に関する既知の問題

### <a name="apksigner"></a>apksigner

Apksigner と JDK 9 を既知の問題がある、`apksigner.bat`を呼び出して、`apksigner.jar`で`-Djava.ext.dirs`の代わりに`-classpath`を JDK 9 を必要とします。 JDK 8 (1.8) を使用することをお勧めします。 JDK 8 をインストールする方法については、次を参照してください。 [Java Development Kit (JDK) バージョンを更新する方法でしょうか。](~/android/troubleshooting/questions/update-jdk.md)

JDK 9 をインストールした場合に、次のパスが設定されていないことを確認、`PATH`環境変数は JDK 9 を指している:`C:\ProgramData\Oracle\Java\javapath`します。 それを削除した後`java-version`コマンドラインで JDK 8 を表示する必要があります。
