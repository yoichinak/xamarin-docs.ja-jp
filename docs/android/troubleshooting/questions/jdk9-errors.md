---
title: Xamarin.Android と Java Development Kit 9
description: この記事では、Xamarin.Android で Java Development Kit (JDK) 9 以降のエラーを解決する方法について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 2ea7c9b9f900bc339d183c2f5b317792ebec5232
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73026832"
---
# <a name="xamarinandroid-and-java-development-kit-9-or-later"></a>Xamarin.Android と Java Development Kit 9 以降

_この記事では、Xamarin.Android で Java Development Kit (JDK) 9 以降のエラーを解決する方法について説明します。_

## <a name="overview"></a>概要

Xamarin.Android では、Java Development Kit (JDK) を使用して、Android アプリのビルドと Android デザイナーの実行を行う Android SDK と統合されます。 最新バージョンの Android SDK (API 24 以降) では、JDK 8 (1.8) または Microsoft Mobile OpenJDK Preview が必要です。 **Google から入手できる Android SDK ツールはまだ JDK 9 と互換性がないため、Xamarin.Android は JDK 9 以降では動作しません。**

## <a name="jdk-errors"></a>JDK エラー

JDK 8 より後のバージョンの JDK を使用して Xamarin.Android プロジェクトをビルドしようとすると、このバージョンの JDK はサポートされていないことを示す明示的なエラーが発生します。 次に例を示します。

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors
```

これらのエラーを解決するには、「[Java Development Kit (JDK) のバージョンの更新方法を教えてください](~/android/troubleshooting/questions/update-jdk.md)」で説明されているように、JDK 8 (1.8) をインストールする必要があります。
または、[Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md) をインストールすることもできます。Microsoft Mobile OpenJDK は、最終的に、Xamarin.Android 開発用の JDK 8 を置き換えられます。

## <a name="checking-the-jdk-version"></a>JDK のバージョンを確認する

次のコマンドを入力して、インストールされている Java のバージョンを確認できます (JDK の `bin` ディレクトリが `PATH` 内に存在する必要があります)。

```shell
java -version
```

JDK 9 がインストールされている場合は、次のようなメッセージが表示されます。

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

JDK 9 以降がインストールされている場合は、Java JDK 8 (1.8) または Microsoft Mobile OpenJDK Preview をインストールする必要があります。 JDK 8 のインストール方法の詳細については、「[Java Development Kit (JDK) のバージョンの更新方法を教えてください](~/android/troubleshooting/questions/update-jdk.md)」を参照してください。 Microsoft Mobile OpenJDK のインストール方法については、「[Microsoft Mobile OpenJDK のプレビュー](~/android/get-started/installation/openjdk.md)」を参照してください。

JDK の新しいバージョンをアンインストールする必要はないことに注意してください。ただし、Xamarin で、後の JDK のバージョンではなく JDK 8 が使用されていることを確認する必要があります。 Visual Studio で、 **[ツール] > [オプション] > [Xamarin] > [Android 設定]** をクリックします。 **[Java Development Kit の位置情報]** が JDK 8 の場所 (**C:\\Program Files\\Java\\jdk1.8.0_111** など) に設定されていない場合は、 **[変更]** をクリックし、JDK 8 がインストールされている場所に設定します。 Visual Studio for Mac では、 **[設定] > [プロジェクト] > [SDK の場所] > [Android] > [Java SDK (JDK)]** に移動し、 **[参照]** をクリックして、このパスを更新します。

## <a name="known-issues-with-jdk-9"></a>JDK 9 での既知の問題

### <a name="apksigner"></a>apksigner

apksigner と JDK 9 には既知の問題があり、`apksigner.bat` ファイルで `apksigner.jar` が呼び出されるときに、JDK 9 で想定される `-classpath` ではなく、`-Djava.ext.dirs` が使用されます。 JDK 8 (1.8) を使用することをお勧めします。 JDK 8 のインストール方法の詳細については、「[Java Development Kit (JDK) のバージョンの更新方法を教えてください](~/android/troubleshooting/questions/update-jdk.md)」を参照してください

JDK 9 をインストールした場合は、`PATH` 環境変数でパス `C:\ProgramData\Oracle\Java\javapath` が設定されていないことを確認します。このパスではまだ JDK 9 が指定されています。 それを削除した後は、コマンド ラインの `java-version` で JDK 8 が表示されるはずです。
