---
title: Xamarin.iOS アプリ ストアのアイコン
description: このドキュメントでは、資産カタログを使用して、Xamarin.iOS アプリケーション用のアプリ ストア アイコンを管理する方法について説明します。 以前は、App Store のアイコンは、iTunes Connect で管理されていました。
ms.prod: xamarin
ms.assetid: BFB5665A-F557-46E1-B35E-870CC2026AD9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/26/2017
ms.openlocfilehash: 749dbf01af382a54fe24652706f6a605ac7b20b4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783611"
---
# <a name="app-store-icons-in-xamarinios"></a>Xamarin.iOS アプリ ストアのアイコン

Xcode 9 の前に、アプリ ストアのすべてのアイコンは、iTunes Connect で追加されました。 ただし、これは、ケースではなくなりました。 App Store のアイコンは、プロジェクト、バンドルの一部として含まれ、アセット カタログ内に追加するようになりました必要があります。 App Store のアイコンが含まれていないアプリは Apple によって拒否されます。

アプリ ストアのアイコンは、覚えやすい場合がありますので、ユーザーと表示の小さなサイズで適切にアプリケーションの表面です。 覚えやすいアイコンとは、クリーンで単純で、すぐに認識できるものです。

Apple では、アプリケーションのアイコンをデザインするときに、次のガイドラインを示しています。

- アプリケーションに適したアイコンにする。
- アプリケーションのデザインと一貫性のある単純なアイコンを作成する。
- アイコンに語句の使用は避ける。
- グローバルに考える: 1 つのアプリ アイコンがストアのすべての販売区域で使用される。

App Store で表示されるアプリ アイコンには、1024 × 1024 ピクセルの画像が必要です。  Apple では、アセット カタログ内の App Store アイコンは、透明だったり、アルファ チャネルを含んだりすることはできないと規定されています。

詳細については、Apple を参照してください。 [iOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/image-size-and-resolution/)です。

## <a name="adding-an-app-store-icon"></a>App Store のアイコンを追加します。

アセット カタログによってアプリケーション ストア アイコンが配信されるようになりました。 

追加するには、アプリ ストアのアイコンは次の操作を行います。

1. 検索、**入った**でイメージを設定、 **Assets.xcassets**プロジェクトのファイルです。 
    - すべての新しいプロジェクトには付属する、 **Assets.xcassets**入ったイメージ セットを含むファイルです。
    - 新しい資産カタログを追加するを選択してプロジェクトを右クリックし**追加 > 新しいファイル > アセット カタログ**です。
    - 新規のアプリのアイコンのイメージ セットを追加するには、クリックし、アイコン セット領域内を右クリックして**アプリのアイコンと起動イメージ > 新しいアプリ アイコン**:
    
    ![新しいイメージの set オプションを追加します。](app-store-icon-images/image1.png)

2. スクロールして、 **App Store**の一覧のアイコン。

    ![App Store アイコン](app-store-icon-images/image2.png)

3. アイコンをクリックし、1024 × 1024 ピクセルの画像を参照します。 資産カタログを保存します。




## <a name="related-links"></a>関連リンク

- [アイコンと資産カタログを管理します。](~/ios/app-fundamentals/images-icons/app-icons.md#managing)
