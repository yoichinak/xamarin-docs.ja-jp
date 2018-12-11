---
title: Windows と macOS 上で Visual Studio 2019 Preview に接続する
description: このガイドでは、Windows 上で Visual Studio 2019 プレビューを使用して iOS アプリをビルドする方法、さらに macOS 上で Visual Studio 2019 for Mac プレビューを使用して自分のビルドをホストする手順を示します。
ms.prod: xamarin
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/04/2018
ms.openlocfilehash: 1eeb79737bd712da64ce34a6b4fe83ce2823e3eb
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2018
ms.locfileid: "52899365"
---
# <a name="visual-studio-2019-preview-pairing"></a>Visual Studio 2019 プレビューのペアリング

![[プレビュー]](~/media/shared/preview.png)

Visual Studio のサイドバイサイド インストールが macOS で完全にサポートされるのは、[Visual Studio for Mac 2019 プレビュー](https://docs.microsoft.com/visualstudio/mac/install-preview)が初めてです。 これにより、iOS アプリケーションをビルドしている Windows の開発者に新たな状況が作り出されます。Mac ビルド ホストに Visual Studio 2017 for Mac (バージョン 7.x) と Visual Studio 2019 for Mac (バージョン 8.0) のプレビューの両方がある場合、どちらが Windows のインストールのためにビルド ホストとして実行されますか?

_既定では_、Windows 上で Visual Studio 2017 または Visual Studio 2019 プレビューを使用しているかどうかに関係なく、安定したリリースの &ndash; Visual Studio 2017 for Mac (バージョン 7.7) &ndash; が、Windows 上の Visual Studio によって使用されます。

Windows と macOS の両方で Visual Studio 2019 プレビューを正しくテストするには

1. ご利用の Windows コンピューターに Visual Studio 2019 (バージョン 16.0) プレビューをインストールして使用します。
2. ご利用の Mac に Visual Studio 2019 for Mac (バージョン 8.0) プレビューをインストールします。
3. **Visual Studio 2019 for Mac**:

    a.  **[Visual Studio]、[更新のチェック]** メニュー項目の順に選びます。

    b.  **[Visual Studio の更新]** ウィンドウで、更新チャネルから **[Xamarin Preview]** を選択します。 次の警告が表示されます。

    > [!WARNING]
    > 最新の Mono ランタイムと Xamarin SDK と共に、最新の Visual Studio 2019 for Mac Preview ビルドをお試しください。 **このチャネルからビルドをインストールすると、Visual Studio 2017 for Mac リリースが中断されます。** Visual Studio 2019 Preview リリースを使用している Windows オペレーティング システム上で Xamarin アプリをビルドしている場合にのみ、このチャネルを使用します。

    c. **[Switch channel]\(チャネルの切り替え\)** ボタンを押して、プレビュー ビルド ホストを有効にします。

4. 「[Xamarin.iOS 開発のために Mac とペアリングする](index.md)」の手順、および[トラブルシューティングのヒント](troubleshooting.md) (必要な場合) に従います。