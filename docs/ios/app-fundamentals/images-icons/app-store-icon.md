---
title: Xamarin.iOS での app Store アイコン
description: このドキュメントでは、資産カタログを使用して、App Store アイコンの Xamarin.iOS アプリケーションを管理する方法について説明します。 以前は、App Store アイコンは、iTunes Connect で管理されていました。
ms.prod: xamarin
ms.assetid: BFB5665A-F557-46E1-B35E-870CC2026AD9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/26/2017
ms.openlocfilehash: 53e25ae9f4650254f2aaaa03dc8727fae674c9f0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61258113"
---
# <a name="app-store-icons-in-xamarinios"></a>Xamarin.iOS での app Store アイコン

Xcode 9 の前に、iTunes Connect を通してすべての App Store アイコンが追加されました。 ただし、これは、ケースではなくなりました。 App Store アイコンは、プロジェクトのバンドルの一部として含まれているし、アセット カタログ内に追加するようになりましたする必要があります。 App Store アイコンが含まれていないアプリは、Apple によって拒否されます。

App Store のアイコンは、覚えやすい場合がありますので、ユーザーと小さなサイズで適切に表示するアプリケーションの顔です。 覚えやすいアイコンとは、クリーンで単純で、すぐに認識できるものです。

Apple では、アプリケーションのアイコンをデザインするときに、次のガイドラインを示しています。

- アプリケーションに適したアイコンにする。
- アプリケーションのデザインと一貫性のある単純なアイコンを作成する。
- アイコンに語句の使用は避ける。
- グローバルに考える: 1 つのアプリ アイコンがストアのすべての販売区域で使用される。

App Store で表示されるアプリ アイコンには、1024 × 1024 ピクセルの画像が必要です。  Apple では、アセット カタログ内の App Store アイコンは、透明だったり、アルファ チャネルを含んだりすることはできないと規定されています。

詳細については、Apple を参照してください。 [iOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/image-size-and-resolution/)します。

## <a name="adding-an-app-store-icon"></a>App Store アイコンの追加

アセット カタログによってアプリケーション ストア アイコンが配信されるようになりました。 

追加するには、App Store アイコンは次の操作を行います。

1. 検索、 **AppIcon**イメージ セット、 **Assets.xcassets**プロジェクトのファイル。 
    - すべての新しいプロジェクトに用意する必要があります、 **Assets.xcassets** AppIcon イメージのセットを含むファイル。
    - 新しいアセット カタログを追加するには、プロジェクトを選択します右クリックして**追加 > 新しいファイル > 資産カタログ**します。
    - アプリ アイコンの画像セットを新規に追加するをクリックし、アイコン セットの領域で右クリックし**アプリ アイコンと起動イメージ > 新しいアプリ アイコン**:
    
    ![新しいイメージの set オプションを追加します。](app-store-icon-images/image1.png)

2. スクロールして、 **App Store**リストのアイコン。

    ![App Store アイコン](app-store-icon-images/image2.png)

3. アイコンをクリックし、1024 × 1024 ピクセルの画像を参照します。 資産カタログを保存します。




## <a name="related-links"></a>関連リンク

- [資産カタログを使用した管理アイコン](~/ios/app-fundamentals/images-icons/app-icons.md#managing)
