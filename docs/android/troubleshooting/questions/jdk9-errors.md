---
title: Xamarin Android および Java Development Kit 9
description: この記事では、Xamarin Android で Java Development Kit (JDK) 9 以降のエラーを解決する方法について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 58b1b29a34bfb03661959af4dea8ed57b8f504cc
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760869"
---
# <a name="xamarinandroid-and-java-development-kit-9-or-later"></a>Xamarin Android および Java Development Kit 9 以降

_この記事では、Xamarin Android で Java Development Kit (JDK) 9 以降のエラーを解決する方法について説明します。_

## <a name="overview"></a>概要

Xamarin Android は、Java Development Kit (JDK) を使用して、Android アプリをビルドし、Android designer を実行するための Android SDK と統合します。 最新バージョンの Android SDK (API 24 以降) では、JDK 8 (1.8) または Microsoft Mobile OpenJDK Preview が必要です。 **Google から入手できる Android SDK ツールはまだ JDK 9 と互換性がないため、Xamarin は JDK 9 以降では機能しません。**

## <a name="jdk-errors"></a>JDK エラー

Jdk 8 よりも後のバージョンの JDK を使用して Xamarin Android プロジェクトをビルドしようとすると、このバージョンの JDK がサポートされていないことを示す明示的なエラーが表示されます。 例えば:

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors
```

これらのエラーを解決するには、「 [Java Development Kit (jdk) のバージョンを更新操作方法](~/android/troubleshooting/questions/update-jdk.md)」で説明されているように、jdk 8 (1.8) をインストールする必要があります。
または、 [Microsoft Mobile Openjdk Preview](~/android/get-started/installation/openjdk.md)をインストールすることもできます。これにより、Microsoft Mobile openjdk によって、Xamarin Android 開発用の jdk 8 が最終的に置き換えられます。

## <a name="checking-the-jdk-version"></a>JDK のバージョンを確認しています

次のコマンドを入力して、インストールされている Java のバージョンを確認できます`bin` (JDK ディレクトリはに`PATH`存在している必要があります)。

```shell
java -version
```

JDK 9 がインストールされている場合は、次のようなメッセージが表示されます。

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

JDK 9 以降がインストールされている場合は、Java JDK 8 (1.8) または Microsoft Mobile OpenJDK Preview をインストールする必要があります。 JDK 8 のインストール方法の詳細については、「 [Java Development Kit (jdk) のバージョンを更新操作方法](~/android/troubleshooting/questions/update-jdk.md)」を参照してください。 Microsoft Mobile OpenJDK をインストールする方法の詳細については、「 [Microsoft Mobile Openjdk Preview](~/android/get-started/installation/openjdk.md)」を参照してください。

JDK の新しいバージョンをアンインストールする必要はないことに注意してください。ただし、Xamarin では、それ以降の JDK バージョンではなく JDK 8 を使用していることを確認する必要があります。 Visual Studio で、[**ツール] > [オプション > Xamarin > Android 設定**] の順にクリックします。 **Java Development Kit の場所**が jdk 8 の場所 ( **\\C: Program Files\\Java\\JDK 1.8.0 以降 1**など) に設定されていない場合は、 **[変更]** をクリックし、jdk 8 がインストールされている場所に設定します。 Visual Studio for Mac で、[**環境] > [> SDK の場所 > Android > JAVA SDK (JDK)** ] に移動し、 **[参照]** をクリックしてこのパスを更新します。

## <a name="known-issues-with-jdk-9"></a>JDK 9 に関する既知の問題

### <a name="apksigner"></a>apksigner

Apksigner 者と jdk 9 には既知の問題があります`apksigner.bat` 。このファイルで`-Djava.ext.dirs`は、 `-classpath` jdk 9 で想定されるではなく、 `apksigner.jar`を使用してを呼び出します。 JDK 8 (1.8) を使用することをお勧めします。 JDK 8 のインストール方法の詳細については、「 [Java Development Kit (jdk) バージョンの更新操作方法](~/android/troubleshooting/questions/update-jdk.md)」を参照してください。

Jdk 9 がインストールされている場合は、 `PATH`環境変数に次のパスが設定されていないことを確認します。これは、引き続き jdk 9: `C:\ProgramData\Oracle\Java\javapath`を指します。 削除した後`java-version` 、コマンドラインに JDK 8 が表示されます。
