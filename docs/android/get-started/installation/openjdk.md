---
title: Microsoft のモバイル OpenJDK ディストリビューション
description: モバイル開発のための、Microsoft の OpenJDK ディストリビューションの構成とトラブルシューティングに向けたステップ バイ ステップ ガイドです。
ms.prod: xamarin
ms.assetid: B5F8503D-F4D1-44CB-8B29-187D1E20C979
ms.technology: xamarin-android
author: vyedin
ms.author: vyedin
ms.date: 07/22/2018
ms.openlocfilehash: a24edbc10d529878092b474df7f186d14049d5e0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "60955083"
---
# <a name="microsofts-mobile-openjdk-distribution"></a>Microsoft のモバイル OpenJDK ディストリビューション

_このガイドでは、OpenJDK の内部ディストリビューションに切り替える手順について説明します。このディストリビューションは、モバイル開発を目的としています。_

## <a name="overview"></a>概要

Visual Studio 15.9 および Visual Studio for Mac 7.7 以降、Visual Studio Tools for Xamarin は、Oracle の JDK から、**Android 開発に特化した OpenJDK の軽量バージョン**へと移行しました。 Oracle は 2019 年に JDK 8 の商用ディストリビューションのサポートを終了し、すべての Android 開発で JDK 8 の依存関係が必要であるため、この移行が必要となります。

この移行の利点は次のとおりです。

- Android 開発に適した OpenJDK バージョンが常に用意されます。

- Oracle JDK 9 以降をダウンロードしても開発作業に影響がありません。

- ダウンロード サイズと占有領域が削減されます。

- サード パーティ製のサーバーやインストーラーによる問題を回避できます。

改善されたエクスペリエンスにすぐに移行する場合は、Microsoft の Mobile OpenJDK ディストリビューションのビルドを利用して Windows と Mac 上でテストできます。 以下でセットアップ処理について説明しますが、いつでも Oracle JDK に戻すことができます。

## <a name="download"></a>ダウンロード

Windows の Visual Studio インストーラーで Android SDK パッケージを選択すると、モバイル OpenJDK ディストリビューションが自動的にインストールされます。

Mac の場合、モバイル OpenJDK は新規インストール用の Android ワークロードの一部としてインストールされます。 Visual Studio for Mac の既存のユーザーには、更新プログラムの一部としてインストールするかどうかが確認されます。 IDE では、新しい JDK に移行するように求められ、次の再起動時にこれが使用されるように切り替わります。

## <a name="troubleshooting"></a>トラブルシューティング

Mac または Windows のセットアップに関する問題が発生した場合は、手動セットアップ用の次の手順を実行できます。

OpenJDK が、コンピューターの正しい場所にインストールされているかどうかを確認します。

- **Mac** &ndash; **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.x**
- **Windows** &ndash; **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.x**

IDE に新しい JDK を示します。

- **Mac** &ndash; **[ツール] > [SDK マネージャー] > [場所]** の順にクリックして、 **[Java SDK (JDK) の場所]** を OpenJDK インストールの完全なパスに変更します。 次の例では、このパスに **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9** を設定していますが、お使いのバージョンがより新しい可能性もあります。

![Mac での Microsoft の Mobile OpenJDK ディストリビューションに向けた JDK パスの設定](openjdk-images/vsm.png)

- **Windows** &ndash; **[ツール] > [オプション] > [Xamarin] > [Android 設定]** の順にクリックして、 **[Java Development Kit の位置情報]** を OpenJDK インストールの完全なパスに変更します。 次の例では、このパスに **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9** を設定していますが、お使いのバージョンがより新しい可能性もあります。

![Windows での Microsoft の Mobile OpenJDK ディストリビューションに向けた JDK パスの設定](openjdk-images/vs.png)

## <a name="known-issues"></a>既知の問題

### <a name="package-openjdkv1regkeyversion18025chipx64-failed-to-install"></a>パッケージ 'OpenJDKV1.RegKey,version=1.8.0.25,chip=x64' のインストールに失敗する

一部の企業の環境で発生する問題です。 OpenJDK は既にコンピューター上にあるため、[上記のトラブルシューティング手順](#troubleshooting)に従って、IDE を適切な場所にポイントします。 問題の状態については、[こちら](https://developercommunity.visualstudio.com/content/problem/382549/packageidopenjdkv1regkeypackageactioninstallreturn.html)で確認できます。

## <a name="summary"></a>まとめ

この記事では、IDE を構成して Microsoft のモバイル OpenJDK ディストリビューションを構成する方法と、問題が発生した場合のトラブルシューティング方法について学習しました。
