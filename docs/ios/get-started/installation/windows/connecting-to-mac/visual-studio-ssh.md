---
title: "自分の Mac の場所"
description: "Xamarin.iOS プロジェクトをビルドするため、Visual Studio 用に Mac を構成する手順"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4f792690-b3b1-4d56-a5e2-363b4f246059
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 491d7cea4b88fa44bb15e76dd92ecd5216ce9984
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="wheres-my-mac"></a>自分の Mac の場所

_Xamarin.iOS プロジェクトをビルドするため、Visual Studio 用に Mac を構成する手順_

Xamarin for Visual Studio の現在の**安定した**バージョンは、以前のバージョン [^](#earlier-versions) とは異なる方法で、Mac と対話します。

Xamarin Mac エージェントとして Mac を使用するには、Mac で**リモート ログイン**を有効にする必要があります。

1. *Spotlight* (`Cmd-Space`) を開き、**リモート ログイン**を検索し、**[Sharing]\(共有\)** 結果を選択します。 これにより、**[Sharing]\(共有\)** パネルで **[System Preferences]\(システム環境設定\)** が開きます。

  ![](visual-studio-ssh-images/spotlight.png "リモート ログインのスポットライト検索")

2. Xamarin for Visual Studio に Mac への接続を許可するには、左にある **[Service]\(サービス\)** リストの **[Remote Login]\(リモート ログイン\)** オプションをオンにします。

  ![](visual-studio-ssh-images/sharing.png "[サービス] リストの [リモート ログイン] オプションをオンにします")

3. **[Remote Login]\(リモート ログイン\)** のアクセス許可が **[All users]\(すべてのユーザー\)** に設定されているか、使用している Mac のユーザー名またはグループが右側のリストの許可されたユーザーのリストに含まれていることを確認します。

ご使用の Mac が Visual Studio で検出可能になります (同じネットワーク上にある場合)。
Visual Studio が Mac に接続しようとすると、Visual Studio から Mac のユーザー資格情報を使用してログインすることを求められます。

## <a name="where-can-i-find-more-information"></a>詳細情報

新しいエージェントの設定および理解に役立つ多くのガイドがあります。その一部を次に示します。

- [Mac への接続](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- [トラブルシューティング](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Xamarin University - Xamarin Mac Agent](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)

<a name="earlier-versions" />

## <a name="migrating-from-previous-versions"></a>以前のバージョンからの移行

以前のバージョンの Xamarin for Visual Studio を使用していた場合は、 **Xamarin.iOS Build Host** アプリケーションについてよくご存知でしょう。このアプリケーションは Mac で起動する必要があり、PIN 番号を使用して、ご使用の Visual Studio インスタンスと*ペア*にする必要がありました。

Xamarin for Visual Studio のこのバージョンでは、Build Host アプリケーションは必要なくなりました。
