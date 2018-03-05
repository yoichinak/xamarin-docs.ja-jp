---
title: "Xamarin.Android と JDK 9"
description: "この記事では、Xamarin.Android で Java Development Kit (JDK) 9 エラーを解決する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 9e1dd2fac015783e06bad6656f09a687f3abba2c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinandroid-and-jdk-9"></a>Xamarin.Android と JDK 9

_この記事では、Xamarin.Android で Java Development Kit (JDK) 9 エラーを解決する方法について説明します。_


## <a name="overview"></a>概要

Xamarin.Android では、Java Development Kit (JDK) を使用して、Android アプリをビルドして、Android デザイナーを実行するための Android SDK を統合します。 Android SDK API (24 以降) の最新バージョンでは、JDK 8 (1.8) が必要です。 **Google から使用可能な Android SDK ツールは、まだ JDK 9 と互換性がありません、ため Xamarin.Android は JDK 9 では機能しません。**

## <a name="jdk-9-errors"></a>JDK 9 エラー

JDK 9 をインストールした Xamarin.Android プロジェクトをビルドしようとする場合は、JDK 9 がサポートされていないことを示す明示的なエラーが表示されます。

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors  
```

説明したように、これらのエラーを解決するには、JDK 8 (1.8) をインストールする必要があります[Java Development Kit (JDK) バージョンを更新する方法ですか?](~/android/troubleshooting/questions/update-jdk.md)です。


## <a name="checking-the-jdk-version"></a>JDK のバージョンのチェック

どのバージョンの Java を次のコマンドを入力してインストールしたかを確認できます (JDK`bin`ディレクトリがである必要があります、 `PATH`)。

```shell
java -version
```

JDK 9 がインストールされている場合は、次のようなメッセージが表示されます。

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

JDK 9 がインストールされている場合は、Java JDK 8 (1.8) をインストールする必要があります。 JDK 8 をインストールする方法については、次を参照してください[Java Development Kit (JDK) バージョンを更新する方法ですか?。](~/android/troubleshooting/questions/update-jdk.md)

## <a name="known-issues-with-jdk-9"></a>JDK 9 に関する既知の問題

### <a name="apksigner"></a>apksigner

Apksigner と JDK 9 での既知の問題がある、`apksigner.bat`を呼び出して、`apksigner.jar`で`-Djava.ext.dirs`の代わりに`-classpath`JDK 9 を予期しています。 JDK 8 (1.8) を使用することをお勧めします。 JDK 8 をインストールする方法については、次を参照してください[Java Development Kit (JDK) バージョンを更新する方法ですか?。](~/android/troubleshooting/questions/update-jdk.md)

JDK 9 をアンインストールした後で、次のパスが設定されていないことを確認してください、`PATH`いる環境変数がまだ指す JDK 9:`C:\ProgramData\Oracle\Java\javapath`です。 削除するには後、`java -version`コマンドラインで JDK 8 が表示されます。
