---
title: イメージと Xamarin.iOS でのアイコン
description: このセクションには、コントロールとカスタム ドキュメントの種類のアイコンを提供することでアイコンとして使用するなど、Xamarin.iOS アプリでのイメージの操作に対応、画面を起動する記事のまたはそれらを含むさまざまなが含まれています。
ms.prod: xamarin
ms.assetid: 0AB8CC07-11E4-0D75-4119-AED1A1252424
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 60b450cba73166462747de41176575da27190e0a
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832386"
---
# <a name="images-and-icons-in-xamarinios"></a>イメージと Xamarin.iOS でのアイコン

_このセクションには、コントロールとカスタム ドキュメントの種類のアイコンを提供することでアイコンとして使用するなど、Xamarin.iOS アプリでのイメージの操作に対応、画面を起動する記事のまたはそれらを含むさまざまなが含まれています。_

いくつか方法はそのイメージのアセットが iOS アプリ内で使用されます。 単になど、UI コントロールに割り当てるには、アプリの UI の一部としてイメージを表示する、`UIButton`または`UIImageView`アイコンと起動画面を提供する Xamarin.iOS 簡単に、次の方法で優れたアートワークを iOS アプリに追加。 

- **解像度の独立した画像**– 別のデバイスの解像度と種類 (iPhone、iPad など) の間でイメージを操作するための iOS の組み込みサポートを使用します。
- **資産カタログの画像セット**-使用**資産カタログの画像セット**を管理し、アプリで必要な特定のイメージ資産のすべてのバージョンをグループ化します。
- **IOS Designer でイメージ**-iOS Designer を使用して、コントロールのイメージを設定します。
- **コード内のイメージ**– 使用して、`UIImage`クラスのメソッドのイメージ アセットの操作をロードしで UI コントロールに割り当てるC#コード。
- **アプリケーション アイコン**-すべての iOS アプリで必要なアプリのアイコンを定義します。 これは、ユーザーがタップするアイコンのアプリを起動する iOS ホーム画面です。 さらに、このアイコンは、該当する場合、Game Center をによって使用されます。
- **アイコンのスポット ライト**-アプリのスポット ライトのアイコンを定義します。 Spotlight 検索で、アプリの名前を入力すると、ときにこのアイコンが表示されます。
- **設定アイコン**-アプリの定義**設定**アイコン。 ユーザーが入力した場合、**設定**アプリの設定の一覧の最後にこのアイコン、iOS デバイスでアプリが表示されます。 
- **起動画面**-アプリの起動画面を定義します。 ユーザーがアプリのアイコンをタップし、最初のビューが表示されるまで、空白の画面が表示されます。 さいわい、iOS には、ストーリー ボードを使用して空の画面の代わりにイメージを表示するためのサポートが含まれています。 
- **iTunes アイコン**-iTune アイコンを提供します。 (または企業のユーザーのため実際のデバイスでベータ テスト) アプリを配信するアドホック メソッドを使用して場合、開発者も含める必要があります、512 x 512 と 1024 x 1024 イメージを iTunes でアプリを表すために使用されます。
- **ドキュメント アイコン**-Xamarin.iOS アプリをサポートまたは作成されるすべてのドキュメントの種類のアイコンとしてイメージを使用します。

これらの資産を使用するいくつかの領域と同様に、iOS アプリのイメージ アセットを作成するときに、アカウントに実行すべきいくつかの考慮事項があります。 これらの各があるだけでなくイメージ資産の数が、必要になりますが、これらの資産を作成する方法に影響します。 次のトピックでは、必要なイメージの資産がこれらの資産が、アプリケーションのバンドルに含まれている方法、および必要な機能を提供するイメージのアセットがどのように消費されるの種類を説明します。


## <a name="displaying-an-imageiosapp-fundamentalsimages-iconsdisplaying-an-imagemd"></a>[イメージの表示](~/ios/app-fundamentals/images-icons/displaying-an-image.md)

この記事では、Xamarin.iOS アプリと c# コードを使用するか、iOS デザイナーのコントロールに割り当てることで、そのイメージを表示する内のイメージ アセットなどについて説明します。

## <a name="application-iconsiosapp-fundamentalsimages-iconsapp-iconsmd"></a>[アプリケーション アイコン](~/ios/app-fundamentals/images-icons/app-icons.md)

この記事について説明を含むとアプリ アイコンとして使用する Xamarin.iOS アプリ内のイメージ アセットを管理します。

## <a name="alternate-app-iconsiosapp-fundamentalsimages-iconsalternate-app-iconsmd"></a>[代替アプリ アイコン](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)

Apple には、そのアイコンを管理するアプリを許可する iOS 10.3 にいくつかの機能強化が追加されます。

- `ApplicationIconBadgeNumber` -を取得します。 または、スプリング ボードで、アプリ アイコンのバッジを設定します。
- `SupportsAlternateIcons` If`true`アプリには、別のアイコンのセット。
- `AlternateIconName` -現在選択されている代替アイコンの名前を返しますまたは`null`プライマリ アイコンを使用する場合。
- `SetAlternameIconName` -このメソッドを使用してアプリのアイコンを指定した代替アイコンに切り替えます。


## <a name="launch-screensiosapp-fundamentalsimages-iconslaunch-screensmd"></a>[起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)

この記事では、特殊な種類のストーリー ボードを使用してすべての iOS デバイスのサイズと解像度のユニバーサル起動画面を提供するについて説明します。

## <a name="custom-document-typesiosapp-fundamentalsimages-iconscustom-document-typesmd"></a>[カスタム ドキュメント タイプ](~/ios/app-fundamentals/images-icons/custom-document-types.md)

この記事について説明を含む、カスタム ドキュメントの種類のアイコンとして使用する Xamarin.iOS アプリ内のイメージ アセットを管理します。



## <a name="related-links"></a>関連リンク

- [画像 (サンプル) の操作](https://developer.xamarin.com/samples/monotouch/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [カスタム アイコンとイメージの作成のガイドライン](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
