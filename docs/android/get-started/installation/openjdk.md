---
title: Microsoft の Mobile OpenJDK ディストリビューションのプレビュー
description: モバイル開発のための、Microsoft OpenJDK ディストリビューションの構成に向けたステップ バイ ステップ ガイドです。
ms.prod: xamarin
ms.assetid: B5F8503D-F4D1-44CB-8B29-187D1E20C979
ms.technology: xamarin-android
author: vyedin
ms.author: vyedin
ms.date: 07/22/2018
ms.openlocfilehash: 2022337ebd65997c7b2492137193586278f2dffd
ms.sourcegitcommit: bf51592be39b2ae3d63d029be1d7745ee63b0ce1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/06/2018
ms.locfileid: "39573595"
---
# <a name="microsofts-mobile-openjdk-distribution-preview"></a>Microsoft の Mobile OpenJDK ディストリビューションのプレビュー

_このガイドでは、Microsoft の OpenJDK のプレビュー リリースに切り替えるための手順が説明されています。このディストリビューションは、モバイル開発を目的としています。_

![プレビュー機能](~/media/shared/preview.png)

## <a name="overview"></a>概要

Visual Studio 15.9 および Visual Studio for Mac 7.7 以降、Visual Studio Tools for Xamarin は、Oracle の JDK から、**Android 開発に特化した OpenJDK の軽量バージョン**へと移行します。

![VS 15.8 Preview 5 の OpenJDK の Web プレビューを提供する新しいワークフロー](openjdk-images/openjdk.png)

この移行の利点は次のとおりです。

- Android 開発に適した OpenJDK バージョンが常に用意されます。

- JDK 9 や 10 をダウンロードしても開発作業に影響がありません。

- ダウンロード サイズとフットプリントが大幅に削減されます。

- サード パーティ製のサーバーやインストーラーによる問題を回避できます。

改善されたエクスペリエンスにすぐに移行する場合は、Microsoft の Mobile OpenJDK ディストリビューションのビルドを利用して Windows と Mac 上でテストできます。 以下でセットアップ処理について説明しますが、いつでも Oracle JDK に戻すことができます。

## <a name="download"></a>ダウンロード

はじめに、システム用の正しいビルドをダウンロードします。

- **Mac** &ndash; https://dl.xamarin.com/OpenJDK/mac/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x86** &ndash; https://dl.xamarin.com/OpenJDK/win32/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x64** &ndash; https://dl.xamarin.com/OpenJDK/win64/microsoft-dist-openjdk-1.8.0.9.zip

## <a name="configure"></a>構成

正しい場所に解凍します。

- **Mac** &ndash; **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**
- **Windows** &ndash; **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**

> [!IMPORTANT]
> この例ではビルド 1.8.0.9 を使用しますが、ユーザーがダウンロードするのはより新しいバージョンである場合があります。

IDE に新しい JDK を示します。

- **Mac** &ndash; **[ツール] > [SDK マネージャー] > [場所]** の順にクリックして、**[Java SDK (JDK) の場所]** を OpenJDK インストールの完全なパスに変更します。 次の例では、このパスに **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9** を設定します。

![Mac での Microsoft の Mobile OpenJDK ディストリビューションに向けた JDK パスの設定](openjdk-images/vsm.png)

- **Windows** &ndash; **[ツール] > [オプション] > [Xamarin] > [Android 設定]** の順にクリックして、**[Java Development Kit の位置情報]** を OpenJDK インストールの完全なパスに変更します。 次の例では、このパスに **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9** を設定します。

![Windows での Microsoft の Mobile OpenJDK ディストリビューションに向けた JDK パスの設定](openjdk-images/vs.png)

## <a name="revert"></a>元に戻す

Oracle JDK に戻すには、Java SDK の場所を以前に使用していた Oracle JDK のパスに変更して、ソリューションをリビルドします。 Mac では、**[既定値にリセット]** をクリックすることで Oracle JDK のパスに戻すことができます。

Microsoft の Mobile OpenJDK ディストリビューションで問題が発生した場合は、問題の追跡と迅速な修正を可能にするために、IDE のフィードバック ツールを使用して問題を報告してください。

## <a name="known-issues--planned-fix-dates"></a>既知の問題と計画された修正日

`JAVA_HOME` 環境変数が SDK とデバイス マネージャーに正しくエクスポートされない場合があります。 回避策として、この環境変数をコンピューター上の OpenJDK の場所に設定することができます。 これは、15.9 プレビューで修正されます。

## <a name="summary"></a>まとめ

この記事では、IDE を構成して Microsoft の Mobile OpenJDK ディストリビューションのプレビュー リリースを使用する方法について説明しました。安定版は 2018 年後半にリリースされる予定です。
