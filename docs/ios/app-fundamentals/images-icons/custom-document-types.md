---
title: Xamarin. iOS のカスタムドキュメントアイコン
description: この記事では、カスタムドキュメントの種類のアイコンとして使用する Xamarin. iOS アプリでのイメージアセットの追加と管理について説明します。
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/23/2017
ms.openlocfilehash: 09fc582182729d3d8e17b85ac0a3ecc4bdcfce7e
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939816"
---
# <a name="custom-document-icons-in-xamarinios"></a>Xamarin. iOS のカスタムドキュメントアイコン

_この記事では、カスタムドキュメントの種類のアイコンとして使用する Xamarin. iOS アプリでのイメージアセットの追加と管理について説明します。_

Xamarin iOS アプリで特定の種類のドキュメントの読み込みがサポートされている場合、開発者は、次に示すように、ユーザーが*メールアプリケーション*の添付ファイルを停止したときなど、そのドキュメントの種類を検出したときに使用するアイコンを提供できます。

 [![ドキュメントの種類のアイコンの例](custom-document-types-images/17.png)](custom-document-types-images/17.png#lightbox)

開発者は、アプリ `CFBundleTypeName` 内の文字列および配列の辞書エントリを含めることによって、アプリが開くことのできるファイル形式のドキュメント型情報を追加できます `LSItemContentTypes` `Info.plist` 。 ドキュメントの種類のアイコンが配列に格納され `CFBundleTypeIconFiles` ます。 ドキュメントアイコンが指定されていない場合、iOS はアプリアイコンから1つを派生させます。
さまざまなデバイスの解像度に合わせて最適化された複数のサイズに対してアイコンを指定できます。 

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

これらの値を Visual Studio for Mac に割り当てるには、エディターの [**詳細設定**] タブにある [**ドキュメントの種類**] セクションを使用して、 `Info.plist` ドキュメントの種類を追加し、イメージアイコンを割り当てます。 たとえば、PDF サポートの登録を示すスクリーンショットを次に示します。

 [![[情報] エディターの [詳細設定] タブにある [ドキュメントの種類] セクション](custom-document-types-images/18.png)](custom-document-types-images/18.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

これらの値を Visual Studio で割り当てるには、の [**詳細設定**] タブの [**ドキュメントの種類**] セクションを使用し `Info.plist` ます。

 ![[詳細設定] タブの [ドキュメントの種類] セクションを開く](custom-document-types-images/doc01w.png)

[**ドキュメントの種類の追加**] ボタンをクリックし、必要なフィールドを入力します。

![[ドキュメントの種類の追加フォーム](custom-document-types-images/doc02w.png)

-----

ドキュメントの種類の詳細については、「Apple の[Uniform Type Identifier リファレンス](https://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html)」および「 [IOS のドキュメント相互作用プログラミング](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html)に関するトピック」を参照してください。

## <a name="related-links"></a>関連リンク

- [イメージの操作 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [カスタムアイコンとイメージ作成のガイドライン](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
