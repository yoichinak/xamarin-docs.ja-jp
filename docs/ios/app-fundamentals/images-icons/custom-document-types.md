---
title: Xamarin. iOS のカスタムドキュメントアイコン
description: この記事では、カスタムドキュメントの種類のアイコンとして使用する Xamarin. iOS アプリでのイメージアセットの追加と管理について説明します。
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/23/2017
ms.openlocfilehash: 683587e4857ede20096be731b3cfa3b88b3a668d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282517"
---
# <a name="custom-document-icons-in-xamarinios"></a>Xamarin. iOS のカスタムドキュメントアイコン

_この記事では、カスタムドキュメントの種類のアイコンとして使用する Xamarin. iOS アプリでのイメージアセットの追加と管理について説明します。_

Xamarin iOS アプリで特定の種類のドキュメントの読み込みがサポートされている場合、開発者は、次に示すように、ユーザーが*メールアプリケーション*の添付ファイルを停止したときなど、そのドキュメントの種類を検出したときに使用するアイコンを提供できます。

 [![](custom-document-types-images/17.png "ドキュメントの種類のアイコンの例")](custom-document-types-images/17.png#lightbox)

開発者は、アプリ内の`CFBundleTypeName`文字列および`LSItemContentTypes` `Info.plist`配列の辞書エントリを含めることによって、アプリが開くことのできるファイル形式のドキュメント型情報を追加できます。 ドキュメントの種類のアイコンが`CFBundleTypeIconFiles`配列に格納されます。 ドキュメントアイコンが指定されていない場合、iOS はアプリアイコンから1つを派生させます。
さまざまなデバイスの解像度に合わせて最適化された複数のサイズに対してアイコンを指定できます。 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

これらの値を Visual Studio for Mac に割り当てるには、 `Info.plist`エディターの **[詳細設定]** タブにある ドキュメントの **[種類]** セクションを使用して、ドキュメントの種類を追加し、イメージアイコンを割り当てます。 たとえば、PDF サポートの登録を示すスクリーンショットを次に示します。

 [![](custom-document-types-images/18.png "[情報] エディターの [詳細設定] タブにある [ドキュメントの種類] セクション")](custom-document-types-images/18.png#lightbox)
 
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

これらの値を Visual Studio で割り当てるには、の`Info.plist` **[詳細設定]** タブの **[ドキュメントの種類]** セクションを使用します。

 ![](custom-document-types-images/doc01w.png "[詳細設定] タブの [ドキュメントの種類] セクションを開く")

**[ドキュメントの種類の追加]** ボタンをクリックし、必要なフィールドを入力します。

![](custom-document-types-images/doc02w.png "ドキュメントの種類の追加フォーム")

-----


ドキュメントの種類の詳細については、「Apple の[Uniform Type Identifier リファレンス](https://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html)」および「 [IOS のドキュメント相互作用プログラミング](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html)に関するトピック」を参照してください。


## <a name="related-links"></a>関連リンク

- [イメージの操作 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [カスタムアイコンとイメージ作成のガイドライン](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
