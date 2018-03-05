---
title: "カスタム ドキュメント アイコン"
description: "この資料について説明を含むとカスタム ドキュメントの種類のアイコンとして使用する Xamarin.iOS アプリでのイメージ資産を管理します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/23/2017
ms.openlocfilehash: 582fcbacbf1959e05773babb1219817ba319a937
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="custom-document-icons"></a>カスタム ドキュメント アイコン

_この資料について説明を含むとカスタム ドキュメントの種類のアイコンとして使用する Xamarin.iOS アプリでのイメージ資産を管理します。_

Xamarin.iOS アプリでは、特定のドキュメント型の読み込みをサポートする場合、提供するには、ユーザーがの添付ファイルを押したときなど、そのドキュメントの種類を検出すると、システムが使用されるアイコン、*メール アプリケーション*として次に示します。

 [ ![](custom-document-types-images/17.png "ドキュメントの種類のアイコンの例")](custom-document-types-images/17.png)

ディクショナリ エントリを含めることによって、ファイル形式、アプリが開始できるため、開発者がドキュメント型情報を追加できます、`CFBundleTypeName`文字列と`LSItemContentTypes`配列で、アプリの`Info.plist`します。 ドキュメントの種類のアイコンがで go、`CFBundleTypeIconFiles`配列。 ドキュメント アイコンが指定されていません、iOS はアプリ アイコンのいずれかに派生されます。
アイコンは、さまざまなデバイス解像度用に最適化されたいくつかのサイズを指定できます。 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 用 Visual Studio でこれらの値を割り当てるには使用、**ドキュメントの種類**セクション、 **[詳細設定]**タブで、`Info.plist`ドキュメントの種類を追加およびイメージ アイコンを割り当てるのためのエディターです。 たとえば、PDF のサポートのための登録を示すスクリーン ショットを次に示します。

 [ ![](custom-document-types-images/18.png "'Info.plist' エディターの詳細設定 タブの下のセクションでドキュメントの種類")](custom-document-types-images/18.png)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio でこれらの値を割り当てるには、使用、**ドキュメントの種類**セクション、**詳細**タブで、 `Info.plist`:

 ![](custom-document-types-images/doc01w.png "詳細設定 タブの下にあるドキュメントの種類」セクションを開きます")

をクリックして、**ドキュメントの種類の追加** ボタンをクリックし、必要なフィールドを入力します。

![](custom-document-types-images/doc02w.png "ドキュメントの種類の追加フォーム")

-----


ドキュメントの種類の詳細については、Apple を参照してください。 [Uniform 型識別子参照](http://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html)と[ドキュメントの対話のプログラミングについてのトピック iOS](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html)です。


## <a name="related-links"></a>関連リンク

- [イメージ (サンプル) の操作](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [カスタム アイコンをクリックしてイメージ作成のガイドライン](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
