---
title: Xamarin.iOS でのカスタム ドキュメント アイコン
description: この記事について説明を含む、カスタム ドキュメントの種類のアイコンとして使用する Xamarin.iOS アプリ内のイメージ アセットを管理します。
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/23/2017
ms.openlocfilehash: 155847b4f5f6b3e6070f1afb6219db2d3789a075
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668466"
---
# <a name="custom-document-icons-in-xamarinios"></a>Xamarin.iOS でのカスタム ドキュメント アイコン

_この記事について説明を含む、カスタム ドキュメントの種類のアイコンとして使用する Xamarin.iOS アプリ内のイメージ アセットを管理します。_

Xamarin.iOS アプリでは、特定のドキュメント型の読み込みをサポートする場合、開発者がなど、ユーザーが添付ファイルを押したときに、そのドキュメントの種類を見つけたときに、システムが使用するアイコンを指定できます、*メール アプリケーション*として次に示します。

 [![](custom-document-types-images/17.png "ドキュメントの種類のアイコンの例")](custom-document-types-images/17.png#lightbox)

ディクショナリ エントリを含めることによって、ファイル形式、アプリが開始できるため、開発者がドキュメント型情報を追加できます、`CFBundleTypeName`文字列と`LSItemContentTypes`アプリの配列`Info.plist`します。 移動して、ドキュメントの種類のアイコン、`CFBundleTypeIconFiles`配列。 ドキュメント アイコンが指定されない場合は、アプリ アイコンから iOS が継承します。
アイコンは、さまざまなデバイス解像度用に最適化された、いくつかのサイズを指定できます。 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

For Mac に Visual Studio でこれらの値を割り当てるには、使用、**ドキュメントの種類**セクション、 **詳細設定**  タブで、`Info.plist`エディターをドキュメントの種類を追加してアイコンのイメージを割り当てます。 たとえば、PDF サポートのための登録を示すスクリーン ショットを次に示します。

 [![](custom-document-types-images/18.png "'Info.plist' エディターの詳細設定 タブの下のセクションでドキュメントの種類")](custom-document-types-images/18.png#lightbox)
 
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio でこれらの値を割り当てるには、使用、**ドキュメントの種類**セクション、**詳細** タブで、 `Info.plist`:

 ![](custom-document-types-images/doc01w.png "詳細設定 タブの下のセクションでドキュメントの種類を開く")

をクリックして、**ドキュメントの種類の追加**ボタンをクリックし、必要なフィールドに入力します。

![](custom-document-types-images/doc02w.png "ドキュメントの種類の追加フォーム")

-----


ドキュメントの種類の詳細については、Apple を参照してください。 [Uniform 型識別子の参照](https://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html)と[iOS 用ドキュメントの対話プログラミング トピック](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html)します。


## <a name="related-links"></a>関連リンク

- [画像 (サンプル) の操作](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [カスタム アイコンとイメージの作成のガイドライン](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
