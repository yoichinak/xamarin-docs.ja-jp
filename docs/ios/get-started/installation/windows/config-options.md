---
title: iOS 開発用の Visual Studio の構成
description: この記事では、Xamarin.iOS 開発のために Visual Studio 2019 を構成する方法について説明しています。 具体的には、インストールしたバージョンの Xamarin.iOS、iOS ツールバー、[ソリューション プラットフォーム] ドロップダウン メニューを構成する方法について説明しています。
ms.prod: xamarin
ms.assetid: 22D82244-890D-4325-B3CC-C0AC49130BCA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/16/2018
ms.openlocfilehash: eb6be5cd77dddad553376d18808092c6566021bc
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2019
ms.locfileid: "58854679"
---
# <a name="configuring-visual-studio-for-ios-development"></a>iOS 開発用の Visual Studio の構成

_この記事では、Visual Studio 用の Xamarin.iOS のさまざまな構成オプションについて説明します。_

## <a name="using-matching-xamarinios-versions"></a>Xamarin.iOS の一致するバージョンの使用

Visual Studio 2019 または Visual Studio 2017 では、Mac ビルド ホストにインストールされているものと同じバージョンの Xamarin.iOS を使う必要があります。 そのためには次のようにします。

- Visual Studio 2019 または Visual Studio 2017 を使用している場合は、Visual Studio for Mac で **[安定]** 更新チャネルを選択します。

- Visual Studio 2019 Preview を使用している場合は、Visual Studio for Mac で **[アルファ]** 更新チャネルを選択します。

> [!NOTE]
> [Visual Studio 2017 バージョン 15.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning) 以降では、Visual Studio 2017 は Mac ビルド ホストが Windows と同じバージョンの Xamarin.iOS を使用しているかどうかを自動的に検出します。 バージョンが一致しない場合、Visual Studio 2017 では Mac ビルド ホストに正しいバージョンをリモート インストールできます。 詳しくは、「[Mac とペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」ガイドの「[Mac の自動プロビジョニング](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning)」セクションをご覧ください。

## <a name="ios-toolbar"></a>[iOS] ツール バー

Visual Studio 2019 または Visual Studio 2017 で iOS プロジェクトを開くと、[iOS] ツール バーが表示されます。  既定では、このツール バーには Xamarin.iOS の開発に役立つ 4 つのボタンが含まれます。

![Visual Studio 2019 の iOS ツール バー](config-options-images/ios-toolbar.png)

- **[Mac とペアリング]** – [Mac とペアリング] ダイアログを開きます。 Visual Studio 2019 または Visual Studio 2017 で iOS プロジェクトを開くと有効になります。
- **[iOS シミュレーターの表示]** – Mac ビルド ホストで、iOS シミュレーターを前面に移動します。 Visual Studio 2019 または Visual Studio 2017 で iOS プロジェクトを開くと有効になります。
- **[デバイス ログ]** – デバイス ログを検査できるウィンドウを表示します。 Visual Studio 2019 または Visual Studio 2017 で iOS プロジェクトを開くと有効になります。
- **[ビルド サーバーに IPA ファイルを表示]** – Mac ビルド ホストで、アプリの .ipa ファイルの場所を示すウィンドウを開きます。 .ipa が作成されたビルドの完了後に有効になります。

このツール バーが表示されない場合は、Visual Studio 2019 または Visual Studio 2017 で **[表示]** メニューを開いて、**[ツール バー] > [iOS]** を選びます。

![[iOS] ツール バーを有効にする](config-options-images/ios-toolbar-enable.png "[iOS] ツール バーを有効にする")

## <a name="solution-platforms-drop-down-menu"></a>[ソリューション プラットフォーム] ドロップダウン メニュー

**[ソリューション プラットフォーム]** ドロップダウン メニューでは、次のビルドの対象が物理デバイスかシミュレーターかを選択できます。

[標準] ツール バーにこのドロップダウン メニューが表示されるようにするには:

- Visual Studio 2019 または Visual Studio 2017 で、[標準] ツール バーの右端にある下向き矢印をクリックします。
- **[ボタンの表示/非表示]** を選びます 
- **[ソリューション プラットフォーム]** 項目がオンになっていることを確認します。

![[ソリューション プラットフォーム] ドロップダウン メニューを有効にする](config-options-images/solution-platforms-enable.png "[ソリューション プラットフォーム] ドロップダウン メニューを有効にする")

iOS プロジェクトを開くと、**[標準]** および **[iOS]** ツール バーが次のスクリーンショットのようになります。

![[標準] および [iOS] ツール バー](config-options-images/toolbars.png "[標準] および [iOS] ツール バー")
