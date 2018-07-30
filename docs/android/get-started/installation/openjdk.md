---
title: Microsoft の OpenJDK ディストリビューションのプレビュー
description: Microsoft が配布する OpenJDK の構成に向けたステップ バイ ステップ ガイドです。
ms.prod: xamarin
ms.assetid: B5F8503D-F4D1-44CB-8B29-187D1E20C979
ms.technology: xamarin-android
author: vyedin
ms.author: vyedin
ms.date: 07/22/2018
ms.openlocfilehash: 6c1346918ca6881e551f6c5b89ab16ad13d3f804
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242530"
---
# <a name="microsofts-openjdk-distribution-preview"></a>Microsoft の OpenJDK ディストリビューションのプレビュー

_このガイドでは、Microsoft が配布する OpenJDK のプレビュー リリースに切り替えるための手順が説明されています。_

![プレビュー機能](~/media/shared/preview.png)

## <a name="overview"></a>概要

Visual Studio 15.9 および Visual Studio for Mac 7.7 以降、Xamarin 用の Visual Studio ツールは、Oracle の JDK から、Android 開発に特化した OpenJDK の軽量バージョンへと移行します。

![VS 15.8 Preview 5 の OpenJDK の Web プレビューを提供する新しいワークフロー](openjdk-images/openjdk.png)

この移行の利点は次のとおりです。

- Android 開発に適した OpenJDK バージョンが常に用意されます。

- JDK 9 や 10 をダウンロードしても開発作業に影響がありません。

- ダウンロード サイズとフットプリントが大幅に削減されます。

- サード パーティ製のサーバーやインストーラーによる問題を回避できます。

改善されたエクスペリエンスにすぐに移行する場合は、Microsoft の OpenJDK ディストリビューションのビルドを利用して Windows と Mac 上でテストできます。 以下でセットアップ処理について説明しますが、いつでも Oracle JDK に戻すことができます。

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

![Mac での Microsoft の OpenJDK ディストリビューションに向けた JDK パスの設定](openjdk-images/vsm.png)

- **Windows** &ndash; **[ツール] > [オプション] > [Xamarin] > [Android 設定]** の順にクリックして、**[Java Development Kit の位置情報]** を OpenJDK インストールの完全なパスに変更します。 次の例では、このパスに **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9** を設定します。

![Windows での Microsoft の OpenJDK ディストリビューションに向けた JDK パスの設定](openjdk-images/vs.png)

## <a name="revert"></a>元に戻す

Oracle JDK に戻すには、Java SDK の場所を以前に使用していた Oracle JDK のパスに変更して、ソリューションをリビルドします。 Mac では、**[既定値にリセット]** をクリックすることで Oracle JDK のパスに戻すことができます。

Microsoft の OpenJDK ディストリビューションで問題が発生した場合は、問題の追跡と迅速な修正を可能にするために、IDE のフィードバック ツールを使用して問題を報告してください。

## <a name="known-issues--planned-fix-dates"></a>既知の問題と計画された修正日

`JAVA_HOME` 環境変数が SDK とデバイス マネージャーに正しくエクスポートされない場合があります。 回避策として、この環境変数をコンピューター上の OpenJDK の場所に設定することができます。 これは、15.9 プレビューで修正されます。

## <a name="summary"></a>まとめ

この記事では、IDE を構成して Microsoft が配布する OpenJDK のプレビュー リリースを使用する方法について説明しました。安定版は 2018 年後半にリリースされる予定です。
