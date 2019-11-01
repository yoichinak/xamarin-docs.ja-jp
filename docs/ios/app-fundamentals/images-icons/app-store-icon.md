---
title: Xamarin のアプリストアアイコン iOS
description: このドキュメントでは、アセットカタログを使用して、Xamarin iOS アプリケーションのアプリストアアイコンを管理する方法について説明します。 以前は、アプリストアのアイコンは iTunes Connect で管理されていました。
ms.prod: xamarin
ms.assetid: BFB5665A-F557-46E1-B35E-870CC2026AD9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/26/2017
ms.openlocfilehash: 62253adafcd028f459e8ca0cc11a8eb3e54d52c3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73004182"
---
# <a name="app-store-icons-in-xamarinios"></a>Xamarin のアプリストアアイコン iOS

Xcode 9 より前は、すべての App Store アイコンが iTunes Connect を通じて追加されました。 ただし、これではなくなりました。 アプリストアのアイコンは、プロジェクトバンドルの一部として含まれ、アセットカタログ内に追加される必要があります。 アプリストアアイコンが含まれていないアプリは、Apple によって拒否されます。

アプリストアアイコンはユーザーにとってのアプリケーションの顔であるため、覚えやすく、小さいサイズで適切に表示する必要があります。 覚えやすいアイコンとは、クリーンで単純で、すぐに認識できるものです。

Apple では、アプリケーションのアイコンをデザインするときに、次のガイドラインを示しています。

- アプリケーションに適したアイコンにする。
- アプリケーションのデザインと一貫性のある単純なアイコンを作成する。
- アイコンに語句の使用は避ける。
- グローバルに考える: 1 つのアプリ アイコンがストアのすべての販売区域で使用される。

App Store で表示されるアプリ アイコンには、1024 × 1024 ピクセルの画像が必要です。  Apple では、アセット カタログ内の App Store アイコンは、透明だったり、アルファ チャネルを含んだりすることはできないと規定されています。

詳細については、Apple の[IOS ヒューマンインターフェイスガイドライン](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/image-size-and-resolution/)を参照してください。

## <a name="adding-an-app-store-icon"></a>アプリストアアイコンの追加

アセット カタログによってアプリケーション ストア アイコンが配信されるようになりました。 

アプリストアアイコンを追加するには、次の手順を実行します。

1. プロジェクトの**Assets. xcassets**ファイルで**AppIcon**イメージセットを見つけます。 
    - すべての新しいプロジェクトには、アセットが含まれている必要があり**ます。 xcassets**には、AppIcon イメージセットを含むファイルが含まれています。
    - 新しいアセットカタログを追加するには、プロジェクトを右クリックし、 **[追加 > 新しいファイル > アセットカタログ]** を選択します。
    - 新しいアプリアイコンイメージセットを追加するには、アイコン設定領域内で右クリックし、アプリアイコン を選択して **新しいアプリ > 新しいアプリアイコンを起動 &** ます。

    ![新しいイメージセットオプションの追加](app-store-icon-images/image1.png)

2. 一覧の**App Store**アイコンまでスクロールします。

    ![App Store アイコン](app-store-icon-images/image2.png)

3. アイコンをクリックし、1024 x 1024 ピクセルイメージを参照します。 アセットカタログを保存します。

## <a name="related-links"></a>関連リンク

- [資産カタログを使用したアイコンの管理](~/ios/app-fundamentals/images-icons/app-icons.md#managing)
