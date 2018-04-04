---
title: イメージやアイコン
description: このセクションには、コントロールとカスタムのドキュメントの種類のアイコンを提供するのにアイコンとして使用するなど、Xamarin.iOS アプリのイメージの操作の説明、画面を起動するアーティクルまたはそれらを含む、さまざまなが含まれています。
ms.prod: xamarin
ms.assetid: 0AB8CC07-11E4-0D75-4119-AED1A1252424
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: fd191c898d5bb015d2d394d42db1049bb0128fb7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="images-and-icons"></a>イメージやアイコン

_このセクションには、コントロールとカスタムのドキュメントの種類のアイコンを提供するのにアイコンとして使用するなど、Xamarin.iOS アプリのイメージの操作の説明、画面を起動するアーティクルまたはそれらを含む、さまざまなが含まれています。_

あるいくつかの方法が iOS アプリ内の資産で使用されるイメージです。 などの UI コントロールに割り当てると、アプリの UI の一部としてイメージを単に表示されない、`UIButton`または`UIImageView`アイコンと起動画面を設定し、Xamarin.iOS 簡単に次の方法で iOS アプリに優れたアートワークを追加します。 

- **解像度の独立したイメージ**– 別のデバイスの解像度と種類 (iPhone、iPad など) の間でイメージを操作するための iOS の組み込みサポートを使用します。
- **アセット カタログのイメージ セット**-使用**アセット カタログのイメージ セット**を管理し、アプリケーションに必要な特定のイメージ資産のすべてのバージョンをグループ化します。
- **IOS デザイナー内のイメージ**-iOS デザイナーを使用して、コントロールのイメージの設定をします。
- **コード内のイメージ**– を使用して、`UIImage`イメージ アセットの操作をロードし c# コードで UI コントロールに割り当てるクラスのメソッドです。
- **アプリケーション アイコン**-すべての iOS アプリで必要なアプリのアイコンを定義します。 これは、ユーザーがタップするアイコン iOS ホーム画面でアプリを起動します。 さらに、このアイコンがゲーム センターで使用されるは、該当する場合。
- **アイコンのスポット ライト**-アプリのスポット ライトのアイコンを定義します。 Spotlight 検索で、アプリの名前を入力すると、このアイコンが表示されます。
- **設定アイコン**-アプリの定義**設定**アイコン。 ユーザーが入力した場合、**設定**アプリの設定の一覧の最後に、iOS デバイスを次のアイコン上のアプリが表示されます。 
- **画面を起動**-アプリの起動画面を定義します。 ユーザーが、アプリ アイコンをタップした後、最初のビューが表示される前に、空の画面が表示されます。 さいわい、iOS には、ストーリー ボードを使用して、空の画面の代わりにイメージを表示するためのサポートが含まれています。 
- **iTunes アイコン**-iTune アイコンを提供します。 (か企業ユーザー向けの実際のデバイスでテスト ベータ版) アプリの配信のアドホック メソッドを使用する場合、開発者も含める必要がある 512 x 512 と 1024 × 1024 イメージ iTunes でアプリを表すために使用されます。
- **アイコンを文書化**-Xamarin.iOS アプリをサポートしているかを作成する任意の特定のドキュメント型のアイコンとしてイメージを使用します。

これらの資産を使用するいくつかの領域と同様に、iOS アプリのイメージ アセットを作成するときに考慮する必要がありますいくつかの考慮事項があります。 これらの各により、イメージ資産の数が必要ですが、するだけでなく、これらの資産を作成する方法に影響があります。 次のトピックでは、必要なイメージ資産、これらの資産がアプリケーションのバンドルに含まれる方法および必要な機能を提供するのイメージ アセットを使用する方法の種類を説明します。


## <a name="displaying-an-imageiosapp-fundamentalsimages-iconsdisplaying-an-imagemd"></a>[イメージの表示](~/ios/app-fundamentals/images-icons/displaying-an-image.md)

この記事は、Xamarin.iOS アプリと c# コードを使用して、または iOS デザイナー内のコントロールに代入することで、そのイメージを表示するイメージ アセットなどについて説明します。

## <a name="application-iconsiosapp-fundamentalsimages-iconsapp-iconsmd"></a>[アプリケーション アイコン](~/ios/app-fundamentals/images-icons/app-icons.md)

この資料について説明など、アプリ アイコンとして使用する Xamarin.iOS アプリでのイメージ資産を管理します。

## <a name="alternate-app-iconsiosapp-fundamentalsimages-iconsalternate-app-iconsmd"></a>[代替アプリ アイコン](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)

Apple では、そのアイコンを管理するアプリを許可する iOS 10.3 にいくつかの機能強化が追加します。

 - `ApplicationIconBadgeNumber` -を取得またはのスプリング ボードで、アプリ アイコンのバッジを設定します。
 - `SupportsAlternateIcons` If`true`アプリは別のアイコンのセット。
 - `AlternateIconName` 現在選択されている別のアイコンの名前を返しますまたは`null`プライマリ アイコンを使用する場合。
 - `SetAlternameIconName` -指定した代替アイコンに、アプリのアイコンを切り替えるには、このメソッドを使用します。


## <a name="launch-screensiosapp-fundamentalsimages-iconslaunch-screensmd"></a>[起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)

この記事では、すべての iOS デバイスのサイズと解像度をユニバーサル起動画面を提供する特殊な種類のストーリー ボードを使用してについて説明します。

## <a name="custom-document-typesiosapp-fundamentalsimages-iconscustom-document-typesmd"></a>[カスタム ドキュメント タイプ](~/ios/app-fundamentals/images-icons/custom-document-types.md)

この資料について説明を含むとカスタム ドキュメントの種類のアイコンとして使用する Xamarin.iOS アプリでのイメージ資産を管理します。



## <a name="related-links"></a>関連リンク

- [イメージ (サンプル) の操作](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [カスタム アイコンをクリックしてイメージ作成のガイドライン](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
