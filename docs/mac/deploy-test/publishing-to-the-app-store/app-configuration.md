---
title: Mac アプリの構成
description: このガイドでは、パブリケーション用の Xamarin.Mac アプリの構成の手順を説明します。
ms.prod: xamarin
ms.assetid: fea66a34-1581-4cd6-b714-3fbff215a542
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: e47ff676b4dd02d5312a74fb699ed594b5e0f944
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="mac-app-configuration"></a>Mac アプリの構成

_このガイドでは、パブリケーション用の Xamarin.Mac アプリの構成の手順を説明します。_


## <a name="mac-app-configuration"></a>Mac アプリの構成

Visual Studio for Mac で Mac アプリケーション プロジェクトを右クリックして、**[オプション]** を選択します。


### <a name="application-settings"></a>アプリケーションの設定

Xamarin.Mac アプリのアプリケーションの設定を変更するには、**Solution Pad** で **Info.plist** ファイルをダブルクリックします。

![Info.plist ファイルの選択](app-configuration-images/config04.png "Info.plist ファイルの選択")

これにより、アプリの使用可能なオプションが表示されます。

 [![Info.plist ファイルの編集](app-configuration-images/config01.png "Info.plist ファイルの編集")](app-configuration-images/config01-large.png#lightbox)

Xamarin.Mac で作成された Mac アプリケーションを実行するには、次のシステム要件があります。

- Mac OS X 10.7 以上を搭載している Mac コンピューター。


### <a name="signing-settings"></a>署名の設定

**[プロジェクト オプション]** ダイアログ ボックスの **[Mac 署名]** セクションを使用して、開発者は、テスト用、自己リリース用、または Apple App Store からのリリース用に Xamarin.Mac アプリに署名することができます。

[![Mac Signing エディター](app-configuration-images/config02.png "Mac Signing エディター")](app-configuration-images/config02-large.png#lightbox)

ここで選択、ID、プロビジョニング プロファイル、コンパイル時にアプリに署名するために使用するカスタム権利を選択します。 開発者は、オプションで、他の Mac 上でアプリをインストールするために使用されるインストーラーに署名することができます。


### <a name="build-settings"></a>ビルド設定

**[プロジェクト オプション]** ダイアログ ボックスの **[Mac ビルド]** セクションを使用して、開発者は、Xamarin.Mac アプリのアーキテクチャを選択し、アプリがサポートする macOS のバージョンを制御し、オプションでアプリが正常にコンパイルされたときのインストール パッケージを作成することができます。

 [![ビルド設定の編集](app-configuration-images/config03.png "ビルド設定の編集")](app-configuration-images/config03-large.png#lightbox)


## <a name="related-links"></a>関連リンク

- [インストール](/visualstudio/mac/installation/)
- [Hello Mac のサンプル](~/mac/get-started/hello-mac.md)
- [Mac App Store でアプリを配布する](https://developer.apple.com/devcenter/mac/checklist/)
- [Developer ID と GateKeeper](https://developer.apple.com/resources/developer-id/)
